# Reach Supports — Landing Page Design & Handoff Notes

One-page scroll-based brand experience. Live design prototype: `index.html` (self-contained — all CSS/JS inline). Inspired by feedforge / heco.partners: generous whitespace, oversized serif typography, soft accents, full-bleed media rows, reveal-on-scroll motion.

---

## 1. Brand & design tokens

All tokens live in `:root` at the top of `index.html`.

### Colour
| Token | Hex | Use |
|---|---|---|
| `--paper` | `#FAF9F6` | Page background (warm white) |
| `--paper-2` | `#F3EFE7` | Warm sand panel ("What we do") |
| `--ink` | `#1B211D` | Text, dark sections, footer (green-black, softer than pure black) |
| `--sage` | `#5F8D74` | Primary accent — underlines, ticks, step rings |
| `--sage-deep` | `#3E6B54` | Accent text (italic highlights), hover states |
| `--sage-tint` | `#E6F0E9` | Soft fills, glyph chips, gradients |
| `--clay` | `#D89A6E` | Warm secondary (tiles, gradients only — never text) |
| `--clay-tint` | `#F6E7D8` | Soft warm fills |
| `--mist` | `#E4EAF3` | Cool tertiary for tile variety |

Dark section accent text: `#9CC4AC` (eyebrows) and `#B8D8C4` (stats / italics).

### Typography
- **Display: Fraunces** (Google Fonts, variable optical size 9–144, weights 300–600 + italic). Headlines at weight 400, tight line-height 1.06, letter-spacing −0.015em. *Italic Fraunces in sage* is the signature accent — one emphasised phrase per headline, never more.
- **Body/UI: Manrope** (400–800). Body 17px / 1.65. Buttons 700. Eyebrows: 12px, 700, 0.22em tracking, uppercase — this deliberately echoes the spaced-caps "MENTAL HEALTH COMMUNITY SERVICES" tagline in the logo.
- The script logo is artwork, not a font — never set text in a script face anywhere else on the page.

### Scale (fluid, clamp-based)
- Display H1 / final CTA: `clamp(44px → 104px)`
- Section H2: `clamp(36px → 72px)`
- Section padding: `clamp(96px → 168px)`
- Radii: cards 28px, inner cards 20px, pills 999px, big panels `clamp(28→48px)`

### Logo usage
- `assets/logo-black.png` — tight-cropped black wordmark (transparent PNG). Nav 34px high (28px when scrolled), footer 40px with `filter: invert(1)` for the white version.
- `assets/rs-monogram.jpg` — RS monogram, used as favicon. Ask the designer for a transparent-background PNG/SVG export of this for production.
- The white logo files in Downloads ("white font crop" etc.) are corrupted (overlapping duplicate artwork) — do not use; regenerate from `reach supports_option2.ai` if a real white export is needed.

---

## 2. Page structure (sections in order)

1. **Nav** — fixed, transparent → frosted blur + hairline on scroll (`.scrolled` at 40px). Links hidden below 680px (single-page nav; CTA button remains).
2. **Hero** — centred. Eyebrow → H1 with word-by-word rise → lede → 2 CTAs → **full-bleed video card row** → services marquee.
3. **Who we support** (`#who`) — 6 cards, 3×2 grid (2-col tablet, 1-col mobile).
4. **What we do** (`#what`) — sand panel, 6 numbered index rows, hover: indent + arrow rotates −45°.
5. **Our approach** (`#approach`) — sticky left headline, 4 numbered steps scroll on the right.
6. **Referral partners** (`#partners`) — dark ink panel: 3 animated stats, 3-step referral process, light CTA.
7. **Participants & families** (`#family`) — checklist + large photo placeholder.
8. **Community gallery** (`#community`) — drag-to-scroll photo strip.
9. **Video/story** (`#story`) — 16:8.2 media placeholder with pulsing play button.
10. **Final CTA** (`#contact`) — centred display headline, mailto CTAs.
11. **Footer** — ink, inverted logo, link columns, Acknowledgement of Country.

---

## 3. Hero video card row (feedforge-style)

- `.media-track`: full-viewport-width horizontal strip, centred on load via JS so cards bleed off **both** edges. Drag-to-scroll (pointer events) + `scroll-snap-align: center`. Scrollbar hidden.
- `.v-card`: portrait 9:13.6, `clamp(200px → 262px)` wide (64vw on mobile), radius 22px. Odd cards offset +14px vertically for rhythm.
- Card anatomy: gradient placeholder bg → bottom legibility gradient (`::after`) → play badge top-right → caption (bold title + `↗ category` line).
- Hover: lift −10px + scale 1.02, deeper shadow, play badge scales.
- **Production intent:** each card = muted, autoplaying, looping `<video>` (6–10s clips of real support moments — café catch-ups, day trips, skill building, sport, events, celebrations, volunteering). `object-fit: cover`, `playsinline muted loop autoplay`, poster frame fallback. Pause offscreen loops via IntersectionObserver. Keep the caption/gradient overlay.

## 4. Motion system

Respect `prefers-reduced-motion` (already handled — everything snaps to final state).

| Element | Behaviour | Spec |
|---|---|---|
| Hero H1 | Word-by-word rise from overflow mask | 1.1s, `cubic-bezier(.22,.68,0,1)`, stagger 0.07s/word, start 0.08s |
| Hero lede/CTAs/media row | Fade-up | 1s, delays 0.55 / 0.7 / 0.9s |
| All `[data-reveal]` | Fade + 44px rise when 12% visible, once | 0.9s, per-element `--d` stagger (0.05–0.3s) |
| Nav | bg blur + shrink | trigger: scrollY > 40 |
| Hero blobs | Parallax on scroll | factors 0.25 / −0.15, rAF-throttled |
| Marquee | Infinite loop, pause on hover | 34s linear, duplicated content, translateX(−50%) |
| Stats | Count-up when 60% visible | 1.4s, cubic ease-out |
| Card rows | Drag-to-scroll + snap | pointer capture; snap disabled while dragging |

---

## 5. Placeholders to replace before launch

- **Emails:** `referrals@reachsupports.com` / `hello@reachsupports.com` — confirm these exist or swap.
- **Stats (dark section):** "24hr / 100% / 1:1" — the 24hr response promise appears three times (stats, step 2, final CTA). Only keep it if it's a real commitment.
- **Hero video cards (×7)** — real video loops or photos.
- **Photo placeholders:** family section portrait (4:4.6) + 7 gallery tiles (3:4 and 4:3). Candid, warm, natural light; the Christmas-event photos you have are exactly the right tone for "Celebrations".
- **Video section** — brand film; wire the play button to a lightbox or embed when ready.
- **Phone number** — none on the page currently; add to final CTA + footer if wanted.
- Favicon: replace JPG monogram with transparent PNG/ICO.

## 6. Production notes

- Single static file — host anywhere (Netlify/Vercel/S3). No build step, no dependencies beyond Google Fonts.
- Fonts: self-host Fraunces + Manrope woff2 for production (performance + privacy); current setup uses Google Fonts CDN.
- Accessibility: semantic landmarks, aria-labels on icon buttons, focus styles inherit browser defaults (consider custom `:focus-visible` ring in sage), reduced-motion supported, colour contrast AA on all text (ink-60 on paper ≈ 5.9:1).
- SEO: title + meta description set. Add OpenGraph tags + og-image when the domain goes live.
- Local preview: `node .claude/serve.js` → http://localhost:4173
