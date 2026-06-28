# proper-prepper

> Offline-first, ad-free disaster-preparedness toolkit (PWA) + localized content.  ·  **Risk tier:** medium  ·  **Status:** proposed (planning)

An **offline-first, open-source** preparedness toolkit: a installable PWA (checklists, household
plans, hazard explainers, "what to do now" flows) plus localized, accessible preparedness content.
Engineered to work with no connectivity — exactly when disasters strike. "Proper Prepper": calm,
practical, evidence-based readiness for everyone, not doomsday marketing.

**Definition of shipped:** A deployed, offline-capable toolkit adopted/endorsed by a partner emergency-management org.

This is an **Elyos** good-deed project. Contributors pull a task, do it with their own coding agent, and open a PR. Platform: https://github.com/jdev1977/elyos

## Planning
- [PROPOSAL.md](./PROPOSAL.md) — why this qualifies as a good deed (Good Deed Definition)
- [PLAN.md](./PLAN.md) — architecture, roadmap & milestones, risks
- [TASKS.md](./TASKS.md) — the full task backlog
- [tasks/](./tasks/) — ready-to-pull task JSON(s)

## Contribute
```bash
elyos browse
elyos pull --task-file tasks/proper-prepper-pwa-001.json --repo Elyos-Projects/proper-prepper
# do the work with your own agent, then:
elyos submit proper-prepper-pwa-001 --repo Elyos-Projects/proper-prepper
```

## Licensing & review
- **Licensing:** Code: MIT. Content: CC-BY-4.0 (paraphrased + cited; no agency logos/trademarks).
- **Review:** risk tier **medium** — deeds are *delivered, not merged*; a domain reviewer must sign off before merge.

> Status: this project is in **planning** and not yet ratified through Elyos governance; no adopting partner/requestor is secured yet (`verifiedNeed: false` on delivery-dependent tasks).
