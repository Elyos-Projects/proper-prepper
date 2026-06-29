# Competitive & Improvement Analysis — proper-prepper

*Offline-first, evidence-based, non-fearmongering household emergency-preparedness toolkit. Sources cited to authoritative emergency-management bodies (FEMA/Ready.gov, Red Cross/IFRC, WHO).*

Analysis date: 2026-06-29 · Plan reviewed: PLAN.md v0.2.0 (2026-06-28) · TASKS.md v0.2.0

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually rigorous for its risk tier: it correctly self-identifies as **medium risk-tier**, enforces source-cited accuracy review with **no self-approval** (author `reviewedBy` ≠ SME `approvedBy`), a credential bar for the approving SME, a tamper-evident PR-tied review log, and a **source-divergence precedence rule** (jurisdictional authority wins; WHO for health, FEMA/Red Cross for physical hazards; state divergence rather than average). Licensing rigor is strong and accurate: it correctly flags that **FEMA/Ready.gov/CDC are US-federal public domain** while **Red Cross asserts copyright and trademark** and defaults to "paraphrase + cite, never copy verbatim, never reuse logos." Privacy posture (zero PII/telemetry, `connect-src 'none'` + runtime network-interception E2E) and WCAG 2.2 AA as a hard gate with a named AT support matrix are best-in-class. The honesty note that **no partner/MOU exists yet** (verified-need = `false`) is commendably candid.

**Findings / gaps against the guardrails:**

1. **"Not a substitute for official emergency instructions" boundary is under-specified — most important finding.** The guardrail requires a user-facing statement that the guidance does **not** replace live, official instructions (e.g., follow local evacuation orders, boil-water notices, NWS/agency alerts). The plan only carries a *different* disclaimer — "not an official agency product unless endorsed" (a branding/endorsement statement). It also lists "live alerting" as a non-goal. But it never makes the **life-safety deferral** an explicit, surfaced, per-hazard UI element. This should be a first-class, always-visible content component, not an inferred consequence of the non-goal. **Recommend a dedicated task + schema field.**

2. **Anti-myth / anti-grift counter-content is implied by tone but not a deliverable — second most important finding.** The project's explicit purpose includes countering prepper-survivalist myths and commercial grift, yet neither the content data model nor TASKS.md contains a **"myth vs. evidence"** content type or task. "Calm tone" is necessary but not sufficient; debunking specific high-traffic myths (e.g., "you need a bunker / years of freeze-dried food / a weapon," gold-and-ammo framing) with cited evidence is a distinct, high-value unit that is currently absent. **Recommend an explicit myth-vs-evidence module type.**

3. **Inclusivity is partial.** feat-024 covers caregiver/disability checklists and feat-020 adds advisory region selection (good, no geolocation/PII). But **low-income, renter, no-car, and no-storage-space** users are not a first-class content dimension. This matters because the canonical advice (Red Cross: **2-week home water supply** = ~14 gal/person; go-bag gear lists) is space- and budget-infeasible for many renters and unhoused-adjacent households, exactly the populations disasters hit hardest (research: people with disabilities and low-income renters suffer worse outcomes and less recovery support). **Recommend budget-tiered / space-constrained / transit-dependent ("no car") variants** as a tracked axis, not a footnote.

4. **Accuracy of specifics — defer-to-unit is correct, but currency risk is real.** The plan wisely does **not** hardcode quantities in PLAN.md (they live in reviewed units). Reviewers must catch that authoritative guidance has *shifted*: Ready.gov now says store water "**for several days**, for drinking and sanitation" (it softened the old flat "3 days"), and Red Cross now distinguishes **3-day (evacuation) vs. 2-week (shelter-in-place at home)** supplies and notes children/nursing/sick people and hot climates need more. A unit that states the old "1 gallon/person/day for 3 days" without the home-vs-evacuation and individual-variation nuance would be *stale-correct*. The "content goes stale" risk is rated **High likelihood** with `lastReviewed` + annual re-review — appropriate, but the cadence should be tied to source-change monitoring, not just the calendar.

