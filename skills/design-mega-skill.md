---
name: frontend-design
description: Create distinctive, production-grade frontend interfaces with high design quality. Use this skill when the user asks to build web components, pages, artifacts, posters, or applications (examples include websites, landing pages, dashboards, React components, HTML/CSS layouts, or when styling/beautifying any web UI). Generates creative, polished code and UI design that avoids generic AI aesthetics. Always use this skill for any frontend, web app, UI component, or styling task — even if the user doesn't say "design" explicitly.
license: Complete terms in LICENSE.txt
---

This skill guides creation of distinctive, production-grade frontend interfaces. Implement real working code with exceptional attention to aesthetic, performance, and product-thinking quality.

---

## Step 0: Gather Prerequisites

Before writing any code, confirm two things if not already provided:

1. **CSS namespace prefix** — ask the user for a short prefix like `ag-`, `ui-`, `ds-`. Applied to all layout and module classes. State (`is-*`) and modifier (`mod-*`) classes never carry a namespace.
2. **Design system** — if the user has not specified one, use the **default stack** below.

If both are already clear from the conversation, proceed directly to design thinking.

---

## Default Design System (when none is specified)

### UI Components: shadcn/ui
- Use shadcn/ui as the base component library
- Always import from `@/components/ui/...`
- If a needed component does not exist in shadcn, **build it first** as a dedicated component file before using it
- Never inline one-off UI logic — every distinct element is its own component

### Typography
- **Body / UI text**: `Manrope` (Google Fonts) — weights 400, 500, 600, 700
- **Code / monospace**: `JetBrains Mono` (Google Fonts) — weights 400, 500
- Paragraph text line-height: **1.65**
- Never use Inter, Roboto, Arial, or system-ui as primary fonts

### Spacing Scale: 4px base
All spacing, sizing, and layout values must snap to the 4px grid. Sub-grid values (0, 1px, 2px) exist for fine-grained details like borders, dividers, and hairline offsets — not for layout.

| Token | Value | Use |
|---|---|---|
| `--space-0` | 0px | Explicit zero resets |
| `--space-px` | 1px | Hairlines, borders, dividers |
| `--space-0-5` | 2px | Micro gaps, icon nudges, focus ring offsets |
| `--space-1` | 4px | Tight internal padding, icon-to-label gap |
| `--space-2` | 8px | Component internal spacing |
| `--space-3` | 12px | Small gaps |
| `--space-4` | 16px | Default padding |
| `--space-5` | 20px | |
| `--space-6` | 24px | Section padding, card padding |
| `--space-8` | 32px | |
| `--space-10` | 40px | |
| `--space-12` | 48px | Row height baseline |
| `--space-16` | 64px | |
| `--space-20` | 80px | |
| `--space-24` | 96px | Page-level spacing |

Use these tokens in CSS variables and Tailwind classes (`p-4`, `gap-6`, `border` for 1px, etc.). No arbitrary pixel values off this scale.

### Colour Palette
Always craft a bespoke palette per project — never reuse a generic or default theme. Define it as CSS custom properties on `:root`. Every palette must include:
- `--color-bg` — page background
- `--color-surface` — card / panel background
- `--color-surface-raised` — elevated surface (modals, dropdowns)
- `--color-border` — default border
- `--color-text` — body text
- `--color-text-muted` — secondary / caption text
- `--color-heading` — headings
- `--color-accent` — primary brand / action colour
- `--color-accent-hover` — hover state of accent
- `--color-destructive` — error / danger
- `--color-success` — success state

Be creative. The palette should reflect the product's character — don't default to blue-on-white or dark-grey-on-black.

### Component Token Naming

All design tokens for components follow this pattern:

```
--{component}-{role}-{variant?}-{state?}
```

| Segment | Required | Description |
|---|---|---|
| `component` | Yes | The UI element: `btn`, `input`, `card`, `badge`, `toast`, `table`, `nav` |
| `role` | Yes | What it controls: `bg`, `text`, `border`, `shadow`, `ring`, `icon`, `gap`, `radius`, `height` |
| `variant` | Optional | The visual style: `primary`, `ghost`, `danger`, `success`, `muted` |
| `state` | Optional | The interaction state: `hover`, `focus`, `active`, `disabled`, `checked` |

```css
/* Examples */
--btn-bg-primary: var(--color-accent);
--btn-bg-primary-hover: var(--color-accent-hover);
--btn-text-primary: #ffffff;
--btn-bg-ghost: transparent;
--btn-border-ghost: var(--color-border);
--btn-bg-ghost-hover: var(--color-surface-raised);
--btn-text-disabled: var(--color-text-muted);
--btn-bg-disabled: var(--color-surface);

--input-bg: var(--color-surface);
--input-border: var(--color-border);
--input-border-focus: var(--color-accent);
--input-border-error: var(--color-destructive);
--input-text: var(--color-text);
--input-text-placeholder: var(--color-text-muted);

--card-bg: var(--color-surface);
--card-border: var(--color-border);
--card-shadow: 0 1px 3px rgba(0,0,0,0.08);
--card-radius: 0.5rem;
--card-gap: var(--space-6);

--badge-bg-success: …;
--badge-text-success: …;
--badge-bg-danger: …;
```

Always define component tokens as a layer on top of the global palette — never reference raw colour values in component CSS, only tokens.

---

## JavaScript / TypeScript Rules

### Library vs Vanilla Decision

Use **React / Vue / framework** when:
- Significant component reuse across views
- Non-trivial state management
- Full app or multi-page experience
- Complex data flows or routing

Use **vanilla TypeScript** when:
- Single component or page enhancement
- Minimal state (a few toggles, one form)
- Performance or zero-dependency constraints

