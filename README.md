# Household Budget Planner — PWA

A self-contained, installable budget planner. No backend, no build step —
just static files. Data is stored locally in the browser (`localStorage`),
scoped per signed-up email/password on this device.

Why this fixes the earlier "data disappears on refresh" problem: that
version was running inside a sandboxed preview iframe, where browser
storage isn't guaranteed to survive a reload. Once you host these files
yourself on a real domain (GitHub Pages), the browser treats it as a
normal first-party website and `localStorage` persists normally across
refreshes, tab closes, and reopening the browser.

## Files in this folder

```
index.html        the whole app (HTML + CSS + JS)
manifest.json      PWA metadata (name, icons, colors)
sw.js              service worker — enables offline use + "Install App"
icons/icon-192.png
icons/icon-512.png
```

## 1. Deploy to GitHub Pages

1. Create a new GitHub repository (public or private both work for Pages
   on a paid plan; public repos get free Pages on any plan).
2. Upload all the files in this folder to the **root** of that repository,
   keeping the `icons/` folder structure intact.
3. In the repo, go to **Settings → Pages**.
4. Under **Build and deployment**, set **Source** to `Deploy from a
   branch`, pick the branch (usually `main`) and folder `/ (root)`, then
   **Save**.
5. GitHub gives you a URL like:
   `https://<your-username>.github.io/<repo-name>/`
   It can take 1–2 minutes to go live the first time.

That's it — no server, no environment variables, nothing else to
configure.

## 2. Install it as an app

Open the GitHub Pages URL on the device you want to use it on:

- **Desktop Chrome/Edge:** click the install icon in the address bar, or
  the "⬇ Install App" button that appears in the app's header.
- **Android Chrome:** tap the "⬇ Install App" button, or the browser's
  "Add to Home screen" menu option.
- **iPhone/iPad Safari:** Safari doesn't fire the install prompt banner —
  use Share → **Add to Home Screen** instead. It'll behave like an
  installed app (own icon, no browser bar).

Once installed, it opens full-screen like a native app and keeps working
offline (the service worker caches the app shell).

## 3. Data & accounts

- Sign-up creates an account (name, email, password) stored **only in
  this browser** — this is local personalization, not a real login
  system with server-side verification. It's meant to keep your data
  logically separated, e.g. between family members, not to secure it
  against someone with access to the same device/browser.
- Each email gets its own private budget data on this device.
- Use **Settings → Backup** regularly, especially before clearing browser
  data, switching browsers, or moving to a new device — `localStorage`
  never syncs between devices or browsers on its own.
- **Settings → Reset All Data** requires typing `RESET` to confirm and
  only clears the currently signed-in account's data.

## 4. Updating the app later

If you (or Claude) make changes to `index.html`, `manifest.json`, or the
icons:

1. Replace the files in the GitHub repo (commit the changes).
2. Open `sw.js` and bump the version string, e.g.
   `const CACHE_NAME = "budget-planner-v2";` — this forces the service
   worker to fetch fresh files instead of serving the old cached version
   to people who already installed it.
3. Push. GitHub Pages redeploys automatically within a minute or two.

## 5. Limitations (unchanged from the browser-only version)

- No real multi-device sync — data lives in that one browser's storage.
- No true server-side authentication — anyone with access to the same
  browser profile can open any account created there.
- No live Google Sheets sync — that would require a small backend
  (e.g. Google Apps Script Web App) to keep credentials off the client;
  the Settings tab has a placeholder for the endpoint but sync isn't
  wired up.
