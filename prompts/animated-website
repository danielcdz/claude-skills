**Build a single-page dark portfolio landing page using React + Vite + Tailwind CSS + TypeScript + GSAP + Framer Motion + hls.js.**

---

## Global Design System

### Fonts
Google Fonts import: `Inter` (300–700) and `Instrument Serif` (italic, 400).
- `--font-body: 'Inter', sans-serif` → Tailwind `font-body`
- `--font-display: 'Instrument Serif', serif` → Tailwind `font-display`

### CSS Custom Properties (HSL, no `hsl()` wrapper — Tailwind adds it)
```css
--bg: 0 0% 4%;          /* #0a0a0a */
--surface: 0 0% 8%;     /* #141414 */
--text: 0 0% 96%;       /* #f5f5f5 */
--muted: 0 0% 53%;      /* #888888 */
--stroke: 0 0% 12%;     /* #1f1f1f */
--accent: 0 0% 96%;
```

### Tailwind Custom Colors
```ts
bg: "hsl(var(--bg))",
surface: "hsl(var(--surface))",
"text-primary": "hsl(var(--text))",
muted: "hsl(var(--muted))",
stroke: "hsl(var(--stroke))",
```

### Accent Gradient
`linear-gradient(90deg, #89AACC 0%, #4E85BF 100%)` — used on logo ring, hover borders, progress bars. CSS utility class `.accent-gradient`.

### Custom Animations (in index.css)
- `
@keyframes
 scroll-down` — translateY(-100%) → translateY(200%), 1.5s ease-in-out infinite
- `
@keyframes
 role-fade-in` — opacity 0 + translateY(8px) → opacity 1 + translateY(0), 0.4s ease-out
- `
@keyframes
 gradient-shift` — background-position 0% 50% → 100% 50% → 0% 50%, 6s ease infinite (for animated gradient borders)

### Forced dark theme — no light mode toggle. `body` gets `bg-bg text-text-primary`.

---

## Page Structure (Index.tsx)

```

  {isLoading &&  setIsLoading(false)} />}

```

---

## Section 1: Loading Screen

Full-screen overlay (`fixed inset-0 z-[9999] bg-bg`). Uses `requestAnimationFrame` counter from 000→100 over 2700ms.

- **Top-left**: "Portfolio" label — `text-xs text-muted uppercase tracking-[0.3em]`. Animates y:-20→0, opacity 0→1.
- **Center**: Rotating words ["Design", "Create", "Inspire"] cycling every 900ms. `AnimatePresence mode="wait"` with y:20→0→-20 transitions. `text-4xl md:text-6xl lg:text-7xl font-display italic text-text-primary/80`.
- **Bottom-right**: Counter display — `text-6xl md:text-8xl lg:text-9xl font-display text-text-primary tabular-nums`. Shows `String(count).padStart(3, "0")`.
- **Bottom progress bar**: `h-[3px] bg-stroke/50`, inner div with `.accent-gradient`, `scaleX(count/100)` transform, `box-shadow: 0 0 8px rgba(137, 170, 204, 0.35)`.
- On complete (count reaches 100): 400ms delay then calls `onComplete`.

---

## Section 2: Hero

Full-viewport section with background HLS video and centered content.

### Background Video
- HLS source: `https://stream.mux.com/Aa02T7oM1wH5Mk5EEVDYhbZ1ChcdhRsS2m1NYyx4Ua1g.m3u8`
- Uses `hls.js` — if `Hls.isSupported()`, create HLS instance; else if native HLS support, set `video.src` directly.
- Video: `autoPlay muted loop playsInline`, absolutely positioned and centered with `min-w-full min-h-full object-cover -translate-x-1/2 -translate-y-1/2`.
- Dark overlay: `bg-black/20`
- Bottom fade: `h-48 bg-gradient-to-t from-bg to-transparent`

