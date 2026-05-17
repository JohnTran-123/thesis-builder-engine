# Plan Skill — Project Plan for Thesis Execution

**Trigger:** User invokes `/plan` (or natural-language equivalent: "let's plan", "build the outline").

**Required upstream artifact (per `rules/approval-discipline.md`):** `workspace/alignment/alignment.md` must exist.

**Output:** ONE file — `workspace/plan/plan.md` — single source of truth for the project plan.

This file contains: document structure, iteration sequencing, all activities (research, data, methodology, drafting, review, submit), risk register, status, and bibliography. **No other files.** Activities are tagged by type but live in one place.

---

## PROCEDURE

### Sub-step 0 — Pre-check

Read:
- `CLAUDE.md` and all 5 rules
- `workspace/alignment/alignment.md` + `constraints.md` + `register.md` + `reference-thesis-analysis.md`
- `workspace/_session.md`
- List `workspace/plan/` for existing `plan.md` (re-planning scenario)

**Artifact-state gating** (per `rules/approval-discipline.md` §3): if `alignment.md` is missing, HARD STOP and direct user to `/align`.

If `plan.md` already exists, this is a RE-PLAN scenario. Surface the existing plan and ask: "Existing plan detected. Are you (a) revising specific iterations, (b) overhauling the outline, or (c) starting fresh? Reply with letter + specifics." HARD STOP.

### Sub-step 1 — Build outline + per-section page budget

Build a chapter-by-chapter outline using:
- Thesis subject (Q4 from alignment) — REQUIRED specific, not generic template
- Recommended outline from `workspace/alignment/reference-thesis-analysis.md`
- Institutional constraints from `workspace/alignment/constraints.md`
- Total page budget (Q6)

For each section, allocate pages inline. Reserve ~10% for compression headroom.

**Required outline elements:**
- A Limitations section (per `rules/honest-scope.md`) — at least 1–2 pages, more for narrow-scope theses
- Per-section one-line description of what the section ARGUES (not just what it covers)

Present the proposed outline. **HARD STOP.** Wait for "Approved" or revision.

### Sub-step 2 — Sequence iterations

Once outline approved, sequence the work:
- Group sections into iterations (typically 3–5 for an 80-page thesis; no fixed cadence)
- Name each iteration by milestone delivered (e.g., "Theoretical foundation", "Field implementation")
- Identify risk slices (why each boundary is where it is)

### Sub-step 3 — Activities per iteration (tagged tasks)

For each iteration, produce a tagged task list covering ALL activity types:

| **TAG** | **WHAT** |
| :------ | :------- |
| `[research]` | Literature searches, paper summaries, snowballing, citation gap analysis |
| `[data]` | IRB confirmation, data extraction, anonymization, interview scheduling, transcription |
| `[methodology]` | Model selection, evaluation framework, statistical methods setup, tooling |
| `[draft]` | Drafting sections (one task per section in the outline) |
| `[review]` | `/content-review` and `/ai-review` runs |
| `[submit]` | `/presubmit` and external checks (in final iteration) |

**Rule:** every task must be concrete enough that a single `/execute` invocation can complete it. "Read literature" is too vague; "Read + summarize 8 highest-relevance papers on rural diagnostic ML" is concrete.

**Rule:** for `[data]` and `[methodology]` tasks: they must be sequenced BEFORE the `[draft]` tasks that depend on them. Example: data extraction must complete before the methodology section can be drafted faithfully.

### Sub-step 4 — Advisor-style critical pushback (REQUIRED before phase-transition gate)

Before the final gate, the engine must surface the following for user response. These are advisor-level moves the engine does NOT skip:

1. **Are draft RQs articulated?** Propose 2–3 candidate research questions derived from Q4 subject. Ask user to pick or refine.
2. **State contribution in ONE sentence.** Force a single-sentence contribution claim. ("This thesis contributes X to Y by doing Z.")
3. **For empirical / applied / case-study theses (from Q1):**
   - Is there a methodology section addressing data-leakage / cross-sample validation?
   - Is baseline comparison addressed (vs. what is the contribution measured)?
   - Is statistical-power / confidence-interval handling reserved?
   - Are per-disease / per-case / per-population data-density checks scheduled as iteration tasks?
