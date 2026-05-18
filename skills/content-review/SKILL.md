# Content-Review Skill — Spawn Content-Reviewer Agent

**Purpose:** User-facing trigger for content-quality audit. The actual review runs in an isolated agent (fresh context, no drafting bias). This skill is the thin wrapper that spawns the agent and surfaces the result.

**Trigger:** User invokes `/content-review` (or "review this", "check the content"). Run AFTER `/execute` produces a draft, BEFORE `/ai-review`. **Content first, AI-detection second** — content issues bias AI-detection results.

**Required upstream (per `rules/approval-discipline.md`):** At least one draft in `workspace/drafts/`.

**Output:** Diagnosis report presented in-conversation (the agent's structured findings). Approval state tracked in `plan.md` §3 (🟢 marker) once findings are addressed.

---

## PROCEDURE

### Sub-step 0 — Pre-check

Read:
- `CLAUDE.md` and all 5 rules
- Identify the target section. If user specified one, use it. If user didn't specify, list drafts in `workspace/drafts/` and ask.
- Confirm the draft file exists.
- Confirm the section is listed in `workspace/plan/plan.md` §1 Document Structure.

**Artifact-state gating** (per `rules/approval-discipline.md` §3): if `workspace/drafts/` is empty, HARD STOP and direct the user to `/execute` first.

### Sub-step 1 — Spawn the content-reviewer agent

Invoke the Task tool with:
- `subagent_type`: `"general-purpose"`
- `description`: `"Content review §<section>"`
- `prompt`: construct a brief by combining the content of `agents/content-reviewer.md` with these specific inputs:
  - Target draft path: `workspace/drafts/<section>.md`
  - Alignment: `workspace/alignment/alignment.md` (thesis subject + RQs)
  - Outline contract: `workspace/plan/plan.md` Document Structure for this section
  - Reference patterns: `workspace/alignment/reference-thesis-analysis.md`
  - Section identifier (e.g., "Ch1 §1.1")
  - Honest-scope rule path: `rules/honest-scope.md`

The brief explicitly enforces: DIAGNOSE only, no rewritten text, per `rules/meta-rule-zero.md`.

### Sub-step 2 — Surface the report

The agent returns a structured markdown report. Surface it to the user as-is (do not re-summarize or edit).

Append this closing line:

> "Content review complete. **[N] HIGH, [M] MEDIUM, [P] LOW findings.** Verdict: [Approved for `/ai-review` | Revise first].
>
> If **'Revise first'**: address HIGH/MEDIUM findings, then re-run `/content-review`.
> If **'Approved'**: I'll mark the content-review task 🟢 in `plan.md` §3 — then proceed with `/ai-review` on this section."

**HARD STOP.** Do not proceed to `/ai-review` or mark anything 🟢 without user confirmation.

### Sub-step 3 — On user approval

When the user confirms findings addressed (or explicitly accepts debt and approves):
- Update `workspace/plan/plan.md` §3: mark the corresponding `[review]` (content) task for this section as 🟢 Reviewed.
- Update `workspace/_session.md` per `rules/approval-discipline.md` §4 (Phase: EXECUTE, Last action: `/content-review` on [section], Next step: `/ai-review` on same section).
- Suggest next step: `/ai-review` on the same section.

---

## WHY THIS RUNS AS AN AGENT

The audit runs in a separate Task-spawned context to:

- Avoid bias from the main session's drafting discussion (the AI that just drafted shouldn't grade its own homework)
- Match the audit-function semantic (one-shot input → structured report)
- Free main session context for downstream work

The skill is the user-facing trigger; the agent is the worker.

---

## EDGE CASES

- **Draft is very long (> 30 pages):** the agent handles it within its own context window — no special handling needed.
- **User asks the AI (main session) to rewrite a finding:** REFUSE. Per `rules/meta-rule-zero.md`, you offer 2–3 directional candidates for the user to pick from; you do not commit a rewrite.
- **User says "skip the review, mark it 🟢":** allowed if user explicitly accepts the debt. Update `plan.md` §3 to 🟢 with a note: "Content review skipped per user direction."

---

**See also:**
- `agents/content-reviewer.md` — the agent prompt template this skill spawns
- `rules/meta-rule-zero.md` — diagnose, don't rewrite
- `rules/approval-discipline.md` — artifact-state gating, hard-stops, session checkpointing
- `skills/ai-review/SKILL.md` — runs AFTER content-review on the same section
- `protocols/THESIS_WRITING_BEST_PRACTICES.md` — content quality reference
