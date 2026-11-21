> **English** | [简体中文](README_zh.md)

# Lain — a Steam theme

A *Serial Experiments Lain* theme for the Steam client: near-black background, phosphor cyan and dusty pink, full-width labels, with CRT scanlines, film grain and a power-on flash layered on top. The look deliberately targets that 1998 machine — desaturated, grainy and slightly sluggish — rather than modern neon cyberpunk.

Built on the skeleton of [blueArchiveSteamSkin](https://github.com/EugeneXXXie/blueArchiveSteamSkin) by EugeneXXXie (MIT). The file layout and a good part of the settings-page styling come from that project.

## What you get

**Interface**

- Library, Store, Community, Downloads and the friends list all restyled
- 90s window chrome: inset high-light and shadow instead of modern rounded corners
- Hover feedback jumps with `steps()` — no easing curve, because that era reacted instantly
- Consistent palette across titles, icons, context menus and Settings
- A show/hide button on the activity block; the state is remembered locally
- The Navi icon on the Friends button
- Game launching dialog: dark panel, outlined cover art, and a light band sweeping while it loads
- Notifications and achievement toasts restyled as an incoming signal: a light band sweeps across on entry and a receiving indicator blinks in the corner

**Motion** (three tiers)

- **CRT power-on flash**: a bright line unfolds into the full screen, then fades — 2.4 s, timed with the startup chime
- Scanlines, film grain and a slow rolling refresh band
- **Text decode**: hovering a label briefly turns it into `▓▒░01ｱｲ` before resolving letter by letter
- The Full tier adds heavier scanlines, a fast refresh band, a full-screen jolt every 14–24 s, and menu borders that draw themselves in

**Sound**

- CRT hum, hover and click feedback
- Distinct navigation sounds for Store / Library / Community / Profile, plus the Friends & Chat button in the bottom-right corner

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

UI strings are English (a store requirement). Under Steam → Settings → Themes → Lain → **Effects**:

- **Effects** has three tiers
  - `Off` — everything off, cheapest. Disables the power-on flash too.
  - `Subtle` (default) — light scanlines, film grain, a slow refresh band and text-decode on hover
  - `Full` — heavier scanlines, a fast refresh band, an occasional full-screen jolt and self-drawing menu borders
- **Sound** (hum / hover / click) — turn off for silence

The tier is handed to JavaScript through a CSS custom property (`--lain-tier`), so with `Off` the decode and jolt code never starts at all — it isn't running invisibly.

## Sound assets

Seven audio files live in `music/`: the startup chime (`Lain.ogg`), Store, Library, Community, the Profile page click and its background track (the first 10 s of Bôa - Duvet, played once), and the bottom-right Friends button. All Ogg Opus, roughly 136 KB in total.

`js/sound.js` also carries base64 copies of the same clips. Steam's UI runs inside CEF, and injected JS has no reliable way to resolve the theme's directory, so reading `music/*.ogg` directly can get blocked — the embedded copies are the ones guaranteed to play.

If you swap a sound you have to update both sides; see [music/README.md](music/README.md) for details.

## Known limitations

- Steam reshuffles its CSS class hashes on every major update (things like `_1TdaAqMFadi0UTqilrkelR`). Some areas will lose styling when that happens and the theme has to be updated.
- Steam's desktop client disables Chromium's autoplay restriction, so audio starts on load without a click. If Valve ever turns that restriction on, the code already falls back to playing on first interaction.
- The login screen (what you see when starting signed out) cannot be themed: Millennium injects after login, and the login window isn't among the windows it patches — so no styling and no sound there.
- Text-decode temporarily rewrites Steam's own text nodes; React writes them back on the next render. This is fine in practice — if a label ever gets stuck mid-decode, open an issue.
- The Full tier's jolt applies a transform to `body`, so fixed-position elements shift with it for those 160 ms. That's part of the effect.
- When the window loses focus — fully covered by another app, or you click into a child Steam window — the hum fades out and the audio context is suspended, with a 600 ms grace period. It resumes when you come back.
- Only tested on Windows with a recent Millennium build.

## License

Code is under the upstream [MIT License](LICENSE). The original author's copyright notice is preserved; copyright for the Lain-specific changes belongs to SUmmerLunch.

Images, fonts and audio come from the anime *Serial Experiments Lain* and remain the property of their respective rights holders. They're included for personal, non-commercial use only.
