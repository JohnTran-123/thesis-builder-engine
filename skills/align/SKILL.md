# Align Skill — Alignment Intake (the DIFFERENTIATOR)

**Purpose:** Establish project constraints, register baseline, and structural patterns BEFORE any drafting. Alignment is the engine's hard gate. No `/plan`, `/execute`, or any drafting may run until alignment outputs exist in `workspace/alignment/`.

**Trigger:** User invokes `/align` (or natural-language equivalents: "let's align", "help me start", "new project"). The engine also suggests `/align` automatically when `workspace/alignment/` is empty.

**Required upstream:** `QUESTIONNAIRE.md` at repo root (partially or fully filled). Optional but strongly recommended: user uploads (reference theses, sample papers) in `workspace/inputs/`.

**Approval gates inside this skill:** you will be asked **"Approved"** up to 3 times during `/align` (after walkthrough, after reference-thesis analysis, after register calibration), then **"Proceed"** at the final phase-transition gate. If the questionnaire is fully answered and no upload analysis is needed, gates collapse to a single "Proceed".

**Output:** 4 files in `workspace/alignment/`:
- `alignment.md` — consolidated alignment record
- `constraints.md` — confidentiality + length + time + citation constraints
- `register.md` — register baseline extracted from sample papers
- `reference-thesis-analysis.md` — outline patterns extracted from reference theses

---

## PROCEDURE

### Sub-step 0 — Pre-check

