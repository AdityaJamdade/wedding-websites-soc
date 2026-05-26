# Tier 4 — Signature

> Production-ready. The complete wedding companion. 11 sections, live countdown, full design library. Everything guests need in one place.

---

## Status
- **Build status:** Production ready ✓
- **Codebase:** `wedding-invitation-websites-2.0/` *(same codebase as Essential — tier controlled by `sectionOrder` in config)*
- **Framework:** React 19 + Vite
- **Demo client:** `clients/_demo-signature/` — Isabella & James

---

## Pricing
- **Price:** $397 (fixed)
- **Payment:** 50% upfront via Stripe, 50% on delivery
- **Turnaround:** 24 hours from content receipt
- **Client onboarding time:** 1–2 hours

---

## What's Included

### Everything in Essential, plus:
- **6 additional sections:** Schedule, Registry, Location, Accommodation, Music Requests, FAQ
- **Live countdown timer** (days, hours, minutes, seconds)
- **Day-of schedule** — timeline of ceremony and reception events
- **Gift registry** — links to multiple stores
- **Location section** — Google Maps embed + transport notes
- **Accommodation section** — hotel blocks with promo codes, star ratings, shuttle info
- **Music requests form** — guests suggest songs (via EmailJS)
- **FAQ accordion** — custom Q&A, collapsible

### Sections (all 11)
`hero` · `story` · `details` · `schedule` · `gallery` · `rsvp` · `registry` · `location` · `accommodation` · `music` · `faq`

---

## What's NOT Included
*(Upgrade to Prestige for these)*
- Cinematic video hero background
- Exclusive Prestige design styles (auroraGlass, velvetNight, immersive3d)
- Exclusive Prestige presets (#21–27) and colour schemes

*(Upgrade to Luxe for these)*
- Password protection
- Particle animations
- Digital guestbook
- Custom domain setup included
- RSVP notifications to couple

---

## Design Options (identical to Essential — full library available)

### Standard Presets (#1–20)

| # | Name | Style | Vibe |
|---|---|---|---|
| 1 | `romanticBloom` | botanical | Soft florals, garden romance |
| 2 | `modernLuxe` | glassmorphism | Sleek, editorial, cool tones |
| 3 | `goldenHour` | neumorphism | Warm sunset, golden, tactile |
| 4 | `midnightGarden` | darkAcademia | Dark, lush, moody |
| 5 | `blushDreams` | claymorphism | Pastel, playful, soft |
| 6 | `coastalBreeze` | minimalism | Clean, ocean-fresh, airy |
| 7 | `crystalClear` | glassmorphism | Crisp, luminous, modern |
| 8 | `velvetRomance` | darkAcademia | Rich, warm, intimate |
| 9 | `sageGarden` | botanical | Natural, earthy, organic |
| 10 | `champagneDreams` | neumorphism | Soft luxury, champagne tones |
| 11 | `emeraldForest` | botanical | Deep green, lush, forest |
| 12 | `lavenderMist` | claymorphism | Dreamy, purple, soft |
| 13 | `rustedGold` | neoBrutalism | Bold, earthy, raw |
| 14 | `arcticWhite` | minimalism | Ultra-clean, Scandinavian |
| 15 | `dustyRoseGarden` | botanical | Vintage, muted pink, romantic |
| 16 | `slateAndGold` | glassmorphism | Sophisticated, masculine-leaning |
| 17 | `ivoryElegance` | neumorphism | Timeless, classic, warm white |
| 18 | `liquidSunset` | liquidGlass | Fluid, warm, colourful |
| 19 | `boldBotanical` | neoBrutalism | Graphic, punchy, floral |
| 20 | `midnightElegance` | darkAcademia | Deep navy, silver, formal |

### Standard Design Styles (8)
`glassmorphism` · `claymorphism` · `neumorphism` · `minimalism` · `liquidGlass` · `darkAcademia` · `neoBrutalism` · `botanical`

### Standard Colour Schemes (10)
`roseGold` · `blush` · `midnight` · `sage` · `champagne` · `dustyRose` · `emerald` · `slate` · `ivory` · `lavender`

### Font Pairings (6)
`romantic` · `script` · `classic` · `editorial` · `modern` · `bold`

---

## Tech Stack (Minimal Services)

| Purpose | Tool | Notes |
|---|---|---|
| Framework | React 19 + Vite | Production ready |
| Hosting | Vercel | Per-client deployment |
| RSVP form | **EmailJS** (replaces Formspree) | ⚠️ Migration needed — see below |
| Music requests | EmailJS | Same migration |
| Payments | Stripe Payment Link | Static link |

**No Supabase. No Formspree. No Cloudinary.**

---

## EmailJS Migration (same as Prestige)

Current v2.0 uses Formspree. Migrate to EmailJS in RSVP.jsx and Music.jsx.

```js
// appsettings.js — new keys (replace formspreeId)
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

One migration fixes both Signature and Essential simultaneously (same codebase).

---

## appsettings.js Schema

```js
const config = {
  preset: 'modernLuxe',      // any of presets #1–20

  couple: {
    partner1: 'Isabella',
    partner2: 'James',
  },

  event: {
    date:    '2027-08-14',
    time:    '4:00 PM',
    venue:   'The Orangery',
    address: '1 Garden Mews, London, UK',
    city:    'London, UK',
    mapUrl:  'https://maps.google.com/?q=...',
  },

  showCountdown: true,   // ← Signature enables countdown; Essential sets false

  hero: {
    preHeading:      'Together with their families',
    backgroundImage: 'https://...',
    overlayOpacity:  0.45,
  },

  // Signature: all 11 sections enabled
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
    mealOptions: ['Chicken', 'Fish', 'Vegetarian', 'Vegan'],
  },

  music: {
    emailjs: {
      serviceId:  'service_xxxxx',
      templateId: 'template_music_xxxxx',
      publicKey:  'xxxxxxxxxxxxxxx',
    },
  },

  // ... story, schedule, gallery, registry, location, accommodation, faq arrays
};

