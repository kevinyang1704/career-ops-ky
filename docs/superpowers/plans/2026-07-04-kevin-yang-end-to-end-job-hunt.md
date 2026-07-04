# Kevin Yang End-to-End Job Hunt Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Configure Career-Ops around Kevin's approved July 4, 2026 resume, evidence-locked tailoring, curated weekly discovery, and explicit human approval gates.

**Architecture:** Keep all personal facts and preferences in the Career-Ops v1.16 user layer: `cv.md`, `article-digest.md`, `config/profile.yml`, `modes/_profile.md`, `modes/_custom.md`, and `portals.yml`. Reuse the repository's existing scanner, evaluator, PDF generator, tracker, and integrity scripts; store narrative and archetypes in `_profile.md`, and enforce Kevin's stricter provenance and approval procedure through `_custom.md`, which survives system updates. Configure the weekly run as a Codex automation rooted at this repository, with no autonomous application or communication behavior.

**Tech Stack:** Markdown, YAML, Node.js 24, js-yaml, Playwright/Chromium, Career-Ops CLI scripts, Codex recurring automations.

**Privacy and Git rule:** The user-layer files are intentionally ignored by Git and contain personal data. Never force-add or commit them. The repository already contains unrelated staged and unstaged changes; every Git operation must use an explicit pathspec, and no implementation commit may absorb those changes.

---

## File Structure

| Path | Action | Responsibility |
|---|---|---|
| `cv.md` | Replace | Canonical evidence-rich master CV, initially limited to facts in the approved 2026 resume |
| `article-digest.md` | Create | Approved proof-point ledger with stable evidence IDs and source citations |
| `.gitignore` | Modify | Keep approved evidence and interview stories local and out of the upstream repository |
| `config/profile.yml` | Modify | Target roles, America/New_York timezone, compensation, location, and deal-breakers |
| `modes/_profile.md` | Modify | Kevin-specific archetypes, narrative, and evidence mappings |
| `modes/_custom.md` | Modify | Evidence-lock, clarification interview, change-log, and approval gates |
| `portals.yml` | Modify | Curated US senior-leadership discovery filters, queries, and target employers |
| `jds/evidence-lock-smoke-test.md` | Create temporarily | Local fictional JD used to test supported, clarify, and true-gap behavior; remains ignored |
| `data/pipeline.md` / `data/scan-history.tsv` | Scanner-managed | Weekly and manual discovery queue and deduplication history |
| Codex automation | Create | Weekly Monday 8:00 AM America/New_York scan against the moved repository |

No system-layer prompt or JavaScript file should be changed unless the user override demonstrably fails to enforce an approved requirement during the smoke test.

### Task 1: Establish a Safe Baseline

**Files:**
- Read: `docs/superpowers/specs/2026-07-04-kevin-yang-end-to-end-job-hunt-design.md`
- Read: `DATA_CONTRACT.md`
- Read: `.gitignore`
- Inspect: all existing worktree changes

- [ ] **Step 1: Confirm the working directory and repository identity**

Run:

```powershell
git -c safe.directory='C:/Projects/Job Hunt/career-ops-ky' rev-parse --show-toplevel
git -c safe.directory='C:/Projects/Job Hunt/career-ops-ky' branch --show-current
```

Expected:

```text
C:/Projects/Job Hunt/career-ops-ky
main
```

- [ ] **Step 2: Capture the pre-existing change set without modifying it**

Run:

```powershell
git -c safe.directory='C:/Projects/Job Hunt/career-ops-ky' status --short
git -c safe.directory='C:/Projects/Job Hunt/career-ops-ky' diff --name-only
git -c safe.directory='C:/Projects/Job Hunt/career-ops-ky' diff --cached --name-only
```

Expected: the previously observed Career-Ops update/customization changes remain present. Save the three outputs in the execution transcript; do not stage, unstage, stash, reset, or commit them.

- [ ] **Step 3: Record hashes for the personal files before editing**

Run:

```powershell
Get-FileHash cv.md,config/profile.yml,modes/_profile.md,modes/_custom.md,portals.yml -Algorithm SHA256 |
  Select-Object Path,Hash | Format-Table -AutoSize
```

