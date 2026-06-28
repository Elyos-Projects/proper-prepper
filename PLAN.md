# PLAN — proper-prepper (offline-first disaster-prep open toolkit)

> Status: Draft · Version: 0.2.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

## Executive summary

**proper-prepper** is an offline-first, installable open-source Progressive Web App (PWA) plus a
localized, accessible body of disaster-preparedness content. It gives households practical,
evidence-based readiness tools — checklists, household emergency plans, hazard explainers, and
"what to do now" flows — that keep working **with no connectivity**, which is exactly the condition
that prevails when disasters strike. The tone is calm and practical: readiness for everyone, not
doomsday marketing or commercial upsell.

The project has two coupled lanes of work: (1) **app engineering** — a TypeScript/ESM PWA with a
service-worker offline cache, full WCAG 2.2 AA accessibility, and zero telemetry/ads/paywall; and
(2) **safety content** — per-hazard × region × language preparedness units, each sourced and
review-gated against authoritative guidance (FEMA, Red Cross/IFRC, WHO, and local civil-protection
agencies).

This is a **medium risk-tier** project. Mis-stated emergency advice can cause real harm, so every
content unit must pass accuracy review against named official sources before it ships, and the
project carries a hard scope guardrail: **no high-stakes medical, legal, or weapons instruction** —
we link to official sources rather than improvise. Accessibility is a non-negotiable requirement
because emergencies disproportionately affect disabled, elderly, and low-literacy people.

**Honesty note on the partner:** the proposal asserts a verified need "per partner emergency org,"
but **no named partner organization or MOU exists yet**. Until one is secured, the partner and
verified-need status are **TO BE SECURED** and the project ships only as a generic public good. See
*Problem & beneficiaries* and *Open questions*.

## Problem & beneficiaries

**The problem.** Households are routinely under-prepared for the hazards most likely to affect them.
Existing preparedness information is fragmented across many agency websites, is frequently
inaccessible (PDF-only, English-only, poor screen-reader support), and — critically — **assumes
internet access** at the very moment connectivity is most likely to be degraded or gone. The
commercial "prepper" market fills the gap with fear-driven upsells rather than free, calm, accurate
public-good guidance.

**Who is helped (beneficiaries).**
- **At-risk households** in hazard-exposed areas (flood, wildfire, earthquake, storm, heatwave,
  power/utility outage), especially those with disabilities, limited mobility, caregiving
  responsibilities, or limited English proficiency.
- **Local emergency-management and civil-protection organizations** who need a free, brandable,
  multilingual, offline tool to hand to their communities instead of building their own.
- **Community responders / mutual-aid groups** who distribute readiness guidance at the last mile.

**Verified need / partner org: TO BE SECURED.** The proposal claims a verified need per a partner
emergency org, but no organization is named and no agreement is on file. We will treat the need as
**plausible but unverified** until at least one named emergency-management org confirms it in
writing (letter of support or lightweight MOU) and identifies the priority hazard(s) and
language(s). General preparedness need is well-documented by public agencies regardless, so M0–M1
foundation work proceeds; partner-specific endorsement is required before *Definition of Shipped* is
met.

## Goals and non-goals

**Goals.**
- Ship an installable PWA that is **fully usable offline** after first load (checklists, plans,
  hazard explainers, "what to do now" flows, printable outputs).
- Meet **WCAG 2.2 AA** as a hard gate, verified by automated + manual assistive-tech testing.
- Provide **accurate, source-cited** hazard content, each unit traceable to a named official source
  and passing accuracy review.
- Be genuinely **free and private**: MIT code / CC-BY content, no ads, no telemetry, no paywall, no
  account required, no PII collected.
- Support **internationalization** with at least one non-English localization at M2.
- Be **adoptable** by a partner emergency-management org in their community's languages.

**Non-goals.**
- Not a live alerting / early-warning system, not a real-time hazard map, not a 2-way comms tool.
- Not a medical, legal, or weapons advisory; not first-aid *instruction* beyond linking official
  guidance.
- Not a data-collection or analytics product; we will not build user accounts or profiles.
- Not a native iOS/Android app store build at launch (PWA install only; native wrappers are backlog).
- Not a commercial product or lead-gen funnel for any vendor.

## Success metrics (outcomes)

Outcome-centric, beneficiary-first. Baselines are zero at project start unless noted.

