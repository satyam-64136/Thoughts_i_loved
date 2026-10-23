# Quiet Thoughts

<div align="center">

## 🌐 Live HTML Preview

[![Access link here](https://img.shields.io/badge/▶%20Open%20Live%20Site-Current%20Version-brightgreen?style=for-the-badge)](https://thoughts.enigma.qd.je)

| Version | Description | Link |
|:---:|:---|:---:|
| **Latest** | Current live version *(this repo)* | [**Open**](https://thoughts.enigma.qd.je) |
| **v2** | Previous version | [Open](https://satyam-64136.github.io/Thoughts_i_loved/v1_space.html) |
| **v1** | First redesign | [Open](https://satyam-64136.github.io/Thoughts_i_loved/) |
| **v0** | Original version | [Open](https://satyam-64136.github.io/Thoughts_i_loved/index_quo.html) |

**🔗 https://thoughts.enigma.qd.je**

</div>

---

A calm, reflective single-page website that displays personal quotes loaded automatically from a Google Docs document. Built with vanilla HTML, CSS, and JavaScript — no frameworks, no build step. Hosts anywhere static files are served.

---

## Features

- **Google Docs sync** — quotes are fetched from a Google Doc and rendered as cards. The page refreshes every 3 minutes automatically.
- **Reliable loading** — fetches through a Google Apps Script endpoint first, then races several public proxies in parallel as backup.
- **Offline fallback** — the last loaded quotes are cached in the browser and shown instantly, even if every source fails.
- **Glassmorphism cards** — frosted-glass quote cards with smooth hover effects and scroll-reveal animations.
- **Animated ambient background** — soft drifting color orbs that react to cursor/touch movement, with a subtle film-grain overlay.
- **Live search** — instantly filter quotes as you type.
- **Copy to clipboard** — each card has a one-click copy button.
- **3 themes** — Deep Night, Soft Light, Calm Violet. Preference saved in `localStorage`.
- **Mobile responsive** — fluid single-column layout that works on all screen sizes.
- **Zero dependencies** — pure vanilla JS, no npm, no build tools.

---

## How It Works

Quotes are stored in a Google Doc, one per line. A small Google Apps Script exposes the document text, and the site fetches it and splits it by line breaks into individual cards.

```
Google Doc
        ↓
Google Apps Script web app (CORS-enabled)
        ↓  (fallback: public CORS proxies → cached copy)
fetch() in the browser
        ↓
Split by \n → filter empty lines → render as cards
```

To update quotes, simply edit the Google Doc. The site will reflect changes within 3 minutes (or immediately on page reload).

---

## Setup & Deployment

### 1. Fork or clone

```bash
git clone https://github.com/your-username/quiet-thoughts.git
cd quiet-thoughts
```

### 2. Point to your own Google Doc

Open `index.html` and update the `DOC_ID` constant in the `<script>` tag:

```js
const DOC_ID = "YOUR_GOOGLE_DOC_ID_HERE";
```

To find your Doc ID, open the document in Google Docs. The ID is the long string in the URL:

```
https://docs.google.com/document/d/THIS_IS_YOUR_DOC_ID/edit
```

Make sure the document is set to **"Anyone with the link can view"** (Share → General access → Anyone with the link).

### 3. Create the Apps Script endpoint (one-time)

1. Go to [script.google.com](https://script.google.com) → **New project**.
2. Paste this code:

```js
function doGet() {
  return ContentService.createTextOutput(
    DocumentApp.openById("YOUR_GOOGLE_DOC_ID_HERE").getBody().getText()
  );
}
```

3. Click **Deploy → New deployment → Web app**, set **Who has access: Anyone**, and deploy.
4. Copy the `/exec` URL and paste it into `index.html`:

```js
const APPS_SCRIPT_URL = "https://script.google.com/macros/s/XXXX/exec";
```

> After editing the script later, redeploy as a **new version** or changes won't go live.

### 4. Deploy

Upload `index.html` to any static host (GitHub Pages, Netlify, Vercel, etc.).

For GitHub Pages: **Settings → Pages → Source: `main` branch, `/ (root)` → Save**. To use a custom domain like `thoughts.enigma.qd.je`, add it under **Settings → Pages → Custom domain** and point a `CNAME` record to `your-username.github.io`.

No build step required — the single `index.html` file is all that's needed.

---

## Customization

### Change the author name

Search for `Satyam` in `index.html` and replace with your name. It appears in the page title, header, and footer.

### Edit or add themes

Themes are defined as CSS attribute selectors at the top of the `<style>` block:

```css
[data-theme="violet"] {
  --bg-a:#0f0d18; --bg-b:#131020; --bg-c:#181428;
  --accent:#b09cdc;
  /* ...other variables */
}
```

Add a new block with a unique `data-theme` name, then add a matching button in the footer:

```html
<button class="theme-pill" data-theme="yourname">
  <span class="swatch" style="background:#hexcolor"></span>Your Label
</button>
```

### Adjust the auto-refresh interval

The default is 3 minutes. Change it at the bottom of the script:

```js
setInterval(() => loadQuotes(true), 3*60*1000); // change to any ms value
```

---

## File Structure

```
quiet-thoughts/
└── index.html        # Everything — HTML, CSS, JS in one file
```

That's it. No `package.json`, no `node_modules`, no config files.

---

## Google Doc Format

Write one quote per line. Blank lines and lines shorter than 6 characters are automatically ignored.

**Example:**

```
Growth is not a destination, it is a direction.
Healing doesn't mean the damage never existed.
Some truths are only visible in silence.
Be honest with yourself before anyone else.
```

---

## Troubleshooting

| Problem | Fix |
|---|---|
| "Could not reach the document" | Confirm the doc is shared publicly, and open your `/exec` URL in a tab — it should show raw text |
| New quote not showing | Wait up to 3 minutes or hard-refresh |
| Script changes not applying | Redeploy the Apps Script as a new version |

---

## Browser Support

Works in all modern browsers. Requires:
- `fetch` API
- `IntersectionObserver`
- `navigator.clipboard`
- CSS `backdrop-filter` (cards degrade gracefully without it)

---

## License

Personal project - feel free to fork and adapt for your own use.

*Made with quiet intention.*
