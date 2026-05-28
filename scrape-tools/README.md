# Scrape Tools — Bulk Image Downloader

Pulls every image off a website and packages them into a single `.zip` you can drag into a folder. Works in any normal Chrome window. No coding required, no AI required, no Claude required.

---

## What's in this folder

| File | What it is |
|---|---|
| `SKILL.md` | Original instructions written for Claude Code. Reference only — the workflow below replaces it. |
| `assets/downloader-template.html` | The HTML downloader. This is the actual tool. |
| `README.md` | This file. |

---

## The 3-step workflow

### 1. Get the image URLs off the target site

Open the page you want images from in Chrome. Press **`Option + Command + J`** (Mac) to open the JavaScript console. Paste this and hit Enter:

```javascript
const imgs = [];
document.querySelectorAll('img').forEach(img => {
  if (img.src && img.naturalWidth > 200) {
    const url = new URL(img.src);
    imgs.push({ url: url.origin + url.pathname, name: (img.alt || 'image').slice(0,40).replace(/[^a-z0-9]+/gi,'-') + '.jpg' });
  }
});
copy(JSON.stringify([...new Map(imgs.map(i => [i.url, i])).values()], null, 2));
console.log('Copied ' + imgs.length + ' image URLs to clipboard');
```

This filters out tiny icons/logos (anything under 200px), dedupes, and copies a clean list to your clipboard.

**Tip:** Scroll all the way down the page *before* running the snippet — many sites lazy-load images and they won't exist in the DOM until you've scrolled past them.

**To grab from multiple pages:** run the snippet on each page, paste each result into a text file, then merge into one big JSON array.

---

### 2. Paste the URLs into the downloader

Open `assets/downloader-template.html` in a text editor (TextEdit works, VS Code is better).

Find this line near the top of the `<script>` section:

```javascript
const IMAGES = __IMAGES_JSON__;
```

Replace `__IMAGES_JSON__` with the array you copied. Should look like:

```javascript
const IMAGES = [
  { url: "https://example.com/cdn/shop/products/shoe-1.jpg", name: "shoe-1.jpg" },
  { url: "https://example.com/cdn/shop/products/shoe-2.jpg", name: "shoe-2.jpg" }
];
```

Also replace these placeholders anywhere they appear in the file:

| Placeholder | Replace with |
|---|---|
| `__TITLE__` | Page title, e.g. `Nike Running` |
| `__SUBTITLE__` | Short description, e.g. `15 product images` |
| `__ZIP_FILENAME__` | Name for the output zip, e.g. `nike-running.zip` |
| `__BG_COLOR__` | Background hex, e.g. `#0a0a0a` |
| `__TEXT_COLOR__` | Text hex, e.g. `#ffffff` |
| `__MUTED_COLOR__` | Secondary text hex, e.g. `#888888` |
| `__ACCENT_COLOR__` | Button color hex, e.g. `#ff4500` |

Save the file (you can rename it — e.g. `nike-download.html`).

---

### 3. Open the HTML file in Chrome and click DOWNLOAD ZIP

Double-click the HTML file from Finder. It opens in your default browser. Click the big button. A `.zip` lands in your Downloads folder containing every image.

---

## Why it works this way (the load-bearing reason)

Browsers will let a webpage fetch images from another domain and bundle them into a zip *as long as the page is opened from your local filesystem* (not a sandbox, not an extension tab). That's the trick. Most "scrape this site" tools fail because they're running inside a sandbox that blocks the CDN — this one doesn't, because it's running in your normal Chrome with full permissions.

## When images fail to download

Some sites add CORS headers that block cross-origin fetching even from a normal browser. If specific images error out:

1. The downloader will list them at the bottom of the page with red text.
2. Right-click each failed URL in the log → "Open in new tab" → right-click the image → "Save Image As..."

Most sites work fine. Shopify, Squarespace, Webflow, WordPress, most product CDNs all work.

## Common gotchas

- **Empty zip / 0 images:** you didn't scroll the page before running the snippet, so lazy-loaded images weren't in the DOM yet.
- **Wrong filenames:** edit the `name` field in the JSON before pasting. The snippet uses `alt` text — if the site has bad alt text you'll get gibberish names.
- **Big images come down small:** the snippet strips query parameters (which sometimes set image width). For some CDNs you need to keep the query string — remove the `url.origin + url.pathname` line and use `img.src` directly.