### TypeScript + Airbnb Style
- `const` / `let` only — never `var`
- Arrow functions for callbacks
- Explicit return types on all functions
- `interface` over `type` for object shapes
- No unused variables or imports
- Single quotes, semicolons required
- 2-space indentation
- `camelCase` vars/functions, `PascalCase` classes/components, `SCREAMING_SNAKE_CASE` constants

### Code Organisation
- **No inline logic** — every component, hook, or utility lives in its own file
- Co-locate styles with the component that owns them
- Shared utilities go in `lib/` or `utils/`
- Types go in `types/` or alongside the file they describe

---

## CSS Rules

### Naming: SMACSS (BEM is forbidden — never use `__` or `--` suffix syntax)

Apply the user's namespace prefix to layout and module classes only:

| Category | Pattern | Example (prefix `ag-`) |
|---|---|---|
| Layout | `{ns}l-name` | `ag-l-grid`, `ag-l-sidebar` |
| Module | `{ns}name` | `ag-card`, `ag-nav` |
| Sub-module | `{ns}name-part` | `ag-card-title`, `ag-nav-item` |
| State | `is-state` | `is-active`, `is-hidden`, `is-loading` |
| Modifier | `mod-variant` | `mod-large`, `mod-dark`, `mod-compact` |

IDs use the same namespace notation as modules (no `js-` prefix): `{ns}submit-btn`, `{ns}modal-root`.

### Property Order (one line per rule)

Within each rule, follow this fixed sequence:

1. Visibility & display: `display`, `visibility`, `opacity`, `overflow`
2. Flex: `flex`, `flex-direction`, `flex-wrap`, `justify-content`, `align-items`, `align-self`, `gap`, `order`
3. Grid: `grid`, `grid-template-*`, `grid-column`, `grid-row`, `grid-area`
4. Position: `position`, `top`, `right`, `bottom`, `left`, `z-index`
5. Float: `float`, `clear`
6. Box model: `width`, `height`, `min-*`, `max-*`, `margin`, `padding`, `border`, `border-radius`, `box-shadow`, `box-sizing`
7. Text & typography: `font-family`, `font-size`, `font-weight`, `line-height`, `letter-spacing`, `text-align`, `text-transform`, `text-decoration`, `color`, `white-space`
8. Everything else: `background`, `cursor`, `transition`, `transform`, `content`, `pointer-events`, etc.

All rules on a **single line**:

```css
/* Correct */
.ag-card { display: flex; flex-direction: column; position: relative; width: 100%; padding: var(--space-6); border: 1px solid var(--color-border); border-radius: 0.5rem; font-family: 'Manrope', sans-serif; line-height: 1.65; color: var(--color-text); background: var(--color-surface); }
.ag-card-title { display: block; font-size: 1.25rem; font-weight: 700; color: var(--color-heading); }
.is-active { opacity: 1; visibility: visible; }
.mod-large { padding: var(--space-10); font-size: 1.125rem; }

/* Wrong */
.ag-card {
  display: flex;
  padding: 1.5rem;
}
```

---

## HTML Rules

### Semantics
- `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<aside>`, `<footer>` for structure
- `<h1>`–`<h6>` in proper hierarchy (never skip levels)
- `<button>` for actions, `<a>` for navigation only
- `<ul>` / `<ol>` / `<li>` for lists; `<figure>` / `<figcaption>` for media
- `<time>`, `<address>`, `<abbr>`, `<mark>`, `<kbd>` where semantically correct

### ID and Class Discipline
- `id` — JS targeting only, never for styling. Use namespace notation: `ag-submit-btn`
- `class` — styling only, never as JS selectors; use `data-*` attributes for JS hooks instead

---

## Performance

- Lazy-load images and heavy components (`loading="lazy"`, dynamic `import()`)
- Avoid layout thrash — read DOM then write, never interleave
- Prefer CSS transitions/animations over JS-driven ones
- Use `will-change` sparingly and only on elements known to animate
- Code-split at route or feature boundaries
- Prefer `transform` and `opacity` for animations (compositor-only properties)
- Debounce resize/scroll handlers
- Avoid large dependency imports — import only what's used

---

## No Jank — UI Smoothness Rules

Jank is any moment where the UI feels broken, laggy, or unpredictable. It destroys trust faster than bugs. Eliminate it by design.

### Layout Stability
- **Reserve space for async content** — never let images, avatars, or loaded data cause layout shift. Use explicit `width`/`height` on images; use skeleton placeholders that match the final content's dimensions exactly.
- **No content pop-in** — if data loads after render, the container must already exist at the correct size. Use `min-height` on lists and cards.
- **Avoid `width: auto` on elements that change** — it causes reflow. Use fixed or `min-width` values for buttons, badges, and inputs.

### Animation & Transition Quality
- Only animate `transform`, `opacity`, and `filter` — never `width`, `height`, `top`, `left`, `margin`, or `padding` (these trigger layout recalculation)
- All transitions: `150ms` for micro-interactions, `250ms` for panel/modal entries, `200ms ease-out` for exits
- Entrances ease-out (fast start, gentle stop); exits ease-in (gentle start, quick stop)
- Never start an animation on a component that is still loading
- Use `animation-fill-mode: both` so elements don't flash before/after animation

### State Transition Discipline
- Every interactive element must have explicit styles for: default, hover, focus, active, disabled — no missing states
- Focus rings must always be visible — never `outline: none` without a custom replacement
- Disabled elements are visually distinct but never invisible
- Loading states replace content in-place — don't push layout around
- Never show a spinner for operations under 300ms — use optimistic UI instead

### Scroll & Interaction Feel
- `scroll-behavior: smooth` on the document
- Use `overscroll-behavior: contain` on scrollable panels to prevent scroll bleed
- Momentum scrolling on mobile: `-webkit-overflow-scrolling: touch` (or modern equivalent)
- Sticky headers: use `position: sticky` with a defined `top` value — never `position: fixed` inside scroll containers
- Avoid `onScroll` handlers without `passive: true` and debouncing

