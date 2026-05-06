# Feature Specification: Neighborhood Party Request Form

**Feature Branch**: `claude/show-installed-skills-H4S0i`
**Created**: 2026-05-06
**Status**: Draft
**Input**: User description: "Formulaire pour demander l'organisation d'une fête de quartier — collecte d'infos (adresse, organisateur, email), validation email, code de suivi remis à l'organisateur, persistance via service tiers, transitions de statut manuelles côté back-office, notifications email à chaque changement de statut."

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Submit a party request and receive a tracking code (Priority: P1)

A resident wants to organize a neighborhood party. They visit the public site, fill in a form
with their name, email, the address of the planned event, the date and time, the expected
number of attendees, and a short description. On submit they see a confirmation screen
showing a unique tracking code they can use later to check progress.

**Why this priority**: Without this, there is no product. Capturing the request and giving the
requester a way to come back is the irreducible MVP.

**Independent Test**: Open the form, submit it with valid data, observe that a tracking code is
displayed and the request is visible in the operator's persistence backend.

**Acceptance Scenarios**:

1. **Given** the form is loaded, **When** the user submits valid name, email, address, date,
   time, attendees, and description, **Then** a confirmation screen is shown with a unique
   tracking code and the request is persisted with state "Pending email validation".
2. **Given** the form is loaded, **When** the user submits with an invalid email format,
   **Then** the form blocks submission and shows an inline validation error.
3. **Given** the form is loaded, **When** the user submits with a required field empty,
   **Then** the form blocks submission and indicates which field is missing.

---

### User Story 2 - Validate email ownership (Priority: P1)

After a request is submitted, the requester receives an email with a validation link. Clicking
the link confirms they control that mailbox; the request then moves into the review queue.

**Why this priority**: Without email validation, anyone can submit on someone else's behalf and
the operator cannot trust the contact channel. Required for MVP alongside submission.

**Independent Test**: Submit a request, receive the validation email, click the link, observe
the request moves to "Pending review".

**Acceptance Scenarios**:

1. **Given** a request was just submitted, **When** the system processes the submission,
   **Then** a validation email is sent to the address on the request within 5 minutes.
2. **Given** the validation email has been received, **When** the requester opens the validation
   link, **Then** the request transitions from "Pending email validation" to "Pending review"
   and the requester sees a confirmation page.
3. **Given** the validation link has already been used or has expired, **When** the link is
   opened, **Then** a clear message explains the link is no longer valid and offers next steps.

---

### User Story 3 - Check the status of a submitted request (Priority: P2)

The requester wants to know where their request stands. They return to the site, enter their
tracking code on the status page, and see the current state of their request (Pending email
validation / Pending review / Accepted / Rejected).

**Why this priority**: Useful and reassuring, but the request can still be processed and
notified by email without the lookup page existing.

**Independent Test**: Submit a request, note the tracking code, navigate to the status page,
enter the code, observe the correct state is shown.

**Acceptance Scenarios**:

1. **Given** a valid tracking code, **When** the requester enters it on the status page,
   **Then** the page shows the current state of that request and no information about other
   requests.
2. **Given** an unknown or malformed tracking code, **When** it is submitted on the status
   page, **Then** a generic "no request found" message is shown (no enumeration possible).

---

### User Story 4 - Be notified when the request status changes (Priority: P2)

When the operator changes a request's state in the back-office (Pending review → Accepted or
Rejected), the requester receives an email summarizing the new state.

**Why this priority**: Improves the experience but is not strictly required for an MVP demo —
the requester can still poll the status page.

**Independent Test**: Submit and email-validate a request, have the operator transition the
state in the persistence backend, observe an email arrives at the requester's address within
5 minutes describing the new state.

**Acceptance Scenarios**:

1. **Given** a request is in "Pending review", **When** the operator marks it "Accepted",
   **Then** the requester receives an email confirming acceptance within 5 minutes.
