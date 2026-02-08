# AGN Technology Website - Design Reasoning

## Overview

This document captures the design decisions and reasoning behind the AGN Technology company website, based on research using the UI/UX Pro Max design intelligence system and Context7 documentation.

---

## Design System Selection

### Style: Light Theme with Teal/Cyan Accent
**Why**: The founders wanted a modern, professional look that isn't overly corporate. A light theme was chosen because older clients are more familiar with it, making the site inviting and easy on the eyes. Teal/cyan was selected as the accent color to differentiate from the typical blue startup aesthetic while maintaining a tech-forward feel.

**UI/UX Pro Max recommendation**: The design system generator suggested a "Motion-Driven" style with "Portfolio Grid" pattern for tech company portfolios. We adapted this with a light background instead of the default dark suggestion.

### Typography: Space Grotesk / DM Sans
**Why**: Recommended by UI/UX Pro Max for tech companies and startups. Space Grotesk provides a bold, modern feel for headings with geometric character. DM Sans is clean and highly readable for body text. Both are available on Google Fonts.

**Mood keywords matched**: tech, startup, modern, innovative, bold, futuristic.

### Color Palette
| Token | Hex | Reasoning |
|-------|-----|-----------|
| Primary | `#0F766E` | Deep teal - professional without being cold. Used for accent elements and stats section background |
| Primary Light | `#14B8A6` | Lighter teal for hover borders and interactive feedback |
| CTA | `#0369A1` | Blue CTA buttons stand out from the teal palette, creating visual hierarchy. Deliberate contrast from primary |
| Background | `#FFFFFF` | Clean white - familiar, professional, easy on eyes |
| Background Alt | `#F0FDFA` | Very light teal tint for section differentiation without heavy borders |
| Text | `#134E4A` | Dark teal-gray, 9.4:1 contrast ratio on white (WCAG AAA) |
| Text Light | `#5F7A77` | Muted teal-gray for secondary text, 4.6:1 contrast (WCAG AA) |
| Heading | `#042F2E` | Near-black dark teal for maximum heading prominence |

**Contrast verification** (from UI/UX Pro Max accessibility guidelines):
- Body text `#134E4A` on `#FFFFFF`: 9.4:1 (AAA pass)
- Light text `#5F7A77` on `#FFFFFF`: 4.6:1 (AA pass)
- White text on `#0F766E` (stats section): 4.56:1 (AA pass)
- All exceed the required 4.5:1 minimum ratio

---

## Layout Decisions

### Single-Page Scrolling Architecture
**Why**: The user requested a single-page site. This works well for a startup company site because:
- Lower bounce rate than multi-page sites
- Tells a complete story in one scroll
- Works naturally on mobile
- Simpler to maintain

### Section Visual Weight Hierarchy
The user specified that projects should be the main focus and founders should be secondary. This was achieved through deliberate differences in visual treatment:

| Property | Projects (Primary) | Founders (Secondary) |
|----------|-------------------|---------------------|
| Vertical padding | `py-20 md:py-28 lg:py-32` | `py-16 md:py-20` |
| Heading size | Up to `text-5xl` | Up to `text-3xl` |
| Container width | `max-w-7xl` (full) | `max-w-5xl` (narrower) |
| Card complexity | Border, hover shadow, tech tags, CTA link | Simple avatar + name + role |
| Hover effects | Shadow elevation, border color change | None |
| Background | White (neutral) | Light teal tint (subtle) |

### Stats Section as Visual Break
**Why**: The teal-background stats band serves two purposes:
1. Creates visual variety in the scrolling experience
2. Separates the primary content (projects) from secondary content (founders)

The animated counters add engagement without being distracting.

### 7-Project Grid Layout
**Grid behavior across breakpoints**:
- Mobile (375px): 1 column - all cards stack
- Tablet (768px): 2 columns - 7th card spans full width (`md:col-span-2`)
- Desktop (1024px+): 3 columns - 7th card stays single width (`lg:col-span-1`)

