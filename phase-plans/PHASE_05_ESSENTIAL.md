# Phase Plan — Essential (v2.0)

> **Status: Production ready.** Shares the codebase with Signature. The EmailJS migration in `PHASE_04_SIGNATURE.md` fixes Essential too. There is no separate build work for Essential.

**Codebase:** `wedding-websites/wedding-invitation-websites-2.0/` — shared with Signature
**Framework:** React 19 + Vite
**Demo client:** `clients/_demo-essential/` — Emma & Oliver

---

## What Essential Has (do not touch)

- 5 sections: Hero, Our Story, Wedding Details, Gallery, RSVP
- 20 presets, 8 design styles, 10 colour schemes, 6 font pairings
- RSVP form (currently Formspree → migrating to EmailJS)
- Full mobile responsiveness
- Framer Motion scroll animations

---

## Cascade Check — What Essential Does NOT Have

These are removed from Signature when stepping down to Essential. Confirm none of these are present:

- [ ] No countdown timer (`showCountdown: false` in config)
- [ ] No Schedule section
- [ ] No Registry section
- [ ] No Location section
- [ ] No Accommodation section
- [ ] No Music section (so no music.emailjs needed in config)
- [ ] No FAQ section

The `sectionOrder` in `_demo-essential/appsettings.js` should be exactly:
```js
sectionOrder: ['hero', 'story', 'details', 'gallery', 'rsvp'],
```

---

## Phase 1 — EmailJS Migration  `[ ]`

**This is done as part of the Signature migration — same codebase, same fix.**

- [ ] Confirm `PHASE_04_SIGNATURE.md` Phase 1 is complete
- [ ] Confirm `clients/_demo-essential/appsettings.js` has `rsvp.emailjs` block
- [ ] Run `CLIENT=_demo-essential npm run dev` — RSVP form works
- [ ] Submit test RSVP → email arrives

That's the entire Essential migration. No further work needed.

---

## Phase 2 — Verify  `[ ]`

- [ ] `CLIENT=_demo-essential npm run dev` — all 5 sections render
- [ ] `showCountdown: false` — no countdown shown
- [ ] RSVP sends via EmailJS
- [ ] Mobile: looks clean at 375px
- [ ] No console errors, no missing imports
- [ ] Update `TIER_05_ESSENTIAL.md` — mark migration complete
- [ ] Update `PLANS.md` — mark Essential done

---

## Essential's Role in the Business

Essential is the entry point and the upsell funnel. Couples who see the live Signature demo (11 sections vs 5) almost always upgrade themselves — the side-by-side comparison does the work.

**Keep it simple.** Do not add features to Essential. If a client asks for something like a countdown or a schedule, the answer is: "That's our Signature tier — I can have it ready for you in 24 hours."

---

## Estimated Timeline

| Phase | Work | Notes |
|---|---|---|
| 1 | EmailJS migration | Happens with Signature |
| 2 | Verify | 15 min |
| **Total** | **0.25 days** | Piggybacks on Signature |
