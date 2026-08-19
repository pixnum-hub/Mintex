# Mintex — PWA package

## Files
```
index.html            the app
manifest.json         PWA manifest (name, icons, colors, display mode)
sw.js                  service worker (offline caching)
favicon.ico
icons/
  favicon-16.png, favicon-32.png
  apple-touch-icon.png                      (180×180)
  icon-72/96/128/144/152/180/192/384/512.png
  icon-maskable-192.png, icon-maskable-512.png
```

Keep this folder structure intact — `index.html` expects `manifest.json`, `sw.js`,
`favicon.ico`, and `icons/` to sit right next to it.

## About the icon
Your artwork already had real transparency, so the standard icons use it as-is.
The **maskable** icons (192/512) are scaled to 78% and centered on a white square,
since the letters and laptop reach close to the edges — without that padding,
Android's adaptive icon shapes (circle, squircle, etc.) would clip them. The
**apple-touch-icon** is flattened onto white at full bleed, since iOS renders
transparent icon pixels as black. Brand colors (`theme_color` #1647C4,
`background_color` #FFFFFF) were sampled from the artwork's blue and white.

## Hosting requirements
Service workers — and therefore installability and offline support — only work
over **HTTPS**, or on `localhost` for local testing. Plain `http://` hosting will
load the app fine but the install prompt and offline caching won't activate.

## Testing locally
```
cd this-folder
python3 -m http.server 8080
```
Then open `http://localhost:8080` — this counts as a secure context.

## No install prompt / no "Add to Home Screen"?
This package has been checked against Chrome's full installability checklist:
valid manifest with name/short_name/start_url/display, a 192px and 512px
"any"-purpose icon, every icon file present at its declared pixel size, and a
service worker that registers with a fetch handler. If it's still not
installable after a real HTTPS/localhost deploy:

- **Opening the file directly (`file://...`)** never works — this is the #1 cause.
- There's a **"⬇ Install app"** button in the top-right corner. It stays hidden
  until the browser itself confirms installability, then appears automatically.
  If it never appears after a proper deploy, check DevTools → Application →
  Manifest for the specific error.
- **iOS Safari has no automatic prompt at all** — Apple doesn't support
  `beforeinstallprompt`. "Add to Home Screen" there is always manual: Share
  button → Add to Home Screen. This is a platform limitation, not fixable in code.
- Desktop Chrome/Edge show an install icon (⊕) in the address bar once criteria
  are met — it can take a reload or two to appear the first time.
