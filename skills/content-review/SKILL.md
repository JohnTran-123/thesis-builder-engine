# Content-Review Skill — Diagnose Content Quality

**Purpose:** Diagnose drafted content for argument quality, logic, structure, connectivity, and RQ alignment. **Does NOT rewrite** (per `rules/meta-rule-zero.md`). Output is a diagnosis report; the author applies fixes.

**Trigger:** User invokes `/content-review` (or "review this", "check the content", "is this argument working"). Run AFTER `/execute` produces a draft, BEFORE `/ai-review`. **Content first, AI-detection second.**

**Required upstream (per `rules/approval-discipline.md`):** At least one draft in `workspace/drafts/`.

**Output:** Diagnosis report presented in-conversation. **No file written.** Author addresses findings; the content-review task for this section gets marked 🟢 in `workspace/plan/plan.md` §3 once findings are resolved. Re-running `/content-review` produces fresh findings against the current draft.

---

## PROCEDURE

### Sub-step 0 — Pre-check

Read:
- `CLAUDE.md` and 5 rules
- The target draft from `workspace/drafts/<section>.md`
- `workspace/alignment/alignment.md` (for thesis subject + RQs)
- `workspace/plan/plan.md` (single source of truth — Document Structure section says what this section should cover)
- `workspace/alignment/reference-thesis-analysis.md` (for structural patterns)

**Artifact-state gating (per `rules/approval-discipline.md` §3):** if `workspace/drafts/` is empty, HARD STOP and direct the user to `/execute` first. If drafts exist but the user didn't specify a section, list drafts in `workspace/drafts/` and ask which to review.

### Sub-step 1 — Run the four checks

#### Check A — Argument quality (per paragraph)

For each paragraph in the draft, ask:

1. Does this paragraph advance the thesis argument?
2. Is each factual claim supported by a citation or marked as author analysis?
3. Is the topic sentence a substantive claim (not scaffold like "This section presents...")?
4. Is the paragraph's conclusion clearly stated?
5. Does the paragraph contribute something a reader could not derive from the previous paragraph?

Flag paragraphs failing any check. Severity:
- **HIGH**: no contribution / unsupported claim
- **MEDIUM**: weak topic sentence / unclear contribution
- **LOW**: style issue

#### Check B — Logic & flow

1. Do claims follow from evidence?
2. Are transitions between paragraphs logical and varied?
3. Are there gaps (claims requiring evidence not yet presented)?
4. Are there contradictions across paragraphs?
5. Does the section build to a conclusion?

#### Check C — Structure & connectivity

1. Does the section deliver what the outline promised?
2. Is the abstraction altitude consistent (no jumping from concrete example to abstract framework without bridging)?
3. Does this section reference other sections it depends on?
4. Are tables / figures introduced and analyzed (introduce → table → analyze → bridge)?
5. Does the section end with a forward link (what comes next)?

#### Check D — RQ alignment & honest scope

1. Does this section answer (or contribute to) one of the research questions?
2. Are claims appropriately hedged per `rules/honest-scope.md`?
3. Are author observations marked explicitly ("From the author's field engagement...")?
4. Are aspirational claims labeled ("Aspirational — not validated in this study")?
5. Is the Limitations subsection (if expected here) substantive?

### Sub-step 2 — Compose diagnosis report (in-conversation)

Present the report to the user in this format (no file write — chat output only):

```markdown
# Content Review — <section name>

**Date:** <date>
**Draft reviewed:** `workspace/drafts/<section>.md`
**Reviewer:** Engine (content-review skill)

## SUMMARY

- [N] HIGH severity findings
- [M] MEDIUM severity findings
- [P] LOW severity findings
- **Verdict:** [Approved for /ai-review | Revise first]

## FINDINGS

### HIGH

- **[Paragraph 3]** Claim "X" is unsupported. Cite a source or mark as author analysis.
- **[Paragraph 7]** This paragraph could be cut entirely — its content is repeated from §2 of paragraph 5.
- ...

### MEDIUM

- **[Paragraph 5]** Topic sentence is scaffold ("This section presents..."). Replace with substantive claim.
- **[Paragraph 9]** Abstraction jumps from concrete case to global generalization without bridge.
- ...

### LOW

- **[Paragraph 12]** Transition "Moreover" appears 3 times in 4 paragraphs.
- ...

## SUGGESTED FIX DIRECTION (not rewrites)

For each HIGH finding, suggest what direction the fix should take. Examples:
- "[Paragraph 3, claim X]: cite Smith (2019) — already in bibliography — OR remove the claim if not central to the argument."
- "[Paragraph 7]: cut entirely. Content lives in §2 ¶5 already."

**NOTE:** Per `rules/meta-rule-zero.md`, this skill does NOT rewrite. The author applies fixes.

## CONTENT-ARGUMENT TREE

A brief visualization of how this section contributes to the thesis argument:

- RQ-1 → ¶1 establishes context → ¶3 makes claim → ¶5 supports with evidence → ¶7 [REPEATED, cut] → ¶9 generalizes [BRIDGE NEEDED]
```

### Sub-step 3 — Present + hard-stop

> "Content review complete.
> **Findings:** [N] HIGH, [M] MEDIUM, [P] LOW.
> **Verdict:** [Approved for /ai-review / Revise first]
>
> If **'Revise first'**: address HIGH/MEDIUM findings, then re-run `/content-review` or move on with acknowledged debt.
> If **'Approved for /ai-review'**: proceed with `/ai-review` on the same section."

HARD STOP. Do not proceed.

---

## SESSION CHECKPOINT

On approval, update `workspace/_session.md`:
- Last action: `/content-review` on [section]
- Next step: `/ai-review` on [section]

---

## RULES THAT APPLY

- **Meta-Rule Zero**: DIAGNOSE only. Never rewrite. If the user asks the AI to "fix this paragraph," REFUSE per `rules/meta-rule-zero.md`. Offer 2–3 candidate directions for the author to choose.
- **Approval discipline**: hard-stop after the report.
- **Honest scope**: flag missing disclosure or over-claiming.

---

## EDGE CASES

- **Draft is short (< 1 page)**: still run all four checks. A short draft can fail every check.
- **Draft is very long (> 30 pages)**: consider running `/content-review` on each major sub-section separately to keep main-session context manageable.
- **User asks for "quick review"**: refuse the shortcut. Run all four checks. A quick review misses systemic issues.

---

**See also:**
- `rules/meta-rule-zero.md` — the diagnose-don't-rewrite cardinal rule
- `skills/ai-review/SKILL.md` — runs AFTER content-review
- `protocols/THESIS_WRITING_BEST_PRACTICES.md` — content quality reference