4. **Is the heaviest argument chapter weighted appropriately** (typically Ch3 findings or Ch4 contribution should get the largest page allocation, NOT the literature review)?
5. **Are honest-scope hooks threaded into findings chapters** (per `rules/honest-scope.md`), not just concentrated in Limitations?
6. **Is per-iteration task list complete** (not just section coverage)? Each iteration should list 3–8 concrete tasks across activity tags.
7. **Per-section data adequacy:** are there sections that depend on data the user hasn't confirmed exists?
8. **Committee read-path:** for each chapter, name the primary committee reader (from Q11) and confirm that reader's expertise area is addressed.

Surface these as a checklist. HARD STOP. Walk through with user until each is resolved or explicitly deferred.

### Sub-step 5 — Risk register

Capture risks specific to this thesis. Format:

| **#** | **RISK** | **PROBABILITY** | **IMPACT** | **MITIGATION** | **RESOLVED BY ITERATION** |

Risks must be Maya-specific, not generic. Examples:
- "IRB extension expires mid-Iteration 1" not "IRB delay"
- "Anemia data sparse vs. diabetes/hypertension" not "data issues"
- "ML jargon AI-detection score > 25%" not "AI detection issues"

### Sub-step 6 — Bibliography section (initial state)

Add an empty `## Bibliography` section to `plan.md`. Format expected per the citation style from `constraints.md` Q8 (APA 7, Harvard, etc.).

If the user has key sources to seed, ask for them. Otherwise, recommend invoking `agents/research-assistant.md` during Iteration 1. **Per `rules/no-fabricated-citations.md`, do NOT auto-populate citations from memory.**

Each bibliography entry, when added, must follow the format:
```
[citation in chosen style]
Summary: [1-3 sentence relevance summary]
Status: [verified / [NEEDS VERIFICATION]] | Source: [URL / file path / database]
```

### Sub-step 7 — Status section (initial state)

Initialize the status section:
- Current iteration: 1
- Tasks: 🔴 [count] To Do · 🔵 0 Draft · 🟢 0 Approved · 🟠 0 Stuck · ⚪ 0 Pending
- Pages: 0 / [Q6] drafted
- Weeks elapsed / remaining: 0 / [Q7]

### Sub-step 8 — Compile and write `plan.md`

Assemble all sections into `workspace/plan/plan.md`. Section order:

1. Title / metadata
2. Document structure (outline + page budget inline)
3. Iterations (with tagged activities + deliverable per iteration)
4. Risk register
5. Bibliography (initially empty section with format note)
6. Status

### Sub-step 9 — Phase-transition gate

Present:

> "Plan complete. **Single source of truth:** `workspace/plan/plan.md`.
> - Outline: [N sections, 80 pages]
> - Iterations: [M] with [T] total tagged activities
> - Risks: [R] tracked with mitigations
> - Advisor pushback checklist: [X resolved / Y deferred]
>
> Reply **'Proceed'** to start `/execute`, **'Re-plan'** to revise, or **'Seed bibliography'** to invoke the research-assistant first."

**HARD STOP** until the user replies.

---

## ARTIFACT CONTRACT

| **FILE** | **CONTAINS** |
| :------- | :----------- |
| `workspace/plan/plan.md` | Single source of truth: outline + iterations + activities + risks + bibliography + status |

No other files. Activities of all types (research, data, methodology, drafting, review, submit) are tagged within `plan.md`.

---

## SESSION CHECKPOINT

On final approval ("Proceed"), update `workspace/_session.md`:
- Phase: PLAN → EXECUTE
- Last action: `/plan` complete ([date])
- Last approved: [date], evidence = `workspace/plan/plan.md`
- Next step: invoke `/execute` for the first 🔴 To Do task in Iteration 1
- Open blockers: list any DEGRADED states or unresolved advisor-pushback items

---

## WHY ONE FILE

Per `rules/approval-discipline.md` §5 (simplicity discipline) and the engine's design principle of single source of truth: split files create multiple places to look for "what do I do next?" Plan activities are activities — research, data, methodology, drafting are all just tasks. Splitting them by activity type creates redundancy (cross-references between files) and cognitive load (which file holds the IRB task?). One file. Tagged. Searchable. Single source.

---

**See also:**
- `rules/approval-discipline.md` — micro-task pattern, artifact-state gating, hard-stops
- `rules/honest-scope.md` — Limitations section is mandatory; threading into findings
- `rules/no-fabricated-citations.md` — bibliography integrity
- `skills/align/SKILL.md` — upstream
- `skills/execute/SKILL.md` — downstream
- `agents/research-assistant.md` — bibliography seeding + literature work