Expected: four SHA-256 hashes. These are the rollback reference for this implementation.

- [ ] **Step 4: Run read-only baseline validation**

Run:

```powershell
node cv-sync-check.mjs
node verify-pipeline.mjs
```

Expected: CV sync passes. Pipeline verification exits successfully or reports pre-existing tracker diagnostics that are captured before any implementation edit.

### Task 2: Synchronize the Evidence-Rich Master CV

**Files:**
- Source: `../Kevin_Yang_Resume_2026_ProductLeader.md`
- Modify: `cv.md`
- Create: `article-digest.md`

- [ ] **Step 1: Replace stale `cv.md` content with the approved resume**

Use `apply_patch` to make `cv.md` match `../Kevin_Yang_Resume_2026_ProductLeader.md` exactly. Preserve the approved facts, including:

```text
$1B+ in incremental value
35+ product lines
3,000+ practitioners
80% YoY velocity increase
180B+ monthly events
50-person Data and Analytics team
20+ reusable data products
20% solicitation email reduction with performance held constant
~$500M incremental annual balance-transfer volume
```

Do not carry forward these superseded or unapproved phrasings from the old `cv.md`:

```text
160B+ monthly customer and product decisions
architecting its core features and statistical engine
driving personalization, higher customer conversion, and lower cost to serve
increased share of wallet
```

- [ ] **Step 2: Verify exact synchronization**

Run:

```powershell
$source = Get-Content -Raw '..\Kevin_Yang_Resume_2026_ProductLeader.md'
$master = Get-Content -Raw '.\cv.md'
if ($source -cne $master) { throw 'cv.md does not exactly match the approved source resume' }
'PASS: cv.md exactly matches the approved 2026 resume'
```

Expected:

```text
PASS: cv.md exactly matches the approved 2026 resume
```

- [ ] **Step 3: Create the approved proof-point ledger**

Use `apply_patch` to create `article-digest.md` with this exact initial structure and content:

```markdown
# Kevin Yang Approved Evidence Ledger

Only facts explicitly approved by Kevin may appear here. Tailored materials may rephrase these facts narrowly but may not strengthen, combine, or infer beyond them.

## Experimentation Platform

- **EXP-001 — Enterprise value:** Scaled an enterprise experimentation platform from zero to more than $1B in incremental value. Source: `cv.md`, Professional Summary and JPMorganChase — Experimentation and Measurement.
- **EXP-002 — Adoption:** Scaled adoption to more than 3,000 practitioners across product, engineering, data science, and design. Source: `cv.md`, JPMorganChase — Experimentation and Measurement.
- **EXP-003 — Organizational reach:** Platform operated across more than 35 product lines. Source: `cv.md`, Professional Summary.
- **EXP-004 — Velocity:** Increased experimentation velocity 80% year over year. Source: `cv.md`, JPMorganChase — Experimentation and Measurement.
- **EXP-005 — Data scale:** Built resilient data infrastructure processing more than 180 billion monthly events and supporting thousands of concurrent feature flags and large-scale experiments. Source: `cv.md`, JPMorganChase — Experimentation and Measurement.
- **EXP-006 — Measurement:** Built a centralized metrics repository that standardized enterprise measurement and aligned executives on success and guardrail metrics. Source: `cv.md`, JPMorganChase — Experimentation and Measurement.
- **EXP-007 — AI/ML integration:** Led the design and integration of experimentation with AI/ML recommendation systems and GenAI features. Source: `cv.md`, JPMorganChase — Experimentation and Measurement.

## Audience and Analytics Leadership

- **ANL-001 — Team leadership:** Led a 50-person Data and Analytics team. Source: `cv.md`, JPMorganChase — Audience Management and Customer Insights.
- **ANL-002 — Reusable products:** Built more than 20 reusable data products combining ML-identified features and business-rule attributes. Source: `cv.md`, JPMorganChase — Audience Management and Customer Insights.
- **ANL-003 — Applied ML:** Used XGBoost and Random Forest to identify features predictive of outcomes including email opens and offer activation. Source: `cv.md`, JPMorganChase — Audience Management and Customer Insights.
- **ANL-004 — Targeting efficiency:** Reduced solicitation email volume 20% while holding campaign performance constant. Source: `cv.md`, JPMorganChase — Audience Management and Customer Insights.

## Financial Products and Markets

- **FIN-001 — Pricing impact:** Designed and implemented tiered pricing for balance-transfer programs, generating approximately $500 million in incremental annual transfer volume. Source: `cv.md`, JPMorganChase — Home Lending and Credit Card Marketing.
- **FIN-002 — Markets foundation:** Developed quantitative modeling, pattern-recognition, and signal-detection skills through options and securities trading from 2007 to 2014. Source: `cv.md`, Early Career in Financial Markets.

## Approval Protocol

New evidence follows this sequence:

1. A role evaluation identifies a potentially relevant but unsupported requirement.
2. Career-Ops asks Kevin one focused question.
3. Career-Ops writes a proposed normalized fact statement with no embellishment.
4. Kevin explicitly approves, edits, or rejects that statement.
5. Only an approved statement receives an evidence ID and becomes reusable.
```

