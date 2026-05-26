# Tier 3 — Prestige

> Production-ready. The top of the existing template tiers. Cinematic video hero, exclusive design styles, 11 full sections.

---

## Status
- **Build status:** Production ready ✓
- **Codebase:** `wedding-invitation-websites-3.0/` *(rename to `v3.0-prestige/` when convenient)*
- **Framework:** React 19 + Vite
- **Demo client:** `clients/_demo-prestige/` — Sofia & Alexander

---

## Pricing
- **Price:** $797 (fixed)
- **Payment:** 50% upfront via Stripe, 50% on delivery
- **Turnaround:** 24–48 hours from content receipt
- **Client onboarding time:** 2–4 hours

---

## What's Included

### Everything in Signature, plus:
- **Cinematic video hero** — full-screen looping video background (`hero.videoUrl`)
- **3 exclusive design styles** — not available on Signature or Essential
- **3 exclusive premium colour schemes**
- **7 exclusive premium presets** (#21–27)
- **27 total presets**, 11 design styles, 13 colour schemes, 6 font pairings

### Sections (all 11)
`hero` · `story` · `details` · `schedule` · `gallery` · `rsvp` · `registry` · `location` · `accommodation` · `music` · `faq`

### Live countdown timer (days, hours, minutes, seconds)

---

## What's NOT Included
*(Upgrade to Luxe for these)*
- Password protection
- Ambient particle animations
- Digital guestbook
- Custom domain setup included
- RSVP notifications to couple
- Exclusive Luxe design styles (sakuraDrift, grandHotel, etc.)
- Post-wedding gallery update

---

## Design Options

### Exclusive Prestige Design Styles (Prestige only)

| Key | Class | Vibe |
|---|---|---|
| `auroraGlass` | `style-aurora-glass` | Iridescent aurora borealis glassmorphism, shifting prismatic shimmer on headings |
| `velvetNight` | `style-velvet-night` | Deep velvet purple luxury with warm gold accents, opulent and tactile |
| `immersive3d` | `style-immersive-3d` | Layered 3D shadows, perspective card tilt on hover, extruded heading depth |

### Exclusive Prestige Colour Schemes

| Key | Primary | Background | Vibe |
|---|---|---|---|
| `cosmicGold` | `#D4AF6A` | `#0A0818` | Deep space / black-tie |
| `auroraBlue` | `#6DD5D0` | `#060E1A` | Deep ocean / ethereal |
| `velvetNight` | `#C9A0DC` | `#0E0614` | Deep purple / opulent |

### Exclusive Prestige Presets (#21–27)

| # | Name | Style | Colour Scheme | Best For |
|---|---|---|---|---|
| 21 | `cosmicLuxe` | immersive3d | cosmicGold | Luxury evening galas, black-tie |
| 22 | `darkVelvet` | velvetNight | velvetNight | Candlelit, castle, masquerade |
| 23 | `auroraWedding` | auroraGlass | auroraBlue | Winter, destination, Scandinavian |
| 24 | `velvetRose` | velvetNight | velvetNight | Evening receptions, floral-heavy |
| 25 | `celestialDawn` | auroraGlass | auroraBlue | Sunrise, outdoor luxury, beach |
| 26 | `starCrossed` | immersive3d | cosmicGold | Film lovers, observatory venues |
| 27 | `crystalAurora` | auroraGlass | cosmicGold | Crystal ballrooms, NYE galas |

### Standard Options (inherited from Signature + Essential)

- **20 standard presets** (#1–20): romanticBloom → modernLuxe
- **8 standard design styles:** glassmorphism, claymorphism, neumorphism, minimalism, liquidGlass, darkAcademia, neoBrutalism, botanical
- **10 standard colour schemes:** roseGold, blush, midnight, sage, champagne, dustyRose, emerald, slate, ivory, lavender
- **6 font pairings:** romantic, script, classic, editorial, modern, bold

---

## Tech Stack (Minimal Services)

| Purpose | Tool | Notes |
|---|---|---|
| Framework | React 19 + Vite | Production ready, no changes needed |
| Hosting | Vercel | Per-client deployment |
| RSVP form | **EmailJS** (replaces Formspree) | ⚠️ Migration needed: swap Formspree fetch for EmailJS SDK |
| Music requests | EmailJS | Same — replace Formspree |
| Domain | Client manages or we set up | Vercel domain assignment |
| Payments | Stripe Payment Link | Static link |

**Migration note:** Current v3.0 uses Formspree. Before onboarding new Prestige clients, migrate RSVP.jsx and Music.jsx to use EmailJS. Backwards-compatible — just swap the submit handler.

---

## EmailJS Migration (RSVP.jsx)

Replace the current Formspree fetch call:

```js
// BEFORE (Formspree)
const res = await fetch(`https://formspree.io/f/${formspreeId}`, {
  method: 'POST',
  headers: { 'Content-Type': 'application/json', Accept: 'application/json' },
  body: JSON.stringify({ ...form, attending }),
});

// AFTER (EmailJS) — add to package.json: "@emailjs/browser": "^4.0.0"
import emailjs from '@emailjs/browser';

