# Privacy HQ — Android project

Offline-ready wrapper around the Privacy HQ game (`www/index.html`).

## Method A: native `.apk` / `.aab` (Capacitor + Android Studio)
Requirements: Node 18+, JDK 17, Android Studio.
1. Extract this ZIP and open a terminal in the folder.
2. Run:
   ```bash
   npm install
   npx cap add android
   npx cap sync android
   npx cap open android
   ```
3. In Android Studio: **Build → Build Bundle(s) / APK(s) → Build APK(s)**.

After any change inside `www/`, run `npx cap sync android` again before rebuilding.

The app uses three Capacitor plugins (installed by `npm install`):
- `@capacitor/app`: the Android back button closes popups, and asks for a second press before exiting a run.
- `@capacitor/filesystem` + `@capacitor/share`: "Download CSV" in the admin panel opens the Android share sheet. Android WebViews ignore normal file downloads, so this is needed.

## Method B: PWA (no Android Studio)
1. Host the contents of `www/` on any HTTPS host (GitHub Pages, Netlify, Vercel).
2. Open the URL in Chrome on Android and tap **Install app / Add to Home screen**, or paste the URL into https://www.pwabuilder.com to get an `.apk`.

The service worker is network-first, so redeploying updates installed copies on their next online launch. Offline, it falls back to the last cached copy. It is not registered inside the native app.

## Method C: build the APK online with GitHub (no Android Studio)
1. Create a free GitHub account and a new repository.
2. Upload everything in this folder to it, including the hidden `.github` folder.
3. Open the repository's **Actions** tab, choose **Build Android APK**, then **Run workflow**.
4. After a few minutes, open the finished run and download **privacy-hq-apk** from the Artifacts section. Unzip it to get `app-debug.apk`.
5. Send the APK to your phone and tap it to install (allow "Install unknown apps" for the app you opened it from).

The APK is a debug build signed with a debug key. It installs fine for testing and sharing, but the Play Store needs a release build signed with your own key.

## Things to know
- **Admin PIN:** set `adminPin` in the `CONFIG` block near the top of the script in `www/index.html`. The default is `1234`. Change it before you hand the game out. The PIN is checked in the app itself, so it keeps casual players out but is not real security.
- **Leaderboard is per device.** Scores are stored in the app's local storage on each phone. Uninstalling or clearing app data erases them. Phones do not share a leaderboard; export the CSV from each device if you need to combine results.
- **Ranking:** wins rank above failed runs, then by score. Only each player's best run per level is kept (names are compared ignoring case). Use the level filter in the admin panel to compare like with like.
- **Touch:** one finger scrolls the page, two fingers pan and zoom the building (or use the + / − buttons). Tap a room to collect.
- **Icons:** PNG icons (`icon-192.png`, `icon-512.png`, `icon-maskable-512.png`) are included for installability. Edit `icon.svg` / `icon-maskable.svg` and re-export them if you change the artwork.
