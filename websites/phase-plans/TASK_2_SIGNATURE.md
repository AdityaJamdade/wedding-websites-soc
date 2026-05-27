# Phase Plan — Signature (v2.0)

> **Status: Production ready.** One task only: migrate Formspree → EmailJS in the shared v2.0 codebase. This also fixes Essential simultaneously since they share the same codebase.

**Codebase:** `wedding-websites/wedding-invitation-websites-2.0/`
**Framework:** React 19 + Vite
**Demo client:** `clients/_demo-signature/` — Isabella & James
**Note:** Signature and Essential live in the same codebase. The migration in this plan fixes both tiers at once. Do this AFTER Prestige migration so you have a reference.

---

## What Signature Already Has (do not touch)

- 11 sections — all working
- Live countdown timer
- 20 presets, 8 design styles, 10 colour schemes, 6 font pairings
- Full RSVP form (currently Formspree)
- Music requests form (currently Formspree)
- Schedule, Registry, Location/Maps, Accommodation, FAQ sections
- Smooth Framer Motion scroll animations
- Section registry + CLIENT env var system

---

## Cascade Check — What Signature Does NOT Have

These are features removed from Prestige going into Signature. Confirm they are NOT present:

- [ ] No video hero (`hero.videoUrl` should be ignored or not in the Hero.jsx)
- [ ] No `auroraGlass`, `velvetNight`, `immersive3d` styles
- [ ] No presets #21–27 in the config
- [ ] No premium colour schemes (`cosmicGold`, `auroraBlue`, `velvetNight`)

If any of these leaked into v2.0, remove them.

---

## Phase 1 — Formspree → EmailJS Migration  `[ ]`

**Estimated time: half a day**
**Reference:** Follow the exact same pattern as Prestige (see `PHASE_03_PRESTIGE.md` Phase 1). It's the same two files.

### Step 1: Add the EmailJS package
- [ ] Inside `wedding-invitation-websites-2.0/template/`, run:
  ```bash
  npm install @emailjs/browser
  ```

### Step 2: Update `sections/RSVP.jsx`

This file currently uses `formspreeId`. Replace the Formspree submit logic with EmailJS:

```js
import emailjs from '@emailjs/browser';

// In submit handler:
const { serviceId, templateId, publicKey } = cfg.rsvp.emailjs;
await emailjs.send(serviceId, templateId, {
  partner1: cfg.couple.partner1,
  partner2: cfg.couple.partner2,
  attending,
  ...form,
}, publicKey);
setStatus('success');
```

- [ ] Remove any `formspreeId` references from props and config reads
- [ ] Keep the same form layout, same error handling, same success state

### Step 3: Update `sections/Music.jsx`

- [ ] Same migration — replace Formspree fetch with `emailjs.send()`
  ```js
  await emailjs.send(
    cfg.music.emailjs.serviceId,
    cfg.music.emailjs.templateId,
    { partner1: cfg.couple.partner1, partner2: cfg.couple.partner2, ...form },
    cfg.music.emailjs.publicKey
  );
  ```

### Step 4: Update both demo appsettings files

- [ ] `clients/_demo-signature/appsettings.js` — replace `rsvp.formspreeId` with `rsvp.emailjs` block:
  ```js
  rsvp: {
    emailjs: {
      serviceId:  'service_REPLACE',
      templateId: 'template_rsvp_REPLACE',
      publicKey:  'publickey_REPLACE',
    },
    maxGuests: 4,
    mealOptions: ['Chicken', 'Fish', 'Vegetarian'],
  },
  music: {
    emailjs: {
      serviceId:  'service_REPLACE',
      templateId: 'template_music_REPLACE',
      publicKey:  'publickey_REPLACE',
    },
  },
  ```
- [ ] `clients/_demo-essential/appsettings.js` — same update (Essential has no music section, but add `rsvp.emailjs` block):
  ```js
  rsvp: {
    emailjs: {
      serviceId:  'service_REPLACE',
      templateId: 'template_rsvp_REPLACE',
      publicKey:  'publickey_REPLACE',
    },
    maxGuests: 4,
  },
  ```

### Step 5: Test
- [ ] `CLIENT=_demo-signature npm run dev` — no errors
- [ ] Submit test RSVP → email arrives in inbox
- [ ] Submit test music request → email arrives
- [ ] `CLIENT=_demo-essential npm run dev` — confirm RSVP still works (no music section)

---

## Phase 2 — Demo Client Refresh (optional)  `[ ]`

**Estimated time: 0.25 day**

Isabella & James should feel clearly below Prestige (no video, no premium styles) but still beautiful and complete.

- [ ] Confirm `preset: 'modernLuxe'` is a solid showcase — if not, try `midnightElegance` or `goldenHour`
- [ ] Ensure all 11 sections have real, complete content
- [ ] The showcase value is: "look how much content is on this page — every section your guests need"

---

## Phase 3 — Verify & Launch  `[ ]`

- [ ] Signature: all 11 sections render, countdown ticking, RSVP and Music forms send
- [ ] Essential: 5 sections render, RSVP form sends (run via `CLIENT=_demo-essential`)
- [ ] Mobile: both demos look correct at 375px
- [ ] Update `TIER_04_SIGNATURE.md` — mark migration complete
- [ ] Update `TIER_05_ESSENTIAL.md` — mark migration complete (same codebase fix)
- [ ] Update `PLANS.md` — mark both Signature and Essential as fully migrated

---

## Estimated Timeline

| Phase | Work | Days |
|---|---|---|
| 1 | EmailJS migration | 0.5 |
| 2 | Demo refresh | 0.25 |
| 3 | Verify | 0.25 |
| **Total** | | **~1 day** |

> **Do Signature and Prestige migrations at the same time** — they are nearly identical. Set up one EmailJS account, create the templates once, use the same keys for both demos initially.
