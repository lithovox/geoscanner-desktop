# GeoScanner desktop

The GeoScanner web app (`../geoscanner`) as an installable Windows app, made
with [Tauri](https://tauri.app) 2: a small Rust program that shows the app in
the system's WebView2. There's no copy of the frontend here. Both commands
below build `../geoscanner` with `--mode desktop`, so its `src/saveFile.ts`
saves through a native "Save as" dialog (the Rust command `save_file` in
`src-tauri/src/lib.rs`) instead of a browser download.

Needs Node, Rust (rustup, stable MSVC) and the Visual Studio C++ build tools;
WebView2 comes with Windows 10/11.

- `npm install` once.
- `npm run dev`: the app in a window, with ../geoscanner's Vite dev server on
  port 1420 (changes reload).
- `npm run build`: the installers, in
  `src-tauri/target/release/bundle/nsis/` (setup .exe) and `.../msi/`.

The app icons in `src-tauri/icons/` are made from `app-icon.png` (the logo on
a square): `npx tauri icon app-icon.png`.

Not done yet: code signing (without it Windows SmartScreen warns on install),
a CI workflow and auto-updates. The mini-map still uses OpenStreetMap's tiles,
so it needs the internet; the rest works offline.