5. **Regional hazard variation is modeled but US-centric in sourcing.** `appliesTo` region tags + region overrides + jurisdictional precedence are well-designed. However, the source stack (FEMA/Ready.gov/CDC) is US-federal; for non-US locales the plan leans on unspecified "local/national civil-protection agencies" with an open question (#7) on *how to vet* them. This is honestly flagged but unresolved, and it gates the i18n/Spanish-then-RTL ambition.

6. **Accessibility & offline:** Excellent and largely complete. Minor: the precache budget (~15 MB, active-locale pinned, LRU eviction of non-active locales, non-dismissible hard-invalidation of corrected safety units) is genuinely thoughtful and a differentiator. Verify the print/PDF path is itself accessible (tagged PDF / high-contrast print CSS).

**Net:** plan is strong on engineering, privacy, a11y, and provenance; weakest on (a) the explicit life-safety deferral boundary, (b) operationalizing the anti-grift mission as content, and (c) economic/transit inclusivity.

---

## 2. Competitive landscape

**FEMA / Ready.gov (US federal).** The authoritative baseline; public domain (freely reusable text — a major asset for this project). *Strengths:* canonical guidance ("Build A Kit," water, plan), trusted, free. *Weaknesses:* dry, fragmented across pages, much content PDF-first, online-dependent, and accessibility is inconsistent in the broader Ready ecosystem — a peer-reviewed study found **76% of Ready.gov state-affiliated sites had W3C Level AA failures** on the home page and **only 20% offered tailored info for people with disabilities** (https://www.worldscientific.com/doi/10.1142/S2689980923500069). Water guidance: https://www.ready.gov/water · kit: https://www.ready.gov/kit.

**American Red Cross / IFRC.** *Strengths:* trusted brand, free "Emergency" app in English + Spanish with customizable checklists and step-by-step first aid, clear "Get a Kit / Make a Plan / Be Informed" framing, and the useful **3-day evacuation vs. 2-week home** distinction (https://www.redcross.org/get-help/how-to-prepare-for-emergencies/survival-kit-supplies.html · https://www.redcross.org/get-help/how-to-prepare-for-emergencies.html). *Weaknesses for reuse:* **copyrighted text and trademarked logos — not freely reusable** (plan handles this correctly); app is online-leaning and brand-locked; not offline-first or fully open.

**CDC emergency preparedness.** *Strengths:* authoritative, public domain, strong on water/sanitation and health (e.g., emergency water creation/storage: https://www.cdc.gov/water-emergency/about/how-to-create-and-store-an-emergency-water-supply.html). *Weaknesses:* topical, not a household-planning product; scattered; clinical tone.

**The Prepared (theprepared.com).** The closest *non-government* analog and the most direct tonal competitor. *Strengths:* explicitly **rejects fearmongering** ("Sane Prepper Mantra: common sense rules… No politics or other craziness"), budget-conscious, broad and well-regarded reviews and courses. *Weaknesses:* it is a **commercial business** — affiliate product reviews, paid courses, curated kits for sale, premium memberships — so its incentives tilt toward gear; not open-licensed, not offline-first, not WCAG-gated, US/English-centric. (Positioning confirmed via the site's mission/"Sane Prepper" framing.)

**Commercial prepper/survivalist market (the grift this counters).** *"Strength"* only as a foil: huge and growing — preppers spent ~$11B in a 12-month period and the broader "incident & emergency management" market is projected from ~$75.5B (2017) toward ~$423B (2025); fear is the explicit sales lever (Marketplace: https://www.marketplace.org/story/2024/09/27/the-doomsday-prepping-business-is-booming · The Hustle: https://thehustle.co/coronavirus-prepping-doomsday-business). *Weaknesses:* critics describe "retail therapy wrapped in fear-porn packaging," ghostwritten guides by authors with no real preparedness experience, and products of "questionable" real-world utility (NPR 1A: https://www.npr.org/2021/06/30/1011858393/doomsday-prepping-goes-mainstream). This is precisely the misinformation/grift proper-prepper exists to displace.

**Wirecutter (NYT) emergency kit / go-bag guides.** *Strengths:* trusted, tested, practical product picks; updated ~Oct 2025 (overview via Sawyer reprint: https://www.sawyer.com/blog/wirecutter-the-best-gear-for-your-bug-out-bag-3). *Weaknesses:* **affiliate-revenue / product-purchase model**, paywalled, gear-centric (buy this), US/English, online — not planning, not equity-oriented, not offline.

**Local/state emergency management & Prepared4ALL-type efforts.** *Strengths:* jurisdictional authority, the *correct* source for live instructions, growing disability-inclusion work (Prepared4ALL: https://collaborations.miami.edu/articles/10.33596/coll.112). *Weaknesses:* wildly uneven quality/accessibility, often PDF/online-only, rarely multilingual or offline.

---

## 3. Gaps we can fill

1. **Offline-first.** Every competitor assumes connectivity at the exact moment it fails. A WCAG-AA PWA that works fully offline after first load is genuinely unmet.
2. **Open-licensed, brandable, multilingual.** Local EM orgs can fork/rebrand (MIT/CC-BY) in their community's languages — no competitor offers this.
3. **Equity as a design axis.** Budget-tiered, renter/small-space, no-car/transit-dependent, disability- and caregiver-specific variants — addressing the populations every competitor under-serves.
4. **Anti-grift truth layer.** A *cited* myth-vs-evidence module that names and refutes fear-marketing claims — neither Ready.gov (won't editorialize) nor commercial sites (conflict of interest) will do this.
5. **No ads, no telemetry, no upsell, no account.** A trust posture commercial players structurally cannot match.
6. **Synthesis across authorities.** One calm, plain-language unit reconciling FEMA + Red Cross + CDC + WHO + local overrides, with explicit divergence handling — replacing the user's fragmented tab-hunting.
7. **Print/export that works on-device** for households who will physically post a plan on the fridge.

---

## 4. Differentiators to win

- **vs. Ready.gov (authoritative but dry, fragmented, online, uneven a11y):** same/cited authority, but **calm plain-language, offline, genuinely WCAG-AA, multilingual, and consolidated** into task-flows ("what to do now") instead of a wall of PDFs.
- **vs. The Prepared / Wirecutter / commercial preppers (fear- or gear-driven, paid, closed):** **zero commercial incentive, zero fear** — every claim traces to an emergency-management authority, the only "product" is a free public good, and we **actively debunk** the grift those models depend on.
- **The single strongest differentiator:** **trustworthy by construction** — offline-first + zero-telemetry + open license + cited-to-authority + no-self-approval review + non-dismissible safety corrections. No competitor combines authority, privacy, accessibility, and offline reliability with no profit motive.

---

## 5. Claude API leverage (and where Claude must NOT decide)

**High-value, in-guardrail uses:**
1. **Plain-language drafting from authoritative sources.** Given FEMA/CDC/WHO source text (public-domain ones especially), draft calm, low-literacy-friendly explainers and "before/during/after" flows — as **reviewable drafts** carrying their `sources[]`, never as final published guidance.
2. **Inclusive scenario adaptation.** Transform one approved base unit into renter/no-car/disability/caregiver/budget-tier variants and reading-level simplifications, flagging where physical advice (e.g., 2-week water storage) is infeasible and needs a human-vetted alternative.
3. **Myth-vs-evidence structuring.** Draft "claim → why it's marketed → what authorities actually say (cited)" cards for SME review — operationalizing the anti-grift mission.
4. **Translation + back-translation drafts** and source-divergence summarization (surface where FEMA vs. local guidance differ) for the human precedence decision.
5. **Authoring-tooling assists:** citation/retrieval-date completeness checks, stale-source flagging, plain-language/contrast lint suggestions.

**Where Claude must NOT be the decider (hard human gates):**
- **Final accuracy & sourcing** to emergency-management authorities — every claim verified by a credentialed SME; **no self-approval**; no fabricated facts, quantities, or sources.
- **The "not a substitute for official instructions" boundary** — life-safety deferral is a policy decision, always present, human-owned.
- **No fear content** — tone/claims that imply catastrophe-marketing are rejected by a human, regardless of model output.
- **Inclusivity sign-off** — variant correctness for disabled/low-income/no-car users reviewed by humans (ideally lived-experience reviewers).
- **Regional hazard accuracy & jurisdictional precedence** — which authority governs a region, and any region override, is a human/SME call.
- **Licensing** — Claude must not be trusted to judge whether a source (esp. Red Cross/national agencies) is reusable; default to paraphrase-and-link under human verification.

---

## 6. Ten concrete optimizations

1. **Add a first-class "official-instructions deferral" component** (schema field + always-visible UI + per-hazard task) — close the most important guardrail gap.
2. **Add a `myth-vs-evidence` content type and tasks** (claim / fear-hook / cited authority answer) to operationalize the anti-grift mission.
3. **Make economic & mobility inclusion a tracked content axis** — budget-tier, renter/small-space, no-car/transit, no-fridge/no-power variants — alongside the existing disability/caregiver work.
4. **Encode the home-vs-evacuation water/food distinction and individual-variation caveats** into the schema so units can't ship the oversimplified "1 gal × 3 days" without the nuance.
5. **Tie re-review cadence to source-change monitoring,** not just annual dates — e.g., a lightweight watch on cited Ready.gov/Red Cross/CDC/WHO URLs that flags units when source pages change.
6. **Resolve open question #7 (local-source vetting) before M2** with a written rubric for accrediting non-US/non-English civil-protection sources; it gates the Spanish-then-RTL roadmap.
7. **Publish a content style guide** codifying calm/non-fear tone, reading-level target, and banned fear/scarcity rhetoric — give reviewers an objective rubric.
8. **Ship an accessible print/PDF "fridge plan"** with tagged-PDF/high-contrast print CSS, verified in the a11y audit (not just the on-screen app).
9. **Lean on public-domain federal sources for verbatim-eligible text** and reserve Claude paraphrasing for the copyrighted Red Cross/national-agency material — reduces legal risk and review load.
10. **Add a "calm metrics" guard:** since reach is partner-self-reported, also track *outcome-proxy* signals partners can attest (households that completed a plan/built a kit), keeping success "delivered, not downloaded."

---

## 7. Parallel & perpendicular spin-offs

- **first-aid-open:** proper-prepper deliberately links out for first-aid *instruction*; a sibling open, cited first-aid project is the natural complement and a clean linking target.
- **community-resource-maps / food-assistance-maps:** "where is my nearest shelter / cooling center / water distribution" is the local-resource layer prep guidance points to; share the region model.
- **climate-adaptation-guides:** heatwave/wildfire/flood hazards overlap directly; share hazard explainers and regional applicability tagging.
- **water-quality-explainers & wash-guides:** emergency water storage/treatment and sanitation are shared content — reuse units across both projects.
- **emergency-phrasebooks / vital-info-translations / multilingual-signage-templates:** the i18n/RTL pipeline and translated-safety-content review process are directly reusable.
- **Reusable "sourced-guidance engine":** the strongest perpendicular asset. The content-unit schema (sources[], retrieval dates, no-self-approval review log, divergence precedence, integrity hash + non-dismissible hard-invalidation, offline precache) is a **general pattern for any cited, life-safety-adjacent, offline public-good content** — first-aid, WASH, know-your-rights, benefits-navigator, prevention-info-open. Factor it into a shared Elyos package so future projects inherit the provenance + offline + a11y guarantees.

---

## 8. Open questions

1. **Life-safety deferral surfacing:** exact wording and placement of the "not a substitute for official instructions" boundary — who owns the canonical text, and is it a blocking gate per unit?
2. **Anti-grift scope:** how aggressively do we name specific products/claims vs. debunk categories, given legal/defamation exposure on an open repo?
3. **Local-source accreditation (plan #7):** what is the rubric for trusting a non-US civil-protection source, and who signs off — gates i18n credibility.
4. **Inclusivity reviewers:** can we recruit lived-experience reviewers (disabled, low-income, transit-dependent users), or does SME-only review risk missing the populations we target?
5. **Partner vs. public-good launch:** is the 6-month-post-M3 "Publicly Shipped (generic public good)" fallback acceptable to Elyos governance as a true success state, or does it weaken the outcomes bar?
6. **Currency operations:** who owns source-change monitoring, and what's the SLA to hard-invalidate a unit after an authority corrects guidance?
7. **Self-reported reach honesty:** how do we prevent partner self-report from becoming a vanity metric while collecting zero telemetry?

---

### Sources

- Ready.gov — Water: https://www.ready.gov/water · Build A Kit: https://www.ready.gov/kit
- CDC — Create/store emergency water supply: https://www.cdc.gov/water-emergency/about/how-to-create-and-store-an-emergency-water-supply.html
- American Red Cross — Survival kit supplies: https://www.redcross.org/get-help/how-to-prepare-for-emergencies/survival-kit-supplies.html · How to prepare: https://www.redcross.org/get-help/how-to-prepare-for-emergencies.html
- The Prepared — theprepared.com ("Sane Prepper" positioning; courses/affiliate/kit business model)
- Ready.gov inclusivity/accessibility study (76% AA failures; 20% disability-tailored): https://www.worldscientific.com/doi/10.1142/S2689980923500069
- Prepared4ALL (disability inclusion in local EM): https://collaborations.miami.edu/articles/10.33596/coll.112
- Marketplace — doomsday prepping business booming: https://www.marketplace.org/story/2024/09/27/the-doomsday-prepping-business-is-booming
- The Hustle — end-of-the-world business: https://thehustle.co/coronavirus-prepping-doomsday-business
- NPR 1A — Doomsday prepping goes mainstream: https://www.npr.org/2021/06/30/1011858393/doomsday-prepping-goes-mainstream
- Wirecutter go-bag/emergency supplies (overview reprint): https://www.sawyer.com/blog/wirecutter-the-best-gear-for-your-bug-out-bag-3
- FEMA equity gap (disaster recovery): https://www.americanprogress.org/article/how-fema-can-prioritize-equity-in-disaster-recovery-assistance/
