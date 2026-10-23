# Putting Undercard on your website

The game lives at **https://pridefightingchampionships.com/undercard/**.

Your site is the GitHub Pages repository `brentbrookhouse/UFCELO.github.io`. A copy is cloned at
`C:\UFCELO.github.io`, next to the game project (not inside it). The game sits in that repo's
`undercard` folder. Everything else on the site (`index.html`, `methodology.html`, `CNAME`) is left alone.

The web build is in `C:\MMARoguelikeGame\build\undercard-web\`. It's the public build (no Fight Lab, no
developer testing panel) and single-threaded, so GitHub Pages serves it with no special settings.

## Publishing an update (what Claude Code does when you ask)

1. Bump `version` in `data/build.json`, so testers' bug reports show which build they played. The
   version is in the bottom-right corner of every screen.
2. Export the web build:
   ```
   <console exe> --headless --path . --export-release "Web" build/undercard-web/index.html
   ```
3. Get the latest site, so nothing changed on GitHub gets overwritten:
   ```
   git -C C:\UFCELO.github.io pull
   ```
   (If `C:\UFCELO.github.io` doesn't exist yet: `git clone https://github.com/brentbrookhouse/UFCELO.github.io.git`
   from `C:\`.)
4. Copy every file from `build\undercard-web\` into `C:\UFCELO.github.io\undercard\`, replacing the
   old ones. Don't touch anything outside the `undercard` folder.
5. Make sure the site root has an empty `.nojekyll` file (it's already there). It tells GitHub Pages
   to serve the files exactly as they are.
6. Commit and push from `C:\UFCELO.github.io`:
   ```
   git add undercard .nojekyll
   git commit -m "Update Undercard web build to v0.x.y"
   git push
   ```
7. Wait a minute or two. GitHub Pages usually republishes within 1–2 minutes, occasionally up to
   about 10. Then open the address above.

## If the new version doesn't show up

- **Browser cache:** the browser may still have the old files. Reload with Ctrl+F5, or open the page
  in a private window.
- **Build status:** on github.com, open the UFCELO.github.io repository and click **Actions**. The
  latest "pages build and deployment" run should have a green tick.
- **Old saves:** they still load, because saves carry a version number. A save from a newer build
  shows a message instead of Continue.

## What's in the folder

| File | What it is |
| --- | --- |
| `index.html` | The page that starts the game |
| `index.js` | The loader |
| `index.wasm` | The game engine (about 38 MB) |
| `index.pck` | The game itself: screens, data and fonts (about 2 MB) |
| `index.audio.worklet.js`, `index.audio.position.worklet.js` | Audio helpers |
| `index.png`, `index.icon.png`, `index.apple-touch-icon.png` | Loading image and icons |
| `HOW-TO-UPLOAD.md` | A copy of this guide |

The first visit downloads about 40 MB (GitHub Pages compresses it), so it takes a few seconds; after
that the browser keeps a copy. Each player's save lives in their own browser. Clearing browser data,
or using a private window, removes it. Their profile (achievements, unlocks, hall of fame) is saved the same way. Testers can use Settings → Export save to send you their run and profile in one file.

## Other hosting

To put it on an ordinary web host instead, upload every file from `build\undercard-web\` into a folder
on the host with its file manager or FTP. If the page stays blank with an error about `.wasm`, add
`AddType application/wasm .wasm` to a `.htaccess` file in that folder.