### Font & Asset Loading
- Fonts loaded with `font-display: swap` — no invisible text during load
- Preload critical fonts: `<link rel="preload" as="font">`
- Images that appear above the fold: `fetchpriority="high"`, no `lazy`
- Use `srcset` and `sizes` for responsive images

---

## Product Design Principles

### Job to Be Done
Every design decision must serve the user's goal. Ask: *what is the user trying to accomplish?* Design the shortest path to completing that task.

### Task-First, Not Persona-First
Personas are not used. Focus on the **task** the user is executing:
- What triggered them to open this UI?
- What is the single most likely next action?
- What does success look like for them?

### Action Accessibility
- The most common action must be the most prominent and reachable element
- Destructive actions require confirmation; they are never the default
- Keyboard navigation must be complete — no mouse-only flows

### Bulk & Automation
- Any action performable on one item must be offerable on many
- Provide multi-select, batch operations, and automation triggers wherever relevant
- Expose keyboard shortcuts for power-user flows

### Metrics Awareness
Design for the outcomes users track. If users care about conversion rate, response time, completion rate, etc., surface those numbers clearly and without noise.

### Transitions & Perceived Performance
- Use smooth, purposeful transitions (150–300ms ease) to signal state changes
- Skeleton screens and optimistic UI updates reduce perceived wait time
- Introduce micro-delight (subtle motion, satisfying feedback) at completion moments
- Never animate for animation's sake — every motion must have a reason

### The Hard Path
Always design for error and edge states, not just the happy path:
- Empty states: give context, a clear call-to-action, and a hint of what's possible
- Error states: explain what went wrong, offer a recovery action
- Loading states: skeleton or spinner with progress indication
- Zero-permission / restricted states: explain why and what the user can do
- Partial data: show what's available, indicate what's missing

### Empty States
Empty states are **not** blank pages. Every empty state must have:
1. An illustration or icon that sets the tone
2. A short headline stating the situation
3. A single, clear call-to-action to populate it
4. Optional: a brief explanation of the benefit

### Onboarding
Onboarding must be lightweight and action-oriented:
- Show value immediately — don't gate it behind a form
- Inline hints and tooltips beat a separate tour
- Progress indicators motivate completion
- Let users skip and return later
- First action should feel like a win

---

## Component Architecture

1. **Audit first**: before building, list all UI pieces needed. Check if shadcn/ui covers them.
2. **Build missing components** before the feature that needs them — never inline ad-hoc UI.
3. **One concern per component**: a Button doesn't know about forms; a Form doesn't know about routing.
4. **Prop discipline**: components accept typed props; no untyped `any`; no prop drilling beyond 2 levels (use context or composition).
5. **Accessibility built in**: every interactive component has correct ARIA roles, labels, and focus management from the start — not added later.

---

## Aesthetic Guidelines

Go beyond stereotype. Understand what the product is, then deliberately subvert the expected aesthetic in one meaningful dimension. A fintech app doesn't have to be navy-and-white. A creative tool doesn't have to be dark-and-neon.

- Craft the palette first — it sets the entire emotional register
- Typography is character: Manrope for warmth and clarity, JetBrains Mono for precision contexts
- Spatial rhythm from the 4px grid creates calm and order
- Motion should feel like physics, not magic tricks
- Accessible contrast is not a constraint — it's a design quality signal

NEVER produce generic AI aesthetics: no purple-gradient-on-white, no Inter-everywhere, no cookie-cutter card grids without visual intent.

---

## Delivery Checklist

Before submitting any code:

**Conventions**
- [ ] Namespace prefix confirmed and applied consistently
- [ ] No BEM syntax (`__` / `--`) anywhere
- [ ] `is-*` state classes have no namespace; `mod-*` modifier classes have no namespace
- [ ] CSS property order: visibility → flex → grid → position → float → box model → text → rest
- [ ] All CSS rules are single-line
- [ ] `id` used only for JS targeting, using namespace notation
- [ ] `class` used only for styling
- [ ] Full semantic HTML with no div-soup

**Stack & Tokens**
- [ ] TypeScript throughout, Airbnb style
- [ ] shadcn/ui used as base; missing components built before use
- [ ] Manrope (body) + JetBrains Mono (code) loaded with `font-display: swap`
- [ ] All spacing on 4px grid via tokens (`--space-0`, `--space-px`, `--space-0-5` through `--space-24`)
- [ ] Component tokens follow `{component}-{role}-{variant?}-{state?}` naming
- [ ] Global palette tokens defined; component tokens reference palette only (no raw values)
- [ ] Bespoke colour palette defined as CSS custom properties
- [ ] Paragraph line-height 1.65

**Responsive & Theming**
- [ ] Mobile-first layout; tested at 320px
- [ ] Named breakpoints used consistently
- [ ] Touch targets ≥ 44×44px
- [ ] Light and dark mode both implemented
- [ ] `prefers-reduced-motion` rule present in every stylesheet

**Forms**
- [ ] Validation triggers on blur (first time) and on change (after first error)
- [ ] No errors shown on initial render
- [ ] Every error associated via `aria-describedby`; `aria-invalid` set correctly
- [ ] Labels always visible; no placeholder-as-label

**Tables**
- [ ] Global search with 300ms debounce and clear action
- [ ] Smart filter panel (not column-header filters)
- [ ] Active filter count badge; clear-all action
- [ ] All columns sortable; sort direction visible
- [ ] Rich content types used (badges, avatars, relative dates, etc.)
- [ ] Pagination with total count displayed