### Navbar (fixed, floats at top center)
`fixed top-0 left-0 right-0 z-50 flex justify-center pt-4 md:pt-6 px-4`.

Inner pill: `inline-flex items-center rounded-full backdrop-blur-md border border-white/10 bg-surface px-2 py-2`. Gets `shadow-md shadow-black/10` when `scrollY > 100`.

Contents (left to right):
1. **Logo**: 9×9 circle with accent gradient border (reverses direction on hover). Inner `bg-bg` circle with "JA" in `font-display italic text-[13px]`. Scales 110% on hover.
2. **Divider**: `w-px h-5 bg-stroke mx-1` (hidden on mobile)
3. **Nav links**: ["Home", "Work", "Resume"] — `text-xs sm:text-sm rounded-full px-3 sm:px-4 py-1.5 sm:py-2`. Active: `text-text-primary bg-stroke/50`. Inactive: `text-muted hover:text-text-primary hover:bg-stroke/50`.
4. **Divider**
5. **"Say hi" button**: Same size as nav links. On hover, shows accent gradient border behind (using absolute span with `inset: -2px`). Inner content wrapped in `bg-surface rounded-full backdrop-blur-md`. Includes "↗" arrow.

### Hero Content (centered, z-10)
- **Eyebrow**: `text-xs text-muted uppercase tracking-[0.3em] mb-8` — "COLLECTION '26". Class `blur-in`.
- **Name**: `text-6xl md:text-8xl lg:text-9xl font-display italic leading-[0.9] tracking-tight text-text-primary mb-6` — "Michael Smith". Class `name-reveal`.
- **Role line**: "A {role} lives in Chicago." — roles cycle every 2s through ["Creative", "Fullstack", "Founder", "Scholar"]. Role word uses `font-display italic text-text-primary animate-role-fade-in inline-block` with `key={roleIndex}` for re-triggering animation.
- **Description**: `text-sm md:text-base text-muted max-w-md mb-12` — "Designing seamless digital interactions by focusing on the unique nuances which bring systems to life."
- **CTA Buttons** (inline-flex gap-4):
  - "See Works": Solid button. Default: `bg-text-primary text-bg`. Hover: `bg-bg text-text-primary` with accent gradient border ring.
  - "Reach out...": Outlined button. Default: `border-2 border-stroke bg-bg text-text-primary`. Hover: `border-transparent` with accent gradient border ring.
  - Both: `rounded-full text-sm px-7 py-3.5 hover:scale-105`.

### GSAP Entrance
Timeline with `ease: "power3.out"`:
- `.name-reveal`: opacity 0→1, y 50→0, duration 1.2s, delay 0.1s
- `.blur-in`: opacity 0→1, filter blur(10px)→blur(0px), y 20→0, duration 1s, stagger 0.1, delay 0.3s

### Scroll Indicator
Bottom-center, `text-xs text-muted uppercase tracking-[0.2em]` "SCROLL" label above a `w-px h-10 bg-stroke` line with animated highlight using `.animate-scroll-down`.

---

## Section 3: Selected Works

`bg-bg py-12 md:py-16`. Inner: `max-w-[1200px] mx-auto px-6 md:px-10 lg:px-16`.

### Header
Framer Motion `whileInView` — opacity 0→1, y 30→0, duration 1s, ease [0.25,0.1,0.25,1], viewport once margin "-100px".
- Eyebrow: `w-8 h-px bg-stroke` + "Selected Work" `text-xs text-muted uppercase tracking-[0.3em]`
- Heading: "Featured *projects*" — italic word in `font-display italic`
- Subtext: "A selection of projects I've worked on, from concept to launch."
- "View all work" button (desktop only, hidden md:inline-flex) — rounded-full with gradient hover border ring + right arrow

### Bento Grid
`grid grid-cols-1 md:grid-cols-12 gap-5 md:gap-6`. Column spans alternate: 7/5/5/7.

