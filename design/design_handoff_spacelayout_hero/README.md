# Handoff: SpaceLayout Hero — Scroll-Driven Landing Page

## Overview
A cinematic, scroll-controlled hero for **SpaceLayout** (architecture / design / interiors / construction). On load, a luxury residence rises onto a white sky with a looping single-testimonial ticker above its roofline. As the user scrolls (pinned sequence), the building enlarges and blurs away, the "SpaceLayout" wordmark fades in front of it, and finally the type zooms until the screen resolves to full black — flowing into a dark stats/intro section.

## About the Design Files
The file in this bundle (`SpaceLayout Hero.dc.html`) is a **design reference created in HTML** — a prototype showing intended look and behavior, not production code to copy directly. Recreate this design in your target codebase's environment (Next.js/React, Vue, etc.) using its established patterns. If no environment exists yet, a React + a scroll library (native `scroll` listener or GSAP ScrollTrigger / Framer Motion `useScroll`) is the natural fit.

## Fidelity
**High-fidelity.** Colors, type, spacing, copy, and animation timings are final intent. Recreate pixel-perfectly.

## Screens / Views

### 1. Hero (pinned scroll sequence)
- **Purpose**: Brand reveal; the only scroll interaction on the page.
- **Structure**: Outer wrapper `height: 480vh`; inner stage `position: sticky; top: 0; height: 100vh; overflow: hidden`.
- **Stage background**: `linear-gradient(180deg, #ffffff 0%, #f4f3f0 55%, #e9e7e2 100%)`.
- **Header** (absolute top, z-6, padding `clamp(16px,2.6vw,28px) clamp(20px,4.5vw,56px)`):
  - Logo text "SpaceLayout" — Playfair Display 500, `clamp(22px,2.4vw,30px)`, `#0a0a0a`, letter-spacing -0.01em.
  - Nav links (Residences, Portfolio, About) — Manrope 600, 13.5px, `#0a0a0a`, hover `#666`. Portfolio/About hidden < 640px.
  - "Enquire" pill — white text on `#0a0a0a`, padding 12px 26px, radius 999px, hover `#333`.
- **House image**: `uploads/ChatGPT Image Jul 17, 2026, 11_16_59 AM.png` (transparent PNG), bottom-anchored, centered; `max-width: min(94vw, 1060px); max-height: 76vh`; `filter: drop-shadow(0 26px 50px rgba(10,10,10,0.22))`; container `transform-origin: 50% 82%`, z-2.
- **Testimonial ticker** (z-1 — BELOW the house layer so exits pass behind the facade):
  - Container: horizontally centered, `bottom: calc(min(76vh, min(94vw,1060px)/1.5) + 16px)` (i.e. just above the roofline), width `min(86vw, 660px)`, inner relative box height 128px, cards bottom-anchored. Hidden < 920px viewport width.
  - Quote: Cormorant Garamond italic, `clamp(18px,2vw,23px)`, line-height 1.4, `#0a0a0a`, centered, `text-wrap: balance`, wrapped in curly quotes.
  - Author: Manrope 700, 11px, uppercase, letter-spacing 0.2em, `rgba(10,10,10,0.45)`, "— Name", margin-top 7px.
- **Giant reveal wordmark** (z-3, centered at `left:50%; top:44%`):
  - "SpaceLayout" — Playfair Display 500, rendered at 3× (`clamp(156px,34.5vw,504px)`) and displayed at base scale ≈ 0.3 for crispness during deep zoom; color `#0a0a0a`; white-space nowrap.
  - "designed by STUDIO23" — Playfair italic 400, `clamp(33px,3.6vw,48px)` (at 3×), `rgba(10,10,10,0.7)`; **STUDIO23** in Playfair 600 upright at `1.4em` with `font-variant-numeric: lining-nums; font-feature-settings: 'lnum' 1` (Playfair defaults to old-style figures — lining figures are required for baseline alignment).
  - Tagline "ARCHITECTURE · DESIGN · INTERIORS · CONSTRUCTION" — Manrope 600, `clamp(30px,3.3vw,42px)` (at 3×), letter-spacing 0.42em, `rgba(10,10,10,0.55)`.
- **Scroll hint**: bottom-left corner (`left: clamp(20px,4.5vw,56px); bottom: 30px`) — "SCROLL" in Manrope 700 11px, letter-spacing 0.26em, `rgba(10,10,10,0.55)` + 14×20 arrow SVG bobbing (2s ease-in-out loop). Must NOT overlap the building.
- **Black overlay**: full-stage `#0a0a0a` div, z-7, opacity 0 → 1 at sequence end.

