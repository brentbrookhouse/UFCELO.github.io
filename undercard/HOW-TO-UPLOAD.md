# Putting Undercard on your website

The web build is in `C:\MMARoguelikeGame\build\undercard-web\`. It's the public build: no Fight Lab
and no developer testing panel. It's a single-threaded build, so it works on ordinary web hosting
with no special server settings.

## What's in the folder

| File | What it is |
| --- | --- |
| `index.html` | The page that starts the game |
| `index.js` | The loader |
| `index.wasm` | The game engine (about 38 MB) |
| `index.pck` | The game itself: screens, data and fonts (about 2 MB) |
| `index.audio.worklet.js`, `index.audio.position.worklet.js` | Audio helpers |
| `index.png`, `index.icon.png`, `index.apple-touch-icon.png` | Loading image and icons |

Upload every file. Don't rename any of them.

## Uploading to a subpage (for example pridefightingchampionships.com/undercard)

1. Open your web host's file manager, or connect with an FTP program such as FileZilla.
2. Go to your site's main folder. It's often called `public_html`, `www` or `htdocs`.
3. Make a new folder named `undercard`.
4. Open the new folder and upload every file from `build\undercard-web\` into it.
5. Visit `https://pridefightingchampionships.com/undercard/` in a browser. You should see the
   loading bar, then the UNDERCARD title screen.

The first visit downloads about 40 MB, so it takes a few seconds. After that the browser keeps a copy.

## If something doesn't work

- **A blank page or "failed to load .wasm":** some hosts send `.wasm` files with the wrong type.
  In the file manager, look for a file called `.htaccess` in the `undercard` folder (create it if
  it's missing) and add this line:

  ```
  AddType application/wasm .wasm
  ```

- **Very slow first load:** turning on "gzip" or "compression" in your host's settings shrinks the
  download to about a quarter. It's optional.
- **Saves:** each player's save lives in their own browser. Clearing browser data, or using a
  private window, removes it. Testers can use Settings → Export save to send you their save.

## Making a new web build later

Ask Claude Code to "export the web build". It runs:

```
<console exe> --headless --path . --export-release "Web" build/undercard-web/index.html
```

Then upload the new files over the old ones. Bump `version` in `data/build.json` first, so you
can tell builds apart in bug reports. The version shows in the bottom-right corner of every screen.
