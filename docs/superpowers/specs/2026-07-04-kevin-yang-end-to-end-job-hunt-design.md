# Kevin Yang End-to-End Job Hunt Design

**Date:** 2026-07-04
**Status:** Approved for specification; implementation requires Kevin's review of this document

## Objective

Configure Career-Ops as Kevin Yang's operational job-search system. The system will maintain an evidence-rich master CV, discover strong-fit senior roles weekly and on demand, evaluate them against Kevin's constraints, and prepare role-specific application materials without inventing or strengthening any claim beyond approved evidence.

## Success Criteria

- `cv.md` reflects Kevin's July 4, 2026 resume and becomes a richer, factual master CV.
- All stale user-profile facts are reconciled, including the outdated 160B monthly-event reference versus the approved 180B+ figure.
- Weekly and on-demand discovery feed the same deduplicated review queue.
- Discovery prioritizes senior product, experimentation, decision-intelligence, growth, and strategic analytics leadership roles at established employers.
- Only Kevin-selected roles advance to research and tailoring.
- Every tailored claim is traceable to approved evidence.
- No application, message, email, or form is submitted without Kevin's explicit approval.
- The configured pipeline passes repository health, synchronization, deduplication, and representative end-to-end checks.

## Source of Truth

Career-Ops will be the operational home of the search, while Kevin's latest resume remains the factual baseline for the initial synchronization. Older resumes and existing Career-Ops files may identify possible proof points, but no fact found only in those sources will enter the approved master CV or proof-point library without Kevin's review.

| File or directory | Responsibility |
|---|---|
| `cv.md` | Evidence-rich master CV containing approved professional facts |
| `config/profile.yml` | Identity, target roles, compensation, location, and work preferences |
| `modes/_profile.md` | Kevin-specific positioning, archetypes, scoring overrides, and narrative |
| `modes/_custom.md` | Evidence-lock procedure, approval gates, and output rules that override automatic modes |
| `article-digest.md` | Approved proof points and reusable evidence that should not crowd the CV |
| `portals.yml` | Target employers, sources, and title filters |
| `data/` | Discovery queue, applications, follow-ups, and scan history |
| `reports/` | Evidence-backed role evaluations and tailoring change logs |
| `output/` | Approved role-specific resumes and PDFs |
| `interview-prep/` | Approved story bank and company-specific preparation |

User-specific changes belong only in the Career-Ops user layer. In Career-Ops v1.16, personal facts and positioning belong in `modes/_profile.md`, while procedural rules and approval gates belong in `modes/_custom.md`. System-layer files will remain unchanged unless a genuinely reusable system capability is required.

## Targeting Strategy

The pipeline combines two complementary lanes:

1. **Curated discovery:** scan broadly enough to catch unexpected strong matches, then apply hard constraints and evidence-backed scoring.
2. **Target accounts:** monitor established technology, fintech, financial-services, and banking employers that are plausible destinations for Kevin.

Priority role families are:

- Director, VP, or Head of Product roles involving platforms, growth, personalization, or AI/ML-enabled products.
- Head or senior leader roles in experimentation, measurement, or decision intelligence.
- Director or VP roles in analytics or data science when they are strategic or people-led.

The pipeline should exclude or heavily penalize:

- Pure data engineering, analytics engineering, or hands-on execution roles.
- Roles below Kevin's target level.
- Early-stage or sub-100-person startups unless Kevin explicitly overrides the constraint.
- Strictly onsite roles outside the Philadelphia/Delaware area.
- Roles whose known compensation is below the $300K total-compensation floor.

The target total-compensation range is $350K-$500K. Unknown compensation remains unknown and is never guessed.

## End-to-End Workflow

Both scheduled and manual discovery use the same stateful workflow:

1. Discover roles from configured sources.
2. Verify that each posting is live.
3. Deduplicate against scan history, pipeline entries, and applications.
4. Apply hard constraints for level, role family, company maturity, location, and known compensation.
5. Score viable roles for evidence-backed fit, gaps, company stability, location, compensation, and posting legitimacy.
6. Produce a clear `pursue`, `consider`, or `skip` recommendation.
7. Wait for Kevin to select roles for deeper work.
8. Research the selected company and role.
9. Run an evidence-gap check against the master CV and proof-point library.
10. If useful evidence may exist but is not recorded, conduct a short role-specific interview one question at a time.
11. Ask Kevin to approve each proposed reusable fact derived from the interview.
12. Draft a narrowly tailored resume and optional outreach material with a source-linked change log.
13. Wait for Kevin's explicit approval of the materials.
14. Generate the final PDF or assist with form completion, stopping before submission.
15. Track the application, follow-up cadence, outcomes, and interview preparation.

