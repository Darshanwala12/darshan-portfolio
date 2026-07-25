# Design Specification — Darshan Wala Portfolio

## Design Philosophy

Dark, premium, developer-aesthetic. Inspired by terminal/IDE interfaces — deep space background with electric blue/cyan/purple accents. No external libraries or CDNs (CSP-safe for Claude Artifacts).

---

## Color Tokens

```css
--bg0:   #050816   /* page background — near black with a blue bias */
--bg1:   #0b1120   /* secondary bg — nav, footer */
--bg2:   #111827   /* card / panel bg */
--bg3:   #1f2937   /* input fields, hover surfaces */
--text:  #f1f5f9   /* primary text */
--muted: #94a3b8   /* secondary text, labels */
--dim:   #475569   /* placeholder, disabled */
--blue:  #3b82f6   /* primary accent */
--cyan:  #22d3ee   /* secondary accent */
--purple:#a78bfa   /* tertiary accent */
--grad:  linear-gradient(135deg, #3b82f6 0%, #22d3ee 50%, #a78bfa 100%)
--border:#1e293b   /* subtle dividers */
--glow:  rgba(59,130,246,0.15)  /* card hover glow */
```

---

## Typography

All system stack — no webfont imports (avoids CDN blocking in Artifact CSP).

```css
font-family: 'SF Pro Display', 'Segoe UI', system-ui, -apple-system, sans-serif;
/* Monospace (terminal window, code labels) */
font-family: 'SF Mono', 'Fira Code', 'Cascadia Code', monospace;
```

| Role | Size | Weight |
|---|---|---|
| Name / H1 | 3.5rem–5rem fluid | 800 |
| Section heading | 2rem | 700 |
| Sub-heading | 1.2rem | 600 |
| Body | 1rem | 400 |
| Caption / label | 0.75–0.85rem | 500 |
| Terminal text | 0.85rem monospace | 400 |

---

## Layout

- Max content width: `1200px`, centered
- Section padding: `100px 0` (desktop), `60px 0` (mobile)
- Grid: CSS Grid + Flexbox; no external grid system
- Responsive breakpoint: `768px` (mobile), `1024px` (tablet)

---

## Key Components

### Custom Cursor
- `#cd` — 8px blue dot, follows mouse directly
- `#cr` — 36px ring, follows with 80ms lag (CSS `transition`)
- Hidden on touch devices (`@media (hover: none)`)

### Scroll Progress Bar
- Fixed top bar, `height: 3px`, gradient fill
- Width driven by JS `scrollY / scrollHeight`

### Nav
- Fixed, blur backdrop (`backdrop-filter: blur(20px) saturate(180%)`)
- Gradient text logo, gradient underline on active link
- Mobile: hamburger → slide-down menu

### Hero Terminal Window
- Fake macOS traffic lights (red/yellow/green circles)
- Typed lines revealed with CSS `animation-delay` stagger
- Cursor blink animation on last line

### Typing Roles
- 6 roles cycle with JS: typewriter character-by-character, pause, delete, next
- Roles: `Full-Stack Developer`, `React & Next.js Expert`, `WordPress Architect`, `Laravel & Node.js Dev`, `AI Integration Specialist`, `eCommerce Builder`

### Skill Cards
- `background: var(--bg2)`, `border: 1px solid var(--border)`
- Hover: `border-color: var(--blue)`, `box-shadow: 0 0 20px var(--glow)`, `translateY(-4px)`
- Emoji icon + name + tag list inside

### Experience Timeline (Expandable)
- Left border `2px solid var(--blue)` with dot connector
- Click `.tl-tog` → toggles `.tl-body` via `max-height` CSS transition (0 → 500px)
- JS adds/removes `.open` class; rotates `▶` arrow to `▼`

### Project Cards
- Same hover as skill cards
- Tech tag pills: `background: rgba(59,130,246,0.1)`, `color: var(--blue)`, `border-radius: 20px`

### Tech Ticker
- Two identical `<span>` sets inside `.ticker-inner`
- `@keyframes ticker { 0% { transform: translateX(0) } 100% { transform: translateX(-50%) } }`
- Speed: `30s linear infinite`
- Pauses on hover (`animation-play-state: paused`)

### Ctrl+K Command Palette
- Overlay (`position: fixed`, `z-index: 1000`, backdrop blur)
- Items: scroll to Hero / About / Skills / Experience / Projects / Contact, open GitHub, open LinkedIn, copy email, toggle theme
- Keyboard: `ArrowUp/Down` to navigate, `Enter` to run, `Escape` to close
- Opened by `Ctrl+K` or clicking `⌕` button in nav

### Contact Form
- Pure HTML `<form>` — no backend wired up (action="" / JS preventDefault)
- Fields: Name, Email, Subject, Message
- Styled inputs with `var(--bg3)` background, blue focus ring

---

## Animations

| Element | Trigger | Effect |
|---|---|---|
| Sections | Intersection Observer (threshold 0.1) | `opacity 0→1`, `translateY(30px→0)`, `0.6s ease` |
| Stat counters | Intersection Observer | Count up from 0 over 2s |
| Hero gradient text | CSS `@keyframes gradientShift` | Background-position shift on `--grad` |
| Nav links | Hover | Gradient underline slides in |
| Cards | Hover | `translateY(-4px)` + glow shadow |
| Timeline body | Click | `max-height` transition |

---

## File Size Reference

`index.html` ≈ 1,270 lines, ~85 KB uncompressed. Everything inline — no external requests.