| Outcome | Baseline | Target (first 12 months post-launch) | How measured (privacy-preserving) |
| --- | --- | --- | --- |
| Partner orgs endorsing/adopting the toolkit | 0 | ≥ 1 named emergency-management org adopts; goal 3 | Signed letters of support / MOUs (manual record) |
| Hazard content units shipped & accuracy-reviewed | 0 | ≥ 6 hazards, each source-cited and signed off | Review log in repo |
| Languages fully localized (UI + shipped content) | 1 (en) | ≥ 3 languages | Translation completeness check in CI |
| Accessibility conformance | none | WCAG 2.2 AA verified; 0 critical axe violations; manual AT pass | Automated (axe/pa11y) + manual screen-reader audit |
| Offline reliability | none | 100% of core flows work with network disabled after install | Automated offline E2E test in CI |
| Households reached (partner-reported, opt-in) | 0 | ≥ 5,000 distinct households reached over the first 12 months post-launch (denominator: distinct households, not page loads or downloads) | Partner self-report via standard template (no in-app tracking) |
| Privacy posture | n/a | 0 telemetry endpoints, 0 third-party trackers, 0 PII fields | Static audit in CI + manifest review + **runtime network-interception E2E** (CSP `connect-src 'none'`; test fails on any unexpected request) |

We deliberately avoid vanity metrics (downloads alone, stars). Because we collect **no telemetry**,
reach is measured via partner self-report, not in-app analytics — an honest trade-off that
prioritizes user privacy over precise measurement.

**Reach measurement — windows, denominators, and anti-double-counting.** "Households reached" is a
count of **distinct households** (not page views, downloads, installs, or distributed leaflets) over
a **rolling 12-month window** that starts at production launch. To keep the number honest across
multiple partners and channels:
- Each partner counts a household **once per reporting period** even if it received the toolkit
  through several touchpoints (event + clinic + download), and households reached by more than one
  partner are de-duplicated by the steward at roll-up (best-effort, partner-attested, regional
  overlap noted) rather than summed naively.
- Recurring distributions (e.g. the same standing class each quarter) count the **household**, not
  each session.
- Estimates and extrapolations are recorded **as estimates**, separately from confirmed counts.

**Partner self-report template (standard fields).** Each partner reports, per period: partner name;
reporting period (start/end); channel(s) used; **distinct households reached (confirmed)**; estimated
additional reach (clearly flagged as estimate); region(s)/language(s) served; de-dup notes (known
overlap with other partners/channels); and a free-text outcomes note. The steward keeps these
reports in the manual outcomes record and publishes only de-duplicated, aggregated figures.

## Scope

**In scope.**
- Offline-first PWA shell: installable (web app manifest), service-worker precache + runtime caching,
  offline fallback, update flow.
- Core user features: interactive checklists, household emergency plan builder, go-bag generator,
  per-hazard explainers, "what to do now" decision flows, printable/exportable outputs (print CSS /
  local PDF generation, no server round-trip).
- Accessibility to WCAG 2.2 AA: semantics, keyboard, focus management, contrast, reduced-motion,
  screen-reader labels, large-text/low-literacy friendly content.
- Content pipeline: a structured content schema (hazard, region applicability, language, source
  citations, review status), authoring + validation tooling.
- i18n framework + message extraction + at least one non-English locale.
- Provenance + review log for every content unit.

**Out of scope (explicit).**
- Real-time alerts, push notifications of live hazards, sensor/IoT integration.
- Medical/first-aid *instructions*, legal advice, firearms/weapons content (link out only).
- User accounts, cloud sync, social features, or any server-side PII storage.
- Telemetry, analytics, advertising, A/B testing on users, or any monetization.
- Native app-store binaries at launch (backlog).
- Authoritative localized content for languages we cannot source-verify or review.

## Solution approach & architecture

**Overview.** A static-first PWA with all content bundled or precached, so the app is fully
functional offline. No backend is required for core use; the only network needs are the initial app
load and optional update checks.

**Components.**
- **App shell (UI):** TypeScript/ESM, component-based (framework TBD — lightweight, a11y-friendly,
  e.g. Preact/Svelte/Lit; decision recorded in M0). Routing is client-side and offline-capable.
