# Voice Suite — Design & UI/UX Documentation

**Projects:** `index.html` (Utter) · `vocra.html` (Vocra)
**Category:** Voice-first Web Applications
**Status:** Design-complete · Pre-launch

---

## Overview

This repository contains two companion voice applications sharing a unified design language: **Utter**, a personal voice-to-notes recorder, and **Vocra**, an AI voice cloning and speech synthesis tool. Together they form a cohesive voice product suite built around a stripped-back, editorial aesthetic.

---

## Project Summaries

### Utter (`index.html`)
> *"Speak. Capture. Remember."*

A privacy-first, offline-capable voice journaling and note-taking app. Users hit a mic button, speak freely, and Utter transcribes, categorizes, and structures their words into notes, to-dos, or journal entries — instantly. AI summaries are available on the Pro tier

**Core user flows:**
1. Tap mic → speak → get structured note
2. Switch mode (Note / To-Do / Journal) before or after recording
3. Review auto-extracted to-dos
4. Export or sync (Pro)

**Target users:** Knowledge workers, journalers, people who think better out loud

---

### Vocra (`vocra.html`)
> *"Clone Any Voice."*

A professional voice cloning and speech synthesis platform. Users upload a short reference clip to clone a voice, design voices from text description alone, or generate TTS using a curated voice library. Built for creators and developers who need high-fidelity, low-friction audio production.

**Core user flows:**
1. **Clone** — upload reference audio → name it → save to library
2. **Generate** — pick a voice → type text → export WAV/MP3
3. **Design** — describe a voice → preview candidates → save best

**Target users:** Podcasters, game/animation studios, localization teams, developers

---

## Shared Design Language

Both products share the same visual DNA — intentionally, to signal they belong to the same suite — while maintaining distinct personalities.

### Typography

| Role | Utter | Vocra |
|---|---|---|
| Display / Headings | Playfair Display (serif, italic) | Bebas Neue (condensed, all-caps) |
| UI / Labels / Nav | Syne (geometric sans) | Space Mono (monospace) |
| Body / Transcripts | DM Mono (monospace, light) | Space Mono (monospace) |

**Rationale:** Utter leans into a literary, notebook-like warmth using a serif headline to evoke the feel of handwriting. Vocra uses compressed display type and a strict mono stack to signal precision and technical power — like a professional audio console.

---

### Color System

Both projects use a CSS custom-property-based theming system with full dark mode support via `[data-theme="dark"]`.

```css
/* Utter — Warm Parchment */
--bg:    #f5f4f0   /* warm off-white */
--fg:    #0a0a0a   /* near-black */
--muted: #6b6b6b
--border:#d8d6d0   /* warm grey */
--card:  #ffffff
--tag-bg:#eceae3   /* parchment chip */

/* Vocra — Cool Stone */
--bg:    #f0eeea   /* cooler off-white */
--fg:    #0d0d0d
--muted: #888
--border:#d4d1cc
--card:  #fafaf8
--invert-bg: #0d0d0d  /* used for mock UI panels */
```

**Dark mode** inverts both palettes cleanly: Utter goes to near-black (`#0a0a0a`) with warm-toned foreground; Vocra mirrors this with a slightly cooler dark base. Both maintain full contrast ratios.

**No accent color is used** — this is a deliberate choice. Both products communicate premium quality through restraint rather than brand color. All interactive states use foreground-on-background inversion.

---

### Motion & Animation

All animations follow the same underlying principles across both products:

| Type | Approach |
|---|---|
| Page load | Staggered `fadeUp` (opacity + translateY), 0.1s delay increments |
| Scroll reveal | `IntersectionObserver` adds `.visible` / `.on` class to trigger CSS transitions |
| Hover states | `transform: translateY(-4px to -6px)` lift on cards; opacity dimming on buttons |
| Custom cursor | Dot + ring, ring lags with `lerp(0.1–0.12)` for organic feel; scales on hover |
| Hero widget | Infinite `floatWidget` keyframe (Utter) — gentle 12px vertical float |
| Mic recording | `pulse` / `recordPulse` radial box-shadow animation while active |
| Waveform | `scaleY` + opacity cycle on each bar with staggered `animation-delay` (Utter); random SVG rects drawn procedurally (Vocra) |

**Performance notes:** All animations are CSS-only or `requestAnimationFrame`-based. No GSAP or JS animation libraries required. The scroll reveal uses a single `IntersectionObserver` per element.

---

### Component Inventory

#### Shared Components

| Component | Description |
|---|---|
| **Custom Cursor** | `cursor` dot + `cursor-ring` circle, both `position:fixed`. Mouse lag on ring via RAF lerp. Hover state enlarges dot. |
| **Sticky Nav** | `backdrop-filter: blur` + `color-mix` semi-transparent background. Theme toggle + CTA right-aligned. |
| **Theme Toggle** | Pill-shaped toggle using `::after` pseudo-element sliding on `translateX`. No JS framework needed. |
| **Scroll Reveal** | `.reveal` / `.rev` class + `IntersectionObserver`. Stagger via `.reveal-delay-{n}` / `.rev-d{n}`. |
| **Ticker / Marquee** | Infinite scroll marquee using `translateX` animation. Pauses on hover. Duplicated content for seamless loop. |
| **Pricing Cards** | Grid of 2, featured card inverted (bg = fg). Clean feature list with `::before '—'` or `'□'` markers. |
| **Mobile Nav** | Hamburger → dropdown. Closes on link click, outside click, and scroll. |
| **Footer** | Logo + link row + copyright. Flexbox with `flex-wrap` for responsive. |