### 2. "The Practice" section (below the sequence)
- Background `#0a0a0a`, text `#fafaf8`; flex-wrap row, gap `clamp(28px,5vw,80px)`, padding `clamp(72px,11vw,140px) clamp(24px,6vw,72px)`.
- Left column (max 520px): eyebrow "THE PRACTICE" (Manrope 700 12px, ls 0.24em, `rgba(250,250,248,0.55)`), H2 "Every home begins with how a space should feel" (Cormorant Garamond 500, `clamp(28px,3.2vw,44px)`, lh 1.12), body paragraph (Manrope 15px, lh 1.75, `rgba(250,250,248,0.65)`).
- Right: 3-col stat grid with 1px `rgba(250,250,248,0.16)` gaps/border. Stats: 120+ Residences placed · 14 Years of practice · €2.1B Portfolio value. Value in Cormorant `clamp(26px,3vw,38px)` 600; label Manrope 600 11px uppercase ls 0.16em `rgba(250,250,248,0.45)`.

## Interactions & Behavior

### Load sequence (CSS keyframes, run once)
1. House: rise from `translateY(7%)` + fade, 1.6s `cubic-bezier(0.22,1,0.36,1)`, delay 0.3s.
2. Header: fade down from -14px, 1s ease, delay 0.9s.
3. Scroll hint: same fade, delay 1.6s.

### Testimonial loop (timer, 5.6s interval)
- Exactly ONE quote visible, sitting just above the roofline.
- New quote drops in from -34px / scale 1.03 (`1.5s cubic-bezier(0.22,1,0.36,1)`).
- Outgoing quote slides down 300px (1.8s, behind the facade — lower z-index than house) while fading to 0 in 0.7s ease-out so it never ghosts through the building's semi-transparent pixels.
- Loop of 10 quotes (see Content).

### Scroll sequence (progress t = scrolled distance / (480vh − 100vh), clamped 0–1)
All updates via rAF-throttled scroll listener writing transforms directly (no re-render). Split into phases:
- `p = clamp(t / 0.55, 0, 1)` — reveal phase; `z = clamp((t − 0.58) / 0.42, 0, 1)` — zoom phase.
- **House**: scale `1 + p·1.9` (origin 50% 82%); opacity 1 until p=0.5 then → 0 by p=0.8; blur `(p−0.35)·10px` after p=0.35.
- **Testimonials**: opacity follows the same fade curve as the house (gone when facade is gone).
- **Wordmark**: opacity `(p−0.28)/0.34`; scale = `base · zoom` where base = `(0.9 + 0.1·min(p/0.62,1)) / 3` (÷3 because type is authored at 3×) and zoom = `1 + z^2.4 · 110` — accelerating typographic zoom.
- **Header**: opacity `1 − z·2.2` (pointer-events disabled when < 0.1).
- **Black overlay**: opacity `(z − 0.68) / 0.24` → screen fully black at end, matching the next section's `#0a0a0a`.
- **Scroll hint**: opacity `1 − p·4`, bob animation stopped once scrolling starts.

### Anti-blur note
Do NOT put `will-change: transform` on the zooming type — it freezes the rasterization at small scale. Author the type at 3× and display at ⅓ scale; let the browser re-rasterize during the zoom.

## State Management
- `ti` (int): current testimonial index; timer advances `(ti+1) % 10` every 5600ms.
- Scroll progress is derived, not state — computed per frame from `scrollY`.
- Resize listener re-evaluates responsive visibility (nav links < 640px, ticker < 920px).

## Design Tokens
- **Colors**: ink `#0a0a0a`; paper `#fafaf8`; sky gradient `#ffffff → #f4f3f0 → #e9e7e2`; ink-55 `rgba(10,10,10,0.55)`; ink-45 `rgba(10,10,10,0.45)`; paper-65 `rgba(250,250,248,0.65)`; paper-55/45/16 variants; hover gray `#666` / `#333`.
- **Type**: Playfair Display (brand/wordmark, 400–600), Cormorant Garamond (quotes & display serif, 400–600 + italic), Manrope (UI, 400–800). Google Fonts.
- **Easing**: `cubic-bezier(0.22, 1, 0.36, 1)` everywhere (smooth out-expo feel); zoom curve `z^2.4`.
- **Radii**: pill 999px only. No card radii (testimonials are bare text).

## Content
Testimonials (loop order): Rahul Mehta, Priya Nair, Arjun Reddy, Sneha Kapoor, Karthik Srinivasan, Ananya Sharma, Vikram Joshi, Meera Desai, Rohan Malhotra, Neha Verma — exact quote text is in the HTML file's `TESTIMONIALS` array.

## Assets
- `uploads/ChatGPT Image Jul 17, 2026, 11_16_59 AM.png` — transparent-background residence render (1536×1024). Included in this bundle.
- `uploads/spacelayout-logo-white.svg` — original client wordmark SVG (superseded by live Playfair type, included for reference).

## Files
- `SpaceLayout Hero.dc.html` — full prototype (template markup + `Component` logic class with the scroll handler and testimonial loop).
- `uploads/` — image assets above.
