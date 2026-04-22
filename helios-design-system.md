# Helios Design System

**Version:** 1.0  
**Source of truth:** `index.html` (single-file static site — all tokens defined in `<style>` block, lines 78–546)  
**Audited:** 2026-04-22  

---

## 1. Token Centralization

All design tokens are defined in a single `:root {}` block at **`index.html:79–91`**. There is no separate CSS file, Tailwind config, or preprocessor — the entire stylesheet lives inline in the HTML `<head>`. This is workable for a single-page site but constitutes light technical debt: any future multi-page expansion would require extracting tokens to a shared `variables.css` or `tokens.json`.

---

## 2. Color Tokens

All values confirmed from `index.html:79–91`.

| Token | Hex Value | Role |
|---|---|---|
| `--gold` | `#C9A84C` | Primary accent, borders, labels |
| `--gold-light` | `#E8C97A` | Hover states, headings emphasis |
| `--gold-pale` | `#F5E4B2` | Card headings, subdued accent |
| `--sun` | `#F4A832` | Logo sun glyph, sun-core gradient |
| `--deep` | `#08090F` | Page background (deepest) |
| `--void` | `#0D0E18` | Secondary deep background |
| `--mid` | `#141524` | Mid-layer surface (dropdowns, overlays) |
| `--surface` | `#1A1C30` | Card / section surface |
| `--muted` | `#8a8070` | UI labels, captions, placeholder copy |
| `--text` | `#EDE8DC` | Primary body text |
| `--text-dim` | `#b8b0a0` | Secondary / de-emphasized body text |

### Color usage notes
- `--deep` (`#08090F`) is the page `background` on `body`.
- `--void` (`#0D0E18`) appears in the mobile overlay `background: rgb(8,9,15)` — **note:** the overlay uses literal `rgb(8,9,15)` rather than `var(--void)`. Minor consistency gap. [Added from codebase audit]
- Error state color is `#e07070` (red) — hardcoded inline on `.field-error` and `.form-group.has-error`; not a CSS custom property. [Added from codebase audit]
- `--gold` as rgba in borders throughout: typically `rgba(201,168,76, <opacity>)` — these are raw values, not token references. Common opacities: `0.08`, `0.1`, `0.12`, `0.15`, `0.18`, `0.25`, `0.35`, `0.5`. [Added from codebase audit]

---

## 3. Typography

### Fonts loaded

Source: `index.html:77` (Google Fonts `<link>`)

```
https://fonts.googleapis.com/css2?
  family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;0,600;1,300;1,400
  &family=Jost:wght@200;300;400;500
  &display=swap
```

| Family | Weights loaded | Source |
|---|---|---|
| Cormorant Garamond | 300, 400, 500, 600 (normal) · 300, 400 (italic) | Google Fonts |
| Jost | 200, 300, 400, 500 | Google Fonts |

### Font roles

| Role | Family | Weight | Notes |
|---|---|---|---|
| Body copy | Jost | 400 | `font-family: 'Jost', sans-serif; font-weight: 400` on `body` |
| UI elements (buttons, nav, labels, form) | Jost | 300–500 | Buttons: 500 · Nav links: 400 · Form inputs: 300 |
| Display headings (h1, h2, h3) | Cormorant Garamond | 300 (upright) · italic | All section headings use `font-weight: 300` |
| Logo wordmark | Cormorant Garamond | 500 | `.nav-logo`: `font-size: 1.6rem; font-weight: 500; letter-spacing: 0.12em` |
| Quotes / blockquote | Cormorant Garamond | 300, italic | `font-style: italic` |

### Type scale (key values)

| Element | Size | Weight | Letter-spacing | Notes |
|---|---|---|---|---|
| Hero h1 | `clamp(3rem, 7vw, 5.8rem)` | 300 | `-0.01em` | Cormorant Garamond |
| Hero tagline | `clamp(1.5rem, 3.5vw, 2.9rem)` | 400 | `0.08em` | Jost |
| Section h2 | `clamp(2.2rem, 4vw, 3.4rem)` | 300 | — | Cormorant Garamond |
| Eyebrow / label | `0.65–0.68rem` | — | `0.35em` | Jost, `text-transform: uppercase` |
| Nav links | `0.72rem` | — | `0.22em` | Jost, uppercase |
| Body copy | `0.95–1rem` | 400 | — | Jost |
| Small / caption | `0.7–0.78rem` | — | varies | Jost |

---

## 4. Spacing & Layout

- Max-width containers: `1200px` (intro, method, specialties) · `1100px` (contact, team)
- Desktop section padding: `100px 60px`
- Mobile section padding: `60px 28px`
- Grid gaps: `100px` (intro desktop) · `80px` (contact desktop) · `2px` (step cards) · `40px` (orbit grid)

---

## 5. Breakpoints

Source: media queries in `index.html:503–545`.

