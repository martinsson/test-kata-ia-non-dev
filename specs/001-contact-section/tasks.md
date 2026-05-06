# Tasks: Contact Section

**Input**: Design documents from `specs/001-contact-section/`
**Prerequisites**: plan.md ✓, spec.md ✓

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to

---

## Phase 1: HTML Structure (User Story 1 — P1)

**Purpose**: Add the contact section markup to `index.html`

- [X] T001 [US1] Add `<section id="contact">` with heading and three contact links to `index.html` (after `</main>`)
  - Heading: `<h2>Contact</h2>`
  - GitHub link: `<a href="https://github.com/votre-utilisateur" target="_blank" rel="noopener noreferrer">`
  - LinkedIn link: `<a href="https://linkedin.com/in/votre-profil" target="_blank" rel="noopener noreferrer">`
  - Email link: `<a href="mailto:votre@email.com">`
  - Each link labelled with an inline icon (✉ / ⌥ / text) and descriptive text

---

## Phase 2: CSS Styling (User Story 1 + User Story 2)

**Purpose**: Style the contact section to match the existing page and be responsive

- [X] T002 [P] [US1] Add `#contact` base styles to `style.css`
  - White text, centred layout, padding top/bottom
  - Separator line above the section
- [X] T003 [P] [US2] Add `.contact-links` responsive styles to `style.css`
  - Flex row wrapping to column on narrow screens
  - Pill-shaped link buttons with hover/focus states
  - Minimum 44×44 px touch target for each link

---

## Phase 3: Validation

**Purpose**: Confirm all success criteria are met

- [X] T004 [US1] Verify all three links open correctly in browser (GitHub/LinkedIn in new tab, email opens mail client)
- [X] T005 [US2] Verify layout at 320 px, 768 px, and 1440 px viewport widths (no overflow, readable text)
- [X] T006 [US1] Verify section is visible without JavaScript enabled
