# LA VIE — Urgences Médicales · Prototype

Interactive prototype of the **LA VIE — Urgences Médicales** app (emergency ambulance service, Kinshasa).
Clickable mock-up with **fictional data**, for design and approval review. The app interface is in French (its target market); this document is in English.

## View the prototype

Open **`index.html`** in any browser (Chrome, Safari, Edge…).
No install, no server — everything is in a single file.

If the project is published with **GitHub Pages**, it's also available online at:
`https://<username>.github.io/<repo-name>/`

## What's inside

A switch at the top toggles between two apps:

- **Patient app** — home screen, SOS button, choose which family member, emergency type,
  live ambulance tracking, medical file, subscription.
- **Paramedic app** — shift dashboard, live fleet, operational map, and the full mission flow:
  incoming → accept → navigate → face verification → medical file → care → transport → close.

Tip: on the patient side, press **SOS**; on the paramedic side, a mission arrives automatically.

## Editing the code

The whole prototype lives in **`index.html`** (HTML + CSS + JavaScript, no dependencies).
- For quick tweaks: edit `index.html` directly on GitHub (the ✏️ pencil button).
- Colors, fonts, and labels are near the top of the file, in the `:root { … }` block and `<style>`.
- No build step — save and reload the page.

## Status

Approval mock-up — features (GPS, calls, payment, face recognition) are **simulated**.
No real data is used or transmitted.
