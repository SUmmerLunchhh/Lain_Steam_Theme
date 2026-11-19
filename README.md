> **English** | [简体中文](README_zh.md)

# Lain — a Steam theme

A *Serial Experiments Lain* theme for the Steam client: near-black background, warm beige text, dusty-pink borders, full-width labels, and a light CRT scanline/flicker overlay.

Built on the skeleton of [blueArchiveSteamSkin](https://github.com/EugeneXXXie/blueArchiveSteamSkin) by EugeneXXXie (MIT). The file layout and a good part of the settings-page styling come from that project.

## What you get

- Library, Store, Community, Downloads and the friends list all restyled
- Consistent palette across titles, icons, context menus and Settings
- A show/hide button on the activity block; the state is remembered locally
- The momo icon on the Friends button
- Sound: startup chime, CRT hum, hover and click feedback
- Navigation sounds for Store / Library / Community / Profile, plus the Friends & Chat button in the bottom-right corner

## Installation

### 1. Install Millennium

The theme runs on [Millennium](https://steambrew.app/) — install that first, otherwise Steam won't read `skin.json` at all.

### 2. Drop the theme in

Put the whole `LainTheme` folder into Millennium's themes directory:

- **Windows**: `C:\Program Files (x86)\Steam\millennium\themes\`
- **Linux**: `~/.local/share/Steam/millennium/themes/` (or wherever your Steam lives)

The result should look like:

```
millennium/themes/LainTheme/skin.json
```

> Note: some older guides tell you to use `steamui/skins`. That's Steam's own built-in skin directory and has nothing to do with Millennium.

### 3. Enable it

Steam → Settings → Themes → pick **Lain**.

## Theme settings

Under Settings → Themes → Lain → **Effects**:

- **Effects** has three tiers — `Off` (cheapest), `Subtle` (default: light scanlines, film grain, text-decode on hover), `Full` (heavier scanlines, a fast 1.6 s refresh band, a full-screen jolt every 14–24 s, and menu borders that draw themselves in)
- **Sound** (hum / hover / click) — turn off for silence

## Sound assets

Six audio files live in `music/`: the startup chime (`Lain.ogg`), Store, Library, Community, Profile, and the bottom-right Friends button. All Ogg Opus, roughly 76 KB in total.

`js/sound.js` also carries base64 copies of the same clips. Steam's UI runs inside CEF, and injected JS has no reliable way to resolve the theme's directory, so reading `music/*.ogg` directly can get blocked — the embedded copies are the ones guaranteed to play.

If you swap a sound you have to update both sides; see [music/README.md](music/README.md) for details.

## Known limitations

- Steam reshuffles its CSS class hashes on every major update (things like `_1TdaAqMFadi0UTqilrkelR`). Some areas will lose styling when that happens and the theme has to be updated.
- The startup sound is subject to Chromium's autoplay policy: if the page hasn't received a real mouse/keyboard interaction yet, playback is deferred until your first click. That's a hard browser rule, so the chime sometimes feels like it lands a beat late.
- Only tested on Windows with a recent Millennium build.

## License

Code is under the upstream [MIT License](LICENSE). The original author's copyright notice is preserved; copyright for the Lain-specific changes belongs to SUmmerLunch.

Images, fonts and audio come from the anime *Serial Experiments Lain* and remain the property of their respective rights holders. They're included for personal, non-commercial use only.
