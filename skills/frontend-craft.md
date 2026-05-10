---
name: frontend-craft
description: Use for any frontend, web app, UI component, or styling work — including building new screens, fixing UI bugs, polishing existing interfaces, or creating components from scratch. Covers HTML, CSS, TypeScript, React, design systems, and product design judgment. Always invoke for frontend tasks even if the user doesn't say "design".
author: Andy Gongea
homepage: https://gongea.com
repository: https://github.com/andygongea/my-skills/blob/main/skills/frontend-craft.md
license: Complete terms in LICENSE.txt
---

# Frontend Craft Skill

Build distinctive, production-grade interfaces. The skill has four layers: **hard rules** (must follow), **process** (when starting fresh), **defaults** (when nothing specified), **guidelines** (judgment calls). The checklist at the end is the gate.

---

## Hard Rules

Non-negotiable. Violations are wrong, full stop.

### Naming & structure
- Use SMACSS naming. **Never BEM** (no `__element` or `--modifier` syntax in new code)
- Apply the namespace prefix to layout (`{ns}l-name`) and module (`{ns}name`) classes only
- State classes: `is-active`, `is-hidden`, `is-loading` — no namespace
- For modifiers, use separate classes starting with `mod-` (`mod-large`, `mod-dark`, `mod-compact`) — no namespace, no BEM-style concatenation
- Modifier classes: `mod-large`, `mod-dark`, `mod-compact` — no namespace
- `id` is for JS targeting only, never CSS
- `class` is for styling only, never `querySelector` (use `data-*` for JS hooks instead)
- Semantic HTML: `<button>` for actions, `<a>` for navigation, `<header>`/`<nav>`/`<main>`/`<section>`/`<footer>` for structure, headings in order

### CSS
- Every CSS rule on one line
- Property order: visibility/display → flex → grid → position → float → box model → typography → everything else
- Component CSS references tokens only — never raw values
- All styling via classes — zero inline `style` attributes
- Tailwind utilities composed into named classes via `@apply` (v3) or `@utility` (v4) — never sprayed across JSX. Exception: `className` on shadcn primitives is the library's contract

### TypeScript
- Airbnb style: `const`/`let`, single quotes, semicolons, 2-space indent
- `interface` over `type` for object shapes
- Explicit return types on all functions
- Every component, hook, utility in its own file

### Accessibility
- All five interactive states defined per element: default, hover, focus, active, disabled
- Focus rings always visible
- Touch targets ≥ 44×44px
- WCAG AA contrast in both light and dark mode
- Decorative icons: `aria-hidden="true"`. Meaningful icons: visible label or `aria-label`
- Form errors: linked via `aria-describedby`, `aria-invalid="true"` on the field
- All images have `alt` (decorative: `alt=""`)

### Motion
- `prefers-reduced-motion` reset in every stylesheet — no exceptions
- Animate only `transform`, `opacity`, `filter` — never layout properties
- Easing curves defined as CSS variables, never raw `cubic-bezier()` inline

### Forms
- No errors on first render
- Validate on blur first time, live after first error
- Labels always visible — no placeholder-as-label

### Performance
- Mobile-first; supports 320px minimum
- Above-fold images: `fetchpriority="high"`. Below-fold: `loading="lazy"` with explicit `width`/`height`
- No spinners under 300ms — use optimistic UI

### Testing
- Every component ships a smoke test + a11y test (Vitest + Testing Library + jest-axe)

---

## Process — Light-Touch vs Full

**Light-touch** — for tweaks, bug fixes, single-property changes, "make this purple". Skip the process; jump to relevant rules; run the Critical checklist at the end.

**Full process** — for new components, new screens, redesigns, anything labelled "design", "build", "create from scratch":

### 1. Gather prerequisites
Ask only for what's missing from context:
- CSS namespace prefix (e.g. `ag-`)
- Body and monospace fonts (defaults if none)
- Primary/accent colour (chosen if none)
- Design system (defaults if none)

### 2. Write the creative brief
Top of the first file as a comment block. Two minutes here saves twenty later.

```
CREATIVE BRIEF
Product:           [what + who]
Job to be done:    [the one thing users come to do]
Aesthetic:         [one sentence, e.g. "editorial precision meets warm utility"]
Emotional register: [calm / energising / serious / playful / trusted]
The unexpected:    [one choice that subverts the obvious]
Memorable moment:  [the single thing users will remember]
Primary task path: [shortest sequence to complete the JTBD]
```

If context is sufficient, fill it yourself. If unclear, ask one targeted question.

---

## Defaults — When Nothing Is Specified

### Stack
- **Components**: shadcn/ui — import from `@/components/ui/...`. If a component doesn't exist, build it before using it.
- **Body font**: Manrope (Google Fonts), weights 400/500/600/700
- **Mono font**: JetBrains Mono, weights 400/500
- **Paragraph line-height**: 1.65
- **Font loading**: `font-display: swap`; preload above-fold

### Spacing — 4px grid

