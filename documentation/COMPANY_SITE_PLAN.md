# AGN Technology - Company Website Plan

## Context

AGN Technology needs a single-page company website deployed via GitHub Pages. The existing repo has a minimal Jekyll skeleton with the Cayman theme and placeholder content. This plan replaces it with a fully custom, modern site that showcases projects as the primary focus, company stats, founder profiles (secondary), and contact details.

**Design direction**: Light theme, teal/cyan accent, modern & inviting startup feel (not overly corporate). Moderate motion (scroll animations, counter animations, hover effects). Typography: Space Grotesk (headings) / DM Sans (body).

---

## File Structure

```
D:\Development\PersonalWebsite\
  _config.yml                         [MODIFY] Update title, remove Cayman theme, add company vars
  index.md                            [DELETE]  Replaced by index.html
  index.html                          [CREATE] Single-page entry with include directives
  _layouts/
    default.html                      [CREATE] Full HTML shell, Tailwind v4 CDN, Google Fonts, theme
  _includes/
    nav.html                          [CREATE] Sticky nav with mobile hamburger menu
    hero.html                         [CREATE] Full-height hero with tagline + CTA
    projects.html                     [CREATE] 7-card project grid (MAIN FOCUS)
    stats.html                        [CREATE] Animated stat counters on teal background
    founders.html                     [CREATE] 3 founder cards (secondary)
    contact.html                      [CREATE] Email CTA + GitHub/LinkedIn links
    footer.html                       [CREATE] Dark footer with copyright
    icons/                            [CREATE] 7-8 inline SVG icon partials (Lucide)
  _data/
    navigation.yml                    [CREATE] Nav links
    projects.yml                      [CREATE] 7 placeholder projects
    founders.yml                      [CREATE] 3 founders
    stats.yml                         [CREATE] 4 stat counters
  assets/
    js/main.js                        [CREATE] Scroll animations, counters, mobile menu, smooth scroll
    images/founders/placeholder.svg   [CREATE] Generic avatar SVG
```

---

## Design System

| Token | Value | Usage |
|-------|-------|-------|
| Primary | `#0F766E` | Teal accent, stat section bg, hover states |
| Primary Light | `#14B8A6` | Hover borders, lighter accents |
| CTA | `#0369A1` | Buttons (blue stands out from teal) |
| Background | `#FFFFFF` | Main bg |
| Background Alt | `#F0FDFA` | Founders section, subtle tint |
| Text | `#134E4A` | Body text (9.4:1 contrast on white) |
| Text Light | `#5F7A77` | Secondary text (4.6:1 contrast) |
| Heading | `#042F2E` | Headings |
| Font Heading | Space Grotesk | h1-h3, nav logo, stat numbers |
| Font Body | DM Sans | Body text, descriptions, labels |

---

## Section Order & Visual Weight

1. **Nav** - Fixed top, `z-50`, backdrop blur, shadow on scroll
2. **Hero** - `min-h-screen`, centered, large heading (4xl-7xl), CTA button
3. **Projects** (MAIN) - Largest padding (`py-20 md:py-28 lg:py-32`), largest headings (`text-5xl`), full `max-w-7xl` container, rich cards with hover effects + tech tags. Grid: 1col / 2col / 3col
4. **Stats** - Teal background band, animated counters, 2col / 4col grid
5. **Founders** (SECONDARY) - Smaller padding (`py-16 md:py-20`), smaller headings (`text-3xl`), narrower `max-w-5xl`, simple avatar + name + role, no hover effects
6. **Contact** - Email button + social icon links (44x44px touch targets)
7. **Footer** - Dark (`#042F2E`), dynamic year copyright

---

## Technical Approach

- **Tailwind CSS v4** via Play CDN (`@tailwindcss/browser@4`) with `@theme` directive for custom tokens
- **Google Fonts** via `<link>` with `preconnect` hints
- **Jekyll data files** (`_data/*.yml`) for all content - keeps templates clean, easy to update
- **Liquid includes** for each section - modular, maintainable
- **Vanilla JS** (no frameworks): Intersection Observer for scroll animations, `requestAnimationFrame` for counter animation, classList toggles for mobile menu
- **`prefers-reduced-motion`** respected in both CSS and JS

---

## Implementation Order

1. **Config & Data**: `_config.yml`, all `_data/*.yml` files
2. **Assets**: `placeholder.svg`, all `_includes/icons/*.html`, `assets/js/main.js`
3. **Layout & Includes**: `_layouts/default.html`, then all `_includes/*.html` sections
4. **Index**: Delete `index.md`, create `index.html`

---

## Verification

1. Run `bundle exec jekyll serve --livereload` locally
2. Test all 4 breakpoints: 375px, 768px, 1024px, 1440px
3. Verify smooth scroll navigates to correct sections with nav offset
4. Verify counter animation triggers when stats section scrolls into view
5. Test mobile hamburger menu open/close and link navigation
6. Test keyboard navigation (Tab through all interactive elements)
7. Enable `prefers-reduced-motion: reduce` and verify no animations play
8. Verify all hover states have smooth transitions and `cursor-pointer`
