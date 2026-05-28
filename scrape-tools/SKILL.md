---
name: multi-images-online-download
description: |
  Scrape and bulk-download images from any website. Use this skill whenever the user asks to
  "pull images from a site", "grab photos from a website", "download images from [URL]",
  "scrape images", "save all the photos from [brand/company]", "get product images", or any
  request that involves collecting multiple images from one or more web pages and saving them
  locally. Also triggers when the user mentions "image scraping", "bulk image download",
  "save website images", "pull assets from a site", or wants reference/mood board images
  from a specific URL. Even if the user just says "grab me some images from [site]" or
  "I need photos from their website", use this skill.
---

# Multi Images Online Download

Bulk-scrape images from any website and deliver them as a zip file the user can download
in one click. This skill exists because the sandbox proxy blocks most CDN domains, and
Chrome extension tabs can't trigger real downloads on the user's Mac. The workaround is
to generate a standalone HTML downloader page that runs in the user's normal browser
with full download permissions.

## How it works (three phases)

### Phase 1: Browse and collect image URLs

Use the Chrome MCP (`mcp__Claude_in_Chrome__*`) to visit the target site and extract
image URLs. The goal is to build a clean list of high-quality image URLs with
descriptive filenames.

**Setup:**
1. Get the Chrome tab context (`tabs_context_mcp` with `createIfEmpty: true`)
2. Navigate to the target URL
3. Dismiss cookie banners and popups (click X or close buttons)

**Collecting images — use JavaScript execution to extract URLs:**

```javascript
// Run this on each page to collect images
const imgs = [];
document.querySelectorAll('img').forEach(img => {
  if (img.src && img.naturalWidth > 200) {
    const url = new URL(img.src);
    imgs.push({ url: url.origin + url.pathname, alt: (img.alt || '').slice(0, 60) });
  }
});
JSON.stringify([...new Map(imgs.map(i => [i.url, i])).values()], null, 2);
```

**Tips for getting good coverage:**
- Visit multiple pages: homepage, product pages, blog, about, gallery
- Scroll down on each page before collecting (lazy-loaded images need scrolling to appear)
- Check for product gallery images specifically (they often use slider/carousel classes)
- Filter out icons, logos, SVGs, and tiny images (naturalWidth > 200 is a good threshold)
- Strip query parameters from URLs to get clean paths (but keep them for CDN URLs that need width params)
- Use relative URLs (like `/cdn/shop/files/...`) when on the same domain — they fetch more reliably

**Building the image list:**
As you collect URLs across pages, deduplicate and assign descriptive filenames based on
the URL path, alt text, or page context. Group them mentally into categories
(product, lifestyle, hero, etc.) so the filenames are useful.

### Phase 2: Generate the HTML downloader page

This is the key part. Because neither the sandbox nor the Chrome extension can reliably
save files to the user's machine, you create a self-contained HTML file that the user
opens in their browser. It fetches all images client-side, bundles them into a zip using
JSZip, and triggers a real browser download.

**Create the HTML file in the user's workspace folder.** Use the template in
`assets/downloader-template.html` as a starting point — read it and customize:

1. Replace the `IMAGES` array with the collected URLs and filenames
2. Update the title and subtitle text to describe what's being downloaded
3. Set the zip filename to something descriptive (e.g., `UE-images.zip`)
4. Optionally adjust the color scheme to match the brand being scraped

Then save the HTML file to the user's workspace folder and present it using `present_files`.

### Phase 3: Tell the user what to do

After generating the HTML file:

1. Let the user know the file is in their folder
2. Tell them to open it in Chrome (double-click from Finder)
3. Explain they click the download button and it generates a zip
4. Mention roughly how many images and the expected zip size
5. Offer to help them move/organize the images after download

## Important constraints

**The sandbox cannot reach most CDN domains.** Shopify CDN, Cloudflare, and most
image hosting services are blocked by the egress proxy. Do not waste time trying
`curl`, `wget`, or Python `requests` — they will fail with 403/connection errors.

**Chrome extension downloads don't land in the user's Downloads folder.** The extension
runs in a sandboxed tab group. JavaScript-triggered downloads (`a.click()` with
`download` attribute, or fetch-to-blob downloads) from the extension tab do not appear
in Finder. Do not attempt multiple rounds of this — it won't work.

**The HTML downloader page works because it runs in the user's normal Chrome context**
when they open the file directly. Same-origin fetch works for most CDN images, and
the download attribute works properly for blob URLs created in a normal browsing context.

## Handling cross-origin failures

Some images may fail to fetch from the HTML downloader due to CORS restrictions.
If the user reports that some images failed:

1. Identify which CDN domain is blocking
2. For those specific images, provide direct URLs the user can open in a browser tab
   and save manually (right-click → Save Image As)
3. Or suggest the user visit the page directly and save from there

## Example workflow

User: "Pull a bunch of images from nike.com/running"

1. Open Chrome tab, navigate to nike.com/running
2. Scroll through the page, collect image URLs via JS
3. Visit 2-3 linked product pages for more images
4. Build list of ~15-20 high-quality images with descriptive names
5. Generate `nike-running-images/download-images.html` in workspace
6. Present the file and tell the user to open it in Chrome
7. User clicks download, gets `nike-running-images.zip`