```
--space-0: 0       --space-5: 20px    --space-12: 48px
--space-px: 1px    --space-6: 24px    --space-14: 56px
--space-0-5: 2px   --space-7: 28px    --space-16: 64px
--space-1: 4px     --space-8: 32px    --space-20: 80px
--space-2: 8px     --space-10: 40px   --space-24: 96px
--space-3: 12px
--space-4: 16px
```

Sub-grid (0/1/2px) for hairlines and micro-nudges only. Pseudo-Fibonacci progression after `--space-6`. No off-scale values.

### Type scale — 4px aligned

```
--text-xs: 12px / 16px line-height    --text-2xl: 24px / 32px
--text-sm: 14px / 20px                --text-3xl: 30px / 36px
--text-base: 16px / 1.65 (paragraphs) --text-4xl: 36px / 40px
--text-lg: 18px / 28px                --text-5xl: 48px / 52px
--text-xl: 20px / 28px
```

### Colour palette
Always craft bespoke. Always ask for the primary/accent first; choose one that fits the aesthetic if none provided.

Required tokens on `:root`:
```
Surfaces:  --color-bg, --color-surface, --color-surface-raised
Borders:   --color-border, --color-border-strong
Text:      --color-text, --color-text-muted, --color-heading
Brand:     --color-accent, --color-accent-hover, --color-accent-fg
Semantic:  --color-destructive(-fg), --color-success(-fg), --color-warning(-fg)
```

### Component tokens
Pattern: `--{component}-{role}-{variant?}-{state?}` — `role` covers `bg`, `text`, `border`, `shadow`, `ring`, `radius`, `height`. Component CSS references these, not the global palette directly.

```css
--btn-bg-primary: var(--color-accent);
--btn-bg-primary-hover: var(--color-accent-hover);
--btn-text-primary: var(--color-accent-fg);
--input-border-focus: var(--color-accent);
--card-bg: var(--color-surface);
```

### Elevation
Four levels: `--shadow-0` (flat) through `--shadow-4` (overlay). Cards lift level 1 → 2 on hover. In dark mode, shadows weaken — compensate with inset highlights. No shadow-on-shadow.

### Motion

Easing curves:
```
--ease-expressive: cubic-bezier(0.34, 1.56, 0.64, 1)  /* springy */
--ease-snappy:     cubic-bezier(0.2, 0, 0, 1)         /* fast out */
--ease-gentle:     cubic-bezier(0.4, 0, 0.2, 1)       /* general */
--ease-out, --ease-in
```

Durations: 100/150/200/250/350/500ms — match to element size and importance.

### Breakpoints
`--bp-sm` 480px · `--bp-md` 768px · `--bp-lg` 1024px · `--bp-xl` 1280px · `--bp-2xl` 1536px

### Theme: light + dark — both required
`:root` for light, `[data-theme="dark"]` overrides, `prefers-color-scheme` as default with `localStorage` toggle.

### shadcn integration
Map shadcn's variables (`--background`, `--primary`, `--card`, etc.) to project palette tokens at install time so the project palette stays the single source of truth:

```css
:root {
  --color-accent: hsl(255 80% 60%);  /* project palette */
  --primary: var(--color-accent);     /* shadcn alias */
}
```

---

## Guidelines — Judgment Calls

### Aesthetic
- Pick one emotional register and commit. Avoid generic AI aesthetics: no purple-on-white gradients, no Inter-everywhere, no symmetrical card grids without intent
- Make at least one unexpected choice — palette, layout, motion, or type — that subverts the obvious for this product category
- Craft the palette with intent. The colours should feel chosen for *this* product, not interchangeable
- Negative space is a design tool. The most important element on the page deserves room to breathe — generous space around it signals importance better than colour or weight

### Information architecture
The structure of meaning matters more than the styling of components. Before composing a page, decide the hierarchy.

