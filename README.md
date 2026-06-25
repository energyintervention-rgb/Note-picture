# 📷 Note Picture

A mobile-first Progressive Web App (PWA) for capturing photos with timestamped notes and GPS coordinates. Single-file, no dependencies, works offline.

---

## Features

| Feature | Details |
|---|---|
| 📸 Camera | Live viewfinder with front/rear flip |
| 🔄 Retake | Preview before saving, retake any time |
| 📝 Note | Free-text note attached to each photo |
| 🕐 Timestamp | Live clock badge; exact capture time stored |
| 📍 GPS | Coordinates + accuracy radius; taps to Google Maps |
| 🗂 Gallery | Auto-fit grid (1–2 col by screen width) |
| 🗑 Delete | Per-entry delete or Delete All |
| 📴 Offline | Service worker caches the app shell |
| 📲 Installable | Full PWA — Add to Home Screen on Android & iOS |

---

## File Structure

```
index.html          ← entire app (single file, ~272 KB)
README.md           ← this file
```

Everything else — manifest, service worker, icons (32 / 180 / 192 / 512 px) — is embedded inside `index.html` as base64 data URIs and runtime Blob URLs. No build step, no dependencies.

---

## Deployment

### GitHub Pages (recommended — free HTTPS)

1. Create a new repository on GitHub (e.g. `note-picture`)
2. Upload `index.html` to the repository root
3. Go to **Settings → Pages → Branch: main → / (root) → Save**
4. App is live at `https://<your-username>.github.io/note-picture/`

### Netlify (drag & drop)

1. Go to [netlify.com](https://netlify.com) → **Add new site → Deploy manually**
2. Drag the `index.html` file into the drop zone
3. Done — Netlify assigns an HTTPS URL instantly

### Cloudflare Pages

1. Go to [pages.cloudflare.com](https://pages.cloudflare.com) → **Create a project → Direct Upload**
2. Upload `index.html`
3. Deploy

### Any static host

Upload `index.html` to any web server that serves over **HTTPS**. HTTP will not work — camera and GPS require a secure context.

> ⚠️ **HTTPS is mandatory.** Camera access (`getUserMedia`), GPS (`geolocation`), PWA install, and service workers are all blocked by browsers on plain HTTP.

---

## Installing as a Phone App

### Android (Chrome / Edge)
1. Open the app URL in Chrome
2. Tap the **"Add to Home Screen"** banner at the bottom, or go to browser menu → **Install app**
3. App icon appears on your home screen and runs in standalone (no browser chrome)

### iOS (Safari only)
1. Open the app URL in **Safari** (Chrome on iOS cannot install PWAs)
2. Tap the **Share** button (rectangle with arrow)
3. Scroll down and tap **"Add to Home Screen"**
4. Tap **Add** — icon appears on your home screen

> ℹ️ iOS does not show an automatic install prompt. The Safari share sheet method above is the only way.

---

## Permissions

The app requests two permissions on first load:

| Permission | Why | If denied |
|---|---|---|
| **Camera** | Live viewfinder and photo capture | App shows "Camera unavailable" message; no photos can be taken |
| **Location** | Embed GPS coordinates in each entry | Entry saves with "No GPS" — all other features still work |

To reset permissions: browser **Settings → Site permissions** → find the app URL → reset Camera / Location.

---

## Data Storage

Photos and notes are stored in **`localStorage`** under the key `notepic_entries` as a JSON array. Each entry contains:

```json
{
  "id": 1719302400000,
  "photo": "data:image/jpeg;base64,...",
  "note": "Panel assessment at Site A",
  "timestamp": "2026-06-25T09:42:00.000Z",
  "gps": {
    "lat": 3.81234,
    "lng": 103.32456,
    "acc": 12
  }
}
```

> ⚠️ **Storage limit:** `localStorage` is capped at ~5–10 MB per origin depending on the browser. Each JPEG photo is approximately 200–600 KB. Expect to store roughly 10–30 photos before hitting the limit. For heavy use, consider exporting entries and clearing the gallery periodically.

---

## Browser Support

| Browser | Camera | GPS | PWA Install |
|---|---|---|---|
| Chrome Android | ✅ | ✅ | ✅ |
| Safari iOS 16.4+ | ✅ | ✅ | ✅ (via Share sheet) |
| Firefox Android | ✅ | ✅ | ⚠️ Limited |
| Chrome Desktop | ✅ | ⚠️ (IP-based) | ✅ |
| Safari macOS | ✅ | ⚠️ (IP-based) | ✅ |
| Firefox Desktop | ✅ | ⚠️ (IP-based) | ❌ |

---

## Known Limitations

- **localStorage cap** — not suitable for very large photo collections; no export feature in current version.
- **Service worker scope** — the inline blob-URL service worker cannot cache external requests. Only the app shell (`./`) is cached. Full offline photo viewing works because photos are stored in localStorage, not the network cache.
- **iOS PWA** — iOS PWA camera access requires user gesture on every session; the viewfinder starts automatically on page load which covers this.
- **GPS accuracy** — accuracy radius depends on device hardware and environment. Indoor accuracy may be 20–100 m. The value is displayed alongside coordinates so the user is always informed.

---

## Tech Stack

| Layer | Technology |
|---|---|
| UI | Vanilla HTML5 + CSS3 (no framework) |
| Logic | Vanilla JavaScript (ES2020, no bundler) |
| Camera | `MediaDevices.getUserMedia()` |
| Capture | `HTMLCanvasElement.toDataURL()` |
| GPS | `Geolocation.getCurrentPosition()` |
| Storage | `localStorage` |
| Offline | Service Worker (Cache API) |
| PWA | Web App Manifest (runtime blob) |
| Icons | Embedded base64 PNG (32 / 180 / 192 / 512 px) |

---

## License

MIT — free to use, modify, and deploy for personal or commercial projects.

---

*Built with Claude — Anthropic*