| Breakpoint | Value | Trigger |
|---|---|---|
| Mobile | `max-width: 900px` | Nav collapses to hamburger, layouts go single-column |
| Extra small | `max-width: 374px` | Team stats grid goes single-column |

No tablet-specific breakpoint; the jump goes directly from 900px+ (desktop) to ≤900px (mobile). There is no min-width / desktop-first equivalent — all responsive rules use `max-width`.

Cursor styles also hidden at `max-width: 900px` (`index.html:107`).

---

## 6. Button Styles

### `.btn-primary` (`index.html:214–215`)

```css
padding:          16px 44px
background:       linear-gradient(135deg, #C9A84C, #E8C97A)
color:            var(--deep)   /* #08090F */
font-family:      'Jost', sans-serif
font-weight:      500
font-size:        0.75rem
letter-spacing:   0.2em
text-transform:   uppercase
border-radius:    2px
box-shadow:       0 4px 30px rgba(201,168,76,0.3)
transition:       all 0.3s
```

**Hover:**
```css
transform:    translateY(-2px)
box-shadow:   0 8px 40px rgba(201,168,76,0.45)
```

**Mobile override:** `padding: 14px 16px; font-size: 0.78rem`

### `.btn-ghost` (`index.html:216–217`)

```css
padding:          16px 44px
border:           1px solid rgba(201,168,76,0.35)
color:            var(--gold-light)
font-family:      'Jost', sans-serif
font-weight:      300
font-size:        0.75rem
letter-spacing:   0.2em
text-transform:   uppercase
border-radius:    2px
transition:       all 0.3s
```

**Hover:**
```css
background:     rgba(201,168,76,0.08)
border-color:   var(--gold)
```

### Nav CTA `.nav-cta` (`index.html:141–142`)

```css
padding:        10px 28px
border:         1px solid rgba(201,168,76,0.5)
border-radius:  2px
color:          var(--gold)
transition:     all 0.3s
```

---

## 7. Form Styles

Source: `index.html:396–466`.

### Form container `.booking-form`

```css
background:   rgba(13,14,24,0.7)
border:       1px solid rgba(201,168,76,0.1)
padding:      48px
```
Mobile: `padding: 32px 24px`

### Input / Select / Textarea (`.form-group input`, `select`, `textarea`)

```css
padding:          14px 16px
background:       rgba(8,9,15,0.6)
border:           1px solid rgba(201,168,76,0.15)
border-radius:    2px
color:            var(--text)
font-family:      'Jost', sans-serif
font-size:        1rem
font-weight:      300
transition:       border-color 0.3s, background 0.3s
-webkit-appearance: none
```

**Focus state:**
```css
outline:      none
border-color: rgba(201,168,76,0.5)
background:   rgba(13,14,24,0.8)
```

**Error state (`.form-group.has-error`):**
```css
border-color: #e07070
background:   rgba(224,112,112,0.04)
```

**Valid state (`.form-group.is-valid`):**
```css
border-color: rgba(201,168,76,0.45)
```

### Form submit `.form-submit`

```css
width:            100%
padding:          18px
background:       linear-gradient(135deg, #C9A84C, #E8C97A)
color:            var(--deep)
font-family:      'Jost', sans-serif
font-weight:      500
font-size:        0.75rem
letter-spacing:   0.22em
text-transform:   uppercase
border:           none
border-radius:    2px
transition:       all 0.3s
box-shadow:       0 4px 30px rgba(201,168,76,0.25)
```

---

## 8. Hero Animation

Source: `index.html:187–221` (CSS) and `index.html:581–603` (HTML).

### Sun

```css
width/height:   clamp(140px, 16vw, 280px)
border-radius:  50%
background:     radial-gradient(circle at 35% 35%, #FFE5A0, #F4A832 40%, #C97A15 75%, #7A3E00)
box-shadow:     0 0 60px 28px rgba(244,168,50,0.4), ...
animation:      pulse-sun 4s ease-in-out infinite
will-change:    box-shadow
```

Mobile override: `width: 50px; height: 50px`

### Orbital rings

Rendered as **CSS `border` on `div` elements** (not SVG). Each `.orbit` has:
```css
border-radius:  50%
border:         1px solid rgba(201,168,76, <opacity>)
animation:      spin linear infinite
will-change:    transform
```

### Planets — count and durations

**7 planets total** (one per orbit ring).

| Orbit | Duration | Direction | Planet size | Planet color |
|---|---|---|---|---|
| `.orbit-1` | `14s` | normal | 12×12px | Blue — `#4E9BB5` |
| `.orbit-2` | `22s` | normal | 16×16px | Tan — `#8B6914` |
| `.orbit-3` | `35s` | **reverse** | 10×10px | Mauve — `#8B5A7E` |
| `.orbit-4` | `50s` | normal | 14×14px | Green — `#4A7D42` |
| `.orbit-5` | `68s` | normal | 18×18px | Blue — `#5B99CC` |
| `.orbit-6` | `95s` | **reverse** | 11×11px | Rust — `#8B4A32` |
| `.orbit-7` | `132s` | normal | 22×22px | Indigo — `#3A456E` |

