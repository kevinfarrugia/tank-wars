# Tank Wars

An artillery duel for up to eight tanks, installable as a full-screen web app.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole game: engine, UI, saves |
| `manifest.webmanifest` | App metadata, set to `display: fullscreen` and `orientation: landscape` |
| `sw.js` | Service worker, precaches everything for offline play |
| `icon-192.png`, `icon-512.png` | App icons |
| `icon-maskable-512.png` | Android adaptive icon with a 12% safe zone |
| `apple-touch-icon.png`, `favicon-64.png` | iOS home screen and browser tab |

Keep all of them in the same folder — the manifest and service worker use relative paths, so the set works from any subdirectory.

## Running it

Service workers require `http://localhost` or HTTPS. Opening `index.html` from the file system works as a game but will **not** install or cache.

Locally:

```bash
cd tank-wars-pwa
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

To host it, drop the folder on any static host — GitHub Pages, Netlify, Cloudflare Pages, S3 — all of which serve HTTPS by default. No build step and no dependencies.

## Installing

- **Android / Chrome / Edge:** an Install button appears on the title screen, or use the browser menu's Install app.
- **iPhone / iPad:** Share → Add to Home Screen. iOS ignores `display: fullscreen` and uses standalone mode, so you get a full-bleed app without browser chrome but with the status bar.
- **Desktop Chrome / Edge:** the install icon in the address bar.

Launched from the home screen it opens full screen in landscape. In a normal browser tab, starting or resuming a match requests full screen and tries to lock the orientation; the in-game menu has a toggle for it.

## Notes

- Saved matches live in `localStorage` under `tankwars.save.v2`, separate from the service worker cache. Clearing site data drops both.
- To ship an update, bump `VERSION` in `sw.js`; the old cache is deleted on activation.