- [ ] **Step 4: Check the master sources for stale or unsupported language**

Run:

```powershell
rg -n '160B|share of wallet|higher customer conversion|architecting its core features|statistical engine' cv.md article-digest.md
rg -n '180B\+|EXP-005|ANL-001|FIN-001' cv.md article-digest.md
```

Expected: the first command returns no matches. The second returns the current approved metric and evidence IDs.

- [ ] **Step 5: Verify the migrated privacy boundary**

Run:

```powershell
git -c safe.directory='C:/Projects/Job Hunt/career-ops-ky' check-ignore -v cv.md article-digest.md config/profile.yml modes/_profile.md modes/_custom.md portals.yml interview-prep/story-bank.md
Test-Path interview-prep/story-bank.md
git -c safe.directory='C:/Projects/Job Hunt/career-ops-ky' status --short -- cv.md article-digest.md config/profile.yml modes/_profile.md modes/_custom.md portals.yml interview-prep/story-bank.md
```

Expected: all personal files are ignored, the migrated story-bank file still exists locally, and none appears in Git status. Never force-add `cv.md`, `article-digest.md`, profile, custom instructions, portal configuration, tracker data, reports, generated files, or interview stories.

### Task 3: Align Kevin's Profile and Install the Evidence Lock

**Files:**
- Modify: `config/profile.yml`
- Modify: `modes/_profile.md`
- Modify: `modes/_custom.md`

- [ ] **Step 1: Update profile facts and timezone**

Use `apply_patch` to make these exact YAML changes in `config/profile.yml`:

```yaml
target_roles:
  primary:
    - "Director / VP of Product"
    - "Head of Product / Product Lead"
    - "Head of Experimentation, Measurement, or Decision Intelligence"
  archetypes:
    - name: "Director / VP of Product"
      level: "Director / VP / Head"
      fit: "primary"
    - name: "Head of Experimentation, Measurement, or Decision Intelligence"
      level: "Executive Director / Director / Head"
      fit: "primary"
    - name: "VP / Director of Analytics & Data Science"
      level: "VP / Director (Strategic or People Leader)"
      fit: "secondary"

narrative:
  headline: "Executive Product Leader | Enterprise Experimentation, Measurement & Decision Intelligence"
  exit_story: "Conceived, built, and scaled JPMorgan Chase's enterprise experimentation platform from zero to production, reaching 3,000+ practitioners across 35+ product lines and contributing $1B+ in incremental value. My background spans product leadership, data and analytics, financial markets, and enterprise measurement. I am now targeting Director, VP, or Head-level product leadership roles at established technology, fintech, financial-services, or banking organizations."
  superpowers:
    - "Enterprise experimentation and measurement at scale"
    - "Building and scaling platform products from zero to broad adoption"
    - "Decision intelligence, metrics strategy, and executive alignment"
    - "Strategic Data and Analytics leadership"
```

