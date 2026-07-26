# Handoff: Brittany Nails Salon Website ("Sweet by Day, Bold by Night")

## Overview
Marketing website for Brittany Nails Salon (Carmichael, CA) built around a "good girl baddie" concept: soft/everyday nail looks vs. bold/glam nail looks, presented as a day-vs-night duality throughout. Includes hero, mood-based service menu, filterable photo gallery, before/after comparison slider, about/team, reviews, first-time client info, salon standards, appointment request form, location/contact, and FAQ.

## About the Design Files
The files in this bundle are **design references built in HTML** — a working prototype demonstrating layout, content, styling, and interaction, not production code to lift as-is. The task is to **recreate this design in the target codebase's environment** (React, Vue, a CMS theme, static site generator, etc.), using that project's existing component patterns, build tooling, and conventions. If no environment/framework exists yet, choose the one best suited to a small marketing site with a contact form (e.g. Next.js, Astro, or plain static + serverless form endpoint).

## Fidelity
**High-fidelity.** Colors, typography, spacing, copy, and interaction behavior are final/near-final. Recreate pixel-close using the values below; the HTML file is the source of truth for anything not captured here.

## Screens / Sections
All sections live in one continuously-scrolling page, navigated via an anchor-link header.

1. **Header** — sticky, transparent at top, transitions to a solid cream/burgundy background on scroll (JS-driven scroll listener). Logo left, anchor nav links center/right, "Book Now" CTA button, hamburger menu below 960px.
2. **Hero ("Home")** — split day/night layout: left card labeled "Everyday" showing a soft manicure photo, right card labeled "After Dark" showing a glam manicure photo, with the H1 "Polished by Day. Unforgettable by Night." (second line rendered as a gradient burgundy text-clip). CTA buttons under the heading.
3. **Welcome** — centered single-column intro copy, max-width 820px.
4. **Services ("Choose Your Mood")** — service cards grouped by "mood" categories (not by service type) — e.g. Soft & Sweet, Everyday Polished, Midnight Glam, Pretty but Dangerous, French with a Twist, Bring the Sparkle, After Dark, Floral Mood. Each mood group has a name, description, and price-listed services.
5. **Gallery ("Which Side Are You Today?")** — 16-photo filterable grid, filter chips at top (by mood category), clicking a photo opens a lightbox (focus-trapped, Escape to close, arrow-key navigation, body scroll-locked while open).
6. **Transformation slider ("From Everyday Pretty to After-Dark Glam")** — custom before/after comparison slider, drag or arrow keys to reveal more of either photo, `role="slider"` with live aria-valuetext.
7. **About** — salon description, centered, max-width 820px.
8. **Why Clients Come Back** — 3–4 column responsive grid of value-prop cards (auto-fit minmax(220px,1fr)).
9. **Team ("Meet Your Nail Artists")** — grid of artist profile cards; photos are **placeholders** (owner has not supplied staff photos yet — do not treat placeholder silhouettes as final).
10. **Reviews** — pull-quote style testimonial cards + an aggregate rating chip.
11. **First-Time Visit tips** — grid of short tip cards (what to expect, arrival time, payment, etc.).
12. **Salon Standards / Cleanliness** — placeholder copy block, explicitly flagged in the source as pending owner confirmation.
13. **Appointment Request (Contact form)** — dark "night sky" gradient section (`linear-gradient(160deg,#1C1216,#3A1730 60%,#4A0E22)`) with twinkling dot accents (CSS keyframe `bn-twinkle`), containing a full appointment request form: name, phone, service, preferred date (min-date enforced), notes, and an optional inspiration-photo upload with client-side validation and a remove control.
14. **Location & Contact** — two-column layout (map/placeholder + address block), responsive down to one column.
15. **FAQ** — accordion list, driven by a `faqs` data array in the logic class.
16. **Footer** — salon name, hours, social link, phone, address, copyright.