**Mobile:** orbits 3–7 are hidden (`display: none`) on ≤900px; only orbits 1 and 2 are shown, with a scaled-down sun (50px).

### Performance
- `will-change: transform` — set on all `.orbit` elements ✓
- `will-change: box-shadow` — set on `.sun-core` ✓

---

## 9. Animation & Motion

| Name | Duration | Easing | Usage |
|---|---|---|---|
| `fade-up` | `1s` | ease | Hero content entrance (staggered: 0.3s–1.4s delays) |
| `pulse-sun` | `4s` | ease-in-out | Sun glow pulse |
| `spin` | varies | linear | Orbit rotation |
| `scroll-pulse` | `2s` | ease-in-out | Scroll indicator fade |
| `dot-pulse` | `1.2s` | ease-in-out | Form submit loading spinner |

`prefers-reduced-motion: reduce` is respected: all animation/transition durations collapse to `0.01ms` (`index.html:493–500`).

---

## 10. Navigation

- Fixed top bar, z-index `200`
- Desktop padding: `28px 60px` → scrolled: `16px 60px`
- Logo: Cormorant Garamond, `1.6rem`, weight 500, `letter-spacing: 0.12em`
- Nav links: Jost, `0.72rem`, `letter-spacing: 0.22em`, uppercase
- Mobile menu: full-screen overlay with Cormorant Garamond `2.2rem italic` links, `font-weight: 300`
- Custom cursor (dot + ring) hidden on mobile / pointer: coarse devices

---

## 11. Elevation & Surfaces

| Layer | Token / Value | Usage |
|---|---|---|
| Background | `--deep` `#08090F` | Page base |
| Overlay / void | `--void` `#0D0E18` | Mobile overlay |
| Mid | `--mid` `#141524` | Dropdowns, select options |
| Surface | `--surface` `#1A1C30` | Cards, booking form |
| Card semi-transparent | `rgba(13,14,24,0.7)` | Method steps, form bg |
| Hover surface | `rgba(20,21,36,0.9)` | Step card hover |

---

## 12. Accessibility

- `:focus-visible` outline: `2px solid var(--gold)`, offset `3px`, `border-radius: 2px`
- Mouse-click `:focus:not(:focus-visible)` outline suppressed
- `prefers-reduced-motion` supported
- Custom cursor hidden on `pointer: coarse` (touch) devices
- Noise texture overlay (`opacity: 0.018`) is decorative, `pointer-events: none`

---

## 13. Verification Checklist

- [x] CSS custom property names confirmed (`index.html:79–91`)
- [x] All `:root` property values recorded in Section 2
- [x] Body font confirmed: **Jost, weight 400** (`index.html:96–99`)
- [x] Display/heading font confirmed: **Cormorant Garamond** (`index.html:77`)
- [x] Exact hex values confirmed for all 11 color tokens
- [x] Breakpoints confirmed: `900px` (mobile), `374px` (extra-small)
- [x] `.btn-primary` padding, border-radius, font-size, font-weight, transition documented
- [x] `.btn-ghost` styles documented
- [x] Form input padding, border, background, focus state documented
- [x] Hero planet count confirmed: **7 planets**
- [x] Hero animation durations confirmed per orbit (14s–132s)
- [x] Sun diameter confirmed: `clamp(140px, 16vw, 280px)`
- [x] Orbital rings confirmed: **CSS borders on `div` elements** (not SVG)
- [x] `will-change: transform` confirmed on animated orbit elements ✓

---

## 14. Consistency Gaps & Technical Debt

1. **Mobile overlay uses raw `rgb(8,9,15)` instead of `var(--void)`** — the value matches `--void` / near `--deep` but bypasses the token system. Line 162.
2. **Gold border opacities are inline `rgba()` values, not tokens** — `rgba(201,168,76, 0.08/0.1/0.12/0.15/0.25/0.35/0.5)` appear throughout. A sub-token set (`--gold-border-subtle`, `--gold-border`, `--gold-border-strong`) would reduce duplication.
3. **Error color `#e07070` is not a token** — appears on `.field-error`, `.form-group.has-error`, and `.form-net-error` as a hardcoded literal. Should become `--color-error`.
4. **`--void` (`#0D0E18`) vs `--deep` (`#08090F`) distinction is unclear in usage** — `--deep` is the body background; `--void` doesn't appear as a `var(--void)` reference anywhere in the stylesheet (card backgrounds use raw `rgba()` values). Token may be vestigial.
5. **All styles are inline in `index.html`** — no external `.css` file. Acceptable for a single-page site today, but extracting to `styles.css` + `tokens.css` would be the correct move before adding a second page.
