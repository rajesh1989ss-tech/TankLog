TANK LOG — ANDROID (GitHub Pages PWA)
=====================================
Upload EVERYTHING in this folder to your GitHub repo (same level, no subfolder):
  index.html  manifest.webmanifest  sw.js  icon-192.png  icon-512.png

1. github.com -> your repo (e.g. Tank-Log) -> Add file -> Upload files -> drop all 5 -> Commit.
2. Settings -> Pages -> Deploy from branch -> main -> / (root) -> Save.
3. Open https://<username>.github.io/<repo>/ on the phone in Chrome.
4. Chrome menu (three dots) -> "Add to Home screen" / "Install app".
   It opens full-screen with the R-tanker icon, works offline, and keeps
   its data in the phone (localStorage) exactly like the browser version.

Updating later: upload the new index.html over the old one. The service
worker is network-first, so the phone picks up the new version on the next
online open (pull-down refresh once if needed).

NOTE: data lives per-device. Move data between phone and desktop with
Master -> Settings -> Export / Import JSON (merge-safe, import twice is harmless).