**No Jank**
- [ ] Async content has reserved space; no layout shift on load
- [ ] Only `transform`, `opacity`, `filter` are animated — never layout properties
- [ ] All interactive states defined: default, hover, focus, active, disabled
- [ ] Focus rings always visible
- [ ] `font-display: swap` on all fonts; critical fonts preloaded
- [ ] `overscroll-behavior: contain` on scrollable panels
- [ ] No spinners for operations under 300ms — optimistic UI used instead

**Notifications**
- [ ] Transient toasts: auto-dismiss 4s, progress bar, `role="status"`, bottom-right
- [ ] Persistent toasts: no auto-dismiss, action button, `role="alert"`, stacked above transient
- [ ] Max 3 toasts visible; queue overflow
- [ ] Toast animations use `transform` + `opacity` only
- [ ] Empty states designed with illustration, headline, and CTA
- [ ] Error and loading states designed
- [ ] Bulk actions available where relevant
- [ ] Keyboard navigation complete
- [ ] Copy uses plain language, active voice, present tense
- [ ] No jargon or idioms; acronyms expanded on first use
- [ ] Brand voice: friendly, direct, specific

**Creative & Design**
- [ ] Creative brief written before any code
- [ ] Aesthetic direction is specific and committed — not generic
- [ ] Palette crafted for this product with stated intent
- [ ] One unexpected design choice present
- [ ] Motion personality chosen and applied consistently
- [ ] Mid-process design audit passed

**Motion**
- [ ] Named easing curves defined as CSS custom properties
- [ ] Duration tiers applied correctly by element size/importance
- [ ] Choreography rule respected — no more than 3 things animate simultaneously
- [ ] `prefers-reduced-motion` reset present in every stylesheet
- [ ] Entrance/exit easing correct (ease-out in, ease-in out)

**Iconography**
- [ ] Lucide used; only required icons imported
- [ ] Icon size on 4px grid and appropriate for context
- [ ] Stroke width consistent throughout
- [ ] Decorative icons have `aria-hidden="true"`
- [ ] Meaningful icons paired with visible label or `aria-label`

**Elevation & Shadow**
- [ ] Four-level shadow system defined as tokens
- [ ] Dark mode shadow overrides defined
- [ ] Each elevation level used for its designated element class only
- [ ] Shadow transitions use `--ease-gentle` at `150ms`

**Images**
- [ ] All images in containers with explicit aspect ratio
- [ ] Above-fold: `fetchpriority="high"`, no lazy
- [ ] Below-fold: `loading="lazy"`, explicit `width`/`height`
- [ ] `.webp` format used; art direction via `<picture>` where relevant

**Component API**
- [ ] Composition preferred over configuration
- [ ] Boolean props for states; union strings for variants
- [ ] No component has more than 8 props without justification
- [ ] All components use `React.forwardRef` with `displayName`
- [ ] Every prop has JSDoc documentation

**Copy**
- [ ] Confirmation dialogs: destructive action on right, not default focus
- [ ] Success messages are specific (not "Action completed")
- [ ] Tooltips under 10 words, describe outcome not mechanism
- [ ] Empty states follow the formula: icon + headline + body + CTA
- [ ] Voice consistent throughout — reads like one person wrote it

**Code Quality**
- [ ] Performance optimisations applied (lazy load, compositor-only animations, code split)
- [ ] No inline logic — every component in its own file
- [ ] All components, props, and functions documented with JSDoc
- [ ] Inline comments explain *why*, not what
- [ ] Smoke test + a11y test for every component

---

## Responsive Strategy

Design **mobile-first**. Build for the smallest viewport first, then layer complexity upward via breakpoints.

### Named Breakpoints (4px-grid aligned)

| Token | Value | Context |
|---|---|---|
| `--bp-sm` | 480px | Large phones, landscape |
| `--bp-md` | 768px | Tablets |
| `--bp-lg` | 1024px | Small laptops |
| `--bp-xl` | 1280px | Desktops |
| `--bp-2xl` | 1536px | Wide screens |

In Tailwind: `sm:`, `md:`, `lg:`, `xl:`, `2xl:`. In plain CSS: `@media (min-width: 768px)`.

### Rules
- Touch targets: **minimum 44×44px** on all interactive elements
- Never hide critical actions on mobile — reflow them instead
- Navigation collapses to a drawer or bottom bar below `md`
- Tables become cards or horizontally scrollable containers on `sm`
- Typography scales: body 14px on mobile → 16px on `md`+
- Prefer `clamp()` for fluid type and spacing where appropriate: `font-size: clamp(1rem, 2.5vw, 1.25rem)`
- Test every layout at 320px (minimum supported width)

---

## Theming: Light + Dark Mode

Every project ships both modes. Use CSS custom properties on `:root` for light, override on `[data-theme="dark"]`. Respect `prefers-color-scheme` as the default, allow manual toggle stored in `localStorage`.

```css
:root { --color-bg: #ffffff; --color-text: #111111; /* ... full palette */ }
[data-theme="dark"] { --color-bg: #0f0f0f; --color-text: #f0f0f0; /* ... */ }
@media (prefers-color-scheme: dark) { :root:not([data-theme="light"]) { /* same dark overrides */ } }
```

### Motion Accessibility — mandatory
Every animation or transition must respect reduced-motion preference:

```css
@media (prefers-reduced-motion: reduce) { *, *::before, *::after { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; } }
```

This rule is **non-negotiable** and goes in every stylesheet.

---

## Forms & Validation

### When to Trigger Feedback
- **Never on first render** — no errors before the user has interacted
- **On blur** — validate a field when the user leaves it (after first touch)
- **On change after first error** — once a field has shown an error, update live as the user corrects it
- **On submit** — validate all fields; focus the first error

