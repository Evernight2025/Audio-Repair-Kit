# Audio Repair Kit (Portable)

> Unzip and run · 100% local processing · Your audio never leaves your device · Windows / macOS / Linux / Android

[中文 README](README.md) 

A portable, out-of-the-box local audio batch repair / enhancement tool. Download the archive, unzip, double-click to launch, and process audio in your browser: noise reduction, vocal enhancement, quality upsampling, format conversion, volume control, fade in/out, spatial surround rendering, and more. Everything runs locally on your machine — no audio file is ever uploaded to any server.

---

## Features

- **Batch processing**: add single files / whole folders (recursive scan) / drag & drop; select, remove, or clear
- **60+ mainstream audio formats**: mp3 / wav / flac / aac / m4a / ogg / opus / wma / ape / dsf / dts / ac3 and more
- **Noise reduction**: Off / Normal (light hiss & hum) / Extreme (heavy noise)
- **Voice clarity**: vocal-band EQ boost + compression + de-esser
- **8 output quality tiers**: Original / CD Lossless (44.1kHz 16bit) / Hi-Res Lossless (96kHz 24bit) / Stereo / Surround (5.1) / Dolby Atmos (7.1 spatial) / Master (192kHz 24bit) / Custom (sample rate + bit depth)
- **Output format conversion**: FLAC / WAV / MP3 / AAC / OGG / Opus (multichannel tiers auto-fall back to FLAC)
- **Volume / loudness**: smart loudness normalization (-14 LUFS) or ±3 / ±6 dB manual gain
- **Fade in / out**: 0.5 / 1 / 2 / 3 seconds
- **File naming**: auto (append processing tag) / original name / custom (auto-numbered for multiple files)
- **List filters**: All / Pending / Completed / Errors
- **Error popup**: failures show the error message and error log with one-click copy
- **Live progress**: per-file progress bar + processing speed + overall progress + ETA
- **4-language UI**: 中文(简体) / 中文(繁體) / English / 日本語, switchable instantly and remembered
- **Cross-platform**: Windows / macOS / Linux / Android (Termux), responsive mobile layout
- **Crash-resistant**: auto-reconnect on disconnect, tasks keep running, close-guard confirmation

---

## System Requirements

| Platform | Notes |
| --- | --- |
| Windows | 7+ (10/11 recommended), launch via .bat |
| macOS | 10.13+, launch via .sh in Terminal |
| Linux | glibc environment (most distros), launch via .sh |
| Android | Install Termux, then launch via .sh |

On first run the tool automatically downloads its runtime (portable Python ~11MB, FFmpeg engine ~30–170MB) with automatic retries; internet is only needed once.

---

## Quick Start

### Windows
1. Unzip the archive anywhere
2. Double-click `启动音频修复器.bat`
3. Your browser opens the UI automatically (default http://127.0.0.1:8765)

### macOS / Linux
```bash
bash 启动音频修复器.sh
```
If Python3 / FFmpeg is missing, it automatically tries brew / apt / dnf / apk.

### Android (Termux)
```bash
bash 启动音频修复器.sh
```
The script runs `pkg install python` and `pkg install ffmpeg` automatically; then open the shown URL in your phone browser.

---

## Usage Example

Add files → choose processing options → click "Start Processing":

```
Noise reduction: Normal (recommended)
Voice clarity: Off / Boost vocals
Output quality: Hi-Res Lossless (default)
Output format: Follow quality tier (lossless)
Volume: Smart loudness (-14 LUFS)
Naming: Auto / Original / Custom
Save path: empty = save to source folder
```

When done, click "Open Output Folder" to jump straight to the results. Full logs (filter chain, exact command, duration) scroll live at the bottom.

---

## Output Notes

- Lossless tiers output FLAC by default; custom 32-bit outputs WAV; the Original tier keeps the source format (MP3 320k / AAC 320k / PCM, etc.)
- Surround / Atmos tiers auto-fall back to FLAC when a lossy format is chosen, preserving all channel data
- Filenames never collide with existing files and source files are never overwritten

### Honest note about "Dolby Atmos"

Real Dolby Atmos encoding requires an official Dolby-licensed tool. This tool's "Dolby Atmos" tier outputs a **7.1 bed + Haas phase-expansion spatial render** (FLAC 7.1 / 48kHz / 24bit) for an immersive sound field — it is not an official Dolby DD+/TrueHD encode.

### About "Master" quality

The Master tier = 192kHz / 24bit high-precision resampling + EBU R128 loudness normalization (-14 LUFS / -1dB TP), matching or exceeding studio master specs — but it cannot fix flaws in the original recording itself.

---

## Architecture

```
AudioRepairKit/
├── 启动音频修复器.bat        # Windows launcher (auto-installs Python/FFmpeg with retries)
├── 启动音频修复器.sh         # macOS / Linux / Android launcher
├── 使用说明.txt              # Full user manual (Chinese)
├── README.md / README_EN.md  # This documentation (ZH / EN)
├── app/
│   ├── server.py             # Pure Python stdlib HTTP server (zero pip dependencies)
│   └── www/index.html        # Single-file web UI (4-language i18n)
├── runtime/                  # FFmpeg engine (auto-installed on first run, not committed)
└── app_data/                 # Upload staging / default output / logs (not committed)
```

Core design:

- **Zero pip dependencies**: the server uses only the Python standard library (http.server / subprocess / zipfile / tarfile) for portability
- **FFmpeg filter chain**: denoise(afftdn) → vocal(EQ+compressor+deesser) → volume(loudnorm/volume) → fade(afade) → resample(soxr) → channel(pan/surround) → limiter(alimiter)
- **Capability probing & fallback**: probes soxr / surround / afftdn / loudnorm etc. at startup and switches to fallback plans automatically
- **End-to-end fault tolerance**: 5 download retries + fallback channels, per-file auto-retry, HTTP error guards, auto-reconnect

---

## Privacy & Security

- The server listens on `127.0.0.1` (loopback) only — never exposed to the internet by default
- All processing is local; no audio is ever uploaded
- LAN access (optional): `python3 app/server.py --host 0.0.0.0`

---

## FAQ

| Issue | Answer |
| --- | --- |
| Firewall prompt? | Loopback-only by default; just allow it |
| Antivirus false positive? | The .bat is only a launcher; ffmpeg is an official open-source build — add an exception |
| Linux engine download fails? | glibc required; or `sudo apt install ffmpeg` then click the engine badge to retry |
| Termux can't download the engine? | You must use `pkg install ffmpeg` (the script tries automatically) |
| Large files are slow? | Hi-Res / Master tiers need high-precision resampling — that's expected |

---

## License

MIT License for this project — free for learning and personal use. FFmpeg is distributed under its own [LGPL/GPL license](https://ffmpeg.org/legal.html).

---

## About the Author

Author's bilibili homepage: [Visit](https://space.bilibili.com/3493267335285205)

If this tool helps you, a Star / share would be much appreciated — your support keeps it updated.