## Interactions & Behavior
- **Sticky header color shift**: background/text color interpolates on scroll.
- **Mobile nav**: hamburger below 960px breakpoint; opens a full-screen/panel menu; body scroll is locked while open; focus returns to the trigger button on close; Escape closes it.
- **Gallery filter**: clicking a mood chip filters the 16-item grid by category (client-side state, no reload).
- **Lightbox**: click a gallery photo to open an enlarged view with caption; Escape closes; focus is trapped and restored to the triggering thumbnail on close.
- **Before/after slider**: pointer-driven (`pointerdown`/`pointermove`/`pointerup`/`pointercancel`) drag of a circular handle; click-to-jump on the track; keyboard support on the handle (`role="slider"`, tabIndex 0): ArrowLeft/ArrowRight step 5%, Shift+Arrow step 10%, Home/End jump to 0/100. `aria-valuenow`/`aria-valuetext` update live. Implemented via `clip-path: inset()` on the top image layer — no external libraries.
- **Appointment form validation**: required-field checks, min-date on the date input (cannot pick a past date), photo upload accepts a single image with basic type/size validation, inline error messages appear under each invalid field, and focus jumps to the first invalid field on submit.
- **Reduced motion**: `@media (prefers-reduced-motion: reduce)` disables smooth scroll and shortens/removes animation durations site-wide.
- **Focus visibility**: custom `:focus-visible` outline styling on interactive elements (links, buttons, form fields, slider handle) — do not lose this in the rebuild; it's the primary keyboard-navigation affordance.
- **Anchor nav**: sections have `scroll-margin-top` so the sticky header doesn't cover the target section heading when jumping via anchor links.

## State Management
The current build (a single component with local state) tracks:
- `galleryFilter` — active mood filter for the gallery grid.
- `sliderPos` (0–100) — before/after slider position; derived `sliderClip = 100 - sliderPos` drives the clip-path.
- `lightboxIndex` / open state — which gallery photo is enlarged, if any.
- Mobile menu open/closed state.
- Appointment form field values + per-field validation error state + a submitted/success state.
- Scroll position → header "scrolled" boolean (for the color-shift header).

A production rebuild should keep the same state shape; form submission currently has no backend — wire it to whatever booking/email endpoint the target stack uses (see `galleryData()`/`faqs` in the logic class of the .dc.html for exact copy and data shape to port over).

## Design Tokens
**Colors**
- Deep burgundy (primary/brand): `#8A1B3E`, darker burgundy accents `#6E1330`, `#4A0E22`
- Near-black plum (dark text / dark bg): `#2A1015`, `#1C1216`, `#3A1730`
- Body copy (muted mauve-brown): `#5B3B44`, lighter caption gray-pink: `#9C7480`
- Cream/background neutrals: `#F3E3D4` (night-section text), `#FBF6F0`, `#FBEAEE` (soft pink section bg), `#F0DCE0` (borders)
- Blush/pink accents: `#F6D9DF`, `#D9A5B2`
- Gold accent (slider handle track, night-section accents): `#C9A27A`

**Typography**
- Display/headings: `'Playfair Display', serif`, weights 600–900 (headings use 700–800)
- Body/UI: `'Poppins', sans-serif`, weights 400–800
- Both loaded via Google Fonts (`family=Playfair+Display:wght@600;700;800;900&family=Poppins:wght@400;500;600;700;800`)
- H1: `clamp(34px, 6vw, 58px)`, line-height 1.06
- H2 (section titles): `clamp(26px–28px, ~3.6–4vw, 36–42px)`, weight 800
- Eyebrow/kicker labels: 11.5–13px, weight 700, letter-spacing 1.5–2px, uppercase
- Body copy: 14.5–15.5px, line-height 1.6–1.7

**Spacing / radius**
- Section vertical padding: 64–80px (desktop), 20px horizontal gutter
- Card/image corner radius: 26–28px typical; pill/chip radius: 12–24px
- Grid gaps: 14–34px depending on context; cards use `repeat(auto-fit, minmax(220px,1fr))`

**Other**
- Slider handle: 44×44px circle, white bg, `0 4px 10px rgba(0,0,0,0.25)` shadow
- Twinkle keyframe animation on small dot accents in the dark contact section

## Assets
16 real salon photos across 8 mood categories, plus 4 hero/slider photos, all in `assets/gallery/` (17 files total — `after-dark-glam.jpg` is used only in the before/after slider's "after" side, `classic-1.jpg` doubles as the slider's "before" side and a gallery item). File → content mapping is in `galleryData()` inside the .dc.html (each entry has `src`, `label`, `cat`, `mood`, `ratio`). Team/staff member photos are **not** included — those sections currently render placeholder avatars pending real photos from the owner.

## Files
- `Brittany Nails Salon.dc.html` — full page markup + inline logic class (all interaction/state code); open directly in a browser to view/interact with the live prototype.
- `image-slot.js` — custom `<image-slot>` element used for placeholder/drop-in images (only relevant to this prototyping tool; not needed in the rebuild — replace with normal `<img>`/framework image components).
- `support.js` — prototyping-runtime support script (not needed in the rebuild).
- `assets/gallery/` — the 17 real photo assets.