Also set:

```yaml
location:
  timezone: "America/New_York"
```

Keep the existing verified contact information, compensation range, visa status, and location policy unchanged.

- [ ] **Step 2: Replace stale adaptive-framing evidence**

Use `apply_patch` in `modes/_profile.md` so the three adaptive-framing rows cite approved evidence IDs rather than unsupported or stale summaries:

```markdown
| Director / VP of Product | Built an enterprise platform from zero to production; scaled it across 35+ product lines and 3,000+ practitioners; integrated experimentation with AI/ML recommendation systems and GenAI features. | EXP-001, EXP-002, EXP-003, EXP-007 |
| Head of Experimentation, Measurement, or Decision Intelligence | Owned vision, strategy, execution, measurement standards, and enterprise adoption for the experimentation platform; increased velocity 80% YoY and supported 180B+ monthly events. | EXP-004, EXP-005, EXP-006 |
| VP / Director of Analytics & Data Science | Led a 50-person Data and Analytics team; built 20+ reusable data products; applied tree-based ML; reduced solicitation volume 20% while maintaining performance. | ANL-001, ANL-002, ANL-003, ANL-004 |
```

Append this compact gateway to `modes/_profile.md` so modes that already load the profile cannot miss the procedural file:

```markdown
## Mandatory Tailoring Gateway

Before evaluating, tailoring, generating a PDF, or drafting candidate-facing material, read and follow `modes/_custom.md`. Never generate a tailored resume or PDF automatically. Stop for Kevin's explicit approval after the evidence map and again after the complete tailored Markdown and source-linked change log.
```

- [ ] **Step 3: Add the hard evidence and approval policy to the procedural override**

Replace the placeholder content in `modes/_custom.md` with this exact section:

```markdown
## Evidence-Locked Tailoring — Mandatory User Override

This section overrides any system mode that would otherwise generate a tailored CV or PDF automatically.

### Approved sources

A candidate claim is usable only when it is traceable to:

1. `cv.md`;
2. an evidence ID in `article-digest.md`; or
3. a fact statement that Kevin explicitly approved during a role-specific clarification interview and that was then added to `article-digest.md`.

Older resumes, existing reports, model memory, company research, and the job description are not evidence of Kevin's experience.

### Required role flow

1. Map every material JD requirement into **Supported**, **Clarify**, or **True gap**.
2. Cite exact CV language or evidence IDs for every Supported item.
3. Ask one focused question at a time only for Clarify items that could materially improve the application.
4. Convert each answer into a concise proposed fact statement and ask Kevin to approve, edit, or reject it.
5. Do not reuse an interview answer until Kevin approves the normalized statement and it is recorded in `article-digest.md`.
6. Treat True gaps honestly; do not hide them with adjacent experience or keyword substitution.

### Allowed tailoring

- Reorder approved bullets and sections.
- Remove less relevant content.
- Slightly reword approved facts without changing ownership, scope, seniority, tools, metrics, outcomes, or dates.
- Add exact or equivalent JD terminology only when the approved evidence already supports the term.

### Forbidden tailoring

- Never invent a skill, tool, responsibility, project, metric, outcome, date, credential, or reporting relationship.
- Never convert exposure, partnership, leadership, or oversight into hands-on execution.
- Never strengthen or combine claims in a way that creates a new unapproved claim.
- Never infer missing compensation, experience, or impact.
- Never keyword-stuff unsupported requirements.

### Mandatory review gates

- After evaluation, stop and let Kevin select the role before deep research or tailoring.
- Before tailoring, show the Supported / Clarify / True gap evidence map.
- Before PDF generation, show the complete tailored Markdown plus a change log with original text, proposed text, reason, and evidence source. Wait for Kevin's explicit approval.
- Before any application, email, LinkedIn message, form submission, or external communication, stop and wait for Kevin's explicit approval.
- The automatic pipeline must never bypass these gates, even if another mode says to continue after an earlier step fails or to generate a PDF automatically.
```

- [ ] **Step 4: Validate fact consistency across the user layer**

