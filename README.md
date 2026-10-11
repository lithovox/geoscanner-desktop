# GeoScanner desktop

The GeoScanner web app (`../geoscanner`) as an installable app for Windows,
macOS and Linux, made with [Tauri](https://tauri.app) 2: a small Rust program
that shows the app in the system's webview. There's no copy of the frontend
here. Both commands below build `../geoscanner` with `--mode desktop`, so its
`src/saveFile.ts` saves through a native "Save as" dialog (the Rust command
`save_file` in `src-tauri/src/lib.rs`) instead of a browser download, and its
`src/website.ts` opens the website in the system browser (`open_url`).

## Download

The latest version:

| System | Installer |
| --- | --- |
| Windows (64-bit) | [GeoScanner-windows-x64-setup.exe](https://github.com/lithovox/geoscanner/releases/latest/download/GeoScanner-windows-x64-setup.exe) or [GeoScanner-windows-x64.msi](https://github.com/lithovox/geoscanner/releases/latest/download/GeoScanner-windows-x64.msi) |
| macOS (Apple Silicon and Intel) | [GeoScanner-macos-universal.dmg](https://github.com/lithovox/geoscanner/releases/latest/download/GeoScanner-macos-universal.dmg) |
| Linux (Debian/Ubuntu, 64-bit) | [GeoScanner-linux-amd64.deb](https://github.com/lithovox/geoscanner/releases/latest/download/GeoScanner-linux-amd64.deb) |

Older versions: [all releases](https://github.com/lithovox/geoscanner/releases).

The installers aren't code-signed yet. On Windows, SmartScreen may warn about
an unknown publisher ("More info" → "Run anyway"). On macOS the first start is
blocked; allow it in System Settings → Privacy & Security ("Open Anyway"). On
Linux, install with `sudo apt install ./GeoScanner-linux-amd64.deb`.

## How releases are made

The workflow `.github/workflows/desktop.yml` in the
[geoscanner](https://github.com/lithovox/geoscanner) repo builds this app on a
push to its master, on GitHub's Windows, macOS and Ubuntu runners, and
publishes the installers as the release `v<version>` there, with the version
from `../geoscanner/package.json`. It releases each version once: to publish a
new one, bump that version. It builds this repo's master as it is at that
moment; a change here alone doesn't start a build (run the workflow by hand
from the repo's Actions tab, after bumping the version).

## Building it yourself

Needs Node and Rust (rustup, stable), plus per system:

- Windows: the Visual Studio C++ build tools (WebView2 comes with Windows 10/11).
- macOS: the Xcode command line tools.
- Linux: `libwebkit2gtk-4.1-dev libappindicator3-dev librsvg2-dev patchelf`
  (see Tauri's prerequisites).

Then:

- `npm install` once.
- `npm run dev`: the app in a window, with ../geoscanner's Vite dev server on
  port 1420 (changes reload).
- `npm run build`: the installers, in `src-tauri/target/release/bundle/` (on
  Windows `nsis/` with the setup .exe and `msi/`; elsewhere pass e.g.
  `-- --bundles deb` or `-- --bundles dmg`).

The app icons in `src-tauri/icons/` are made from `app-icon.png` (the logo on
a square): `npx tauri icon app-icon.png`.

Not done yet: code signing (see Download) and auto-updates. The mini-map still
uses OpenStreetMap's tiles, so it needs the internet; the rest works offline.
