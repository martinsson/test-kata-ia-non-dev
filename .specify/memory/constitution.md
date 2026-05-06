<!--
Sync Impact Report
==================
Version change: (template) → 1.0.0
Initial ratification of the constitution from the template.

Principles (template → adopted):
- [PRINCIPLE_1_NAME]            → I. Spec-First
- [PRINCIPLE_2_NAME]            → II. Simplicity & YAGNI (NON-NEGOTIABLE)
- [PRINCIPLE_3_NAME]            → REMOVED (only 2 principles requested)
- [PRINCIPLE_4_NAME]            → REMOVED (only 2 principles requested)
- [PRINCIPLE_5_NAME]            → REMOVED (only 2 principles requested)

Sections:
- Section 2 ([SECTION_2_NAME])  → Technical Constraints
- Section 3 ([SECTION_3_NAME])  → Testing & Quality Gates
- Governance                    → Filled in

Templates reviewed:
- .specify/templates/plan-template.md   ✅ no change needed (generic; reads constitution at gate time)
- .specify/templates/spec-template.md   ✅ no change needed (no principle-specific references)
- .specify/templates/tasks-template.md  ✅ no change needed (tests already optional and gated by spec)

Follow-ups / deferred:
- None.
-->

# Demande Fête du Quartier Constitution

## Core Principles

### I. Spec-First

Every feature MUST begin with a written specification approved before any implementation work
starts. Code without a corresponding spec entry MUST NOT be merged. Specs capture intent, scope,
and acceptance criteria; ambiguity MUST be resolved in the spec (via clarification) before tasks
are generated.

**Rationale**: Spec-first prevents wasted implementation work on misunderstood requirements and
keeps non-developer stakeholders able to validate intent before code exists.

### II. Simplicity & YAGNI (NON-NEGOTIABLE)

Build only what the current spec requires. Defer abstractions, configuration options, and
infrastructure until a concrete need exists. Prefer existing third-party services over custom code
when they cover the requirement. Three similar lines is better than a premature abstraction.
Complexity that violates this principle MUST be explicitly justified in the pull request.

**Rationale**: The project has no server-hosting budget and limited maintenance capacity, so
every added moving part has a real long-term cost. Complexity, once introduced, is rarely
removed.

## Technical Constraints

- The project MUST NOT require self-hosted server infrastructure. Persistence, form handling, and
  email delivery MUST rely on managed third-party services (e.g., Formspree, or equivalent).
- Persistence MUST be configured so end-users cannot mutate authoritative state (such as the
  status of a submitted request). Status transitions are performed by an operator out-of-band
  (manual back-office edit) until a justified need for automation appears.
- Outbound email (validation, status notifications) MUST go through a managed service; no SMTP
  server is operated by the project.
- The frontend MUST be deployable as static assets only.

## Testing & Quality Gates

- Backend / domain logic MUST be covered by unit tests.
- Every release MUST run a post-deployment smoke test (e.g., a `curl` against the deployed
  endpoint) until end-to-end browser coverage exists.
- End-to-end browser coverage SHOULD migrate to Playwright once the user-facing flows stabilize.
- A change MUST NOT be marked complete until the acceptance criteria from its spec are
  demonstrated by one of the test layers above.

## Governance

This constitution supersedes ad-hoc practices. Amendments require a pull request with rationale
and a version bump per the policy below. All pull requests MUST verify compliance with the
principles and constraints; complexity that appears to violate Simplicity & YAGNI MUST be
justified in the PR description.

Versioning policy:

- **MAJOR**: backward-incompatible removal or redefinition of a principle or governance rule.
- **MINOR**: a new principle, section, or materially expanded guidance.
- **PATCH**: clarifications, wording fixes, non-semantic refinements.

**Version**: 1.0.0 | **Ratified**: 2026-05-06 | **Last Amended**: 2026-05-06