Run:

```powershell
rg -n '160B|monthly decisions|multi-billion|statistical engine|share of wallet' cv.md article-digest.md config/profile.yml modes/_profile.md modes/_custom.md
rg -n '180B\+|50-person|20\+ reusable|Evidence-Locked Tailoring|Mandatory review gates' cv.md article-digest.md config/profile.yml modes/_profile.md modes/_custom.md
node cv-sync-check.mjs
```

Expected: the stale-language search returns no matches; the approved-evidence search returns matches; sync check passes.

### Task 4: Curate Discovery for Senior US Product Leadership

**Files:**
- Modify: `portals.yml`

- [ ] **Step 1: Narrow title inclusion to target leadership roles**

Replace `title_filter.positive` with:

```yaml
  positive:
    - "Director of Product"
    - "Product Director"
    - "VP of Product"
    - "Vice President, Product"
    - "Head of Product"
    - "Product Lead"
    - "Platform Product"
    - "Growth Product"
    - "Personalization"
    - "Experimentation"
    - "Measurement"
    - "Decision Intelligence"
    - "Director of Analytics"
    - "VP of Analytics"
    - "Head of Analytics"
    - "Director of Data Science"
    - "VP of Data Science"
    - "Head of Data Science"
```

Replace `title_filter.negative` with:

```yaml
  negative:
    - "Junior"
    - "Associate Product Manager"
    - "Product Analyst"
    - "Data Analyst"
    - "Business Analyst"
    - "Software Engineer"
    - "Software Developer"
    - "Data Engineer"
    - "Analytics Engineer"
    - "Machine Learning Engineer"
    - "ML Engineer"
    - "MLOps"
    - "DevOps"
    - "Site Reliability"
    - "Solutions Architect"
    - "Solutions Engineer"
    - "Forward Deployed"
    - "Intern"
```

Set `seniority_boost` to:

```yaml
  seniority_boost:
    - "Executive"
    - "Vice President"
    - "VP"
    - "Head"
    - "Director"
```

- [ ] **Step 2: Replace broad or contradictory search queries**

Keep only these enabled `search_queries` entries:

```yaml
search_queries:
  - name: Senior Product Leadership — Major ATS
    query: '(site:jobs.ashbyhq.com OR site:job-boards.greenhouse.io OR site:jobs.lever.co) ("Director of Product" OR "VP of Product" OR "Head of Product") (platform OR growth OR personalization OR AI) (remote OR "New York" OR Philadelphia)'
    enabled: true

  - name: Experimentation and Decision Intelligence Leadership
    query: '(site:jobs.ashbyhq.com OR site:job-boards.greenhouse.io OR site:jobs.lever.co) (experimentation OR measurement OR "decision intelligence") (director OR "vice president" OR head) (remote OR "New York" OR Philadelphia)'
    enabled: true

  - name: Analytics and Data Science Leadership
    query: '(site:jobs.ashbyhq.com OR site:job-boards.greenhouse.io OR site:jobs.lever.co) ("Director of Analytics" OR "VP of Analytics" OR "Head of Analytics" OR "Director of Data Science" OR "VP of Data Science") (remote OR "New York" OR Philadelphia)'
    enabled: true

  - name: Established Fintech Product Leadership
    query: '(Stripe OR Plaid OR Mastercard OR Visa OR PayPal OR Block OR Capital One) ("Director of Product" OR "VP of Product" OR "Head of Product" OR experimentation) jobs'
    enabled: true

  - name: Financial Services Product and Measurement Leadership
    query: '(Goldman Sachs OR Morgan Stanley OR Citi OR "Bank of America" OR "Wells Fargo" OR Barclays OR Vanguard) (product OR experimentation OR measurement OR analytics) (director OR "vice president") jobs'
    enabled: true
```

- [ ] **Step 3: Curate tracked employers**

Set `enabled: true` only for established target employers already represented in `portals.yml` and disable the remaining tracked companies. The initial enabled target set is:

```text
OpenAI
Anthropic
Google
Meta
Microsoft
Amazon
Stripe
Plaid
Brex
Salesforce
Mastercard
Visa
PayPal
Block
Capital One
Goldman Sachs
Morgan Stanley
Citi
Bank of America
Wells Fargo
Barclays
PNC
Vanguard
Comcast
Gopuff
Independence Blue Cross
Cencora
```

If a listed employer lacks a `tracked_companies` entry, do not invent an ATS endpoint. Keep its enabled search query coverage and add a tracked entry only after verifying the branded careers URL and provider format.

- [ ] **Step 4: Parse and inspect the finished YAML**

Run:

```powershell
node -e "const fs=require('fs'),y=require('js-yaml');const p=y.load(fs.readFileSync('portals.yml','utf8')); console.log(JSON.stringify({positive:p.title_filter.positive.length,negative:p.title_filter.negative.length,queries:p.search_queries.filter(q=>q.enabled).length,companies:p.tracked_companies.filter(c=>c.enabled).map(c=>c.name)},null,2))"
```

Expected: 18 positive titles, 18 negative titles, 5 enabled queries, and only verified target companies enabled.

- [ ] **Step 5: Confirm location behavior**

Run:

```powershell
node -e "const fs=require('fs'),y=require('js-yaml');const p=y.load(fs.readFileSync('portals.yml','utf8')); console.log(p.location_filter)"
```

Expected: remote, United States, Philadelphia/Delaware, and New York remain allowed; clearly foreign locations remain blocked. Ambiguous or missing locations remain reviewable rather than being guessed.

### Task 5: Prove the Approval Gate with a Fictional JD

**Files:**
- Create temporarily: `jds/evidence-lock-smoke-test.md`
- Read: `cv.md`
- Read: `article-digest.md`
- Read: `modes/_profile.md`

- [ ] **Step 1: Create the smoke-test JD**

Use `apply_patch` to create:

```markdown
# Director of Product, Decision Platforms — Fictional Test Company

The Director of Product will own strategy for an enterprise decision platform, scale experimentation adoption, align executives on common metrics, and partner with AI/ML recommendation teams.

Requirements:

- Led an enterprise platform from zero to scaled adoption.
- Managed product strategy across multiple business lines.
- Built experimentation and measurement capabilities at high data scale.
- Hands-on production experience with Kubernetes and Terraform.
- Managed a team of 25 product managers.
- Experience integrating AI/ML recommendations into customer products.
```

- [ ] **Step 2: Run the Career-Ops evaluation only through the evidence map**

Invoke Career-Ops on this local JD with this explicit instruction:

```text
Evaluate jds/evidence-lock-smoke-test.md under Kevin's Evidence-Locked Tailoring override. Stop after producing Supported, Clarify, and True gap lists. Do not generate a tailored resume, PDF, application answers, outreach, or tracker update.
```

Expected evidence classification:

```text
Supported: zero-to-scale platform, multi-line strategy, experimentation/measurement at scale, AI/ML recommendation integration.
Clarify: whether Kevin directly managed product managers and, if so, how many; existing approved evidence does not establish this.
True gap: hands-on Kubernetes and Terraform experience unless Kevin supplies and approves new evidence.
```

- [ ] **Step 3: Verify the system stopped at the gate**

Run:

```powershell
Get-ChildItem output,reports -Recurse -File |
  Where-Object LastWriteTime -gt (Get-Date).AddMinutes(-10) |
  Select-Object FullName,LastWriteTime
```

Expected: no new PDF, tailored resume, application answers, or external action from the smoke test.

- [ ] **Step 4: Reject an unsupported rewrite explicitly**

Ask Career-Ops to assess this proposed bullet without adding it:

```text
"Managed 25 product managers and personally deployed Kubernetes and Terraform infrastructure for the experimentation platform."
```

Expected: reject the entire claim as unsupported, identify both unsupported components, and offer one-at-a-time clarification rather than rewriting it into the CV.

### Task 6: Restore PDF Capability and Validate the Existing Pipeline

**Files:**
- Generated test artifact only: `output/`
- No source-file modifications expected

- [ ] **Step 1: Install the repository's required Chromium runtime**

