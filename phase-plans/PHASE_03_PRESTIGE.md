# Phase Plan — Prestige (v3.0)

> **Status: Production ready.** No new features. Two tasks only: migrate Formspree → EmailJS, refresh the demo. After that, Prestige is fully launch-ready and stays untouched.

**Codebase:** `wedding-websites/wedding-invitation-websites-3.0/`
**Framework:** React 19 + Vite
**Demo client:** `clients/_demo-prestige/` — Sofia & Alexander

---

## What Prestige Already Has (do not touch)

- 11 sections — all working
- Live countdown timer
- Cinematic video hero
- 3 exclusive styles: `auroraGlass`, `velvetNight`, `immersive3d`
- 3 premium colour schemes: `cosmicGold`, `auroraBlue`, `velvetNight`
- 7 exclusive presets: `cosmicLuxe`, `darkVelvet`, `auroraWedding`, `velvetRose`, `celestialDawn`, `starCrossed`, `crystalAurora`
- 27 total presets, 11 styles, 13 colour schemes, 6 font pairings
- Smooth Framer Motion scroll animations
- Section registry + CLIENT env var system
- ThemeProvider, Nav, Footer, all section components

**Do not refactor or restructure any of the existing code.** Only add EmailJS, and only change the submit handlers in RSVP.jsx and Music.jsx.

---

## Phase 1 — Formspree → EmailJS Migration  `[ ]`

**Estimated time: half a day**

**Why:** Every new Prestige client needs EmailJS keys in their config. Formspree had a separate account per form and is being retired across the business.

### Step 1: Add the EmailJS package
- [ ] Inside `wedding-invitation-websites-3.0/template/`, run:
  ```bash
  npm install @emailjs/browser
  ```
- [ ] Confirm `package.json` now lists `"@emailjs/browser": "^4.0.0"`

### Step 2: Update `sections/RSVP.jsx`

Current submit handler (find and replace this block):
```js
// BEFORE — remove this
const res = await fetch(`https://formspree.io/f/${formspreeId}`, {
  method: 'POST',
  headers: { 'Content-Type': 'application/json', Accept: 'application/json' },
  body: JSON.stringify({ ...form, attending }),
});
setStatus(res.ok ? 'success' : 'error');
```

Replace with:
```js
// AFTER — EmailJS
import emailjs from '@emailjs/browser';

const { serviceId, templateId, publicKey } = cfg.rsvp.emailjs;
await emailjs.send(serviceId, templateId, {
  partner1:  cfg.couple.partner1,
  partner2:  cfg.couple.partner2,
  attending,
  ...form,
}, publicKey);
setStatus('success');
```

- [ ] Remove the `formspreeId` prop from `RsvpForm` and its parent
- [ ] Update the error catch block — `emailjs.send` throws on failure, so keep the existing `try/catch`

### Step 3: Update `sections/Music.jsx`

Same pattern — replace the Formspree fetch with:
```js
import emailjs from '@emailjs/browser';
await emailjs.send(
  cfg.music.emailjs.serviceId,
  cfg.music.emailjs.templateId,
  { partner1: cfg.couple.partner1, partner2: cfg.couple.partner2, ...form },
  cfg.music.emailjs.publicKey
);
```

- [ ] Remove `formspreeId` from Music component props

### Step 4: Update appsettings.js schema
- [ ] Update `clients/_demo-prestige/appsettings.js` — replace `rsvp.formspreeId` with:
  ```js
  rsvp: {
    emailjs: {
      serviceId:  'service_REPLACE',
      templateId: 'template_rsvp_REPLACE',
      publicKey:  'publickey_REPLACE',
    },
    maxGuests: 4,
  },
  music: {
    emailjs: {
      serviceId:  'service_REPLACE',
      templateId: 'template_music_REPLACE',
      publicKey:  'publickey_REPLACE',
    },
  },
  ```
- [ ] Set up a real EmailJS account for testing — fill in real keys in the demo config
- [ ] Submit a test RSVP → confirm email arrives
- [ ] Submit a test music request → confirm email arrives

---

## Phase 2 — Demo Client Refresh  `[ ]`

**Estimated time: half a day**

Sofia & Alexander's demo should look clearly more cinematic than Luxe's demo. The differentiation is in the video hero + premium design styles.

- [ ] Verify `hero.videoUrl` is a working, high-quality `.mp4` link (current link may be a placeholder)
  - Use: a cinematic wedding/ballroom clip from a royalty-free source
  - Alternatively: a beautiful Unsplash video URL
- [ ] Confirm preset is `cosmicLuxe` — the most dramatic Prestige preset
- [ ] Review the story copy — make it cinematic and slightly editorial in tone
- [ ] Ensure all 11 sections have real, well-written content (not placeholder text)
- [ ] Check `hero.overlayOpacity` — should be ~0.52 for the video to feel atmospheric without losing the text

---

## Phase 3 — Positioning Check  `[ ]`

**Estimated time: 15 minutes**

The client-facing copy should make Prestige feel clearly below Luxe (which has password + particles + guestbook) but clearly above Signature (which has no video or premium styles).

- [ ] Read the Prestige description in `TIER_03_PRESTIGE.md` — confirm it doesn't promise features only available in Luxe
- [ ] If the marketing site is being built, confirm Prestige tier card mentions: video hero, 3 exclusive styles, 7 premium presets — and clearly notes what it does NOT include (password, guestbook, particles)

---

## Phase 4 — Verify & Launch  `[ ]`

- [ ] Run `CLIENT=_demo-prestige npm run dev` — confirm no console errors
- [ ] Test RSVP with real EmailJS keys → email arrives
- [ ] Test Music form → email arrives
- [ ] All 11 sections render correctly
- [ ] Video hero plays on mobile (muted + autoplay + playsInline)
- [ ] Countdown shows correct time to 2027-12-31
- [ ] Update `TIER_03_PRESTIGE.md` — mark migration complete
- [ ] Update `PLANS.md` — mark Prestige EmailJS migration done

---

## Estimated Timeline

| Phase | Work | Days |
|---|---|---|
| 1 | EmailJS migration | 0.5 |
| 2 | Demo refresh | 0.5 |
| 3 | Positioning check | 0.1 |
| 4 | Verify | 0.25 |
| **Total** | | **~1.5 days** |