### Inline Feedback Rules
- Error message sits directly below the field, associated via `aria-describedby`
- Use `aria-invalid="true"` on the input when in error state
- Success indication: subtle (a check icon or border colour change) — never intrusive
- Never use colour alone to signal state — pair with icon + text
- Field labels are always visible — no placeholder-as-label

### Form UX Patterns
- Disable submit only during active submission — not before interaction
- Show a loading state on the submit button during async operations
- On success: confirm clearly; on failure: explain what went wrong and what to do
- Long forms: show progress; allow saving partial state
- Autofill-friendly: use correct `autocomplete` attributes on every field

---

## Toast Notifications

Toasts are the default feedback mechanism for system-level events (saves, errors, async completions). They are **never** used for inline field validation — that stays in the form.

### Two Tiers — strictly separated

#### Tier 1 — Transient (self-dismissing)
For low-stakes, informational outcomes the user doesn't need to act on:
- Auto-dismisses after **4 seconds**
- Has a visible progress bar showing time remaining
- Can be hovered to pause the timer
- Can be manually dismissed with ×
- Positioned: **bottom-right** on desktop, **bottom-center** on mobile
- Visual weight: minimal — small icon, single line of text, no action button required
- Examples: "Changes saved", "Link copied", "File uploaded", "3 items deleted"

```
┌─────────────────────────────┐
│ ✓  Changes saved        ×   │
│ ▓▓▓▓▓▓▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒   │  ← progress bar
└─────────────────────────────┘
```

Tokens:
```css
--toast-bg-transient: var(--color-surface-raised);
--toast-border-transient: var(--color-border);
--toast-text-transient: var(--color-text);
--toast-progress-transient: var(--color-accent);
```

#### Tier 2 — Persistent (requires user action)
For high-stakes events the user must acknowledge or act on:
- **Does not auto-dismiss** — stays until the user explicitly acts or closes it
- Has a clear primary action button (e.g. "Retry", "Review", "Undo")
- Optionally has a secondary dismiss action
- Positioned: **bottom-right** on desktop, stacked above transient toasts
- Visual weight: heavier — distinct background, stronger border, icon + title + description
- Examples: "Export failed — check your connection and try again", "Payment declined", "Session expiring in 2 minutes", "Unsaved changes will be lost"

```
┌──────────────────────────────────┐
│ ⚠  Couldn't save your changes   │
│    Your session timed out.       │
│                                  │
│  [Sign in again]    Dismiss      │
└──────────────────────────────────┘
```

Tokens:
```css
--toast-bg-persistent: var(--color-surface-raised);
--toast-border-persistent: var(--color-destructive);
--toast-text-persistent: var(--color-text);
--toast-icon-persistent: var(--color-destructive);
--toast-bg-persistent-action: var(--color-accent);
--toast-text-persistent-action: #ffffff;
```

### Stacking & Layout
- Maximum **3 toasts** visible at once — queue additional ones
- Toasts stack upward from the anchor point
- New toasts animate in from the bottom (`transform: translateY(100%)` → `translateY(0)`, `opacity: 0` → `1`, `250ms ease-out`)
- Dismissed toasts collapse their height smoothly (`200ms ease-in`) before removing from DOM
- Persistent toasts always appear above transient ones in the stack
- On mobile, toasts take full width with `16px` side margin

### Accessibility
- Transient toasts: `role="status"` (polite announcement)
- Persistent toasts: `role="alert"` (assertive announcement)
- Dismiss button always has `aria-label="Dismiss notification"`
- Action button receives focus automatically for persistent toasts

---

## Data Tables

### Aesthetic
Minimal surface, rich content. Tables should feel calm and scannable — no heavy borders, no zebra striping by default. Let content density and typography carry the weight.

- Thin `1px` dividers between rows using `--color-border`
- Generous row height: minimum 48px (12 × 4px)
- Column headers: uppercase, `letter-spacing: 0.06em`, muted colour, lighter weight
- Hover state: subtle `--color-surface-raised` background on row
- Selected rows: accent-tinted background

### Content Types
Tables must support rich cell types beyond plain text:

| Type | Treatment |
|---|---|
| Status / badge | Coloured pill with semantic colour + icon |
| User / avatar | Avatar image + name + optional subtitle |
| Number / metric | Monospace font (JetBrains Mono), right-aligned, formatted with locale |
| Date | Relative (`2 days ago`) with absolute on hover via `<time>` + tooltip |
| Progress | Inline bar or arc with percentage label |
| Actions | Icon buttons revealed on row hover; never always-visible icon clutter |
| Boolean | Toggle or icon — never raw "true" / "false" |
| Currency | Right-aligned, locale-formatted, colour-coded positive/negative |

### Search & Filtering — required on every table

Every table must have:
1. **Global search** — full-text across all visible columns; debounced 300ms; clears with a single keystroke or × button
2. **Smart filter panel** — not column-header dropdowns. A dedicated filter surface (sidebar drawer, popover, or top panel) that lets users build compound conditions: field → operator → value. Filters stack and display as dismissible chips above the table. Use natural language hints: "show me orders from last week over $500".
3. **No column-header filter icons** — filtering lives in the smart filter panel only
4. **Active filter indicators** — a count badge on the filter trigger button shows how many filters are active
5. **Clear all** — single action to reset all filters and search

### Sorting
- All columns sortable by default unless explicitly non-sortable
- Click header to sort ascending; click again for descending; third click clears sort
- Show sort direction with a subtle arrow icon in the header
- Only one sort active at a time unless shift-click multi-sort is explicitly enabled
- Current sort persists through filter changes

### Pagination / Infinite Scroll
- Default to pagination with a page size selector (10 / 25 / 50 / 100)
- Infinite scroll is acceptable for feeds; not for data tables where position matters
- Show total count: "Showing 1–25 of 348 results"

---

## Testing