#### Utter-Specific Components

| Component | Description |
|---|---|
| **Hero Mic Widget** | Floating card with animated waveform bars, mic button, and live transcript preview. Simulates typing via `setInterval` character-by-character reveal. |
| **Live Demo Area** | Mode switcher (Note / To-Do / Journal) + big mic button + output area. Uses Web Speech API with fallback to simulated typing. Timer counts up during recording. |
| **Notes Masonry** | CSS `columns: 3` masonry layout. Cards use `break-inside: avoid`. Hover adds slight rotation. |
| **Feature Cards Grid** | 2-column grid with a full-width "large" card variant. Grain texture overlay via `::before`. |
| **Steps Grid** | 3-column bordered grid (2px gap, border-radius on container). `step-num` in large Playfair Display. |
| **Waitlist Form** | Email input + submit button. Inline validation, count increment, success message swap. |

#### Vocra-Specific Components

| Component | Description |
|---|---|
| **Voice Cards** | 2-column grid. Each card: name/language header, placeholder with Japanese/Chinese glyph + SVG noise overlay, Original/Clone toggle, Download button, simulated waveform player. |
| **SVG Waveforms** | Procedurally generated 60-bar `<rect>` waveforms drawn on mount with `Math.random()`. Progress overlay uses absolute-positioned `div`. |
| **Feature Blocks** | Alternating 2-column `feat-block` + `feat-block.reverse` (uses `direction:rtl` trick). Left: text + checklist. Right: inverted-background mock UI panel. |
| **Mock UI Panels** | Dark-background form panels (`--invert-bg`) with input fields, selects, textareas. Simulate Clone / Generate / Design UIs. |
| **Credit Rate Cards** | 3-column grid showing credit costs per operation type with Bebas Neue large numerals. |
| **FAQ Grid** | 3-column grid of Q&A cards. Compact; `Space Mono` label + answer pairs. |
| **Demo Box** | Voice selector + language + textarea → "Generate Speech" → simulated response with voice-specific output text. |
| **Scroll-spy Nav** | Active state on nav links driven by `scrollY + offset >= section.offsetTop`. Updates on scroll with `passive: true`. |

---

## Responsive Breakpoints

Both products use a single breakpoint at `768px`:

```css
@media (max-width: 768px) {
  /* Feature grids collapse to 1 column */
  /* Steps go vertical */
  /* Pricing stacks */
  /* Waitlist form stacks */
  /* Hamburger nav replaces link row */
  /* Padding reduces from 3rem → 1.5rem */
}
```

An additional `500px` breakpoint collapses the masonry to 1 column in Utter.

---

## Interaction Design Notes

### Utter — Mic Flow
The Live Demo section supports real browser microphone access via the Web Speech API (`window.SpeechRecognition || window.webkitSpeechRecognition`). If unavailable (Firefox, some mobile), it falls back gracefully to a simulated character-by-character animation. No error states are shown — the fallback is invisible to the user.

Transcript output formatting changes based on the selected mode:
- **Note**: Raw transcript
- **To-Do**: Sentence-split, each prefixed with `☐`
- **Journal**: Raw transcript (same as note — formatting is semantic, not visual)

### Vocra — Player Simulation
Voice card players use a `setInterval`-based fake progress system. Only one voice plays at a time — switching to a new card stops the previous one and resets its progress bar and timestamp. The progress bar is an absolutely-positioned `div` growing from `width: 0%` to `100%`.

---

## Merge Rationale

These two files represent complementary sides of a voice product ecosystem:

| Dimension | Utter (`index.html`) | Vocra (`vocra.html`) |
|---|---|---|
| User relationship with voice | Input (speak to capture) | Output (generate from voice) |
| Privacy posture | Offline-first, no account needed | Account + credit model |
| Aesthetic register | Warm, literary, personal | Cool, technical, professional |
| Complexity | Consumer / prosumer | Creator / developer |
| Monetization | Freemium ($0 / $6/mo) | Credit-based ($10 / $25/mo) |

A merged product suite would unify authentication, voice library (Vocra voices available for Utter summaries), and shared billing — with Utter as the entry product and Vocra as the power-user upgrade path.

---

## File Reference

```
.
├── index.html     — Utter: voice notes, journal, to-do app (landing page)
├── vocra.html     — Vocra: voice cloning & TTS platform (landing page)
└── README.md      — This file
```

---

## Fonts Used (CDN)

```html
<!-- Utter -->
https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;0,900;1,400;1,700
  &family=DM+Mono:ital,wght@0,300;0,400;0,500;1,300
  &family=Syne:wght@400;600;700;800

<!-- Vocra -->
https://fonts.googleapis.com/css2?family=Space+Mono:ital,wght@0,400;0,700;1,400
  &family=Bebas+Neue
  &family=Manrope:wght@300;400;500;600;700;800
```

No other external dependencies. No JS frameworks. No build step required — both files are single-file HTML with all CSS and JS inline.

---

*Documentation generated for design handoff and UI/UX review.*