Run:

```powershell
npx playwright install chromium
```

Expected: Chromium installs successfully. This may require explicit network/filesystem approval because Playwright normally writes to the user cache outside the repository.

- [ ] **Step 2: Re-run the doctor**

Run:

```powershell
npm run doctor
```

Expected:

```text
Result: all checks passed.
```

- [ ] **Step 3: Run repository and user-layer integrity checks**

Run:

```powershell
npm run sync-check
npm run verify
node test-all.mjs --quick
```

Expected: sync and pipeline verification pass. Quick tests pass; any failure caused by the pre-existing dirty system-layer work is isolated and reported rather than repaired under this plan.

- [ ] **Step 4: Test approved-draft PDF generation without an application**

After Kevin approves a smoke-test Markdown draft, generate one local PDF using the existing HTML/PDF path. Then run:

```powershell
Get-ChildItem output -Filter '*.pdf' | Sort-Object LastWriteTime -Descending |
  Select-Object -First 1 FullName,Length,LastWriteTime
```

Expected: one non-empty PDF exists. Do not update an application tracker or submit anything for the fictional role.

### Task 7: Configure and Safely Exercise Weekly Discovery

**Files:**
- Scanner-managed: `data/pipeline.md`
- Scanner-managed: `data/scan-history.tsv`
- External: Codex recurring automation configuration

- [ ] **Step 1: Run one manual verified scan**

Record the current hashes first:

```powershell
Get-FileHash data/pipeline.md,data/scan-history.tsv -Algorithm SHA256 |
  Select-Object Path,Hash | Format-Table -AutoSize
```

Then run:

```powershell
node scan.mjs --verify
```

Expected: live, deduplicated jobs matching the configured filters are added to the local review queue; expired listings are recorded but not queued. No evaluation, tailoring, application, or communication occurs.

- [ ] **Step 2: Inspect scan quality before scheduling**

Run:

```powershell
Get-Content data/pipeline.md -Tail 120
Get-Content data/scan-history.tsv -Tail 40
```

Expected: queued roles are predominantly Director/VP/Head product, experimentation, measurement, decision-intelligence, or strategic analytics leadership roles in acceptable locations. If the queue is noisy, adjust only `portals.yml` and repeat the parse checks from Task 4 before another scan.

- [ ] **Step 3: Create the Codex weekly automation**

Use the Codex automation tool to create an enabled workspace automation named `Career-Ops weekly curated scan` with:

```text
Workspace: C:\Projects\Job Hunt\career-ops-ky
Schedule: Every Monday at 8:00 AM America/New_York
Prompt: Run `node scan.mjs --verify` in the Career-Ops workspace. Summarize newly queued live roles, grouped as pursue/consider/manual-review candidates using Kevin's profile constraints. Do not evaluate deeply, tailor resumes, create PDFs, update application status, send messages, fill forms, or submit applications. If the scan fails, report the error and leave existing pipeline data intact.
```

Do not create a duplicate if an automation with this name already exists; inspect and update the existing one instead.

- [ ] **Step 4: Read back the saved automation**

Use the automation view operation on the returned automation ID.

Expected: enabled status, the Career-Ops workspace path, Monday 8:00 AM America/New_York schedule, and the exact safety-limited prompt are present.

- [ ] **Step 5: Final non-interference check**

Run:

```powershell
git -c safe.directory='C:/Projects/Job Hunt/career-ops-ky' status --short
git -c safe.directory='C:/Projects/Job Hunt/career-ops-ky' log -2 --oneline
```

Expected: all pre-existing unrelated changes remain; personal user-layer files remain ignored and uncommitted; no new application or communication was performed. The only repository commits from the design/planning phase are the narrowly scoped design and plan documents.

### Task 8: Run the First Real Role Through the Human-Gated Pipeline

**Files:**
- Scanner/user-managed: `data/pipeline.md`
- Create after evaluation: `reports/{next-id}-{company}-{date}.md`
- Modify after evaluation: `data/applications.md`
- Create after approval: `output/cv-kevin-yang-{company}-{date}.pdf`
- Create after Kevin selects interview preparation: `interview-prep/{company}-{role}.md`
- Modify only with approved new facts: `article-digest.md`