Every component ships with exactly two tests — no more required as a baseline:

```tsx
// ComponentName.test.tsx
import { render, screen } from '@testing-library/react';
import { axe, toHaveNoViolations } from 'jest-axe';
import { ComponentName } from './ComponentName';

expect.extend(toHaveNoViolations);

describe('ComponentName', () => {
  it('renders without crashing', () => {
    render(<ComponentName />);
    expect(screen.getByRole('...')).toBeInTheDocument();
  });

  it('has no accessibility violations', async () => {
    const { container } = render(<ComponentName />);
    expect(await axe(container)).toHaveNoViolations();
  });
});
```

Stack: **Vitest** + **Testing Library** + **jest-axe**. No exceptions on the a11y test — it is part of the definition of done.

---

## Code Documentation

Every file, component, function, and prop must be documented:

### Components
```tsx
/**
 * DataTable — displays paginated, sortable, filterable tabular data.
 *
 * @example
 * <DataTable columns={columns} data={rows} searchPlaceholder="Search orders…" />
 */
```

### Props
```tsx
interface DataTableProps {
  /** Column definitions including type, label, and sort behaviour. */
  columns: ColumnDef[];
  /** Row data. Must be serialisable. */
  data: Record<string, unknown>[];
  /** Placeholder text shown in the global search input. */
  searchPlaceholder?: string;
}
```

### Functions
```tsx
/**
 * Formats a number as a locale-aware currency string.
 * @param value - Raw numeric amount
 * @param currency - ISO 4217 currency code (default: 'USD')
 * @param locale - BCP 47 locale string (default: 'en-US')
 * @returns Formatted string e.g. "$1,234.56"
 */
const formatCurrency = (value: number, currency = 'USD', locale = 'en-US'): string => { ... };
```

Inline comments explain **why**, not what. Avoid restating the code:
```tsx
// Bad: increment the counter
count++;

// Good: optimistic update — server confirms asynchronously
count++;
```

---

## Content & Voice

### Accessibility for Non-Native Speakers
- Use plain, direct language: prefer "Save" over "Persist changes", "Delete" over "Purge record"
- Sentences under 20 words where possible
- Avoid idioms, jargon, or domain-specific terminology unless it is the user's own domain language
- Acronyms always expanded on first use with `<abbr title="...">` in HTML
- Error messages explain what happened and what to do — never just a code or a single word

### Brand Voice
Tones that apply everywhere — UI labels, empty states, error messages, onboarding copy, tooltips:

- **Friendly but not cute** — warm and direct, never patronising or over-enthusiastic
- **Helpful not clever** — clarity wins over wit; a pun that confuses is a failed pun
- **Active voice** — "Save your changes" not "Changes can be saved"
- **Present tense** — "Your file is uploading" not "Your file will be uploaded"
- **Specific** — "3 items deleted" not "Action completed successfully"
- **Encouraging on errors** — "That email doesn't look right — check for a missing @" not "Invalid input"

Empty state example:
> **Nothing here yet**
> Once you add your first project, it'll show up here. Takes about 30 seconds.
> [Create project]

Error example:
> **Couldn't save your changes**
> Your session timed out. Sign in again and your work will still be here.
> [Sign in]

---

## Step 1: Creative Brief (mandatory before any code)

Before writing a single line, produce a short creative brief. This is internal thinking made explicit — it aligns every decision that follows. Write it as a comment block at the top of your first file, or present it to the user before proceeding.

```
CREATIVE BRIEF
──────────────
Product:      [what it is and who uses it]
Job to be done: [the one thing users come here to accomplish]
Aesthetic direction: [one precise sentence — e.g. "editorial precision meets warm utility"]
Emotional register: [how it should feel — calm, energising, serious, playful, trusted]
The unexpected choice: [one decision that subverts the obvious — palette, layout, motion, type]
Palette intent: [why these colours for this product, not just what they are]
The one thing users remember: [the single most distinctive moment in the UI]
```

Do not skip this. A brief written in 2 minutes prevents 20 minutes of generic output. If the user has given enough context, fill it yourself. If genuinely unclear, ask one targeted question — not a list.

---

## Motion Language

Motion is a design material, not a finishing touch. Every moving thing in the UI should feel like it obeys physics — weight, momentum, and intention.

### Named Easing Curves

Define these as CSS custom properties and use them by name — never write raw cubic-bezier values inline:

```css
--ease-expressive: cubic-bezier(0.34, 1.56, 0.64, 1);   /* springy, confident — buttons, badges, selections */
--ease-snappy:     cubic-bezier(0.2, 0, 0, 1);           /* fast out, minimal overshoot — menus, drawers */
--ease-gentle:     cubic-bezier(0.4, 0, 0.2, 1);         /* material standard — general transitions */
--ease-out:        cubic-bezier(0, 0, 0.2, 1);            /* entrances — things arriving */
--ease-in:         cubic-bezier(0.4, 0, 1, 1);            /* exits — things leaving */
```

### Duration Tiers

Match duration to the size and importance of the element moving:

| Tier | Duration | Use |
|---|---|---|
| Instant | 100ms | Icon swaps, checkbox ticks, focus rings |
| Micro | 150ms | Button states, badge changes, tooltips appearing |
| Short | 200ms | Dropdown open/close, tab switches, small panel slides |
| Medium | 250ms | Modal entrance, drawer slide, page section reveals |
| Long | 350ms | Full-page transitions, onboarding steps, large overlays |
| Deliberate | 500ms+ | Hero animations, celebration moments — used sparingly |

### Choreography Rules

When multiple elements animate together, they must be orchestrated — not simultaneous:

- **Staggered list reveals**: delay each item by `30ms` from the previous (`animation-delay: calc(var(--i) * 30ms)`)
- **Entrance order**: background/overlay first → container → content → actions
- **Exit order**: reverse — actions first → content → container → overlay
- **Never animate more than 3 things simultaneously** — it reads as chaos
- **Page load**: only the above-the-fold hero animates on load; everything else animates on scroll into view (`IntersectionObserver`)
- **Scroll-triggered**: use `IntersectionObserver` with `threshold: 0.15` — elements animate when 15% visible

### Motion Personality

Choose one of these and apply it consistently throughout the project:

| Personality | Easing | Duration | Character |
|---|---|---|---|
| **Precise** | `--ease-snappy` dominant | Short/Micro | Enterprise, data-heavy, surgical |
| **Warm** | `--ease-gentle` dominant | Short/Medium | Consumer, helpful, approachable |
| **Expressive** | `--ease-expressive` dominant | Medium | Creative tools, marketing, portfolios |
| **Calm** | `--ease-out` dominant | Long/Deliberate | Finance, health, focus-oriented |

---

## Iconography

Default icon library: **Lucide** (already included with shadcn/ui). Import only what's used — never the full set.

```tsx
import { ArrowRight, Check, X, AlertCircle } from 'lucide-react';
```

### Sizing — always on the 4px grid

| Context | Size | Tailwind |
|---|---|---|
| Inline with small text (12–13px) | 12px | `size-3` |
| Inline with body text (14–16px) | 16px | `size-4` |
| UI controls (buttons, inputs) | 16–20px | `size-4` / `size-5` |
| Section headings, feature icons | 24px | `size-6` |
| Empty states, illustrations | 40–48px | `size-10` / `size-12` |
| Hero / decorative | 64px+ | `size-16`+ |

### Consistency Rules

- **Stroke width**: always `1.5` for Lucide — never the default `2` in dense UI, never `1` in sparse UI. Pick one per project and stick to it.
- **Filled vs outlined**: use outlined (Lucide default) for actions and navigation; use filled variants only for selected/active states to signal the change
- **Alignment**: icons sit on the text baseline — use `flex items-center gap-{n}` with the label, never `vertical-align: middle` in isolation
- **Decorative icons**: `aria-hidden="true"` always; never omit this
- **Meaningful icons**: always paired with a visible label OR a `title` attribute on the SVG AND an `aria-label` on the button
- **Colour**: inherit from parent text colour by default (`currentColor`); only override for semantic meaning (success green, error red)
- **Never scale icons with `font-size`** — use explicit `width`/`height` or Tailwind `size-*`

---

## Shadow & Elevation System

Depth is a communication tool. Use it to show what is above what — not for decoration.

### Four Elevation Levels

```css
/* Level 0 — flat, no shadow. Inline elements, table rows, disabled cards */
--shadow-0: none;

/* Level 1 — resting surface. Cards, panels, sidebars */
--shadow-1: 0 1px 2px rgba(0,0,0,0.06), 0 1px 3px rgba(0,0,0,0.1);

/* Level 2 — raised. Dropdowns, popovers, hover-lifted cards */
--shadow-2: 0 4px 6px -1px rgba(0,0,0,0.08), 0 2px 4px -1px rgba(0,0,0,0.06);

/* Level 3 — floating. Sticky headers, floating action buttons, tooltips */
--shadow-3: 0 10px 15px -3px rgba(0,0,0,0.1), 0 4px 6px -2px rgba(0,0,0,0.05);

/* Level 4 — overlay. Modals, drawers, command palettes */
--shadow-4: 0 20px 25px -5px rgba(0,0,0,0.12), 0 10px 10px -5px rgba(0,0,0,0.04);
```

### Dark Mode Shadows

On dark backgrounds, box shadows disappear. Compensate with borders:

```css
[data-theme="dark"] {
  --shadow-1: 0 1px 2px rgba(0,0,0,0.4), inset 0 1px 0 rgba(255,255,255,0.04);
  --shadow-2: 0 4px 6px rgba(0,0,0,0.4), inset 0 1px 0 rgba(255,255,255,0.06);
  --shadow-3: 0 10px 15px rgba(0,0,0,0.5), inset 0 1px 0 rgba(255,255,255,0.08);
  --shadow-4: 0 20px 40px rgba(0,0,0,0.6), inset 0 1px 0 rgba(255,255,255,0.1);
}
```

### Elevation Rules

- Each level is used for exactly one class of elements — do not mix
- Elevation increases on interaction: a Level 1 card lifts to Level 2 on hover (`transition: box-shadow 150ms var(--ease-gentle)`)
- Modals and drawers always use Level 4 — they are above everything
- Never add shadow to elements that are already inside a shadow container (no shadow-on-shadow)
- Avoid `filter: drop-shadow()` for UI chrome — it's for irregular shapes (SVGs, PNGs with transparency) only

---

## Responsive Images & Art Direction

### Container-First Sizing

Never let images size themselves. Every image lives in a container with explicit dimensions:

```css
.ag-img-container { display: block; position: relative; overflow: hidden; width: 100%; aspect-ratio: 16/9; border-radius: var(--card-radius); }
.ag-img-container img { display: block; position: absolute; top: 0; left: 0; width: 100%; height: 100%; object-fit: cover; }
```

### Art Direction by Breakpoint

Crop and frame images intentionally per viewport — not just scale them:

```html
<picture>
  <source media="(min-width: 1024px)" srcset="hero-wide.webp" width="1440" height="600">
  <source media="(min-width: 768px)"  srcset="hero-md.webp"   width="768"  height="500">
  <img src="hero-mobile.webp" width="390" height="600" alt="..." loading="eager" fetchpriority="high">
</picture>
```

### Rules

