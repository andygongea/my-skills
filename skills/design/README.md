# Frontend Craft

A Claude skill for building distinctive, production-grade frontend interfaces. Covers HTML, CSS, TypeScript, React, design systems, accessibility, motion, and product design judgment in one coherent framework.

**Author:** [Andy Gongea](https://gongea.com)

---

## What it does

Frontend Craft is opinionated. It gives Claude a single coherent worldview for building UI — from the first creative brief through the final delivery checklist. The output is interfaces that feel designed, not generated: bespoke palettes, semantic markup, accessible from the start, with motion that feels like physics and copy that respects non-native speakers.

The skill activates automatically for any frontend, web app, UI component, or styling task — even when the user doesn't say "design".

## How it's structured

Four layers, in order of authority:

1. **Hard rules** — non-negotiable. Naming, CSS formatting, accessibility, validation timing, performance basics. Violations are wrong, full stop.
2. **Process** — the sequence for new work. Light-touch mode for tweaks; full process for substantial builds. Includes a creative brief template.
3. **Defaults** — what to use when the project hasn't specified. shadcn/ui, Manrope + JetBrains Mono, 4px spacing grid, named easing curves, four-tier elevation.
4. **Guidelines** — judgment calls. Aesthetic direction, information architecture, loading state choreography, error recovery, component API, voice.

Plus a **pre-delivery checklist** split into Critical (every delivery) and Comprehensive (substantial work).

## What it covers

- **Code standards** — SMACSS naming (no BEM), single-line CSS in property order, TypeScript with Airbnb style, semantic HTML, component-per-file architecture
- **Design system foundations** — bespoke palettes, component token formula, four-level elevation, named easing curves, light + dark mode, shadcn integration
- **Accessibility** — focus rings, touch targets, WCAG AA contrast, ARIA wiring, keyboard navigation, reduced-motion support
- **Motion** — easing curves as variables, six duration tiers, choreography rules, compositor-only animations
- **Product thinking** — jobs to be done, primary task paths, information architecture, bulk actions, hard-path design
- **UI patterns** — forms with proper validation timing, data tables with smart filters, two-tier toasts, empty states with CTAs
- **Voice & content** — plain language for non-native speakers, active/imperative voice, present tense, specific copy

## Installation

Download `frontend-craft.skill` and upload it via **Settings → Skills** in Claude.ai, or install it through your skill manager.

## Using with projects

The skill is portable expertise. For project-specific conventions (which namespace prefix this codebase uses, which fonts are loaded, which colour is the brand accent), drop a `CLAUDE.md` at the project root. The skill checks for project context first and only asks for what's genuinely missing.

A `CLAUDE.md` template is included alongside the skill — copy it into your project root and fill in the bracketed values.

## When to use it

**Always invoke** for:
- Building new components, pages, or screens
- Styling or polishing existing UI
- Fixing UI bugs or accessibility issues
- Designing forms, tables, or dashboards
- Any task that produces HTML, CSS, or component code

**Light-touch mode** kicks in automatically for small changes — tweaks, single-property changes, bug fixes. The full process applies to substantial work.

## Philosophy

Three commitments shape every rule in this skill:

1. **Quality over speed** — a creative brief written in two minutes prevents twenty minutes of generic output
2. **Respect for the user** — accessibility from the start, copy that works for non-native speakers, the easiest path to the primary action
3. **Honest defaults** — every choice (Manrope, shadcn, 4px grid) has a stated reason; every rule can be broken with reason; every escape hatch is documented

The skill aims to produce interfaces that look like a designer made them — not interfaces that look like an LLM made them.

## Repository

Source and updates: [https://github.com/andygongea/my-skills](https://github.com/andygongea/my-skills)

## License

Apache License 2.0. See `LICENSE.txt` for full terms.

You can use, modify, and distribute this skill freely in personal and commercial projects. If you redistribute it, include the license and note any changes.

## Feedback

Issues, suggestions, and pull requests welcome at the repo above.