export default config;
```

---

## File Structure (current — shared with Essential)

```
wedding-invitation-websites-2.0/    ← shared codebase for Signature + Essential
  clients/
    _demo/                          ← legacy
    _demo-essential/
      appsettings.js                ← Emma & Oliver (5 sections only)
    _demo-signature/
      appsettings.js                ← Isabella & James (all 11 sections)
    [client-name]/
      appsettings.js
  template/
    package.json
    vite.config.js
    index.html
    src/
      (same structure as Prestige, minus premium styles)
```

**Note:** Signature and Essential are the same codebase. The only difference is which sections are listed in `sectionOrder` and whether `showCountdown` is true.

---

## How to Add a New Client

```bash
# 1. Copy demo config
cp -r clients/_demo-signature clients/[client-name]

# 2. Edit appsettings.js
# - Set preset (#1–20), couple, event, emailjs keys
# - Fill story, schedule, gallery, registry, accommodation, faq arrays

# 3. Dev preview
CLIENT=client-name npm run dev

# 4. Build + Deploy
CLIENT=client-name npm run build
# → dist/ → Vercel new project
```

---

## Pending Work
- [ ] Migrate RSVP.jsx and Music.jsx from Formspree → EmailJS (also fixes Essential)
- [ ] Refresh Isabella & James demo to use a more visually impressive preset

---

## Demo Client

| Field | Value |
|---|---|
| Names | Isabella & James |
| Date | 2027-08-14 |
| Venue | The Orangery, London |
| Preset | `modernLuxe` |
| Sections | All 11 |

---

## Client-Facing Description

### Signature — *The complete wedding companion your guests will love*

Everything in Essential, plus a full suite of sections that keep your guests informed, organised, and excited — from save-the-date to last dance.

Your website becomes a one-stop destination: they can check the day's schedule, find where to stay, add a song to the playlist, discover registry details, and get answers to every question — all before they even pick up the phone.

**What your guests get:**
- A live countdown building excitement from the moment they open the link
- The full day-of schedule so no one misses a moment
- Gift registry links so gifting is effortless
- Hotel recommendations with room block codes and shuttle info
- An interactive map and transport directions
- A song request form for the playlist
- A FAQ section that answers questions before they're asked

**Perfect for:** Couples who want their guests to have everything they need in one beautiful place.