4 project cards:
```ts
[
  { slug: "automotive-motion", title: "Automotive Motion", image: "/projects/wireframe.png", gradient: "from-violet-500 via-fuchsia-400/60 via-indigo-500/60 to-transparent" },
  { slug: "urban-architecture", title: "Urban Architecture", image: "/projects/building.png", gradient: "from-sky-500 via-blue-400/60 to-transparent" },
  { slug: "human-perspective", title: "Human Perspective", image: "/projects/person.png", gradient: "from-emerald-500 via-emerald-300/60 via-teal-500/60 to-transparent" },
  { slug: "brand-identity", title: "Brand Identity", image: "/projects/branding.png", gradient: "from-amber-500 via-amber-300/60 via-orange-500/60 to-transparent" },
]
```

Each card: `bg-surface border border-stroke rounded-3xl` with aspect ratios `[4/3, 4/3 md:h-full, 4/3 md:h-full, 4/3]`. Contains:
- Background image with `object-cover group-hover:scale-105`
- Halftone overlay: `radial-gradient(circle, #000 1px, transparent 1px)` at 4×4px, `opacity-20 mix-blend-multiply`
- Hover: `bg-bg/70 opacity-0→1` + `backdrop-blur-lg`
- Hover label: pill with animated gradient border, white bg, "View — *Title*" (title in `font-display italic`)

Framer Motion: each card staggers with `delay: i * 0.1`.

---

## Section 4: Journal

`bg-bg py-16 md:py-24`. Same header pattern (eyebrow + "Recent *thoughts*" + subtext + "View all" button).

4 journal entries displayed as horizontal pills (`rounded-[40px] sm:rounded-full`):
```ts
[
  { title: "The Future of Generative Art in 2026", image: "/explorations/planet.jpeg", readTime: "6 min read", date: "Feb 13, 2026" },
  { title: "Designing for the Next Billion Users", image: "/explorations/cubes.jpeg", readTime: "5 min read", date: "Feb 06, 2026" },
  { title: "The Psychology of Minimalist Motion", image: "/explorations/ascii.jpeg", readTime: "6 min read", date: "Feb 03, 2026" },
  { title: "The Importance of Mobile-First Design", image: "/explorations/smoke.jpeg", readTime: "5 min read", date: "Jan 31, 2026" },
]
```

Each entry: `flex items-center gap-6 p-4 bg-surface/30 hover:bg-surface border border-stroke`. Contains:
- Circular image (100×100px, `rounded-full overflow-hidden`, hover: `scale-110`, border transitions)
- Title: `text-lg md:text-2xl font-medium`, `group-hover:translate-x-1`
- Dotted line separator (desktop): `flex-grow h-px bg-stroke/30`
- Read time + dot + date
- Arrow circle: `w-10 h-10 rounded-full border border-stroke`, hover: fills `bg-text-primary text-bg`

Framer Motion: alternating x:-20/+20 entrance per entry.

---

## Section 5: Explorations (Parallax Gallery)

`min-h-[300vh]` section for scroll-driven parallax.

