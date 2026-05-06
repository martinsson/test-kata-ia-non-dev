<!--
SYNC IMPACT REPORT
Version change: 1.0.0 → 2.0.0
Bump rationale: MAJOR — principles redefined for the project's true scope (public-data
  visualization site with email reports), replacing the placeholder personal-homepage
  principles that were drafted before requirements were elicited.
Modified principles:
  - I. Static-First (No Build Toolchain) — kept, scope clarified ("zero build deps", CDN allowed)
  - II. Accessibility by Default → REMOVED from core (folded into Quality Standards section)
  - III. Progressive Enhancement (No-JS Baseline) → REMOVED (JS is required for charts)
  - IV. Responsive & Mobile-Friendly → REMOVED from core (folded into Quality Standards)
  - V. Visual & Stylistic Consistency → REMOVED from core (folded into Quality Standards)
Added principles:
  - II. Public Data via Runtime API Fetch
  - III. Email Reports via Third-Party Form Service
  - IV. Charting via CDN (No Bundlers)
  - V. Spec-First for New Features
Added sections:
  - Quality Standards (accessibility, responsiveness, visual consistency)
Removed sections: none (Technical Constraints + Development Workflow + Governance retained)
Templates requiring updates:
  - ✅ .specify/templates/plan-template.md (generic Constitution Check gate remains valid)
  - ✅ .specify/templates/spec-template.md (no changes required)
  - ✅ .specify/templates/tasks-template.md (no principle-driven category changes)
  - ⚠ .specify/templates/commands/ — directory not present in this project; nothing to update
  - ✅ CLAUDE.md (no principle references; defers to active plan)
Deferred TODOs: none
-->

# Donnees de la Population Visualisées Constitution

## Core Principles

### I. Static-First (No Build Toolchain)

The site MUST remain a set of plain static files (HTML, CSS, JS, optional images)
servable directly from GitHub Pages. No build steps, bundlers, transpilers,
package managers, lockfiles, or server-side runtimes may be introduced. "Zero
build dependencies" means no `package.json`, no `npm install`, no `yarn`, no
webpack/vite/rollup. Runtime scripts MAY be loaded from CDNs (see Principle IV).
Rationale: keeps the project trivial to clone, host, and maintain; eliminates
toolchain rot for a non-developer maintainer.

### II. Public Data via Runtime API Fetch

All visualised data MUST be fetched at runtime from a publicly accessible HTTP
API using the browser's `fetch` API. Data files MUST NOT be bundled into the
repo as a substitute for the live source. Every API call MUST handle the loading
state, the empty/no-result state, and the error state explicitly in the UI.
Rationale: keeps the site authoritative and current without manual data updates,
and ensures users always see the freshest population data.

### III. Email Reports via Third-Party Form Service

Email reports MUST be sent via a third-party form service (e.g., Formspree,
EmailJS, Web3Forms) configured to deliver to the owner's address. The site MUST
NOT ship credentials or SMTP secrets, and MUST NOT attempt to send mail from
client-side code outside such a service. The chosen service MUST be reachable
without authentication from the browser, and the form MUST display submission
success and failure clearly to the user. Rationale: GitHub Pages cannot run
server code; this is the only safe, maintainable path for outbound email.

### IV. Charting via CDN (No Bundlers)

Chart and visualization libraries (e.g., Chart.js, D3, Plotly) MUST be loaded
from a public CDN via `<script>` tags pinned to a specific version (no
floating `@latest`). Self-hosting a library file in the repo is also acceptable.
Adding a build step or package manager to install a charting library is
forbidden (see Principle I). Rationale: lets the site use mature visualization
tools without compromising the static-first constraint.

### V. Spec-First for New Features

Every new user-facing feature MUST start with a `spec.md` under
`specs/<NNN>-<slug>/`, followed by a `plan.md` (with a Constitution Check) and a
`tasks.md`, generated via the Spec Kit slash commands. Small fixes (typos,
copy edits, single-rule CSS tweaks, dependency version bumps) MAY be committed
directly without a spec. When in doubt, write the spec. Rationale: aligns the
team on intent before implementation while not adding ceremony to trivial work.

## Technical Constraints

- **Hosting**: GitHub Pages. No server-side code, no edge functions.
- **Build**: None. Source files at the repo root are served as-is.
- **Runtime dependencies**: External CDN scripts and the third-party form
  service are permitted; no other external services may be introduced without
  an amendment to this constitution.
- **Secrets**: No API keys or tokens may be committed to the repo or embedded
  in client-side code. If an API requires a key, choose a different API.
- **Browser support**: Latest two stable versions of Chrome, Firefox, Safari,
  and Edge.
- **Performance**: Initial render (excluding chart data fetch) MUST stay under
  2 seconds on a standard broadband connection.

## Quality Standards

These standards apply to every feature and are verified during plan and
implementation reviews:

- **Accessibility**: Semantic HTML elements, keyboard-operable controls,
  descriptive link/button text, sufficient colour contrast, and touch targets
  of at least 44×44 px. Charts MUST expose an accessible alternative (data
  table, ARIA label summary, or downloadable CSV) for screen-reader users.
- **Responsiveness**: Layouts MUST render legibly without horizontal scrolling
  from 320 px to 1440 px viewport width.
- **Visual consistency**: New sections MUST reuse the existing palette,
  typography, and spacing scale defined in `style.css`. Introducing a new
  primitive requires explicit justification in the feature plan.
- **Customization persistence**: User display preferences (chart type, filters,
  etc.) SHOULD persist across reloads via `localStorage` unless the feature
  spec explicitly opts out.

## Development Workflow

- **Spec-driven** (per Principle V): features go through spec → plan → tasks
  before implementation. Spec Kit auto-commit hooks (`speckit.git.commit`) MAY
  be used between phases.
- **Manual verification**: Because there is no automated test suite, every
  change MUST be visually verified in at least one mobile viewport (≤ 480 px)
  and one desktop viewport (≥ 1024 px), and with the keyboard alone, before
  being marked complete.
- **API contract checks**: Any change touching the data-fetch layer MUST be
  verified against the live API (or a recorded sample) and document the
  expected response shape in the feature plan.
- **Form delivery checks**: Any change touching the email-report flow MUST be
  end-to-end tested by submitting a real test message and confirming
  receipt.

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

**Version**: 2.0.0 | **Ratified**: 2026-05-06 | **Last Amended**: 2026-05-06