## Evidence-Locked Tailoring

### Approved Evidence Sources

Every resume claim must trace to one of:

1. Kevin's approved master CV.
2. Kevin's approved proof-point library.
3. A structured interview answer that Kevin has explicitly approved as a reusable fact.

### Allowed Transformations

Tailoring may:

- Reorder existing bullets or sections.
- Slightly rephrase approved facts for clarity and relevance.
- Add exact or closely equivalent job-description keywords when the underlying claim is already supported.
- Emphasize different approved evidence for different roles.
- Remove less relevant content to preserve focus and length.

### Forbidden Transformations

Tailoring may not:

- Invent experience, ownership, scope, tools, metrics, outcomes, dates, or credentials.
- Convert adjacent exposure, partnership, or oversight into direct hands-on experience.
- Strengthen an approved claim beyond what its source supports.
- Infer a missing metric or outcome.
- Insert interview-derived material before Kevin approves the normalized fact statement.

### Evidence-Gap Check

Before drafting, each selected role receives three lists:

- **Supported:** requirements backed by approved evidence.
- **Clarify:** requirements that may be supported but need Kevin's confirmation.
- **True gaps:** unsupported requirements that must remain gaps.

The system may recommend a short interview only for the `Clarify` list. It must not interrogate Kevin merely to manufacture superficial keyword coverage.

### Approval and Audit Trail

Every tailored draft includes a concise change log showing:

- The changed section or bullet.
- The nature of the change.
- The approved source supporting it.
- Any newly approved interview fact.

The workflow stops for Kevin's review before final PDF generation and again before any external submission or communication.

## Weekly and On-Demand Operation

The scheduled scan runs weekly. Its only external effect is to create or update a local review queue; it never applies or communicates with employers.

The same pipeline can be started manually for:

- A fresh scan.
- A pasted job URL.
- A pasted job description.
- A named target employer.

The weekly schedule will default to Monday at 8:00 AM in Kevin's `America/New_York` timezone and remain easy to change.

## Error Handling and Safe Defaults

- An unverifiable or expired posting does not advance.
- A duplicate preserves existing tracker state instead of creating a new entry.
- A missing field is recorded as unknown rather than inferred.
- A parsing or source failure is reported without corrupting the queue.
- An unsupported tailored claim blocks finalization until it is removed or approved evidence is added.
- A failed PDF generation preserves the approved Markdown draft.
- A failed scheduled run records diagnostics and can be rerun manually without duplicating entries.
- Ambiguous seniority, location, or compensation is surfaced for review rather than silently accepted.

## Validation Plan

Implementation is complete only after:

1. `cv.md`, `config/profile.yml`, `modes/_profile.md`, `modes/_custom.md`, and the proof-point store agree on shared facts, metrics, and approval policy.
2. The Career-Ops doctor and pipeline verification checks pass or any environmental limitation is documented.
3. CV synchronization and stale-metric searches show no contradictory user-layer facts.
4. Deduplication and status-normalization checks preserve existing tracker data.
5. A representative sample job description completes discovery/evaluation through the Kevin review gate.
6. A tailoring test demonstrates that supported keywords can be added while an unsupported claim is rejected.
7. A draft PDF can be generated from approved material without external submission.
8. The weekly automation is configured and its command is tested safely before activation.

## Non-Goals

- Mass application or spray-and-pray automation.
- Autonomous submission, messaging, emailing, or outreach.
- Keyword stuffing disconnected from Kevin's evidence.
- Rewriting Career-Ops system defaults for user-specific preferences.
- Building a public portfolio site as part of this phase.
- Claiming complete market coverage from the configured sources.

## Implementation Boundaries

The first implementation will synchronize and enrich Kevin's user-layer data, configure targeting and portals, encode the evidence-lock and review gates in `modes/_custom.md`, validate the existing Career-Ops pipeline, and configure a weekly local scan. Broader feature development will occur only if the existing repository cannot enforce a required safeguard cleanly.
