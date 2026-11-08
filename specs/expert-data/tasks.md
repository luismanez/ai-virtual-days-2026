# Tasks: Skill scoped mock data

Commands run from `src/copilot-expert-finder`. File paths below are relative to `src/copilotComponents/expertFinder` unless they start with config, copilot, docs, README, or Specs. Completion status lives only in [tasks/todo.md](../tasks/todo.md).

## DAT-01

**Define skill and domain contracts**

Create the Expert model, IExpertService, and one canonical skill normalizer.

- Acceptance: Supported casing/whitespace variants normalize; empty/malformed/unknown skill is rejected before retrieval.
- Verification: Review normalization with representative values and run npm run build.
- Dependencies: None.
- Likely files: models/Expert.ts; services/IExpertService.ts; utils/skills.ts.

## DAT-02

**Implement the mock service**

Add fictional records and asynchronous skill-scoped retrieval with stable ordering.

- Acceptance: SPFx returns 6, Copilot 8, DB 1, Spanish 0; no result for another skill reaches React.
- Verification: Use these four skills in Workbench after INL-01 with mock data selected (root-list check returns HTTP 404); compile before that.
- Dependencies: DAT-01.
- Likely files: data/mockExperts.ts; services/MockExpertService.ts.

## DAT-03

**Bundle demo portraits**

Acquire/generate local photographic portraits, bind them to fixtures, and record redistribution rights/provenance.

- Acceptance: No mock avatar relies on an external network service; the first three SPFx profiles match the named mockup examples, and missing images have a fallback.
- Verification: Inspect the images and later verify they load in Workbench and are included in the package.
- Dependencies: DAT-02.
- Likely files: data/mockExperts.ts; assets/README.md; portrait assets as one asset batch.
