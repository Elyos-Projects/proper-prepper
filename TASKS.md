# TASKS — proper-prepper (offline-first disaster-prep open toolkit)

> Status: Draft · Version: 0.2.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

## How these tasks map to Elyos

Each task below becomes an Elyos **Task JSON** validated against
`packages/schema/src/schemas.ts`. Field mapping:

- `id` — stable slug ID, e.g. `proper-prepper-pwa-001`.
- `title` — the task title from the table.
- `project` — `"proper-prepper"`.
- `type` — one of `code | research | writing | data | design-spec | maintenance` (the "Type" column).
- `lane` — `"donated"` for all tasks here (no escrow/API spend). A `funded` task would require
  `fundedBudgetUsd`.
- `priority` — `high | medium | low`.
- `domain` — array, e.g. `["public-safety","software","accessibility","translation"]`.
- `riskTier` — `low | medium | high` (the "Risk" column). Content/safety tasks are **medium**.
- `urgent` — boolean (default `false`; this is preparedness, not live response).
- `deliverable` — `pr | dataset | document | translation` (the "Deliverable" column).
- `tokenEstimate` — `small | medium | large` (the "Size" column).
- `status` — `open | in-progress | review | delivered | done` (all start `open`).
- `context`, `objective`, `acceptanceCriteria[]`, `output` — task narrative + checkable criteria.
- `resources[]` — source/reference URLs or repo paths.
- `requestor` — partner/requestor (**TO BE SECURED**; use `"TBD"` until a partner is named).
- `verifiedNeed` — **`false` until a named partner org confirms need in writing** (honest default).
- `outputLicense` — `"MIT"` for code, `"CC-BY-4.0"` for content/translations.

**Reviewer column legend:** `maintainer` (code review), `a11y` (accessibility reviewer),
`content/SME` (domain + emergency-management subject-matter expert for medium-risk safety content),
`translation+safety` (competent speaker paired with safety-accuracy re-check).

**Content review rule (all `content/SME` tasks):** the unit's **author may not approve their own
work** — `reviewedBy` (author) and `approvedBy` (SME) must be distinct people, and the approving SME
must meet the credential bar (current/former emergency-management or civil-protection professional,
licensed first responder, or equivalent recognized qualification). Each approval writes a **PR-tied,
append-only review-log entry** (PR #, commit SHA, `contentVersion`, reviewer + approver, sources
checked, any source-divergence decision). This is the auditable record the Definition of Shipped
checks against.

**Sequencing rule (M0):** ADR #3 (content format) is decided in **arch-002 before** the content
schema (**data-004**) and any content precaching are finalized; until then the schema is provisional
and format-agnostic. data-004 therefore depends on arch-002 (consistent below).

---

## Milestone M0 — Foundation & cold-start

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| proper-prepper-pwa-001 | Installable PWA skeleton with offline service-worker precache | code | medium | low | pr | — | maintainer |
| proper-prepper-arch-002 | ADRs: UI framework (i18n/RTL scope as input), SW tooling + precache budget/eviction + content-version manifest, **content format (decide first)**, hosting | design-spec | small | low | document | — | maintainer |
| proper-prepper-ci-003 | CI gates: lint, typecheck, unit, axe a11y, offline smoke E2E, no-telemetry audit (CSP `connect-src 'none'` + runtime network-interception E2E) | code | medium | low | pr | pwa-001 | maintainer |
| proper-prepper-data-004 | Content-unit schema + provenance/review-log format | data | small | low | dataset | arch-002 | maintainer, content/SME |
| proper-prepper-content-005 | Sample hazard module (flood), source-cited to official guidance | writing | small | medium | document | data-004 | content/SME |

**Acceptance criteria — key tasks**

- **proper-prepper-pwa-001 (PWA skeleton):**
  - App registers a service worker that precaches the app shell; loads after first visit with the
    network fully disabled.
  - Web app manifest present; app is installable (passes Lighthouse PWA installability).
  - Explicit update flow (prompt or controlled reload); offline fallback page exists.
  - Precache stays within the budget set in arch-002; eviction is LRU over non-active-locale units
    only, with the active locale's hazard set + fallback pinned; `QuotaExceededError` is handled
    gracefully (prune-and-retry, then a clear notice — never silent drop of pinned safety content).
  - No third-party trackers/analytics; no network egress beyond initial load/update check, enforced
    by CSP `connect-src 'none'`.
  - TypeScript/ESM, builds via pnpm; `pnpm build && pnpm test && pnpm lint` pass.