2. **Given** a request is in "Pending review", **When** the operator marks it "Rejected",
   **Then** the requester receives an email indicating the rejection within 5 minutes.

---

### Edge Cases

- The same email submits multiple requests — each is treated as an independent request with
  its own tracking code; no automatic deduplication in v1.
- The validation link is opened a second time after success — the page shows the current state
  rather than re-validating.
- The operator skips the "Pending review" state and goes directly from "Pending email
  validation" to a terminal state — disallowed by the workflow; the operator MUST wait for
  email validation before transitioning to Accepted/Rejected.
- A rejected requester resubmits — allowed; a new request with a new tracking code is created.
- The third-party persistence or email service is temporarily unavailable — the form shows a
  user-friendly error and the requester is invited to retry later.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: System MUST provide a public web form to submit a neighborhood party request.
- **FR-002**: The form MUST collect at minimum: organizer full name, organizer email, event
  postal address, event date, event start time and expected duration, expected number of
  attendees, and a free-text description.
- **FR-003**: The form MUST validate email format and reject submissions with a malformed
  email before they are persisted.
- **FR-004**: System MUST generate a unique tracking code per request (alphanumeric, 8
  characters, case-insensitive) and display it to the requester on the confirmation screen.
- **FR-005**: System MUST persist each submitted request in a managed third-party service in
  state "Pending email validation".
- **FR-006**: System MUST send a validation email to the requester containing a one-time link
  that, when opened, transitions the request to "Pending review".
- **FR-007**: Validation links MUST expire after 7 days; expired or already-used links MUST
  show a clear message and not silently fail.
- **FR-008**: An operator MUST be able to transition a request from "Pending review" to
  "Accepted" or "Rejected" by editing the persistence service directly.
- **FR-009**: End-users MUST NOT be able to modify request state through the public site;
  state is read-only from the public surface.
- **FR-010**: System MUST provide a public status-lookup page that accepts a tracking code and
  returns the current state of the matching request, or a generic "not found" message.
- **FR-011**: System MUST send the requester an email when their request transitions to
  "Pending review", "Accepted", or "Rejected".
- **FR-012**: All user-facing text (form labels, confirmation pages, emails) MUST be in
  French.

### Key Entities

- **Request**: organizer name, organizer email, event address, event date, event start time,
  expected duration, expected attendees, description, tracking code, state, created-at,
  updated-at.
- **Email validation token**: opaque single-use token tied to one request, expires 7 days
  after issuance.
- **Request state**: one of "Pending email validation", "Pending review", "Accepted",
  "Rejected".

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: A requester can submit a complete request and receive a tracking code in under
  2 minutes.
- **SC-002**: 95% of validation emails arrive at the requester's inbox within 5 minutes of
  submission.
- **SC-003**: 99% of valid tracking codes return the correct current state on the status page
  on the first lookup.
- **SC-004**: 100% of operator-initiated state transitions trigger a notification email to
  the requester within 5 minutes.
- **SC-005**: Zero instances of an end-user modifying a request's state through the public
  site (verified by audit of the persistence service).
- **SC-006**: The site is reachable as static assets only — no project-operated server
  required for any user-facing flow.

## Assumptions

- The list of form fields beyond what the user explicitly mentioned (date, time, attendees,
  description) is inferred from common neighborhood-event request forms; this can be revised
  during clarification.
- Tracking code length and format (8 alphanumeric, case-insensitive) is a reasonable default
  balancing uniqueness, ease of typing, and unguessability.
- Validation links expire after 7 days as an industry-standard window.
- Rejected requesters may submit a new request; there is no permanent block.
- The operator is a single trusted role in v1; no multi-role permission model is required.
- Rate limiting and basic spam protection are provided by the chosen third-party form-handling
  service rather than implemented in-app.
- The third-party services chosen for persistence and email delivery offer free tiers
  sufficient for the expected request volume (a handful of requests per week per
  neighborhood).