- [ ] **Step 1: Let Kevin choose one live role**

Present the strongest newly queued roles in a compact table containing company, title, location, known compensation, posting age, and source URL. Wait for Kevin to select exactly one role. Do not deep-evaluate every queued role.

- [ ] **Step 2: Evaluate the selected role and save the report**

Run Career-Ops evaluation for the selected role through Blocks A-G. The report must include:

```markdown
## Evidence Map

### Supported
| JD requirement | Exact approved evidence | Source ID or CV section |

### Clarify
| JD requirement | Why clarification could matter | One proposed question |

### True gaps
| JD requirement | Honest gap statement | Impact on candidacy |

## Recommendation

**Decision:** Pursue | Consider | Skip
**Reason:** Evidence-backed summary with no inferred candidate facts.
```

Save the report using the repository's sequential naming convention and add the evaluated role to `data/applications.md` with status `Evaluated` and PDF pending. Do not generate application answers or a PDF yet, even if the system score exceeds 4.5.

- [ ] **Step 3: Conduct clarification only if it can change the application materially**

For each Clarify item, ask one question at a time. After each answer, present this approval object in plain language:

```text
Proposed reusable fact:
Evidence category:
What this does not claim:
Approve, edit, or reject?
```

Only after Kevin says `approve` may the normalized statement be added to `article-digest.md` with the next stable evidence ID. Rejected or unanswered statements remain unusable.

- [ ] **Step 4: Draft the tailored resume with a source-linked change log**

Produce the complete proposed Markdown resume and this exact audit table:

```markdown
## Tailoring Change Log

| Section | Original approved text | Proposed text | Reason | Evidence source |
|---|---|---|---|---|
```

Every proposed line must cite `cv.md` or an approved evidence ID. Mark deletions and reordering as structural changes. Stop and wait for Kevin's explicit approval; silence or general interest is not approval.

- [ ] **Step 5: Generate the PDF only after resume approval**

After Kevin explicitly approves the complete draft, generate the ATS PDF and verify it is non-empty:

```powershell
Get-Item 'output/cv-kevin-yang-{company}-{date}.pdf' |
  Select-Object FullName,Length,LastWriteTime
```

Expected: a non-zero file length and the selected company/date in the filename. Update the tracker's PDF field to complete while leaving application status short of `Applied`.

- [ ] **Step 6: Prepare but do not send the application package**

Draft role-specific form answers and optional outreach using only approved evidence. Display the complete text to Kevin and stop before any browser submission, email send, LinkedIn send, or external write.

- [ ] **Step 7: Activate post-application operations only after Kevin confirms submission**

When Kevin reports that the application was submitted, update the tracker to `Applied`, calculate the follow-up cadence with:

```powershell
node followup-cadence.mjs
```

Then generate company-specific interview preparation if Kevin wants it. Do not claim submission, schedule follow-ups, or create interview stories before Kevin confirms the real-world state.

## Completion Checklist

- [ ] Latest approved resume is the exact initial master CV.
- [ ] Evidence ledger contains only resume-backed claims with stable IDs.
- [ ] Profile and archetypes match senior product leadership goals.
- [ ] Stale 160B and unsupported claim language is absent from the user layer.
- [ ] Evidence-gap interview and explicit approval gates override automatic PDF behavior.
- [ ] Discovery filters and target accounts reflect established US employers and acceptable locations.
- [ ] Fictional JD test rejects unsupported Kubernetes, Terraform, and team-size claims.
- [ ] Playwright Chromium and PDF generation work.
- [ ] Integrity checks pass or pre-existing failures are documented.
- [ ] Manual verified scan produces a useful local review queue.
- [ ] Weekly Monday 8:00 AM America/New_York automation is enabled and read back.
- [ ] One Kevin-selected live role reaches evaluation, evidence review, approved tailoring, and local PDF generation without external submission.
- [ ] No application, message, email, or form was submitted.