- One primary task per screen — secondary tasks are subordinate or hidden until needed
- Use tabs for parallel views of the same object (an order's details, payments, history). Use sub-pages for genuinely different objects. Use accordions only for optional / progressive content
- Navigation depth: top-level (5–7 items max) → section (sidebar or tabs) → in-page (anchors). Going deeper means restructuring
- Default to spacious. Density is earned, not given — provide a user toggle if power users genuinely need compact views
- Group by user task, not by data type. "Recent orders" beats "all orders sorted by date"

### Loading state choreography
Match the pattern to the wait length and the layout impact. Never default to a spinner.

| Wait | Pattern |
|---|---|
| 0–300ms | Show nothing — optimistic UI; the action appears to have completed |
| 300ms–1s | Inline indicator on the triggering element (button spinner, subtle pulse) |
| 1s+ | Skeleton matching the final layout dimensions exactly — no layout shift on load |
| Streaming | Progressive reveal — render content as it arrives, never block on full load |
| Background sync | Status pill ("Saving…", then "Saved"), no blocking UI |

Skeletons are not generic grey rectangles — they reflect the actual shape and rhythm of the content that's coming.

### Error recovery
Designing the error message is half the work. Designing the recovery is the other half.

- Preserve user input on every error. A failed submission must not lose what they typed
- Provide explicit retry where the system can resolve it: "Try again" button next to the error
- Auto-save drafts on long-form content; show "Restored from earlier draft" on return
- For destructive actions, offer Undo as a toast for 5–10 seconds before committing
- Network errors get queued retry; user errors get inline correction; system errors get an actionable next step (contact support, refresh, sign in again)
- Never end on the error. Every error state has a path forward

### Motion choreography
Entrances animate overlay → container → content; exits reversed. Max 3 elements animating simultaneously. Stagger list reveals at 30ms intervals. Match easing curves and durations to the size and importance of what's moving.

### Component API
- Composition over configuration. A `<Button>` with 12 props is doing too much
- Boolean props for states (`disabled`, `loading`); union strings for variants (`'primary' | 'ghost'`)
- `children` for content; props for behaviour
- Around 8 props is the soft ceiling — form fields and library components may need more
- `forwardRef` with `displayName`
- Zero-config default must work: `<Button>Save</Button>`

### Forms
- Group related fields with `<fieldset>`/`<legend>`. Address blocks and name pairs in one row; everything else stacked
- Helper text (always visible, muted) explains what to enter; error text replaces it on failure
- Multi-step forms: step indicator at top, ≤ 7 fields per step, allow back-navigation, save partial progress
- Use the right `type`: `email`, `tel`, `date`. Searchable combobox over long `<select multiple>`

### Data tables
- Minimal surface, rich content. 48px row minimum, thin dividers, muted uppercase headers
- Rich cell types where appropriate: badges for status, avatars for users, monospace for metrics, relative dates with absolute on hover, progress bars, hover-only action icons
- Required: global search (300ms debounce) + smart filter panel (compound conditions, dismissible chips). **No column-header filter icons**
- All columns sortable by default — click cycles asc → desc → unsorted
- Pagination with page-size selector and total count

### Toasts
Two tiers, strictly separated:
- **Transient** — auto-dismiss 4s, progress bar, `role="status"`. Save confirmations, copies, low-stakes
- **Persistent** — no auto-dismiss, action button required, `role="alert"`. Errors, decisions, anything needing acknowledgement

Max 3 visible. Position bottom-right desktop, bottom-centre mobile. Slide up + fade in 250ms.

### Empty states
Never blank. Icon + 4-word headline + one-sentence body + single-verb CTA. Optional secondary link.

### Voice
Friendly but not cute. Active/imperative voice ("Save your changes"), present tense ("Your file is uploading"), specific ("3 items deleted" not "Action completed"), encouraging on errors ("That email looks incomplete" not "Invalid input"). Sentences under 20 words. No jargon. Acronyms expanded on first use.

### Performance
- Lazy-load below the fold. Code-split at routes
- Named imports only — no barrel imports of full libraries
- Debounced resize/scroll handlers with `passive: true`
- Reserve space for async content via skeletons matching final dimensions
- `overscroll-behavior: contain` on scrollable panels

### Product thinking
- The user's **task** matters more than their persona — what triggered them, what's the next action, what does success look like
- The primary action is the most prominent element. Destructive actions never default-focused
- Any action on one item should be available on many (multi-select, bulk ops)
- Surface metrics users actually track. Make them readable, not decorative
- Design the hard path: empty, error, loading, restricted, partial-data states are not edge cases
- Perceived performance > actual performance: skeletons, optimistic updates, micro-delight at completion

---

## Pre-Delivery Checklist

### Critical — every delivery
- [ ] Semantic HTML, no div-soup
- [ ] All five interactive states present per element
- [ ] Focus rings visible; full keyboard navigation
- [ ] WCAG AA contrast in light + dark mode
- [ ] Touch targets ≥ 44×44px
- [ ] `prefers-reduced-motion` reset present
- [ ] Only `transform`/`opacity`/`filter` animated
- [ ] No layout shift on async content
- [ ] Empty, error, loading states designed
- [ ] Forms validate on blur (not on render)
- [ ] Tested at 320px

### Comprehensive — substantial work
- [ ] Creative brief written
- [ ] Namespace prefix, fonts, accent colour all set
- [ ] One unexpected design choice present
- [ ] Bespoke palette with stated intent
- [ ] Component tokens follow the formula
- [ ] shadcn variables mapped to palette
- [ ] SMACSS naming, zero BEM
- [ ] Single-line CSS in property order
- [ ] All styling via classes (no inline, no scattered utilities)
- [ ] JSDoc on components/props/functions
- [ ] Smoke + a11y test per component
- [ ] Bulk actions where relevant
- [ ] Information architecture: one primary task per screen, hierarchy intentional
- [ ] Loading patterns matched to wait length (no default spinners)
- [ ] Errors preserve user input and offer a path forward
- [ ] Tables: global search + smart filter (no column filters)
- [ ] Toasts: transient vs persistent separated
- [ ] Voice consistent throughout

### Final qualitative pass
- [ ] Does it look designed, not generated?
- [ ] Does it feel alive — immediate, physical, with one moment of delight?
- [ ] Is the primary task the easiest path?
- [ ] Does the copy work for non-native speakers?
