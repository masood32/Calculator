# Zakat & Fitra Calculator — Progressive Web App (PWA)

The PWA version makes the Zakat & Fitra calculator installable directly from a browser — no app store required. Once installed, it works offline and behaves like a native app with its own icon on the home screen.

---

## What is a PWA?

A Progressive Web App is a website that can be "installed" on a device:

- Opens in its own window (no browser chrome/address bar)
- Has a home screen / desktop icon
- Works **offline** after first load
- No app store needed — install directly from the browser

---

## Files Required

| File | Purpose |
|------|---------|
| `zakat-fitra-calculator.html` | The calculator app |
| `manifest.json` | PWA identity (name, icon, colors, display mode) |
| `sw.js` | Service worker — enables offline caching |
| `icon-192.png` | App icon (192×192 px) |
| `icon-512.png` | App icon (512×512 px) |

> The two icon files are not included — see [Creating Icons](#creating-icons) below.

---

## Deployment on masoodai.com (Hostinger)

### Option A — Root Path
Upload files to `public_html/`:
```
public_html/
├── zakat-fitra-calculator.html
├── manifest.json
├── sw.js
├── icon-192.png
└── icon-512.png
```
Access at: `masoodai.com/zakat-fitra-calculator.html`

### Option B — Clean URL (Recommended)
Upload to a subfolder:
```
public_html/
└── zakat/
    ├── index.html        ← renamed from zakat-fitra-calculator.html
    ├── manifest.json
    ├── sw.js
    ├── icon-192.png
    └── icon-512.png
```
Access at: `masoodai.com/zakat/`

### Upload via Hostinger File Manager
1. Log in to hPanel → **File Manager**
2. Navigate to `public_html/` (or create a `zakat/` subfolder)
3. Upload all files listed above
4. Visit the URL in Chrome or Edge to verify it loads

---

## Service Worker

`sw.js` handles caching for offline use:

```javascript
const CACHE = 'zakat-calc-v1';
const ASSETS = ['/zakat-fitra-calculator.html'];
```

**How it works:**
1. **Install** — caches the HTML file on first visit
2. **Activate** — removes old cache versions automatically
3. **Fetch** — serves from cache when offline; falls back to network when online

> If you update the HTML file, increment the cache version in `sw.js` (e.g., `zakat-calc-v2`) so users get the latest version.

---

## PWA Manifest

`manifest.json` defines how the app appears when installed:

```json
{
  "name": "Zakat & Fitra Calculator",
  "short_name": "Zakat Calc",
  "start_url": "/zakat-fitra-calculator.html",
  "display": "standalone",
  "background_color": "#0f2027",
  "theme_color": "#c9a84c"
}
```

| Field | Description |
|-------|-------------|
| `name` | Full name shown during install prompt |
| `short_name` | Name shown under home screen icon |
| `start_url` | Page opened when app icon is tapped |
| `display: standalone` | No browser address bar — looks like a native app |
| `background_color` | Splash/loading background color |
| `theme_color` | Browser UI accent color (status bar on Android) |

---

## How Users Install the PWA

### On Android (Chrome)
1. Open `masoodai.com/zakat/` in Chrome
2. Tap the **three-dot menu** → **Add to Home screen**
3. Tap **Add** — the icon appears on the home screen
4. Tap the icon to launch in standalone mode (no browser bar)

> On some devices, Chrome shows an automatic **"Install App"** banner at the bottom.

### On iPhone / iPad (Safari)
1. Open the URL in Safari
2. Tap the **Share** button (box with arrow)
3. Tap **Add to Home Screen**
4. Tap **Add**

### On Desktop (Chrome / Edge)
1. Open the URL
2. Look for the install icon in the address bar (monitor with down arrow)
3. Click **Install**
4. The app opens in its own window

---

## Creating Icons

The app requires two PNG icons. You can create them for free:

### Option 1 — Online Generator
1. Go to [realfavicongenerator.net](https://realfavicongenerator.net)
2. Upload any square image (logo or crescent/star symbol)
3. Download the generated package
4. Extract `android-chrome-192x192.png` → rename to `icon-192.png`
5. Extract `android-chrome-512x512.png` → rename to `icon-512.png`

### Option 2 — Manual (Any Image Editor)
- Create a square image (recommended: dark `#0f2027` background + gold `#c9a84c` symbol)
- Export at 192×192 px → `icon-192.png`
- Export at 512×512 px → `icon-512.png`

### Option 3 — Use a Placeholder
For testing without icons, remove the `icons` section from `manifest.json`. The PWA will still install but will use a generic icon.

---

## Offline Behaviour

| Scenario | Result |
|----------|--------|
| First visit (online) | Calculator loads and is cached |
| Subsequent visit (online) | Served from cache (faster load) |
| Visit while offline | Works fully — served from cache |
| Export (PNG/PDF) while offline | Fails — requires CDN for html2canvas/jsPDF |

---

## Updating the PWA

After uploading a new version of the HTML:

1. Open `sw.js`
2. Change the cache version:
   ```javascript
   const CACHE = 'zakat-calc-v2'; // increment this
   ```
3. Upload the updated `sw.js`
4. On next visit, the service worker activates, clears the old cache, and caches the new version

---

## HTTPS Requirement

PWA features (service worker, install prompt) require **HTTPS**. Hostinger provides free SSL certificates:

1. hPanel → **SSL** → **Enable** for your domain
2. Ensure your domain uses `https://` — the install prompt will not appear on `http://`

---

## Technical Details

| Property | Value |
|----------|-------|
| Cache strategy | Cache-first with network fallback |
| Cache scope | Main HTML file |
| Service worker scope | Root path (`/`) |
| PWA display mode | `standalone` |
| Orientation | `portrait-primary` |
| Browser support | Chrome 67+, Edge 79+, Firefox 79+, Safari 14.1+ (iOS) |

---

## Disclaimer

This calculator is a guide only. Zakat rules may vary by school of thought. Always verify your calculation with a qualified Islamic scholar.
