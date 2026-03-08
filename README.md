# Quiet Thoughts

<div align="center">

## 🌐 Live HTML Preview

[![Acess link here](https://img.shields.io/badge/▶%20Open%20Live%20Site-Current%20Version-brightgreen?style=for-the-badge)](https://satyam-64136.github.io/Thoughts_i_loved/v1_space.html)

| Version | Description | Link |
|:---:|:---|:---:|
| **Latest** | Current live version *(this repo)* | [** Open**](https://satyam-64136.github.io/Thoughts_i_loved/v1_space.html) |
| **v1** | First redesign | [Open](https://satyam-64136.github.io/Thoughts_i_loved/) |
| **v0** | Original version | [Open](https://satyam-64136.github.io/Thoughts_i_loved/index_quo.html) |

</div>

---

A calm, reflective single-page website that displays personal quotes loaded automatically from a Google Docs document. Built with vanilla HTML, CSS, and JavaScript — no frameworks, no build step. Fully compatible with GitHub Pages.

---

## Features

- **Google Docs sync** — quotes are fetched directly from a Google Doc and rendered as cards. The page refreshes every 3 minutes automatically.
- **Glassmorphism cards** — frosted-glass quote cards with smooth hover effects and scroll-reveal animations.
- **Ambient particle canvas** — 45 softly drifting particles that pick up the active accent color.
- **Copy to clipboard** — each card has a one-click copy button.
- **Dark / Light mode** — toggle at the bottom of the page, preference saved in `localStorage`.
- **4 accent palettes** — Indigo Mist, Dusty Rose, Slate Teal, Warm Sand. All saved across sessions.
- **Instagram link** — footer link to the author's Instagram profile.
- **Mobile responsive** — single-column on mobile, auto-fill grid on tablet, 3-column on desktop.
- **Zero dependencies** — pure vanilla JS, no npm, no build tools.

---

## How It Works

Quotes are stored in a public Google Doc, one per line. The site fetches the plain-text export of that document through a CORS proxy and splits it by line breaks into individual cards.

```
Google Doc (plain text export)
        ↓
corsproxy.io (CORS proxy)
        ↓
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

Open `index.html` and update the `DOC_ID` constant near the bottom of the `<script>` tag:

```js
const DOC_ID = "YOUR_GOOGLE_DOC_ID_HERE";
```

To find your Doc ID, open the document in Google Docs. The ID is the long string in the URL:

```
https://docs.google.com/document/d/THIS_IS_YOUR_DOC_ID/edit
```

Make sure the document is set to **"Anyone with the link can view"** (Share → General access → Anyone with the link).

### 3. Deploy to GitHub Pages

1. Push your code to a GitHub repository.
2. Go to **Settings → Pages**.
3. Under **Source**, select the `main` branch and `/ (root)`.
4. Click **Save**. Your site will be live at `https://your-username.github.io/your-repo-name/`.

No build step required — the single `index.html` file is all that's needed.

---

## Customization

### Change the author name

Search for `Satyam` in `index.html` and replace with your name. It appears in three places: the footer, the copy text format (`"quote" — Satyam`), and the page title.

### Change the Instagram link

Find the `<a class="ig-link"` tag in the footer and update the `href` attribute.

### Add or remove accent colors

Accent palettes are defined as CSS attribute selectors at the top of the `<style>` block:

```css
[data-ac="rose"] {
  --ac:      #c49090;
  --ac-dim:  rgba(196,144,144,0.12);
  --ac-line: rgba(196,144,144,0.28);
  --ac-glow: rgba(196,144,144,0.07);
}
```

Add a new block with a unique `data-ac` name, then add a corresponding dot in the footer HTML:

```html
<div class="ac-dot" data-ac="yourname" style="background:#hexcolor" title="Label"></div>
```

And register it in the `accentMap` object in the script:

```js
const accentMap = { indigo: '', rose: 'rose', teal: 'teal', sand: 'sand', yourname: 'yourname' };
```

### Adjust the auto-refresh interval

The default is 3 minutes (180,000 ms). Change it at the bottom of the script:

```js
setInterval(() => loadQuotes(true), 180_000); // change 180_000 to any ms value
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

## Browser Support

Works in all modern browsers. Requires:
- `fetch` API
- `IntersectionObserver`
- `navigator.clipboard`
- CSS `backdrop-filter` (cards degrade gracefully without it)

---

## License

Personal project — feel free to fork and adapt for your own use.

*Made with quiet intention.*
