# Pin. website

The landing page (`index.html`) and the download page (`download.html`) for
the Pin. app, ready for Vercel. No build step: it is a plain static site.

```
index.html            home page with the scroll demos
download.html         "Get early access" page with the APK download
404.html              page shown for broken links
downloads/Pin.apk     the Android app (version 1.0.0)
assets/css, js, img   styles, scripts, images
assets/fonts          self-hosted fonts (OFL, see LICENSES.txt)
vercel.json           clean URLs, caching and APK download headers
```

## Put it on Vercel (about 5 minutes)

The APK is 79.5 MB. GitHub's website only accepts files up to 25 MB, so
deploy with the Vercel command line instead:

1. Install Node.js from https://nodejs.org (the LTS version).
2. Open a terminal in this folder and run:

   ```
   npm i -g vercel
   vercel
   ```

   Sign in when asked and accept the suggested settings (no framework, no
   build command). You get a preview link.
3. When it looks right, publish it:

   ```
   vercel --prod
   ```

Your site is then live at `https://<project-name>.vercel.app`. The download
page is at `/download` and the APK at `/downloads/Pin.apk`.

To use your own domain, add it in the Vercel dashboard under
Project › Settings › Domains.

## After the first deploy

- **Share previews:** open `index.html` and `download.html` and change
  `content="/assets/img/og-image.jpg"` to the full address, for example
  `content="https://pin.vercel.app/assets/img/og-image.jpg"`. Facebook,
  LinkedIn and X need the full address to show the preview image.

## Releasing a new version of the app

1. Replace `downloads/Pin.apk` with the new file (keep the name `Pin.apk`).
2. In `download.html`, update the version, size, "Needs" and the SHA-256
   checksum. Get the checksum with `certutil -hashfile Pin.apk SHA256`
   (Windows) or `shasum -a 256 Pin.apk` (Mac).
3. Update "79.5 MB" in `index.html` (early access card) and in
   `assets/js/download.js`.
4. Run `vercel --prod` again.

If you change `site.css` or a `.js` file, also raise the `?v=1.0.0` number
where it is linked in the HTML (for example to `?v=1.0.1`). Those files are
cached for a year, and the new number makes browsers fetch the new copy.

## Keeping downloads fast and cheap

Vercel's free Hobby plan includes a monthly bandwidth allowance (about
100 GB at the time of writing), and each download of this APK uses 79.5 MB,
so roughly 1,200 downloads a month fit. Two ways to stretch it:

- **Smaller APK:** build with `flutter build apk --split-per-abi` and upload
  `app-arm64-v8a-release.apk` as `Pin.apk`. It works on almost all phones
  from the last several years and is about a third of the size.
- **Host the APK on GitHub Releases** (free, files up to 2 GB): create a
  release in a GitHub repository, attach the APK, and change the
  `href="downloads/Pin.apk"` link in `download.html` to the release file's
  address.