The 7th card stretches on tablet to avoid looking orphaned in a 2-column layout.

---

## Technical Decisions

### Tailwind CSS v4 via Play CDN
**Why**: Simplest integration path for a Jekyll + GitHub Pages site. No build tooling required. The Play CDN processes Tailwind classes client-side.

**Trade-off**: Brief flash of unstyled content (FOUC) on first load. Acceptable for a company site at this scale. Can be migrated to a build step with Tailwind CLI later if needed.

### Jekyll Data Files (`_data/*.yml`)
**Why**: Separating content from templates follows Jekyll best practices. Makes it trivial to:
- Update project descriptions without touching HTML
- Add/remove projects or founders
- Modify stats numbers
- Keep templates clean and reusable

### Vanilla JavaScript (No Frameworks)
**Why**: The site only needs four JS behaviors:
1. Intersection Observer for scroll animations
2. `requestAnimationFrame` for counter animation
3. classList toggles for mobile menu
4. Smooth scroll with nav offset

No framework is needed for this. Vanilla JS keeps the bundle tiny and loads instantly.

### SVG Icon Partials (Lucide Icons)
**Why**: UI/UX Pro Max mandates "No emojis as icons - use SVG icons (Heroicons/Lucide)." Inline SVGs were chosen over icon fonts because:
- They inherit `currentColor` from parent elements
- No additional HTTP requests
- Accessible with `aria-hidden="true"`
- Consistent 24x24 viewBox from Lucide set

---

## Accessibility Compliance

Based on UI/UX Pro Max accessibility guidelines (Priority 1 - CRITICAL):

| Guideline | Implementation |
|-----------|---------------|
| Color contrast 4.5:1 minimum | All color pairs verified (see palette section) |
| Visible focus rings | `focus:outline-2 focus:outline-primary focus:outline-offset-2` on all interactive elements |
| Alt text for images | Founder images have `alt="{{ founder.name }}"` |
| ARIA labels | `aria-label` on icon-only buttons and social links |
| Keyboard navigation | Tab order matches visual order, no traps |
| Touch targets 44x44px | Social links and mobile menu use `w-11 h-11` |
| Skip-to-content link | Added to layout, visible only on keyboard focus |
| `prefers-reduced-motion` | CSS media query disables transitions; JS skips animations and shows final state |
| Semantic HTML | `<nav>`, `<main>`, `<section>`, `<footer>`, proper heading hierarchy |

---

## UI/UX Pro Max Pre-Delivery Checklist

- [x] No emojis used as icons (all SVGs from Lucide)
- [x] All icons from consistent icon set (Lucide, 24x24 viewBox)
- [x] Hover states don't cause layout shift (uses color/opacity/shadow only)
- [x] All clickable elements have `cursor-pointer`
- [x] Hover states provide clear visual feedback
- [x] Transitions are smooth (200-300ms)
- [x] Focus states visible for keyboard navigation
- [x] Light mode text has sufficient contrast (4.5:1 minimum)
- [x] Borders visible in light mode (`border-border` = `#CCFBF1`)
- [x] Responsive at 375px, 768px, 1024px, 1440px
- [x] No horizontal scroll on mobile
- [x] `prefers-reduced-motion` respected in CSS and JS
- [x] Floating elements (navbar) have proper z-index (`z-50`)
- [x] No content hidden behind fixed navbar (`pt-16` on hero)

---

## Anti-Patterns Avoided

From UI/UX Pro Max recommendations:
- **No corporate templates** - Custom design with startup personality
- **No generic layouts** - Deliberate visual hierarchy with projects as hero
- **No emoji icons** - All SVG
- **No layout-shifting hover states** - Only color/shadow transitions
- **No invisible borders in light mode** - Teal-tinted borders visible on white
- **No same padding everywhere** - Responsive padding scales per breakpoint