- **Service worker / offline layer:** precache of the app shell + content bundle; runtime caching
  strategy (cache-first for content, stale-while-revalidate for the shell); graceful offline fallback
  page. Built with a maintained tooling layer (e.g. Workbox or a vetted equivalent) — choice recorded
  as an ADR in M0.
  - **Precache size budget & eviction (low-end devices).** Multilingual content across the 6 target
    hazards must fit a constrained device. We set an explicit **precache budget** (target ≤ ~15 MB
    for shell + the user's active locale(s); hard ceiling recorded in the SW ADR) and **do not**
    precache every locale × hazard up front. Strategy: precache shell + the negotiated/active locale's
    hazard set; fetch-and-cache other locales on demand (cache-first thereafter). Eviction is **LRU
    over non-active-locale hazard units only** — the active locale's full hazard set and the offline
    fallback are **pinned and never evicted**, so a household never loses the safety content it is
    relying on. On `QuotaExceededError` the SW prunes least-recently-used non-pinned units, retries,
    and if still failing surfaces a clear "storage full — some other-language content unavailable
    offline" notice rather than silently dropping pinned safety content.
  - **Forced update for safety-critical content (not a dismissible prompt).** The app ships a
    **content-version manifest** (per-unit content version + integrity hash, separate from the app
    shell version). On load/SW activation the SW compares cached units against the manifest; a unit
    marked **hard-invalidated** (e.g. corrected after an error in official guidance) is purged from
    cache and **must** be re-fetched before it can be shown — it cannot be served stale and the
    update is **not dismissible** for that unit. Routine, non-safety updates may still use a soft
    "new version available" reload prompt; the hard-invalidation path is reserved for safety
    corrections. See *Security & privacy* for integrity details.
- **Content store:** versioned structured content (see data model) shipped as static JSON/MDX bundles
  loaded into the offline cache. No remote content fetch at runtime is required.
- **i18n layer:** message catalogs per locale; content units keyed by `(hazardId, lang)`; build-time
  completeness validation; locale negotiation with safe fallback to English. **Scope is explicit and
  settled before the UI-framework ADR** (the framework must demonstrably support all of it): full
  **bidirectional/RTL** layout (logical CSS properties, `dir` handling, mirrored components),
  **bundled offline fonts** with the glyph coverage each shipped locale needs (no runtime web-font
  fetch — consistent with zero egress), **ICU-style pluralization and gender/select** rules, and
  **locale-aware formatting** of numbers, dates, lists, and units. **First non-English locale: a
  Spanish (es) localization** as the M2 target (subject to partner override once a partner is
  secured); an RTL locale is the priority for M3 so RTL is exercised early rather than retrofitted.
- **Export/print:** print-optimized CSS and client-side PDF generation (no server).
- **Build/CI:** pnpm workspace; lint, typecheck, unit, a11y, and offline E2E gates.

**Tech stack.** TypeScript, ESM, pnpm workspaces; PWA (web app manifest + service worker); static
hosting (e.g. GitHub Pages / Netlify / Cloudflare Pages — all static, no app server). Testing:
unit (Vitest), a11y (axe-core/pa11y), E2E incl. offline (Playwright). License: MIT (code) / CC-BY
(content).

**Data model (content unit).**
```
HazardModule {
  id: string                 // e.g. "flood"
  title: localized string
  appliesTo: region tags[]   // hazard applicability (advisory, not geolocation)
  sections: { explainer, beforeNow, duringNow, afterNow, checklist[] }
  sources: Citation[]        // { org, title, url, retrievedDate, sourceLicense }
  reviewStatus: "draft" | "in-review" | "approved"
  reviewedBy: string         // reviewer attribution (authoring SME)
  approvedBy: string         // separate approver; MUST differ from reviewedBy (no self-approval)
  reviewLogRef: string       // PR-tied tamper-evident review-log entry (see Quality gates)
  lastReviewed: date
  lang: string
  contentLicense: "CC-BY-4.0"
  contentVersion: string     // per-unit version, independent of app-shell version
  integrityHash: string      // hash of unit payload; checked against content-version manifest
  hardInvalidate: boolean    // true => cached copies must be purged & re-fetched, non-dismissible
}
```

A separate **content-version manifest** lists every unit's `contentVersion` + `integrityHash` and
the `hardInvalidate` flag; the service worker reconciles cached units against it on activation (see
*Solution approach & architecture* and *Security & privacy*).

**Key decisions (to be recorded as ADRs in M0).**
1. UI framework choice (a11y + bundle size + offline simplicity weighted) — its i18n/RTL/pluralization
   support is a hard input, so the *i18n scope above is settled before this ADR is finalized*.
2. Service-worker tooling (Workbox vs. hand-rolled), caching strategy, **precache budget & eviction
   policy**, and **content-version manifest / hard-invalidation** mechanism.
3. Content format (JSON vs. MDX) and how citations/review status are enforced in CI.
4. Hosting target and update/versioning strategy for cached clients.
5. PDF/print approach (print CSS vs. client PDF lib).

**Decision ordering (important).** The **content-format ADR (#3) is decided before** the content-unit
schema is finalized and before precaching content is implemented — both depend on the chosen format.
Until ADR #3 lands, the schema in this document is **provisional** (format-agnostic intent), and any
M0 work on schema/precaching treats the content format as **decided by ADR #3, not assumed**. TASKS.md
sequences this explicitly (schema and content tasks depend on the ADR task).

## Data, licensing & compliance

**This section is load-bearing for a medium-risk safety project. Be conservative.**

**Content sources.** Preparedness guidance is derived from **authoritative public agencies**:
FEMA / Ready.gov (US, public domain), American Red Cross / IFRC, WHO, and **local/national
civil-protection agencies** relevant to each target region/language. Each content unit must cite its
sources with org, title, URL, retrieval date, and the source's license/terms.

**Source-divergence precedence (when official sources conflict).** Authoritative sources sometimes
give different or contradictory guidance for the same hazard. A unit must not silently pick one. The
precedence rule is: **(1) the official agency with jurisdiction over the user's region wins** for that
region (local/national civil-protection > supranational > foreign-national); **(2) for global/no-region
units, prefer WHO for health-related guidance and FEMA/Ready.gov or Red Cross/IFRC for
physical-hazard guidance;** **(3) where credible sources still materially disagree on a safety
action, state the divergence explicitly and link both rather than averaging them.** Each region may
carry a **region override** (the locally-authoritative instruction for that region) layered over the
base unit, recorded with its source. Material divergences and the chosen precedence are noted in the
unit's review-log entry so the decision is auditable.

**Licensing rigor (critical).**
- **Our outputs:** code under **MIT**; content under **CC-BY-4.0**.
- **Source reuse:** We must verify each source's terms before reuse. US federal works (FEMA/Ready.gov,
  CDC) are generally **public domain**, but **Red Cross and many national agencies assert copyright
  and trademarks** — their text and logos are **not** freely reusable. Default stance: **paraphrase
  and cite, do not copy verbatim, and never reuse third-party logos/trademarks.** Where a source's
  license is unclear or restrictive, we link to it rather than embed it. Provenance and license of
  every source are recorded in the content unit and in a provenance log.
- **Translations:** localized content is a derivative; the same source-license rules apply, and
  translations are reviewed for accuracy (medium risk) not just fluency.

**Provenance model.** Every content unit carries its `sources[]` with retrieval dates and a review
log entry (`reviewedBy`, `lastReviewed`, `reviewStatus`). A repo-level provenance/review log is the
auditable record that the *Definition of Shipped* checks against.

**Privacy / PII stance.** **Zero PII.** No accounts, no analytics, no telemetry, no third-party
scripts, no cookies beyond what the service worker needs for offline function. The household plan
and go-bag data the user enters **never leaves the device** — stored locally (e.g. IndexedDB/
localStorage) and only exported by explicit user action (print/local file). This is enforced and
audited in CI (no network egress in core flows; manifest/script audit).

**Attribution.** Source agencies are credited in each content unit and in an in-app credits page.
We do **not** imply endorsement by any agency we cite unless that agency has explicitly endorsed the
toolkit.

## Quality, review & risk gates

**Risk tier: medium.** Domain-accuracy review is mandatory; mis-stated emergency advice can harm.

**Required reviews before a deed is "done":**
- **Code/PWA tasks:** maintainer code review + CI green (lint, typecheck, unit, **a11y**, **offline
  E2E**). Accessibility regressions block merge.
- **Content tasks (medium risk):** a reviewer with relevant domain knowledge verifies each claim
  against the cited official sources; the **scope guardrail** is enforced (no high-stakes medical/
  legal/weapons instruction; link out instead). Where a hazard touches life-safety nuance, an
  **emergency-management or relevant subject-matter expert** signs off. Translations are reviewed by
  a competent speaker **and** re-checked for safety accuracy, not just fluency.
  - **Role separation & no self-approval.** The author of a content unit may **not** approve it: the
    `reviewedBy`/author and the `approvedBy`/SME must be **distinct people**. Approval requires an SME
    who meets a **defined credential bar** — current or former emergency-management / civil-protection
    professional, licensed first responder, or an equivalent recognized preparedness qualification —
    recorded with the sign-off. (For general, non-life-safety content the domain-reviewer bar applies;
    the SME bar is mandatory wherever a unit touches life-safety nuance.)
  - **Tamper-evident review log.** Every approval writes a **PR-tied review-log entry** (PR number,
    commit SHA, unit `contentVersion`, reviewer + approver identities, sources checked, divergence
    notes, decision). The log is append-only and committed in-repo so each entry is bound to an
    immutable commit — the auditable record the *Definition of Shipped* checks against.
- **Accessibility:** every UI-affecting change passes automated checks **and** a **manual
  assistive-technology audit** bound to a defined support matrix, **not** an ad-hoc spot check:
  - **Combos audited:** NVDA + Firefox on Windows, JAWS + Chrome on Windows, VoiceOver + Safari on
    macOS/iOS, and TalkBack + Chrome on Android — plus keyboard-only on each desktop browser.
  - **Cadence:** before each milestone exit and before every production release, and a regression
    pass on any change to navigation, focus management, or form/interaction patterns.
  - **Who is qualified:** the audit is performed/signed off by the **accessibility reviewer** — a
    contributor competent and regularly working with the named assistive technologies (daily AT users
    preferred) — not by an automated tool alone.

**Definition of Shipped (project-level).** A deployed, installable, offline-capable toolkit that:
(1) passes WCAG 2.2 AA (automated + manual), (2) works fully offline after install (verified by E2E),
(3) ships only source-cited, accuracy-reviewed content within scope, (4) collects no telemetry/PII,
and (5) is **adopted/endorsed by a named partner emergency-management org and available in that
community's languages.** Until a partner is secured, criterion (5) is **outstanding** and the
project is "publicly usable" but not yet "shipped" by Elyos's *delivered, not merged* bar.

**"Publicly shipped (no partner)" success state — so a finished toolkit isn't stranded.** Criteria
(1)–(4) can be fully met without any partner. If, by a **decision point set at 6 months after the M3
build is production-ready**, no partner has been secured, the steward + Elyos governance make an
explicit call to declare **"Publicly Shipped (generic public good)"**: the toolkit is deployed,
announced, and distributed directly (and via mutual-aid/community channels) as a self-standing public
good, with outcomes tracked by best-effort self-report rather than a partner. This is a recognized,
honest success state — distinct from the fuller "Shipped (partner-adopted)" state — and partner
outreach continues in parallel so a later endorsement upgrades the status rather than gating launch.

## Roadmap & milestones

Phased; each phase has measurable exit criteria. M0 is a thin cold-start foundation.

- **M0 — Foundation & cold-start (thin slice).**
  Goal: a minimal installable, offline PWA skeleton with a11y + privacy guardrails in CI, plus the
  content schema and one source-cited sample hazard.
  Exit criteria: PWA installs and loads offline after first visit; service worker precaches shell
  within the precache budget; CI runs lint/typecheck/unit/axe and an offline smoke E2E; **content-
  format ADR (#3) recorded before** the content schema + provenance/review-log format are finalized;
  ADRs also recorded for UI framework (with i18n/RTL scope settled as input) + SW tooling (incl.
  precache budget/eviction + content-version manifest); one hazard rendered from a content unit with
  citations; zero telemetry verified by CSP `connect-src 'none'` + runtime network-interception E2E
  (not a static grep alone).

- **M1 — Core features & accessibility hardening.**
  Goal: the real user value — checklists, household plan builder, go-bag generator, "what to do now"
  flows, printable export — all offline and AA-accessible.
  Exit criteria: all core flows work offline (E2E); WCAG 2.2 AA pass with manual AT audit + 0 critical
  axe issues; household data stays on-device (verified no egress); printable outputs generated
  client-side; ≥ 2 hazards content-complete and reviewed.

- **M2 — i18n & first localization.**
  Goal: internationalization framework + at least one non-English locale (UI + shipped content),
  with translation accuracy review.
  Exit criteria: i18n framework with build-time completeness check; ≥ 1 non-English locale complete
  for UI and ≥ 1 hazard; locale fallback safe; translated safety content passes accuracy review.

- **M3 — Content depth & partner adoption.**
  Goal: broaden hazard coverage, secure a named partner, and meet the full Definition of Shipped.
  Exit criteria: ≥ 6 hazards reviewed; ≥ 3 languages; **named partner org endorsement/adoption on
  file**; deployed production build; outcomes-tracking process (partner self-report) in place.

Dependencies: M1 depends on M0 (shell + schema). M2 depends on M1 (stable content + UI to localize).
M3 depends on M2 (multilingual base) and on the **partner being secured** (can start in parallel).

## Work breakdown

The itemized, schema-mapped backlog lives in **TASKS.md**, organized by the M0–M3 milestones above.
Each task maps to an Elyos Task JSON (see schema), is sized (small/medium/large), risk-tagged, and
names a reviewer. TASKS.md also includes acceptance criteria for the most important tasks per
milestone, milestone Definitions of Done, a backlog, and a complete example Task JSON.

## Governance, roles & stakeholders

- **Maintainer (Owner): TBD.** Owns the repo, roadmap, releases, and review standards.
- **Code reviewers:** rotation of TS/PWA-competent contributors; at least one approval + CI green to
  merge.
- **Content/safety reviewers:** contributors with emergency-preparedness domain knowledge; for
  life-safety-sensitive units, an **emergency-management / civil-protection SME** signs off.
- **Accessibility reviewer:** contributor competent with assistive tech; performs manual audits.
- **Translation reviewers:** competent speakers per locale, paired with a safety-accuracy re-check.
- **Steward (last-mile owner): TBD** — owns deployment, partner relationship, and getting the toolkit
  into beneficiaries' hands.
- **Partner / requestor: TO BE SECURED** — a named emergency-management org confirming need,
  priority hazards, and target languages, and ultimately endorsing/adopting.
- **Elyos governance/board:** arbitrates edge cases and risk-tier decisions per the good-deed
  definition.

## Dependencies & integrations

- **External content sources:** FEMA/Ready.gov, Red Cross/IFRC, WHO, local civil-protection agencies
  (read-only references; license-checked).
- **Tooling/libraries:** PWA/service-worker tooling (e.g. Workbox), UI framework (TBD), i18n library,
  test stack (Vitest, Playwright, axe-core/pa11y), client PDF/print.
- **Hosting:** static host (GitHub Pages / Netlify / Cloudflare Pages) — no app server.
- **Elyos pieces:** Task schema (`packages/schema`), CLI workspace prep / PR flow (donated lane),
  good-deed definition & risk-tier governance, review/sign-off process.
- **Human/expert dependency:** domain reviewers and a partner org — the gating non-software
  dependencies.

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
| --- | --- | --- | --- | --- |
| Inaccurate/mis-stated emergency advice causes harm | Medium | High | Mandatory source-cited accuracy review per unit; SME sign-off for life-safety nuance; scope guardrail (link out for medical/legal/weapons) | Content/safety reviewer |
| Copyright/trademark misuse of source content (esp. Red Cross/national agencies) | Medium | High | Paraphrase + cite, never copy verbatim or reuse logos; verify each source license; provenance log; legal-stance default to "link, don't embed" | Maintainer |
| No partner org secured (verified need unconfirmed) | Medium | High | Pursue named partner early in parallel; **6-month-post-M3 decision point** to declare "Publicly Shipped (generic public good)" so a finished toolkit isn't stranded; partner endorsement later upgrades status rather than gating launch | Steward |
| Offline behavior fails in real degraded-network conditions | Medium | High | Offline E2E in CI with network disabled; manual field-style testing; explicit cache/update strategy + fallback | Maintainer |
| Accessibility regressions slip in | Medium | High | a11y gate in CI (axe/pa11y) blocking merge; periodic manual AT audits; AA as hard requirement | A11y reviewer |
| Content goes stale vs. updated official guidance | High | Medium | `lastReviewed` dates + periodic re-review cadence; provenance log flags age; maintenance tasks | Content reviewer |
| Translation introduces safety errors | Medium | High | Translations re-checked for accuracy, not just fluency; competent speaker + safety reviewer pair | Translation reviewer |
| Scope creep into alerting/medical/native apps | Medium | Medium | Explicit non-goals; backlog gate; governance review | Maintainer |
| Misuse/defacement of an open static app (forks implying false endorsement) | Low | Medium | Clear license/branding terms; "not an official agency tool unless endorsed" disclaimer; trademark notice | Maintainer |
| Maintainer bandwidth / bus factor | Medium | Medium | Reviewer rotation, documented processes, CC-BY/MIT lower lock-in | Maintainer |

## Security & privacy

**Threat surface.** As a static, no-backend, no-PII PWA the attack surface is small. Principal
concerns: (1) supply-chain risk in dependencies, (2) service-worker cache poisoning / stale-content
risk, (3) third-party script/tracker creep, (4) hostile forks implying false agency endorsement.

**Controls.**
- **No secrets** in the app; nothing to leak. No API keys, tokens, or credentials in code, logs, or
  receipts (per Elyos rule). Static hosting only.
- **No telemetry / no PII:** enforced by **defense in depth**, not a static grep alone. (1) A strict
  Content-Security-Policy with **`connect-src 'none'`** (plus locked-down `script-src`/`img-src`/
  `font-src 'self'`) blocks runtime exfiltration at the browser level. (2) A **runtime
  network-interception E2E test** (Playwright route interception) exercises every core flow and
  **fails the build on any unexpected outbound request**. (3) The static CI audit (no analytics/
  trackers/PII fields) remains as a cheap first line. User-entered plan/go-bag data stays on-device
  and is exported only by explicit action.
- **Supply chain:** pinned/locked dependencies (pnpm lockfile), dependency review, minimal deps,
  Subresource Integrity where applicable; CI dependency audit.
- **Service worker hygiene:** scoped SW, integrity-checked precache manifest, explicit versioning and
  update flow to avoid users stuck on stale safety content; HTTPS-only.
- **Safety-content integrity & forced invalidation:** the **content-version manifest** carries a
  per-unit `integrityHash`; the SW verifies cached units against it and **hard-invalidates** any unit
  flagged for a safety correction — purging it and forcing a re-fetch before display (non-dismissible
  for that unit), so a corrected hazard instruction can never be served stale from cache.
- **Content integrity:** all guidance source-cited and review-gated, reducing misinformation risk.
- **Abuse/misuse:** prominent disclaimer that the tool is not an official agency product unless a
  named partner endorses it; trademark/branding terms documented; license requires attribution.

## Sustainability & maintenance

- **Ownership after delivery:** the **maintainer** owns code/releases; the **steward** owns
  deployment and the partner relationship. Both are currently **TBD** and must be named before M3.
- **Content freshness:** content units carry `lastReviewed` dates; a **periodic re-review cadence**
  (recommended at least annually and after any major change to official guidance) is enforced via
  maintenance tasks; stale units are flagged.
- **Outcomes tracking:** because we collect no telemetry, outcomes (adoption, reach) are tracked via
  **partner self-report** and a manual record of endorsements/MOUs — an explicit privacy/measurement
  trade-off.
- **Low lock-in:** MIT/CC-BY, static hosting, and standard web tech keep the project forkable and
  cheap to run, improving long-term survivability.

## Open questions

1. **Partner org:** Who is the named emergency-management partner? The proposal claims a verified
   need but names none — this needs a human decision before *Definition of Shipped* can be met.
2. **Priority hazards & regions:** Which hazards and regions are first (should be partner-driven)?
3. **Priority languages:** First non-English locale is **provisionally Spanish (es)** at M2 (with an
   RTL locale prioritized for M3); a partner, once secured, can override the priority for its
   community.
4. **UI framework & SW tooling:** final ADR decisions (M0).
5. **Legal review of source reuse:** do we need a one-time legal check of our paraphrase/citation
   stance for Red Cross / national-agency content?
6. **Hosting/deployment target** and who operates it (steward).
7. **Local-civil-protection sourcing:** how do we vet authoritative local sources per region/language?

## References

- Elyos work rules — `C:\code\elyos\CLAUDE.md`
- Good-deed definition & risk tiers — `C:\code\elyos\docs\good-deed-definition.md`
- Task schema — `C:\code\elyos\packages\schema\src\schemas.ts`
- Project proposal — `C:\code\elyos\governance\proposals\proper-prepper.md`
- Authoritative content sources (license-checked per use): FEMA / Ready.gov, American Red Cross /
  IFRC, WHO, and relevant local/national civil-protection agencies.
- WCAG 2.2 AA (W3C Web Content Accessibility Guidelines).
