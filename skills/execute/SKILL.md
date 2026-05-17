# Execute Skill — Micro-Task Drafting

**Purpose:** Draft exactly ONE section at a time, present evidence, hard-stop for User approval. No batching. No silent advance to the next section.

**Trigger:** User invokes `/execute` (or "draft chapter N", "let's write section X", "next task").

**Required upstream (per `rules/approval-discipline.md`):** `workspace/alignment/alignment.md` AND `workspace/plan/plan.md` must exist.

**Output:** One drafted section in `workspace/drafts/<section-name>.md`.

---

## PROCEDURE

### Sub-step 0 — Pre-check

Read in this order:
- `CLAUDE.md` (engine dispatcher)
- All 5 files in `rules/` (always-loaded)
- `workspace/alignment/alignment.md` + `register.md` + `constraints.md` + `reference-thesis-analysis.md`
- `workspace/plan/plan.md` (single source of truth — outline + iterations + activities + risks + bibliography)
- `workspace/_session.md` (session state)

Verify upstream artifacts exist (artifact-state gating per `rules/approval-discipline.md` §3). If missing, HARD STOP and direct the user to the prior step.

### Sub-step 1 — Identify the active task

From the Iterations section of `workspace/plan/plan.md`, find the first **🔴 To Do** task in the current iteration.

**Important:** tasks may be tagged `[research]`, `[data]`, `[methodology]`, `[draft]`, `[review]`, or `[submit]`. `/execute` is primarily for `[draft]` tasks. For other tag types:
- `[research]` — consider spawning `agents/research-assistant.md` instead
- `[data]` / `[methodology]` — these are typically user-completed activities outside the engine (e.g., running scripts, conducting interviews); `/execute` confirms completion + marks 🟢
- `[review]` / `[submit]` — direct user to the appropriate skill (`/content-review`, `/ai-review`, `/presubmit`)

If no 🔴 To Do task exists:
- Check if the current iteration has unmarked tasks. If so, mark the first as 🔴 and proceed.
- If the current iteration is complete (all 🟢), present the phase-transition gate per `rules/approval-discipline.md` §6. Ask the user to "Proceed" to next iteration or "Re-plan".

If the next 🔴 To Do is in a higher iteration than the last 🟢 Reviewed (iteration boundary), HARD STOP and present the phase-transition gate per `rules/approval-discipline.md` §7.

### Sub-step 2 — Mark active + announce

Update the Iterations section of `workspace/plan/plan.md`: the active task stays 🔴 To Do until completed.

Announce to the user:

> "Now executing: **[task name]** [tag]. Section / scope: **[id]**. Page budget (if [draft]): **[allocated pages]**. Register: per `workspace/alignment/register.md`."

### Sub-step 3 — Draft the section

Apply ALL of the following rules while drafting:

- **Register fidelity** (`rules/register-fidelity.md`): match the baseline in `workspace/alignment/register.md`. Use voice-anchor sentences as reference. Use top transitions from `reference-thesis-analysis.md`.
- **Honest scope** (`rules/honest-scope.md`): label validated / observation / aspirational claims explicitly. Do NOT over-claim.
- **No fabricated citations** (`rules/no-fabricated-citations.md`): every citation has source-access metadata. Unverified → `[NEEDS VERIFICATION]`.
- **Page budget** (`constraints.md`): stay within allocated length. If running long, ask the user before continuing.
- **Outline contract** (`plan.md` Document Structure section): cover the section's planned content. If you want to deviate, ASK first.

If the section requires sources the bibliography (in `plan.md`) lacks:
- Either spawn `agents/research-assistant.md` to find them, OR
- HARD STOP and direct user to invoke research assistant before drafting

