# 🏃 STRIDE — GPS Run Tracker

**A real, working running app.** Live GPS route tracking, per-kilometre splits, pace analysis,
adaptive training plans and a coach that explains your numbers — all in your browser, offline,
with no account and no server.

<p align="center">
  <a href="https://github.com/"><img alt="No build step" src="https://img.shields.io/badge/build-none-brightgreen"></a>
  <img alt="Vanilla JS" src="https://img.shields.io/badge/vanilla-JS-f7df1e">
  <img alt="PWA" src="https://img.shields.io/badge/PWA-installable-5a0fc8">
  <img alt="Offline" src="https://img.shields.io/badge/offline-ready-2ea44f">
  <img alt="License" src="https://img.shields.io/badge/license-MIT-blue">
</p>

<p align="center">
  <img src="screenshots/1-home.jpg" width="210" alt="Home screen">
  <img src="screenshots/5-live-run.jpg" width="210" alt="Live GPS run">
  <img src="screenshots/2-progress.jpg" width="210" alt="Progress and charts">
  <img src="screenshots/4-run-detail.jpg" width="210" alt="Run detail with splits">
</p>

---

## 🚀 Quick start

**Option A — single file (easiest).** Download [`stride-standalone.html`](stride-standalone.html)
and double-click it. Everything (CSS, JS, icons) is inlined. No server, no dependencies.

**Option B — run the multi-file source.**

```bash
git clone <this-repo>
cd <repo>
python3 -m http.server 8080
# open http://localhost:8080
```

> GPS requires **https** or **localhost**. No GPS? Tap **“Try demo mode”** — it simulates a
> realistic loop at 1×/2×/4× speed and exercises the exact same tracking code.

---

## ✨ Features

| | |
|---|---|
| 📍 **Live GPS tracking** | Accuracy gate before recording starts, jitter filter, impossible-speed detection that breaks the route instead of drawing fake straight lines |
| 🗺️ **Live route map** | Canvas map that auto-fits as you move, with scale bar — no tiles, no API keys, works offline |
| ⏸️ **Pause / resume** | Time held, route safe; unfinished runs are recoverable after a crash |
| ⚡ **Splits & analysis** | Per-km/mile splits, fastest 1K and fastest 5K via rolling window, elevation gain, calorie estimate |
| 📈 **Progress** | 8-week volume chart, pace-trend line, personal bests, weekly goal ring, streak |
| 🧠 **Coach insights** | Generated from your data: week-over-week change, pace delta, goal gap, back-to-back warning |
| 🎯 **Training plans** | 5K / 10K / Half × 3 levels × 6/8/12 weeks with base → build → peak → taper → race week |
| 🧬 **Adaptive plans** | First long run is set from your longest run in the last 28 days |
| ✅ **Auto-tick sessions** | A recorded run covering ≥70 % of a planned session marks it complete |
| 🗣️ **Voice coach** | Speech + beeps at every split, pause, resume and target completion |
| 🔄 **km ⇄ mi** | Switches everywhere instantly |
| 💾 **Local-first** | `localStorage`, JSON export, sample data, full reset — nothing leaves your device |
| 📲 **Installable** | PWA manifest + service worker → home-screen app, offline-ready |

---

## 🧱 Tech & structure

Vanilla HTML/CSS/JS — **no framework, no bundler, no npm install**.

```
index.html              app shell, screens, sheets
styles.css              design system (dark + volt accent), phone-first layout
js/01-core.js           state, storage, geo math, splits, PBs, insights, plan engine, audio
js/02-tracker.js        GPS session, demo simulation, canvas maps, wake-lock, recovery
js/03-ui.js             all screens, charts, plan builder, settings, onboarding
js/04-app.js            routing, sheets, global events, bootstrap
sw.js                   offline-first service worker
manifest.json           PWA manifest (standalone, portrait)
build-standalone.py     bundles everything into one self-contained HTML file
stride-standalone.html  generated single-file build
screenshots/            README images
```

### How the distance is kept honest

1. **Accuracy gate** — nothing is recorded until a fix arrives with ≤ 35 m accuracy.
2. **Jitter filter** — fixes closer than 1.4 m to the previous one are dropped (standing still
   shouldn't add metres).
3. **Speed sanity check** — a jump implying > 12 m/s (~43 km/h) is flagged as a *gap*: the line
   breaks and no phantom distance is added.
4. **Splits** — the exact crossing time of each kilometre is interpolated between fixes, so a
   split is never rounded to "the next GPS tick".

---

## 🌐 Deploy your own (GitHub Pages)

1. Push this repo to GitHub.
2. **Settings → Pages → Build and deployment → Source: _Deploy from a branch_**.
3. Branch `main`, folder `/ (root)` → **Save**.
4. Wait ~1 minute, then open `https://<your-username>.github.io/<repo-name>/`.

GPS works on that https URL on your phone. Add it to your home screen for a full-screen,
offline-capable app.

> ⚠️ **Heads-up:** if you only upload `index.html`, the page will render *unstyled* — the CSS and
> JavaScript live in separate files. Upload `styles.css`, the `js/` folder and `icons/` too
> (or use `stride-standalone.html`, which is a single file).

---

## 🔒 Privacy

No analytics, no backend, no accounts. Runs are stored in your browser's `localStorage` on your
device. Clearing site data deletes them — export JSON first if you care about the history.

---

## 🧪 Testing

Manual + automated browser runs (Chromium, mobile viewport) covering onboarding → demo run →
pause/resume → finish → summary → run detail → history → progress charts → plan creation →
session tick → unit switch → sample data → delete → reload persistence → GPS mode with mocked
coordinates. No console errors.

---

## 🗺️ Roadmap

- [ ] Background tracking (screen off) via a native shell — browsers throttle background GPS
- [ ] Map tiles (MapLibre/OSM) as an optional online layer
- [ ] Intervals/workout builder with audio cues per rep
- [ ] Streak badges and monthly challenges
- [ ] Import/merge GPX + Strava export

---

## 📄 License

MIT — see [LICENSE](LICENSE).

> Distance and calorie figures are estimates. Training plans are general guidance, not medical
> advice. Please run responsibly.
