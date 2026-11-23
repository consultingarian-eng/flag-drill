# Flag Drill

Flag Drill is an offline flag quiz made for the iPhone home screen. It covers 197 countries with emoji flags and
keeps progress in `localStorage` under keys that start with `flagdrill:`. The app is a static site with no build
step. It is installed as a PWA and must keep working in airplane mode.

## Files
- `index.html` holds the whole app (markup, styles and one inline script).
  - Countries live in the `RAW` table in that script, one row each: `CODE|Name|Region|alias;alias`.
- `sw.js` is the service worker. It caches the files listed in `FILES` under the name in `CACHE`, then answers
  from that cache first.
- `manifest.json` is the PWA manifest. `icon-180.png`, `icon-192.png` and `icon-512.png` are the home-screen icons.

## Rules
- Bump `CACHE` in `sw.js` on every change to any shipped file (`flag-drill-v1` → `flag-drill-v2` → …). If you
  don't, installed copies keep serving the old files.
- When you add a shipped file, add it to `FILES` in `sw.js` too.
- Keep every path relative (`./`, `index.html`). The site is served from a `/flag-drill/` subpath, so absolute
  paths break.
- Load nothing from another domain: no CDNs, web fonts, analytics or trackers. Anything off-site breaks offline use.
- Never rename existing `flagdrill:` storage keys. Renaming one wipes players' progress.

## Deploy
- GitHub Pages builds from the root of `main`, so a push to `main` is a deploy. There is no CI.
- The repo must stay public, because GitHub Pages on the free plan doesn't serve sites from private repos.

## Check before pushing
- Serve the folder locally with `python3 -m http.server 8000` and open http://localhost:8000/.
- Play a round, then reload and confirm progress persists.
- Test offline:
  - Wait for the service worker to be ready.
  - Switch the browser offline (DevTools, or Playwright's `setOffline(true)`).
  - Reload. The page must still load.

## Working rules
- Run `git pull --ff-only` before starting; other clones of this repo push too.
- Push right after every commit. Know what a push to `main` deploys (see Deploy) before you make one.
- Never commit secrets, keys or connection strings.