If the section has unresolved questions (e.g., methodology specifics the user hasn't decided):
- Mark the task **🟠 Stuck** in `plan.md`.
- Surface the question to the user. HARD STOP.

### Sub-step 3.5 — Plan adherence + drift check (mandatory before writing)

Before writing the draft to disk, verify these two non-redundant checks. The full anti-AI audit (5 principles, 29 patterns, voice gaps) and content-quality audit are `/ai-review` and `/content-review`'s jobs — do NOT duplicate them here.

**A. Plan adherence** (against `workspace/plan/plan.md`):
- Does the draft cover the section's argument descriptor from the Document Structure table?
- Are required subsections / required outline elements present?
- Page budget within ±10% of allocation?
- Bridge to next section present at end?

**B. Drift check vs. alignment** (against `workspace/alignment/*.md`):
- Subject still maps to Q4 (thesis subject)?
- Confidentiality (Q5) respected — anonymized refs where required, no real names where Q5 forbids?
- Register declaration (Q10) applied — baseline metrics from `register.md` referenced during drafting?
- Claims within Q12 evidence scope — no over-reaching what the available data can support?

If any check fails, REVISE the draft before Sub-step 4. Surface to the user only the items that pass; do not silently fix drift issues.

### Sub-step 4 — Write to draft file

Output: `workspace/drafts/<section-name>.md`. Use the section ID from `plan.md` Document Structure (e.g., `ch1-section1.md` or `ch1.md` if a whole chapter is one task).

Include at top of file:
```
<!-- Generated: <date> | Task ID: <id> | Section: <outline section ref> | Status: 🔵 Draft Completed -->
```

### Sub-step 5 — Update task status

Update the Iterations and Status sections of `workspace/plan/plan.md`:
- The active task: 🔴 To Do → **🔵 Draft Completed**.
- Add evidence pointer: file path `workspace/drafts/<section-name>.md`.
- Update Status section counts (🔴/🔵/🟢/🟠/⚪) and pages-drafted total.

### Sub-step 6 — Present evidence + hard-stop

Present to the user:

> "Section **[name]** drafted.
>
> - **File:** `workspace/drafts/<section-name>.md`
> - **Length:** [N] words / [P] pages (budget: [B] pages, [over/under by Q])
> - **Citations:** [C] total (of which [V] verified, [U] marked [NEEDS VERIFICATION])
> - **Honest-scope labels:** [list any aspirational / observation / theoretical markers]
> - **Voice anchors used:** [count] of [N] available
>
> Reply **'Approved'** to mark 🟢 and move to the next task, or give feedback for revision."

HARD STOP. Do NOT proceed to the next task. Do NOT silently fix things.

### Sub-step 7 — On approval

When the user replies "Approved":
- Update `workspace/plan/plan.md`: task **🔵 → 🟢 Reviewed**; refresh Status counts.
- Update `workspace/_session.md` per `rules/approval-discipline.md` §4:
  - Last action: drafted [section]
  - Last approved (date): [date]
  - Next step: invoke `/execute` for the next 🔴 To Do, OR run `/content-review` on this section first
- Suggest to the user: "Approved. Run `/content-review` on this section before moving on, or `/execute` for the next task."

### Sub-step 8 — On feedback

When the user gives feedback (not "Approved"):
- Apply the requested changes to the draft.
- Re-present per Sub-step 6.
- Loop until the user approves.

**Do NOT proactively rewrite.** If the user says "this paragraph is weak," ask for direction (cut? expand? cite more?) before editing.

---

## ARTIFACT CONTRACT

| **FILE** | **WRITTEN** | **CONTAINS** |
| :------- | :---------- | :----------- |
| `workspace/drafts/<section>.md` | new per `[draft]` task | The drafted section |
| `workspace/plan/plan.md` | updated | Task status (🔴 → 🔵 → 🟢) + Status counts |
| `workspace/_session.md` | updated | Last action, next step per approval-discipline §4 |

---

## ITERATION-BOUNDARY GATE

When the next 🔴 To Do is in a higher iteration than the last 🟢:

Present:

> "Iteration **[N]** complete. All tasks 🟢.
> Tasks in Iteration N+1: [list].
> Reply **'Proceed'** to start Iteration N+1, **'Re-plan'** to revise the outline, or run `/content-review` and `/ai-review` on Iteration N drafts first."

HARD STOP per `rules/approval-discipline.md` §7.

---

## WHEN THE USER SAYS "DRAFT EVERYTHING"

REFUSE. Per `rules/approval-discipline.md` §2, this engine does NOT batch. One section at a time. Explain:

> "I draft one section at a time to keep quality control tight. Each section gets an approval gate, then content-review and ai-review before moving on. Want me to start with [first 🔴 task]?"

---

**See also:**
- `rules/approval-discipline.md` — micro-task pattern, status markers, hard-stops
- `rules/register-fidelity.md`, `honest-scope.md`, `no-fabricated-citations.md` — drafting constraints
- `skills/content-review/SKILL.md` — diagnose drafts after this skill
- `skills/ai-review/SKILL.md` — AI-detection audit after content-review
- `agents/research-assistant.md` — bibliography help during drafting