### Layer 1: Pinned Center (z-10)
`h-screen` div pinned with GSAP `ScrollTrigger.create({ pin: contentRef, pinSpacing: false })`.
- Eyebrow: "Explorations"
- Heading: "Visual *playground*"
- Subtext: "A space for creative experiments, motion studies, and visual explorations."
- Dribbble button: pink icon (#ea4c89) + "View on Dribbble" + NE arrow. Gradient hover border ring.

### Layer 2: Parallax Columns (z-20, absolute)
`grid grid-cols-2 gap-12 md:gap-40` inside `max-w-[1400px]`.

6 items split into 2 columns (even→left, odd→right):
```ts
[
  { id: 1, title: "Celestial Planets", category: "3D Visualization", image: "/explorations/planet.jpeg" },
  { id: 2, title: "ASCII Art Study", category: "Generative Art", image: "/explorations/ascii.jpeg" },
  { id: 3, title: "Atmospheric Smoke", category: "Visual Effects", image: "/explorations/smoke.jpeg" },
  { id: 4, title: "Abstract Cylinder", category: "3D Rendering", image: "/explorations/cylinder.jpeg" },
  { id: 5, title: "Organic Waves", category: "Motion Design", image: "/explorations/wave.jpeg" },
  { id: 6, title: "Geometric Cubes", category: "3D Composition", image: "/explorations/cubes.jpeg" },
]
```

- Left column: `y: "10vh" → "-120vh"`, scrub 1, spacer `h-[20vh]`
- Right column: `y: "40vh" → "-100vh"`, scrub 1.5, spacer `h-[40vh]`
- Cards: `aspect-square max-w-[320px]`, rotation `(id%2===0 ? 1 : -1) * (1.5 + id%3)` degrees
- Each card: outer border frame at `-inset-4 rounded-[40px]`, blue tint `bg-blue-500/10 mix-blend-overlay`, halftone texture, hover gradient + content reveal
- Clicking opens lightbox: `fixed z-[100] bg-black/95`, image at `aspect-[16/10]`, Framer Motion scale 0.9→1, close on backdrop click or Escape key

---

## Section 6: Stats

`bg-bg py-16 md:py-24`. Same header pattern (eyebrow "Stats & Facts" + "Making an *impact*" + long subtext).

3-column grid (`sm:grid-cols-2 lg:grid-cols-3`, 3rd card gets `sm:col-span-2 lg:col-span-1`):
```ts
[
  { number: "20+", label: "Years Experience", sublabel: "In the web design industry field." },
  { number: "95+", label: "Projects Done", sublabel: "Around worldwide in last five years." },
  { number: "200%", label: "Satisfied Clients", sublabel: "With a great experience and results." },
]
```

Each: large number `text-6xl sm:text-7xl md:text-8xl lg:text-9xl font-medium tracking-tighter` → `h-px bg-stroke` divider → label (`text-xl md:text-2xl font-bold`) + sublabel (`text-sm text-muted`). Staggered Framer Motion entrance.

---

## Section 7: Contact / Footer

`bg-bg pt-16 md:pt-20 pb-8 md:pb-12 overflow-hidden`.

### Background Video
Same HLS source as hero, but **flipped vertically** (`scale-y-[-1]`). Heavier overlay: `bg-black/60`. Top fade `h-32` + bottom fade `h-24`.

### GSAP Marquee
"BUILDING THE FUTURE • " repeated 10×. `text-5xl md:text-7xl lg:text-8xl font-display italic text-text-primary/10`. GSAP `xPercent: -50`, duration 40, ease "none", repeat -1.

### CTA
- Subtext: "Have a project in mind? I'm always open to new ideas and collaborations."
- Email button: `mailto:hello@michaelsmith.com` — `px-8 py-4 bg-bg border-2 border-stroke rounded-full` with gradient hover border ring. `whileTap={{ scale: 0.97 }}`. NE arrow icon.

### Footer Bar
`border-t border-stroke pt-8 flex flex-col md:flex-row items-center justify-between`.
- Left: Social links [Twitter, LinkedIn, Dribbble, GitHub] — `text-sm text-muted hover:text-text-primary hover:-translate-y-0.5`
- Right: Green pulsing dot (`animate-ping bg-green-400` + solid `bg-green-500`) + "Available for projects"

---

## Required Assets (in `/public/`)
- `/projects/wireframe.png`, `/projects/building.png`, `/projects/person.png`, `/projects/branding.png`
- `/explorations/planet.jpeg`, `/explorations/ascii.jpeg`, `/explorations/smoke.jpeg`, `/explorations/cylinder.jpeg`, `/explorations/wave.jpeg`, `/explorations/cubes.jpeg`

## Dependencies
`gsap`, `framer-motion`, `hls.js`, `react-router-dom`, `tailwindcss-animate`

Add smooth scroll nav
Add page transitions
