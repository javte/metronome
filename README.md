# 🕰️ Jed's Metronome

A personal metronome Progressive Web App (PWA) built for iPhone. Works fully offline, installs to your home screen, and keeps your screen awake while playing.

---

## Features

- BPM range 30–300 with slider, +1 / −1 / +5 / −5 buttons, and tap tempo
- Time signatures: 1/4 through 8/4, plus Free (no accent, steady pulse)
- Three click sounds: Click, Beep, Wood
- Visual beat indicators with accent on beat 1
- Tempo name display (Andante, Allegro, Vivace, etc.)
- Screen stays on while the metronome is running (Wake Lock API)
- **Jed's Party** — hardcoded song list, read-only
- **Personal** — your own saved presets, stored locally on the device
- Drag to reorder personal presets
- Load any preset, tweak it, and save changes back in one tap
- Fully offline after the first visit

---

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire app — HTML, CSS, and JavaScript in one file |
| `manifest.json` | PWA manifest — controls the home screen name, icon, and display mode |
| `sw.js` | Service Worker — caches app files for offline use |
| `icon.svg` | App icon shown on the home screen |
| `jeds-party.js` | Not used at runtime — kept as reference only (songs are now inline in `index.html`) |

---

## Installing on iPhone

The app must be served over **HTTPS** to work as a PWA. The easiest free options are below.

### Option A — Netlify (drag and drop, recommended)

1. Go to [netlify.com](https://netlify.com) and create a free account
2. From the dashboard, drag the entire `metronome` folder onto the page
3. Netlify gives you a URL like `https://random-name.netlify.app`
4. Open that URL in **Safari** on your iPhone
5. Tap the **Share** button → **Add to Home Screen** → **Add**

### Option B — GitHub Pages

1. Create a free account at [github.com](https://github.com)
2. Create a new **public** repository (e.g. `jeds-metronome`)
3. Upload all four files: `index.html`, `manifest.json`, `sw.js`, `icon.svg`
4. Go to **Settings → Pages**, set source to the `main` branch, root folder, and save
5. Your app will be live at `https://yourusername.github.io/jeds-metronome`
6. Open that URL in **Safari** on your iPhone
7. Tap **Share** → **Add to Home Screen** → **Add**

> **Important:** The manifest's `start_url` and `scope` must match your subfolder path if using GitHub Pages. Update `manifest.json` if your repo is not at the root domain.

---

## Adding or Editing Party Songs

All Jed's Party songs are hardcoded directly in `index.html`. To edit them, open the file and find the clearly marked block near the top of the `<script>` section:

```js
// ============================================================
//  JEDS PARTY — edit this list to add/remove/change songs
//  Fields: title, bpm, timeSig, freeTime, sound ('click'|'beep'|'wood')
// ============================================================
const JEDS_PARTY_PRESETS = [
    { title: "Almost There", bpm: 148, timeSig: 1, freeTime: false, sound: "wood" },
    ...
];
```

Add, remove, or change entries as needed, then redeploy.

---

## Deploying Updates

Whenever you push a new version of `index.html`:

1. Update the cache version in `sw.js` — change `jeds-metro-v1` to `jeds-metro-v2` (increment each time)
2. Push all changed files to your host
3. Users will get the update automatically the next time they open the app while online and then close and reopen it — no manual cache clearing needed

---

## Clearing App Data

If the app gets stuck or you want a clean slate:

**Settings → Safari → Advanced → Website Data**

Search for your domain and swipe left to delete it. This clears both saved presets (localStorage) and the offline cache (Service Worker). The app will reload fresh on the next visit.

---

## Personal Presets

Personal presets are saved in the browser's `localStorage` tied to your domain. They survive closing the app, rebooting your phone, and app updates. They are only cleared if you wipe Safari's website data (see above).

---

## Technical Notes

- Built as a single-file PWA — no frameworks, no build step, no dependencies
- Audio engine uses the Web Audio API with a lookahead scheduler for accurate timing
- Wake Lock API keeps the screen on while playing (requires iOS 16.4+; silently ignored on older devices)
- Double-tap zoom disabled via `touch-action: manipulation`
- Service Worker uses a cache-first strategy with versioned cache busting
