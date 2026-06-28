# Proposal: proper-prepper (disaster-prep open toolkit)

**Proposer:** jdev1977 (drafted by Claude Code)
**Date:** 2026-06-28
**Domain(s):** public-safety, software, accessibility, translation
**Lane:** donated
**Requestor / beneficiary:** at-risk communities and local emergency-management orgs  ·  **Verified need:** yes (per partner emergency org)

## Summary
An **offline-first, open-source** preparedness toolkit: a installable PWA (checklists, household
plans, hazard explainers, "what to do now" flows) plus localized, accessible preparedness content.
Engineered to work with no connectivity — exactly when disasters strike. "Proper Prepper": calm,
practical, evidence-based readiness for everyone, not doomsday marketing.

## Why it qualifies as a good deed
- **Tangible public benefit:** better-prepared households survive and recover faster.
- **Freely available:** MIT/CC-BY; no ads, no data harvesting, no paywall.
- **Not primarily for-profit:** the "prepper" market is full of upsells; this is the free public good.
- **No harm/misinformation:** guidance aligns with official sources (FEMA/Red Cross/WHO/local civil
  protection) and is review-gated; no fearmongering, no unsafe DIY-medical/firearms content.
- **Executable by AI sessions:** decomposes into app features and per-hazard × region × language
  content units.

## Scope / first tasks
- (medium) PWA skeleton: offline cache, installable, accessible (WCAG AA), no telemetry.
- (small) One hazard module (e.g., flood) content, sourced to official guidance.
- (small) Household emergency-plan + go-bag checklist generator (printable).
- (medium) i18n framework + first non-English localization.

## Definition of shipped
A deployed, installable, offline-capable toolkit adopted/endorsed by a partner emergency-management
org and usable in their community's languages.

## Risks / review needs
**Risk tier: medium.** Safety accuracy must be reviewed against authoritative guidance for each
hazard; mis-stated emergency advice can cause harm. **Scope guardrail:** no high-stakes medical,
legal, or weapons instruction; link to official sources rather than improvising. Accessibility is a
hard requirement (emergencies disproportionately affect disabled people).
