# Implementation Plan: Contact Section

**Branch**: `claude/speckit-implementation-v64Si` | **Date**: 2026-05-06 | **Spec**: [spec.md](./spec.md)
**Input**: Feature specification from `specs/001-contact-section/spec.md`

## Summary

Add a contact section to the existing static homepage (`index.html` / `style.css`) that displays three labelled links — GitHub, LinkedIn, and email — in a visually consistent, accessible, and responsive layout. No build toolchain or JavaScript is required.

## Technical Context

**Language/Version**: HTML5, CSS3
**Primary Dependencies**: None (static files, no frameworks)
**Storage**: N/A
**Testing**: Manual browser testing (desktop + mobile viewport simulation)
**Target Platform**: GitHub Pages (static site, any modern browser)
**Project Type**: Static web page
**Performance Goals**: Page renders in under 1 second on a standard connection (no additional assets added)
**Constraints**: No JavaScript; must not alter existing page layout or styles; placeholder contact values must be easy to update
**Scale/Scope**: Single-page site, one new HTML section + associated CSS rules

## Constitution Check

No constitution rules are in force (the constitution.md is a template). No gates to check.

## Project Structure

### Documentation (this feature)

```text
specs/001-contact-section/
├── spec.md
├── plan.md               ← this file
├── checklists/
│   └── requirements.md
└── tasks.md              ← created by /speckit-tasks
```

### Source Code (repository root)

```text
index.html    ← add <section id="contact"> below existing <main>
style.css     ← add contact section styles
```

No new files are needed. All changes are additive to the two existing source files.

## Implementation Phases

### Phase 1 — HTML Structure

Add a `<section id="contact">` element to `index.html` after the closing `</main>` tag. The section contains:
- A heading (`<h2>`) with the label "Contact"
- An unordered list (`<ul class="contact-links">`) with three `<li>` items, each an `<a>` tag with `rel="noopener noreferrer"` and `target="_blank"` (except the `mailto:` link which omits `target`).
- Use Unicode / HTML entity icons (✉ for email, ⭐ or a text label for GitHub/LinkedIn) so no icon library is needed.

### Phase 2 — CSS Styling

Extend `style.css` to style `#contact` and `.contact-links` to:
- Match the existing page palette (white text on the gradient background).
- Centre the content.
- Display links as pill-shaped buttons with hover/focus states.
- Ensure touch targets ≥ 44×44 px.
- Remain legible at 320 px viewport width.

## Complexity Tracking

No constitution violations to justify.