- **proper-prepper-ci-003 (CI gates):**
  - CI fails on lint/type/unit errors, on any critical axe violation, and on offline E2E failure.
  - A "no-telemetry/no-PII" check uses defense in depth: the static audit (tracker/analytics/PII
    detection) **plus** a runtime network-interception E2E that exercises every core flow and fails
    on any unexpected outbound request; CSP `connect-src 'none'` is asserted. A static grep alone is
    not sufficient to pass.
- **proper-prepper-data-004 (content schema):**
  - Schema captures hazard id, localized fields, region applicability (incl. region overrides),
    `sources[]` (org/title/url/retrievedDate/sourceLicense), `reviewStatus`, `reviewedBy` (author),
    `approvedBy` (distinct SME), `reviewLogRef`, `lastReviewed`, `lang`, `contentLicense`,
    `contentVersion`, `integrityHash`, `hardInvalidate`.
  - Format follows the content-format ADR (arch-002), which lands first; schema is provisional until
    that ADR is decided.
  - Build-time validation rejects a unit missing required citation/review fields **and** rejects a
    unit where `approvedBy` equals `reviewedBy` (no self-approval).
- **proper-prepper-content-005 (flood sample):**
  - Every claim is traceable to a cited official source (FEMA/Red Cross/WHO/local), with retrieval
    date and source license recorded.
  - No high-stakes medical/legal/weapons instruction; links out where appropriate.
  - Where official sources diverge, the source-divergence precedence rule is applied (region
    jurisdiction first; WHO for health / FEMA·Red Cross·IFRC for physical hazards globally; state and
    link both where they still materially disagree) and the decision is recorded in the review log.
  - Passes content/SME accuracy review before merge by an approver **distinct from the author** who
    meets the SME credential bar; a PR-tied review-log entry is written; no verbatim copying of
    copyrighted source text.

**Definition of Done (M0):** Installable PWA loads fully offline after first visit; CI enforces
lint/type/unit/a11y/offline/no-telemetry gates; content schema + provenance/review-log defined; ADRs
recorded; one source-cited, SME-reviewed hazard module renders from the content schema; telemetry/PII
audit is green.

---

## Milestone M1 — Core features & accessibility hardening

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| proper-prepper-feat-006 | Interactive checklists + household emergency-plan builder (on-device storage) | code | large | low | pr | pwa-001, data-004 | maintainer, a11y |
| proper-prepper-feat-007 | Go-bag generator + printable/exportable output (client-side, no server) | code | medium | low | pr | feat-006 | maintainer, a11y |
| proper-prepper-feat-008 | "What to do now" decision flows (before/during/after) | code | medium | medium | pr | content-005 | maintainer, content/SME |
| proper-prepper-a11y-009 | WCAG 2.2 AA hardening + manual assistive-tech audit | code | medium | low | pr | feat-006, feat-007 | a11y |
| proper-prepper-content-010 | Second + third hazard modules (e.g. wildfire, power outage), source-cited | writing | medium | medium | document | content-005 | content/SME |

**Acceptance criteria — key tasks**

- **proper-prepper-feat-006 (checklists + plan builder):**
  - Fully usable offline; user-entered plan data persists locally (IndexedDB/localStorage) and never
    leaves the device (verified: no network egress).
  - Keyboard-operable, screen-reader-labeled, AA contrast; no critical axe violations.
- **proper-prepper-feat-008 ("what to do now" flows):**
  - Flow content is source-cited and within scope (no medical/legal/weapons instruction; links out).
  - Decision steps reviewed by content/SME for safety accuracy.
- **proper-prepper-a11y-009 (AA hardening):**
  - Automated axe/pa11y pass with 0 critical issues across all core flows.
  - Documented manual audit across the defined support matrix — NVDA+Firefox/Win, JAWS+Chrome/Win,
    VoiceOver+Safari/macOS·iOS, TalkBack+Chrome/Android, plus keyboard-only per desktop browser —
    performed/signed off by the qualified accessibility reviewer; cadence is before each milestone
    exit and each production release, with regression passes on navigation/focus/interaction changes;
    issues fixed or logged.

