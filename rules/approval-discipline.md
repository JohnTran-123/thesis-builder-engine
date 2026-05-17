# Approval Discipline

Covers hard-stops, micro-task pattern, artifact-state gating, session checkpointing, and simplicity discipline. Always loaded on session start.

---

## 1. HARD-STOP PATTERN

After producing any drafted output (a section, an outline, an alignment doc, a review report), the AI MUST STOP and wait for explicit User approval. No silent proceeding.

### Approval phrases

| **SITUATION** | **EXPECTED REPLY** |
| :------------ | :----------------- |
| Section draft / skill output | **"Approved"** |
| Phase boundary (alignment → plan, plan → execute, etc.) | **"Proceed"** or **"Re-plan"** |
| Git commit / docx export | **"Yes"** |
| `/ai-review` fix item | Address it (author rewrite) or explicitly skip it |

Anything else is feedback or a revision request — the AI listens, revises, and presents again. No silent moves.

---

## 2. MICRO-TASK PATTERN

One section at a time. One task. No batching.

### Status markers (use these literal symbols in planning docs)

- 🔴 To Do — task is queued, not started
- 🔵 Draft Completed — AI has produced output, awaiting approval
- 🟢 Reviewed / Approved — User has approved
- 🟠 Stuck — blocker, needs User input
- ⚪ Pending — future iteration

### The loop

1. Pick the first 🔴 To Do task.
2. Mark it 🔵 Draft Completed when output is ready.
3. Present evidence. **HARD STOP.**
4. On **"Approved"** → mark 🟢. Move to the next 🔴.
5. On feedback → revise, present again, return to step 3.

---

## 3. ARTIFACT-STATE GATING

Before invoking any downstream skill, the AI checks the workspace for required upstream artifacts. **Do not run a downstream skill if upstream artifacts are missing.**

| **SKILL** | **REQUIRED UPSTREAM ARTIFACT** |
| :-------- | :----------------------------- |
| `/plan` | `workspace/alignment/alignment.md` exists |
| `/execute` | `workspace/plan/plan.md` exists |
| `/content-review` | At least one draft in `workspace/drafts/` |
| `/ai-review` | The corresponding `[review]` (content) task for this section is 🟢 in `workspace/plan/plan.md` §3 |
| `/presubmit` | All chapters have approved content-review + ai-review reports |

If a required artifact is missing, STOP and direct the User to the prior step:

> "Cannot run `/plan` — alignment is not complete. Run `/align` first."

---

## 4. SESSION CHECKPOINTING

The AI updates `workspace/_session.md` at every **phase boundary** (final "Proceed" in a skill) with:

- **Current phase** (ALIGN | PLAN | EXECUTE | CONTENT-REVIEW | AI-REVIEW | PRESUBMIT)
- **Last action** (skill invoked + section / task name)
- **Last approved (date)** + evidence pointer (file path or task ID)
- **Next step** (concrete next action the user should take)
- **Open blockers** (anything Stuck 🟠)

Intra-skill "Approved" gates (e.g., the 3 sub-step approvals inside `/align`) do NOT trigger a checkpoint by default — only the final phase-transition does. Skills may opt to checkpoint earlier if a sub-step commits substantial work to disk; this is documented per-skill in the SESSION CHECKPOINT block.

On session start, the AI reads `_session.md` first to recover state. The session log is the source of truth across multi-day work.

**If `_session.md` is missing or corrupt:** the AI announces this and reconstructs state by reading `workspace/` artifacts directly. Then it writes a fresh `_session.md`.

---

## 5. SIMPLICITY DISCIPLINE

Match minimum output to the problem.

- No bloat, no unprompted refactors, no extra "flexibility" beyond what was asked
- Surgical edits on existing content; rebuild only when needed
- Default to writing no comments; only add when the WHY is non-obvious
- Three similar lines is better than a premature abstraction

When in doubt, ASK the User instead of inferring.

---

## 6. PHASE-TRANSITION GATES

When moving from one phase to the next, present a phase-transition gate:

> "Phase **[N]** complete. Evidence: [brief summary]. Next phase: **[name]**. Reply **'Proceed'** to continue, **'Re-plan'** to revise upstream, or **'Stop'** to halt."

The User may run `/content-review` or other diagnostic skills before replying. HARD STOP until the user replies.

---

## 7. NO ITERATION JUMP

When the next 🔴 To Do task is in a higher iteration than the last 🟢 Reviewed task, the AI **must stop** before building. Present a phase-transition gate (per §6). Do not silently cross iteration boundaries.

---

**See also:**
- `rules/meta-rule-zero.md` — the cardinal rule about AI rewrites
- `CLAUDE.md` — workflow overview and skill index