Read in this order:
- `CLAUDE.md` (engine dispatcher)
- All 5 files in `rules/` (always-loaded constraints)
- `QUESTIONNAIRE.md` at repo root (user's intake answers)
- `workspace/_session.md` (session state, in case this is a resumption)
- List files in `workspace/inputs/` (uploaded reference theses + sample papers)
- List files in `workspace/alignment/` (to detect re-alignment scenarios)

**Three pre-check gates:**

1. **Empty questionnaire:** If `QUESTIONNAIRE.md` is missing or completely blank, HARD STOP. Tell user: "Open `QUESTIONNAIRE.md` at the repo root, fill what you can, then say 'let's align'. Blanks are OK — I'll walk you through them."

2. **Re-alignment detected:** If `workspace/alignment/` already contains `alignment.md` (prior run), this is a RE-ALIGNMENT scenario. Surface the existing files to the user and ask: "Existing alignment outputs detected. Are you (a) updating specific items that changed, or (b) starting fresh? Reply with (a) and the items, or (b)." HARD STOP and route accordingly.

3. **File-vs-questionnaire cross-check:** After listing `workspace/inputs/`, cross-check filenames against Q9 (reference theses) and Q10 (sample papers). If the user named files that are NOT actually present in `workspace/inputs/`:
   - List the missing files explicitly
   - Tell the user: "You named these files in QUESTIONNAIRE.md but they are not in `workspace/inputs/`: [list]. Either upload them to `workspace/inputs/` and say 'continue', or remove them from Q9/Q10 in `QUESTIONNAIRE.md`. Without uploads, register and reference-thesis analysis will be DEGRADED."
   - HARD STOP. Wait for the user's response.

   Do NOT proceed past Sub-step 0 with named-but-missing files — that path leads to hallucinated analysis.

### Sub-step 1 — Parse questionnaire answers

For each of the 12 questions in `QUESTIONNAIRE.md`, classify:
- **ANSWERED**: user has filled in a non-trivial response
- **BLANK**: empty
- **AMBIGUOUS**: present but unclear or contradictory

For ANSWERED items: capture the value.
For BLANK or AMBIGUOUS items: queue for Sub-step 2 walkthrough.

**Q10 normalization:** the register field expects one of four canonical labels (`strict formal academic`, `standard academic`, `practitioner-academic mixed`, `other`). If the user typed a near-match (e.g., "Standard academic register" or "standard"), normalize to the canonical label. If genuinely ambiguous, treat as AMBIGUOUS and queue for Sub-step 2.

### Sub-step 2 — Conversational walkthrough (THE DIFFERENTIATOR)

**Fast-path:** If Sub-step 1 found ZERO blanks and ZERO ambiguities, skip the walkthrough. Present the parsed-questionnaire snapshot directly to the user and ask "Approved?" — proceed to Sub-step 3 on approval.

**Otherwise**, for each BLANK or AMBIGUOUS item, ask the user. Order by importance:

1. **Q1 (thesis type)** — required, blocks everything downstream
2. **Q4 (thesis subject)** — required, anchors the title and RQs
3. **Q10 (register + sample papers)** — required for register calibration
4. **Q6, Q7, Q8 (length, time, school requirements)** — required for page budget + iteration plan
5. **Q9 (reference theses)** — strongly encouraged
6. **Q2, Q3 (institutional context, subject domain)** — required
7. **Q5 (confidentiality)** — required, sets boundaries
8. **Q11, Q12 (audience, available evidence)** — required for scope

For each ambiguous item, offer concrete clarification:

> "You wrote 'X' for Q6. Did you mean: (a) total length is X pages, (b) X is per chapter, or (c) something else?"

For blanks, offer scaffolding (do NOT auto-fill; ask):

> "Q4 is blank. Describe your thesis in 1–2 sentences. If you're not sure yet, give me the working title and topic area — we can refine the subject statement during planning."

**Honest-scope prompts surface HERE (during walkthrough), not later:** while walking through Q5 (confidentiality) and Q12 (available evidence), raise scope-disclosure constraints per `rules/honest-scope.md`. Examples:

> "You said you don't have empirical data (Q12). The thesis cannot make data-driven claims. Per `rules/honest-scope.md`, claims must be labeled 'aspirational' or 'theoretical'. We'll bake this into `/plan` and `/execute`."

> "You said the host firm is confidential (Q5). We'll anonymize and disclose limitations on generalizability."

These constraints feed into `constraints.md` and become hard rules for downstream skills. (Sub-step 7 below restates this as a checklist for clarity.)

HARD STOP at the end of Sub-step 2. Present the filled questionnaire snapshot and ask for **"Approved"** before proceeding.

### Sub-step 3 — Analyze reference theses (if Q9 is populated)

For each PDF in `workspace/inputs/` that the user identified as a reference thesis:

Load the methodology from `skills/align/references/reference-thesis-extraction.md`. If the analysis is heavy (many theses, large PDFs), spawn `agents/research-assistant.md` for parallel processing.

Extract:
- Chapter / section hierarchy (depth, count, naming convention)
- Section length distribution (pages per chapter)
- Argument structure per chapter
- Common transitional patterns
- Citation density (cites per page)
- First-person frequency

Write findings to `workspace/alignment/reference-thesis-analysis.md`. Include:
- Source files analyzed
- Common outline shape (what every reference thesis does)
- Variation (where reference theses differ)
- Recommended outline for this user
- Implications for `/plan`

HARD STOP. Present the analysis and ask for **"Approved"**.

**If no reference theses uploaded:** skip this sub-step and write `workspace/alignment/reference-thesis-analysis.md` with the note: "No reference theses provided. `/plan` will use a generic outline based on Q1 (thesis type). Strongly recommend uploading 1–3 reference theses to `workspace/inputs/` and re-running `/align` for better calibration."

### Sub-step 4 — Calibrate register (from Q10 + sample papers)

For each PDF in `workspace/inputs/` that the user identified as a sample paper (Q10), plus the user's register choice:

Load methodology from `skills/align/references/register-calibration.md`.

Extract from sample papers:
- Sentence-length distribution (mean, range, variance)
- First-person frequency (% of sentences)
- Definition density (% of sentences that define a term)
- Theory-cite ratio (cites per page)
- Hedging frequency
- Transition patterns (top 10 phrases)

Write findings to `workspace/alignment/register.md`. Include:
- Target register declared by user (Q10)
- Quantitative profile from sample papers
- ESL or native baseline diagnosis
- Examples of register-fitting sentences (drawn verbatim from samples, used as voice anchors)
- Anti-patterns to avoid

If sample-paper profile contradicts declared register (e.g., user said "strict formal" but samples show 8% first-person), SURFACE the discrepancy:

> "You declared 'strict formal' but the samples have 8% first-person frequency, suggesting practitioner-academic. Which actually matches what your advisor expects?"

HARD STOP. Present the register baseline and ask for **"Approved"**.

### Sub-step 5 — Consolidate constraints (Q5, Q6, Q7, Q8)

Write `workspace/alignment/constraints.md`:

- **Confidentiality** (Q5): what cannot be disclosed
- **Length** (Q6): total page budget + per-section budgets if specified
- **Time** (Q7): deadline + hours-per-week + total weeks remaining + computed pages-per-week pace
- **Citation style** (Q8): which style to use, with format example
- **Language requirements** (Q8): English-only, native, bilingual
- **File format** (Q8): .docx, LaTeX, PDF
- **AI-detection threshold** (Q8): the score that must not be exceeded
- **Plagiarism threshold** (Q8): the similarity that must not be exceeded

This file is referenced by `/plan` (page-budget), `/execute` (length per task), `/presubmit` (final-check thresholds).

### Sub-step 6 — Produce consolidated alignment record

Write `workspace/alignment/alignment.md` — the consolidated record. Include:

- All 12 question answers (final values after Sub-step 2 walkthrough)
- Date completed
- Pointers to: `constraints.md`, `register.md`, `reference-thesis-analysis.md`
- Open items / blockers / unverified facts

This is the ONE file downstream skills load to get the full alignment picture.

### Sub-step 7 — Honest-scope checklist (SUMMARY)

NOTE: The honest-scope prompts fire DURING Sub-step 2 (the walkthrough). This sub-step is the checklist used during walkthrough — listed here for reference and for cases where Sub-step 2 was fast-pathed and the prompts need separate surfacing.

Per `rules/honest-scope.md`, raise these prompts when relevant:

- **If Q5 names confidential entities:** "We'll anonymize and disclose limitations on generalizability."
- **If Q12 lacks empirical data:** "Claims must be labeled 'aspirational' or 'theoretical' rather than data-driven."
- **If Q8 sets a tight AI-detection threshold:** "Plan for extra `/ai-review` iterations. Author-rewriting per Meta-Rule Zero is mandatory."
- **If Q6 page budget is very tight relative to Q4 scope:** "Scope may need to narrow during `/plan`; flag for re-alignment if so."

These constraints feed into `constraints.md` and become hard rules for downstream skills.

### Sub-step 8 — Phase-transition gate

Present:

> "Alignment complete. Outputs in `workspace/alignment/`:
> - `alignment.md` (consolidated record)
> - `constraints.md` (length, time, citation, thresholds, confidentiality)
> - `register.md` (register baseline from N sample papers)
> - `reference-thesis-analysis.md` (outline patterns from M reference theses)
>
> Reply **'Proceed'** to run `/plan` and build your outline, or **'Re-plan'** to revise the alignment first."

HARD STOP until user replies.

---

## ARTIFACT CONTRACT

| **FILE** | **CONTAINS** | **READ BY** |
| :------- | :----------- | :---------- |
| `workspace/alignment/alignment.md` | Consolidated 12-Q answers + pointers | all downstream skills |
| `workspace/alignment/constraints.md` | Confidentiality, length, time, citation style, thresholds | `/plan`, `/execute`, `/presubmit` |
| `workspace/alignment/register.md` | Register baseline (quantitative + voice-anchor examples) | `/execute`, `/content-review`, `/ai-review` |
| `workspace/alignment/reference-thesis-analysis.md` | Outline patterns + recommended outline | `/plan` |

---

## SESSION CHECKPOINT

Per `rules/approval-discipline.md` §4, checkpointing happens at phase boundaries (final "Proceed"), not at every intra-skill "Approved". The 3 intra-skill approvals (after walkthrough, reference analysis, register calibration) DO commit work to `workspace/alignment/` but do NOT update `_session.md` — that happens only on the final "Proceed".

On final approval ("Proceed"), update `workspace/_session.md`:
- Phase: ALIGN → PLAN
- Last action: `/align` complete ([date])
- Last approved: [date], evidence = `workspace/alignment/alignment.md`
- Next step: invoke `/plan` to build the outline
- Open blockers: list any DEGRADED states (no samples, no reference theses) or unresolved Q5/Q12 disclosure items

---

## RE-ALIGNMENT (looping back)

After the workflow has started, the user may need to revisit alignment if:
- Scope changes (Q4 substantively shifts)
- Confidentiality changes (Q5: new constraints emerge)
- Length / deadline changes (Q6 / Q7)
- Advisor demands a different register

Re-invoke `/align` on the changed items only (Sub-step 2 walkthrough). Update affected output files. Do NOT discard previous alignment work — flag it as superseded with the date of change.

---

**See also:**
- `rules/approval-discipline.md` — artifact-state gating, hard-stops
- `rules/honest-scope.md` — disclosure requirements
- `rules/register-fidelity.md` — register-baseline use downstream
- `skills/align/references/register-calibration.md` — calibration methodology
- `skills/align/references/reference-thesis-extraction.md` — reference-thesis analysis methodology
- `agents/research-assistant.md` — for heavy reference-thesis analysis
- `QUESTIONNAIRE.md` — the 12-question intake form
- `skills/plan/SKILL.md` — downstream skill