**Definition of Done (M1):** All core flows (checklists, plan builder, go-bag, what-to-do-now,
printable export) work offline and meet WCAG 2.2 AA (automated + manual); on-device data confirmed to
stay local; ≥ 3 hazards content-complete and SME-reviewed.

---

## Milestone M2 — i18n & first localization

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| proper-prepper-i18n-011 | i18n framework (RTL, bundled offline fonts, ICU pluralization, locale-format), message extraction, locale negotiation + safe fallback | code | medium | low | pr | feat-006 | maintainer, a11y |
| proper-prepper-i18n-012 | Translation completeness check in CI | code | small | low | pr | i18n-011 | maintainer |
| proper-prepper-l10n-013 | First non-English UI localization (Spanish/es, provisional; partner may override) | writing | medium | medium | translation | i18n-011 | translation+safety |
| proper-prepper-l10n-014 | Localize ≥ 1 hazard module into the target language (safety-accuracy reviewed) | writing | medium | medium | translation | l10n-013, content-010 | translation+safety, content/SME |

**Acceptance criteria — key tasks**

- **proper-prepper-i18n-011 (i18n framework):**
  - Strings externalized to message catalogs; locale negotiation falls back safely to English when a
    key/locale is missing (no blank/broken safety text).
  - Supports bidirectional/RTL layout (logical CSS, `dir`, mirrored components), bundled offline
    fonts with required glyph coverage (no runtime web-font fetch), ICU pluralization/select, and
    locale-aware number/date/list/unit formatting — these were settled as input to the UI-framework
    ADR (arch-002).
  - i18n layer works offline (catalogs + fonts precached).
- **proper-prepper-l10n-014 (localized hazard):**
  - Translation is accuracy-reviewed for safety meaning, not only fluency, against the source unit
    and official guidance in that language where available.
  - Source license/attribution rules respected for derivative content; `contentLicense` = CC-BY-4.0.

**Definition of Done (M2):** i18n framework with build-time completeness check in CI; ≥ 1 non-English
locale complete for UI and ≥ 1 hazard module; safe fallback verified; translated safety content
passes accuracy review.

---

## Milestone M3 — Content depth & partner adoption

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| proper-prepper-partner-015 | Secure named partner emergency-management org (need, hazards, languages, endorsement) | research | medium | medium | document | — | maintainer, steward |
| proper-prepper-content-016 | Expand to ≥ 6 reviewed hazard modules (earthquake, storm, heatwave, …) | writing | large | medium | document | content-010 | content/SME |
| proper-prepper-l10n-017 | Reach ≥ 3 fully localized languages (UI + key hazards) | writing | large | medium | translation | l10n-014 | translation+safety |
| proper-prepper-deploy-018 | Production deployment + versioned cache/update strategy + disclaimer/branding | code | medium | low | pr | a11y-009, i18n-012 | maintainer, steward |
| proper-prepper-ops-019 | Outcomes-tracking process (partner self-report) + content re-review cadence | maintenance | small | low | document | partner-015 | maintainer, steward |

**Acceptance criteria — key tasks**

- **proper-prepper-partner-015 (secure partner):**
  - A named emergency-management/civil-protection org confirms the need in writing (letter of
    support or MOU) and identifies priority hazards and languages.
  - On success, `verifiedNeed` flips to `true` and `requestor` is set to the named org across the
    project's tasks.
- **proper-prepper-deploy-018 (production deploy):**
  - Static production build deployed over HTTPS; versioned precache driven by the content-version
    manifest, with a soft update prompt for routine changes **and** a non-dismissible hard-invalidation
    path that purges and re-fetches any safety-corrected unit, so users are never stuck on stale
    safety content.
  - Visible disclaimer that the tool is not an official agency product unless a named partner
    endorses it; trademark/attribution terms present.