await emailjs.send(
  config.rsvp.emailjs.serviceId,
  config.rsvp.emailjs.templateId,
  { ...form, attending, partner1: config.couple.partner1, partner2: config.couple.partner2 },
  config.rsvp.emailjs.publicKey
);
```

New `appsettings.js` key (replaces `rsvp.formspreeId`):
```js
rsvp: {
  emailjs: {
    serviceId:  'service_xxxxx',
    templateId: 'template_rsvp_xxxxx',
    publicKey:  'xxxxxxxxxxxxxxx',
  },
  maxGuests: 4,
},
music: {
  emailjs: {
    serviceId:  'service_xxxxx',
    templateId: 'template_music_xxxxx',
    publicKey:  'xxxxxxxxxxxxxxx',
  },
},
```

---

## appsettings.js Schema

```js
const config = {
  preset: 'cosmicLuxe',         // any of presets #1–27

  couple: {
    partner1: 'Sofia',
    partner2: 'Alexander',
  },

  event: {
    date:    '2027-12-31',
    time:    '6:00 PM',
    venue:   'The Grand Celestial Ballroom',
    address: '1 Starlight Plaza, New York, NY 10001',
    city:    'New York, NY',
    mapUrl:  'https://maps.google.com/?q=...',
  },

  showCountdown: true,

  hero: {
    preHeading:      'Together with their families',
    backgroundImage: 'https://...',
    videoUrl:        'https://...',    // null for image-only
    overlayOpacity:  0.52,
  },

  sectionOrder: [
    'hero', 'story', 'details', 'schedule',
    'gallery', 'rsvp', 'registry', 'location',
    'accommodation', 'music', 'faq',
  ],
  sections: {
    hero: true, story: true, details: true, schedule: true,
    gallery: true, rsvp: true, registry: true, location: true,
    accommodation: true, music: true, faq: true,
  },

  rsvp: {
    emailjs: {
      serviceId:  'service_xxxxx',
      templateId: 'template_rsvp_xxxxx',
      publicKey:  'xxxxxxxxxxxxxxx',
    },
    maxGuests: 4,
  },

  music: {
    emailjs: {
      serviceId:  'service_xxxxx',
      templateId: 'template_music_xxxxx',
      publicKey:  'xxxxxxxxxxxxxxx',
    },
  },

  // ... story, schedule, gallery, registry, location, accommodation, faq
};

export default config;
```

---

## File Structure (current)

```
wedding-invitation-websites-3.0/    ← rename to v3.0-prestige/ when ready
  TIER.md
  clients/
    _demo/                          ← legacy demo
    _demo-prestige/
      appsettings.js                ← Sofia & Alexander
    [client-name]/
      appsettings.js
  template/
    package.json
    vite.config.js
    index.html
    src/
      main.jsx
      App.jsx
      config/
      providers/
      sections/
        Hero.jsx · Story.jsx · Details.jsx · Schedule.jsx
        Gallery.jsx · RSVP.jsx · Registry.jsx · Location.jsx
        Accommodation.jsx · Music.jsx · FAQ.jsx
      shell/
      ui/
      styles/
        index.css
        design-styles/
          glassmorphism.css · claymorphism.css · neumorphism.css
          minimalism.css · liquid-glass.css · dark-academia.css
          neo-brutalism.css · botanical.css
          aurora-glass.css · velvet-night.css · immersive-3d.css
```

---

## How to Add a New Client

```bash
# 1. Copy demo config
cp -r clients/_demo-prestige clients/[client-name]

# 2. Edit appsettings.js
# - Set preset (any of #1–27), couple, event, sections, emailjs keys
# - Set hero.videoUrl for video background (or null for image)

# 3. Dev preview
CLIENT=client-name npm run dev   # Windows: $env:CLIENT="client-name"; npm run dev

# 4. Build + Deploy to Vercel
CLIENT=client-name npm run build
# → dist/ → Vercel new project
```

---

## Pending Enhancements

- [ ] Migrate RSVP.jsx and Music.jsx from Formspree → EmailJS
- [ ] Refresh Sofia & Alexander demo to be more spectacular
- [ ] Update client-facing copy to differentiate from Luxe (Prestige = drama; Luxe = features)
- [ ] Rename folder `wedding-invitation-websites-3.0/` → `v3.0-prestige/` (do when no active clients)

---

## Demo Client

| Field | Value |
|---|---|
| Names | Sofia & Alexander |
| Date | 2027-12-31 (New Year's Eve) |
| Venue | The Grand Celestial Ballroom, New York |
| Preset | `cosmicLuxe` |
| Hero | Video background (cinematic ballroom footage) |

---

## Client-Facing Description

### Prestige — *An experience as extraordinary as your wedding*

Prestige is for couples who want their wedding website to feel like a piece of the wedding itself — not just a page on the internet, but an atmosphere.

The moment your guests open the link, a cinematic video fills their screen. The design is drawn from an exclusive visual language not available on any other plan — aurora glass, velvet night, immersive 3D — each one a complete aesthetic, not just a colour palette.

**What makes Prestige different:**
- **Cinematic video hero** — your film plays full-screen the moment guests arrive
- **3 exclusive design styles** — auroraGlass, velvetNight, immersive3d — not on any other plan
- **7 exclusive premium presets** — cosmicLuxe, darkVelvet, auroraWedding, and more
- The full 11-section wedding companion — schedule, registry, maps, accommodation, music, FAQ
- Live countdown timer to the day

**Perfect for:** Couples with a strong visual vision who want their site to feel luxury.
