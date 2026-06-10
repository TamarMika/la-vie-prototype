# Handoff — LA VIE Urgences Médicales interactive prototype (patient + paramedic), published to GitHub

_2026-06-10 07:41_ · session: current · prior: `none`

## Goal
Build an interactive, clickable prototype for **LA VIE — Urgences Médicales**, an emergency ambulance service in Kinshasa (DR Congo). Cover both the **patient app** and the **paramedic (field responder) app**, in a calm/trustworthy style on the brand (red + black, French UI). Then publish it to GitHub so the client can view it for approval and edit the code. All features are simulated (mock data).

## Current state
- [x] Patient app: intro/welcome, home with "you're covered" status + SOS, member select, emergency-type select, live ambulance tracking, medical profile, subscription (plans + Mobile Money).
- [x] Paramedic app via a top **role switcher** (one app on screen at a time). Full mission flow: standby → incoming alert → accept → navigate → face verification → medical file → care/vitals → choose hospital → transport → mission closed.
- [x] Paramedic has a bottom nav with 3 tabs: **Tableau de bord** (shift KPIs + weekly chart + mini-map + recent), **Flotte** (unit statuses + active incidents + fleet map), **Carte** (full operational map).
- [x] Reusable **stylized Kinshasa city-map engine** (SVG: Congo River, roads with casing, blocks, district labels, ambulance/patient/hospital markers). Used by the full map, previews, and all in-mission route maps.
- [x] Everything is in ONE self-contained file (`index.html`). No build, no dependencies, no external requests. Verified headless (Playwright) → no JS errors; patient app still works after paramedic integration.
- [x] Published: repo **TamarMika/la-vie-prototype** (public, branch `main`), `index.html` + `README.md` pushed. **GitHub Pages enabled** → live at https://tamarmika.github.io/la-vie-prototype/.
- [~] Adding this `HANDOFF.md` and pushing it to the repo.
- [ ] **HQ dispatcher** role (third surface) — offered but NOT built.
- [ ] Real backend wiring (GPS, calls, payment, face recognition) — all currently simulated by design.

## Key decisions
- Decided **calm, trustworthy health-app aesthetic** over (a) the original "clinical-urgent" heavy red/black and (b) a Bauhaus brutal-minimal pass. Why: for a real SOS app, reassurance and instant clarity matter more than visual edge; the brutalist version felt cold/alarming and confused users. Red is now reserved ONLY for the SOS button and the live-emergency state; a calm green carries "safe/covered/verified". (Trade-off: less visually striking, but on-brief for emergency UX.)
- Decided **one app visible at a time** (top switcher: patient ⇄ paramedic) over the earlier side-by-side patient+dispatch canvas. Why: side-by-side with a dev-style toggle made users unsure "what am I looking at / what do I do." Added an intro/orientation screen and a header line that updates per role.
- Decided to **add the paramedic app inside the same file** (not a separate file). Why: user wanted it in the same prototype; the role switcher keeps it unconfusing.
- Decided a **single self-contained `index.html`** (inline CSS+JS) over React/Vite. Why: a non-developer can open and share it by double-click, and GitHub Pages serves it with zero config.
- Decided a **hand-drawn SVG map** over real map tiles. Why: works offline, stays on-brand, needs no API key.
- Typography: **Outfit** (display) + **Inter** (body) as free stand-ins for the brand charter's commercial "Scout" font.
- Delivery: **public repo + GitHub Pages** for a free live approval link; client can read/edit via the repo (pencil button) or be added as a collaborator. Fine-grained token, scoped to the one repo, used transiently and deleted after.

## Don't do
- Don't reintroduce the **Bauhaus/brutalist** look or a heavy red/black palette — explicitly rejected by the user ("i dont really like the bauhaus vibez").
- Don't show **patient and dispatch side-by-side** — rejected as confusing ("i dont really understand what the hell is going on").
- Don't write project docs (README/HANDOFF) in **French** — the user reads English. The app UI stays French (its market).
- Don't use **Puppeteer** in the sandbox → its bundled Chrome is x86 and fails on this arm VM ("Syntax error: word unexpected"). Use **Playwright chromium** instead (`npx playwright install chromium chromium-headless-shell`).
- A **fine-grained token without the "Pages" permission** can push code but returns **403** on the enable-Pages API. Don't retry the API; enable Pages in repo **Settings → Pages** (or grant Pages: Read/write on the token). The first token supplied was also invalid (401) — verify tokens with `GET /user` or `GET /repos/{owner}/{repo}` before relying on them.

## Open blockers / questions
- HQ **dispatcher** surface not yet built (the third role: live incoming-calls queue, map tracking, patient file + face-verification confirmation, ambulance status). Decide whether to add as a 3rd switcher option.
- Whether/when to move from simulated features to a real backend (geolocation, telephony, payments via M-Pesa/Airtel/Orange, biometric face match). The uploaded `audit_la_vie_mobile.pdf` lists what a prior build did wrong — use it as the "what not to do" checklist if/when wiring real services.

## Files touched
- `index.html` — the entire prototype (patient + paramedic, CSS+JS inline). Source of truth. Also present as `la-vie-prototype.html` (identical copy) in the Cowork outputs folder.
- `README.md` — English project doc (how to view/edit, what's inside, status).
- `app.js` — scratch copy of the JS used while building; it was **inlined** into `index.html`. Not needed for the standalone file; do not load it separately.
- `HANDOFF.md` — this document.

## Verification
- Open `index.html` in a browser → patient **intro** screen shows. Press **SOS** → pick a member → pick a type → the live screen animates the ambulance along the map with a ticking ETA. Healthy = no console errors, map + markers render.
- Switch to **Application ambulancier** → **Tableau de bord** loads; an incoming mission auto-arrives after ~3.4s on the dashboard; **Flotte** and **Carte** tabs render the stylized map. Run the full mission: accept → arrived → face verify (scan → "Identité confirmée") → file → care (tap vitals) → transport (pick hospital) → "Mission accomplie".
- Headless smoke (from the Cowork outputs folder, where `verify.js` lives — it is NOT committed): `node verify.js` → expect **`NO_JS_ERRORS`**. Anything else means the single-file HTML drifted.
- Live site: open https://tamarmika.github.io/la-vie-prototype/ → should load the prototype identically to the local file.

## Next action
Build the **HQ dispatcher** as a third option in the top role switcher inside `index.html`: a live incoming-calls list (reuse the INCIDENTS data), the full operational map (reuse `fullCityMap()` / city-map engine), a selected-patient file panel with the face-verification confirmation view, and ambulance status. Match the existing calm design tokens (`:root` variables) and the paramedic tab pattern. Then re-run the headless smoke (`node verify.js` → `NO_JS_ERRORS`), re-inline if split, and push to `main`.