- **proper-prepper-ops-019 (outcomes + freshness):**
  - Documented, privacy-preserving outcomes process (partner self-report; no in-app telemetry) using
    a standard template (partner, period, channels, **distinct households reached (confirmed)**,
    flagged estimates, regions/languages, de-dup notes) over a rolling 12-month window; counts
    distinct households (not downloads/page views), counts a household once per period across
    touchpoints, and de-duplicates across partners at roll-up.
  - Re-review cadence defined; stale-content flagging based on `lastReviewed`.

**Definition of Done (M3):** ≥ 6 reviewed hazards; ≥ 3 localized languages; **named partner
endorsement/adoption on file** (verifiedNeed = true); production build deployed with safe update
strategy; outcomes tracking + re-review cadence operational. This satisfies the project-level
*Definition of Shipped (partner-adopted)*.

**Decision point (so a finished toolkit isn't stranded):** if no partner is secured by **6 months
after the M3 production build is ready**, the steward + Elyos governance declare **"Publicly Shipped
(generic public good)"** — criteria (1)–(4) met, deployed and distributed directly/via community
channels, outcomes tracked by best-effort self-report. This is a recognized success state; a later
partner endorsement upgrades the status to "partner-adopted" rather than re-opening launch.

---

## Backlog / future

| ID | Title | Type | Size | Risk | Deliverable | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| proper-prepper-feat-020 | Region-aware hazard relevance (advisory, no geolocation/PII) | code | medium | low | pr | User picks region; no tracking |
| proper-prepper-feat-021 | Native app-store wrappers (TWA/Capacitor) | code | large | low | pr | Out of launch scope |
| proper-prepper-content-022 | Accessible "easy-read"/low-literacy content variants | writing | medium | medium | document | a11y + SME review |
| proper-prepper-content-023 | Pictograph/icon set for low-literacy & multilingual users | design-spec | medium | low | document | License-clean assets only |
| proper-prepper-feat-024 | Caregiver / household-with-disabilities tailored checklists | writing | medium | medium | document | SME review |
| proper-prepper-ops-025 | Annual content re-validation pass against updated official guidance | maintenance | medium | medium | document | Recurring |
| proper-prepper-sec-026 | Dependency/supply-chain audit + SRI hardening | maintenance | small | low | pr | Recurring |

---

## Example task JSON

Complete, schema-valid Task JSON for the first M0 task. `verifiedNeed` is `false` and `requestor`
is `"TBD"` because **no partner org is secured yet** (honest default per the plan).

```json
{
  "id": "proper-prepper-pwa-001",
  "title": "Installable PWA skeleton with offline service-worker precache",
  "project": "proper-prepper",
  "type": "code",
  "lane": "donated",
  "priority": "high",
  "domain": ["public-safety", "software", "accessibility"],
  "riskTier": "low",
  "urgent": false,
  "deliverable": "pr",
  "tokenEstimate": "medium",
  "status": "open",
  "context": "proper-prepper is an offline-first, installable open-source PWA giving households calm, evidence-based disaster-preparedness tools that keep working with no connectivity. This task lays the thin M0 foundation: an installable app shell with a service-worker offline cache and the privacy/no-telemetry posture baked in from day one.",
  "objective": "Create a TypeScript/ESM PWA skeleton that installs, registers a service worker precaching the app shell, loads fully offline after first visit, and ships no telemetry or trackers.",
  "acceptanceCriteria": [
    "Service worker precaches the app shell; the app loads with the network fully disabled after first visit",
    "Web app manifest present and the app passes Lighthouse PWA installability checks",
    "Explicit update flow (update prompt or controlled reload) and an offline fallback page exist",
    "No third-party analytics/trackers and no network egress beyond initial load and update check",
    "TypeScript/ESM project builds with pnpm; pnpm build && pnpm test && pnpm lint all pass"
  ],
  "resources": [
    "C:\\code\\elyos\\planning\\projects\\proper-prepper\\PLAN.md",
    "C:\\code\\elyos\\governance\\proposals\\proper-prepper.md",
    "https://web.dev/learn/pwa/"
  ],
  "output": "A pull request adding the installable, offline-capable PWA skeleton (service worker, manifest, app shell, update flow) with CI-ready build.",
  "requestor": "TBD",
  "verifiedNeed": false,
  "outputLicense": "MIT"
}
```