- Above-the-fold images: `loading="eager"`, `fetchpriority="high"`, no lazy
- Below-the-fold: `loading="lazy"`, explicit `width` and `height` to prevent layout shift
- Always provide `width` and `height` attributes matching the intrinsic ratio — even on fluid images
- Use `.webp` with `.jpg` fallback for photos; `.svg` for icons and illustrations
- Avatar images: always circular clip in CSS (`border-radius: 50%`), always have a text fallback for when image fails
- Hero images: provide 2x resolution for retina (`srcset="img.webp 1x, img@2x.webp 2x"`)

---

## Component API Design

A great component is a joy to use. A poor one accumulates workarounds.

### Guiding Principles

**Prefer composition over configuration**
Instead of a `Button` with 12 props controlling every variation, provide a small focused `Button` and let consumers compose it with `Icon`, `Badge`, `Spinner`. Fewer props, more power.

```tsx
// Avoid — configuration explosion
<Button icon="arrow" iconPosition="right" loading={true} badge={3} size="lg" variant="ghost" />

// Prefer — composition
<Button variant="ghost" size="lg">
  Continue <ArrowRight className="size-4" /> {isLoading && <Spinner />}
</Button>
```

**Boolean props for states, not variants**
States are binary. Variants are named options.

```tsx
// States — boolean
disabled?: boolean;
loading?: boolean;
selected?: boolean;

// Variants — union string
variant?: 'primary' | 'ghost' | 'danger';
size?: 'sm' | 'md' | 'lg';
```

**`children` for content, props for behaviour**
If it's visible text or nested elements, it's `children`. If it's how the component acts, it's a prop.

**Keep prop surfaces small**
A component with more than 8 props is probably doing too much. Split it.

**Never accept `style` or `className` as escape hatches on internal components**
If a consumer needs to override styles, the component API is incomplete. Fix the component.

**Forwarded refs always**
Every DOM-wrapping component forwards its ref:

```tsx
const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(({ children, ...props }, ref) => (
  <button ref={ref} {...props}>{children}</button>
));
Button.displayName = 'Button';
```

**Default props should represent the 80% case**
The most common usage should require zero configuration:

```tsx
<Button>Save</Button>  // works perfectly — primary, medium, enabled
```

### Prop Documentation Template

Every prop, every time:

```tsx
interface CardProps {
  /** Visual treatment of the card surface. Default: 'elevated' */
  variant?: 'elevated' | 'outlined' | 'ghost';
  /** Removes internal padding. Use when the card contains a full-bleed image or custom layout. */
  flush?: boolean;
  /** Elevation level. Overrides the variant default. 0–4. */
  elevation?: 0 | 1 | 2 | 3 | 4;
  /** Card content. */
  children: React.ReactNode;
}
```

---

## Copy Patterns — Templates for Common Moments

Apply voice rules consistently. These templates are starting points — always adapt to the product's specific context.

### Confirmation Dialogs (destructive actions)

```
[Title]   Delete [item name]?
[Body]    This removes it permanently. You can't undo this.
[Actions] [Cancel]  [Delete]          ← destructive action always on the right, never the default focus
```

### Permission Requests

```
[Title]   Allow access to your calendar
[Body]    We'll use this to schedule reminders at the right time.
          We never share your calendar data.
[Actions] [Not now]  [Allow access]
```

### Destructive Action Warnings (inline, not modal)

```
Heads up — this will remove all [X] from [Y]. This can't be undone.
[Cancel]  [Yes, remove all]
```

### Success States

Specific, not generic:
- "Project created — you're ready to invite your team."
- "12 records imported. 1 had an issue — [review it]."
- "Password changed. You're signed in on all your devices."

### Progress Labels

```
Uploading your file… (2 of 5)
Almost there — processing the last few rows
Finishing up…
```

### Tooltip Copy Rules

- Max 10 words
- No punctuation at the end unless it's a question
- Describe the outcome, not the mechanism: "Export to CSV" not "Triggers CSV download function"
- Never tooltip a button that already has a visible label — redundant

### Empty States — Formula

```
[Icon — large, tonal, not black]
[Headline — 4 words max, present tense: "No projects yet"]
[Body — one sentence: what happens when this is populated, or why it's empty]
[CTA — single verb: "Create your first project"]
[Optional secondary link: "Learn more about projects →"]
```

### Onboarding Step Copy

```
[Step indicator: "Step 1 of 3"]
[Headline: verb-first — "Connect your data source"]
[Body: outcome-focused — "Once connected, your dashboard updates automatically."]
[CTA: "Connect now"]
[Skip: "I'll do this later"]
```

---

## Design Audit Checklist (mid-process, before delivery)

Run this before the final delivery checklist. It catches quality issues that rules alone miss.

Ask yourself each question honestly:

**Does it look designed or generated?**
- Is there at least one layout decision that surprises? (asymmetry, unexpected scale, unconventional hierarchy)
- Is every spacing value intentional, or did defaults accumulate?
- Does the colour palette feel chosen for *this* product, or could it belong to any product?

**Does it feel alive?**
- Do interactive elements respond immediately and satisfyingly?
- Is there at least one moment of micro-delight? (a satisfying button press, a smooth reveal, a pleasing empty state)
- Do transitions feel physical — like things have weight and momentum?

**Is it actually usable?**
- Can the primary task be completed in the fewest possible steps?
- Does the UI work at 320px without horizontal scroll or obscured content?
- Is there a clear visual hierarchy — can you identify the most important element in 2 seconds?
- Does every interactive element look interactive? (affordance)

**Is it accessible?**
- Does it work with keyboard-only navigation?
- Is there sufficient contrast in both light and dark modes?
- Are all images, icons, and controls labelled for screen readers?

**Is the copy doing its job?**
- Does every empty state have a CTA?
- Is every error message actionable?
- Is the voice consistent — does it sound like one person wrote it?

If any answer is "no" or "I'm not sure" — fix it before delivery.

---
