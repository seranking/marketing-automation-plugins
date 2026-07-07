# 07 — Dependencies and critical path

Per SKILL.md step 9, the dependency map for the 90-day plan:

```
Phase 1.1 (technical audit) --+
                              |--> Phase 2.2 (ship /vs/* + /agencies — needs no-blocker technical state)
Phase 1.2 (content audit)  --+
                              |--> Phase 2.4 (ship /agencies content brief — needs E-E-A-T baseline)
Phase 1.3 (backlinks profile)
   |--> Phase 3.2 (backlink-gap outreach — needs baseline of own profile)

Phase 1.4 (drift baseline) --> Phase 3.3 (drift compare)

Phase 2.1 (competitor-pages draft) --> Phase 2.2 (ship pages)
Phase 2.3 (content brief) --> Phase 2.4 (ship /agencies)
Phase 2.2 + 2.4 --> Phase 2.6 (schema on new pages)
Phase 2.6 --> Phase 3.1 (GEO analysis needs the new pages structured-data-rich)
```

## Critical-path items (blocking)

1. Phase 1.1 technical audit — blocks every Phase 2 deploy.
2. Phase 1.4 drift baseline — must run BEFORE Phase 2 pages ship for Phase 3.3 to have a comparable anchor.
3. Phase 2.1 competitor-pages draft — blocks Phase 2.2 + 2.6.
4. Phase 2.6 schema — blocks Phase 3.1.
5. Phase 3.3 drift compare — closes the Phase 1.4 loop.

## Off-critical-path (moveable)

- Phase 1.5 (og:image audit) — independent; can run in parallel with anything.
- Phase 1.6 (top-3 page audits) — independent of 1.1/1.2 but depends on having a top-traffic-pages list.
- Phase 3.2 (backlink-gap outreach) — depends only on 1.3 finishing; can start in week 9.

## Risk register

- Risk A: Phase 1.1 surfaces RED-tier technical issues (e.g., noindex on important pages, like the sibling Notion seo-page example found). Mitigation: defer Phase 2 deploys until red issues fixed.
- Risk B: Phase 1.2 surfaces E-E-A-T <70 on top pages. Mitigation: pull editorial support into Phase 2.4 before launching new content.
- Risk C: Wix Studio brand team rejects /vs/competitor pages. Mitigation: pivot Phase 2.2 to Wix-Studio-only landing pages; lose the comparison-SERP capture but retain the agency-cluster capture.
- Risk D: SE Ranking credits run out mid-phase. Mitigation: each specialist's preflight surfaces cost before running; user can decline expensive runs and defer.
