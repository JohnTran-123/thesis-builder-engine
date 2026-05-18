# AI-Review Skill — Spawn AI-Detection Reviewer Agent

**Purpose:** User-facing trigger for AI-detection audit. The actual audit runs in an isolated agent (fresh context, isolated protocol load). This skill is the thin wrapper that spawns the agent and surfaces the result.

**Trigger:** User invokes `/ai-review` (or "check for AI detection", "will this get flagged", "is this AI-coded"). Run AFTER `/content-review` is approved on the same section. **Content first, AI-detection second** — content issues bias AI-detection results.

**Required upstream (per `rules/approval-discipline.md`):** The `[review]` (content) task for this section must be 🟢 in `workspace/plan/plan.md` §3.

**Output:** Diagnosis report presented in-conversation (the agent's structured findings). Voice-gap prompts directed at the author for rewrites. Approval state tracked in `plan.md` §3 (🟢 marker) once findings are addressed.

---

## PROCEDURE

### Sub-step 0 — Pre-check

Read:
- `CLAUDE.md` and all 5 rules
- Identify the target section. If unspecified, ask the user.
- Confirm the draft file exists at `workspace/drafts/<section>.md`.
- Confirm the content-review task for this section is **🟢 in `plan.md` §3**.

**Artifact-state gating:** if content-review is missing or not 🟢, HARD STOP and direct user to `/content-review` first.

### Sub-step 1 — Spawn the AI-detection reviewer agent

Invoke the Task tool with:
- `subagent_type`: `"general-purpose"`
- `description`: `"AI-detection review §<section>"`
- `prompt`: construct a brief by combining the content of `agents/ai-detection-reviewer.md` with these specific inputs:
  - Target draft path: `workspace/drafts/<section>.md`
  - Register baseline: `workspace/alignment/register.md`
  - Protocol path: `protocols/PROTOCOL_ANTI_AI.md` (agent loads this — main session does NOT need to)
  - Section identifier (e.g., "Ch1 §1.1")
  - Confirmation that content-review for this section is 🟢

The brief explicitly enforces **Meta-Rule Zero**: the agent must DIAGNOSE only and produce voice-gap PROMPTS, never rewritten text.

### Sub-step 2 — Surface the report

The agent returns a structured report with macro/micro findings, voice-gap prompts, and predicted GPTZero risk band. Surface verbatim.

Append this closing line:

> "AI-detection review complete. **[N] paragraph-level, [M] sentence-level, [P] hard-rule violations, [G] voice gaps.** Predicted risk: **[LOW / MEDIUM / HIGH / VERY HIGH]**.
>
> **Author's next step** (per `rules/meta-rule-zero.md`): rewrite flagged content using the voice-gap prompts. **Do NOT ask the engine to rewrite — empirically that makes detection worse.**
>
> After your rewrite: re-run `/ai-review` on the revised draft OR run external GPTZero check and report the score back to me. When the section meets your threshold, I'll mark the ai-review task 🟢 in `plan.md` §3."

**HARD STOP.** Do not proceed or mark anything 🟢 without user confirmation.

### Sub-step 3 — On user approval

When the user confirms the section meets the AI-detection threshold (whether by passing internal review or by external GPTZero/Turnitin check):
- Update `workspace/plan/plan.md` §3: mark the `[review]` (ai) task for this section as 🟢 Reviewed.
- Update `workspace/_session.md` per `rules/approval-discipline.md` §4.
- Suggest next step: continue to the next 🔴 task in current iteration. If iteration complete, present phase-transition gate.

---

## CRITICAL — META-RULE ZERO ENFORCEMENT

If the user asks the engine (main session OR via re-spawn of the agent) to rewrite flagged content, **REFUSE**. Cite `rules/meta-rule-zero.md`.

The engine MAY offer 2-3 candidate phrasings for the AUTHOR to choose between (this is "diagnose and propose options", not "commit a rewrite") — but the author picks; the engine does NOT commit to a final version.

This applies whether running as skill or as agent.

---

## WHY THIS RUNS AS AN AGENT

The audit runs in a separate Task-spawned context to:

- Isolate the ~470-line `PROTOCOL_ANTI_AI.md` load from the main session (saves context for downstream work in the same session)
- Avoid bias from the main session's drafting discussion
- Match the audit-function semantic (one-shot input → structured report)

The skill is the user-facing trigger; the agent is the worker.

---

## EDGE CASES

- **External GPTZero score below threshold:** mark ai-review 🟢; proceed.
- **External GPTZero score above threshold:** re-run `/ai-review` on rewritten draft. Author rewrites per voice-gap prompts; AI does NOT.
- **User accepts debt without resolving findings:** allowed. Mark 🟢 with note in `plan.md`: "AI-review approved with [N] open findings — debt accepted."
- **User asks for a rewrite:** REFUSE. Cite Meta-Rule Zero. Offer 2-3 candidate phrasings for user to pick from.

---

**See also:**
- `agents/ai-detection-reviewer.md` — the agent prompt template this skill spawns
- `rules/meta-rule-zero.md` — the cardinal rule (cite often)
- `protocols/PROTOCOL_ANTI_AI.md` — 5 principles + 29 patterns + 3 hard rules (loaded by the agent, not by this skill)
- `rules/register-fidelity.md` — register baseline reference
- `skills/content-review/SKILL.md` — MUST run BEFORE this skill
