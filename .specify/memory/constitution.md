<!--
SYNC IMPACT REPORT
Version change: (uninitialized template) → 1.0.0
Bump rationale: Initial ratification — all placeholders replaced with concrete content.
Modified principles: none (first ratification)
Added principles:
  - I. Static-First (No Build Toolchain)
  - II. Accessibility by Default
  - III. Progressive Enhancement (No-JS Baseline)
  - IV. Responsive & Mobile-Friendly
  - V. Visual & Stylistic Consistency
Added sections:
  - Technical Constraints
  - Development Workflow
  - Governance
Removed sections: none
Templates requiring updates:
  - ✅ .specify/templates/plan-template.md (verified — "Constitution Check" gate is generic and compatible)
  - ✅ .specify/templates/spec-template.md (verified — no constitution-specific sections required)
  - ✅ .specify/templates/tasks-template.md (verified — no principle-driven task categories need adjustment)
  - ⚠ .specify/templates/commands/ — directory not present in this project; no command files to update
  - ✅ CLAUDE.md (no principle references; defers to current plan)
Deferred TODOs: none
-->

# Personal Homepage Constitution

## Core Principles

### I. Static-First (No Build Toolchain)

The site MUST remain a set of plain static files (HTML, CSS, optional images) servable
directly from GitHub Pages or any static host. No build steps, bundlers, transpilers,
package managers, or server-side runtimes may be introduced. Rationale: keeps the
project trivial to clone, host, and maintain by a non-developer owner; eliminates
toolchain rot.

### II. Accessibility by Default

All interactive and informational elements MUST be usable with a keyboard, with
assistive technologies, and with sufficient colour contrast. Links and buttons MUST
have descriptive text (icons alone are not sufficient), touch targets MUST be at
least 44×44 px, and semantic HTML elements (`<header>`, `<main>`, `<section>`,
`<nav>`, headings in order) MUST be used over generic `<div>` wrappers. Rationale:
a personal page is a public front door; it must work for every visitor.

### III. Progressive Enhancement (No-JS Baseline)

Every feature MUST function with HTML and CSS alone. JavaScript MAY be added only
as a non-essential enhancement, and the page MUST remain fully usable when JS is
disabled or fails to load. Rationale: guarantees resilience, performance, and
privacy — and keeps the project aligned with Principle I.

### IV. Responsive & Mobile-Friendly

Layouts MUST render legibly and without horizontal scrolling at viewport widths
from 320 px to 1440 px. Media queries, fluid units (`rem`, `%`, `vw`), and
flex/grid layouts SHOULD be preferred over fixed pixel layouts. Rationale: the
majority of visitors arrive on mobile devices.

### V. Visual & Stylistic Consistency

New sections MUST reuse the existing palette, typography, spacing scale, and
component idioms (e.g., pill-shaped links, gradient background) defined in
`style.css`. Introducing a new colour, font, or layout primitive requires
explicit justification in the feature plan. Rationale: a personal site reads as
trustworthy when its visual language is coherent.

## Technical Constraints

- **Hosting**: GitHub Pages (or equivalent static host). No server-side code.
- **Files**: Source lives at the repo root (`index.html`, `style.css`, plus any
  asset folders). Feature work MUST prefer additive edits to these files over
  creating parallel copies.
- **Dependencies**: Zero runtime dependencies. External assets (fonts, icons)
  MUST be inlined or self-hosted to avoid third-party tracking and outages.
- **Browser support**: Latest two stable versions of Chrome, Firefox, Safari,
  and Edge. Graceful degradation on older browsers is acceptable.
- **Performance**: Initial page load MUST stay under 1 second on a standard
  broadband connection; total page weight SHOULD remain under 200 KB.

## Development Workflow

- **Spec-driven**: Each feature MUST start with a `spec.md` under
  `specs/<NNN>-<slug>/`, followed by `plan.md` and `tasks.md`, generated via the
  Spec Kit slash commands.
- **Constitution Check**: The `plan.md` "Constitution Check" section MUST
  explicitly confirm compliance with each principle above (or document an
  approved exception in the Complexity Tracking section).
- **Manual verification**: Because there is no automated test suite, every
  change MUST be visually verified at 320 px, 768 px, and 1440 px viewport
  widths, and with the keyboard alone, before being marked complete.
- **Commits**: Use small, descriptive commits per task. Spec Kit auto-commit
  hooks (`speckit.git.commit`) MAY be used to keep history aligned with
  workflow phases.

## Governance

This constitution supersedes any ad-hoc convention adopted during a single
feature. Amendments require:

1. A pull request modifying `.specify/memory/constitution.md` with a Sync
   Impact Report at the top of the file.
2. Version bump per semantic versioning:
   - **MAJOR**: removing or redefining a principle in a backward-incompatible way.
   - **MINOR**: adding a new principle or materially expanding existing guidance.
   - **PATCH**: clarifications, wording, typo fixes.
3. Updating any dependent template (`plan-template.md`, `spec-template.md`,
   `tasks-template.md`) impacted by the change in the same pull request.

Compliance is reviewed during every plan and implementation phase. Any
violation MUST be either resolved or recorded — with justification — in the
plan's Complexity Tracking section. Runtime guidance for contributors and
agents lives in `CLAUDE.md` and the active feature `plan.md`.

**Version**: 1.0.0 | **Ratified**: 2026-05-06 | **Last Amended**: 2026-05-06
