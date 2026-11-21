# WoWWalk Web Flasher

Browser-based firmware updater — no Arduino IDE required. Uses
[ESP Web Tools](https://espressif.github.io/esp-web-tools/) (esptool compiled
to WebAssembly, talks to the board over WebSerial).

## For end users

1. Open this page in **Chrome or Edge** (hosted on GitHub Pages — see below).
2. Plug the WoWWalk board into USB.
3. Click **Install**, pick the COM port, wait for success.

## Publishing a firmware release

1. Build in Arduino IDE (board: `XIAO_ESP32C3`).
2. Find the build artifacts in `%TEMP%\arduino\sketches\` or
   `%TEMP%\arduino_build_*\` — you need three files:
   - `WoWWalk.ino.bootloader.bin`
   - `WoWWalk.ino.partitions.bin`
   - `WoWWalk.ino.bin`
3. Create a [GitHub release](https://github.com/wimbe/wowWalk/releases) with a
   tag like `v0.1.0`, and attach the three `.bin` files **renamed to**:
   - `bootloader.bin`
   - `partitions.bin`
   - `WoWWalk.ino.bin`
4. Update `manifest.json`: set `"version"` and replace `REPLACE_WITH_RELEASE_URL`
   with the release download base, e.g.
   `https://github.com/wimbe/wowWalk/releases/download/v0.1.0`
5. Commit + push. GitHub Pages serves the updated page automatically.

## Enabling GitHub Pages

Repo → Settings → Pages → Source: `main` branch / `/flasher` folder (or root
with the page at `/`). Free HTTPS is required — WebSerial refuses plain HTTP.

## Testing locally

```sh
cd flasher
python -m http.server 8000
```

Open `http://localhost:8000` — localhost counts as a secure context so
WebSerial works without HTTPS.
