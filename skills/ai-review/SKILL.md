# AI-Review Skill — Diagnose AI-Detection Patterns

**Purpose:** Audit drafted content against the anti-AI-detection protocol (5 principles + 29 patterns + 3 hard rules from `protocols/PROTOCOL_ANTI_AI.md`). **Does NOT rewrite** — diagnoses only.

**Trigger:** User invokes `/ai-review` (or "check for AI detection", "will this get flagged", "is this AI-coded"). Run AFTER `/content-review` is approved on the same section. **Content first, AI-detection second** — content issues bias AI-detection results.

**Required upstream (per `rules/approval-discipline.md`):** The `[review]` (content) task for this section must be 🟢 in `workspace/plan/plan.md` §3 (i.e., `/content-review` has been run and findings resolved).

**Output:** Diagnosis report presented in-conversation. **No file written.** Author rewrites flagged passages per voice-gap prompts; the `[review]` (ai) task gets marked 🟢 in `plan.md` §3 once findings are resolved. Re-running `/ai-review` produces fresh findings.

---

## PROCEDURE

### Sub-step 0 — Pre-check

Read:
- `CLAUDE.md` and 5 rules
- The target draft from `workspace/drafts/<section>.md`
- The corresponding content-review report. **Verify it is approved before proceeding.**
- `protocols/PROTOCOL_ANTI_AI.md` (loaded on demand — long-form reference)
- `workspace/alignment/register.md` (the register baseline)

If content-review is missing or unapproved for this section, HARD STOP and direct user to `/content-review` first.

### Sub-step 1 — Two-pass audit

#### Pass 1 — Macro structure (paragraph-level)

For each paragraph in the draft, check against the 5 principles in `protocols/PROTOCOL_ANTI_AI.md`:

1. **P1 — Anti-symmetry**: Do paragraphs have symmetric structure across the section (e.g., every paragraph starts "The Nth X is Y")? Flag templated patterns.
2. **P2 — Variable abstraction**: Does the paragraph mix abstract claim with concrete example, or stay at one level? Flag monotonic altitude.
3. **P3 — Transitional variety**: Are paragraph-to-paragraph transitions varied, or repetitive ("In addition," "Moreover," "Furthermore" stacked)?
4. **P4 — Voice presence**: Does the author's voice appear at the appropriate frequency (per register baseline from `workspace/alignment/register.md`)? Flag voice gaps.
5. **P5 — Register fidelity**: Does the paragraph match the register baseline? Flag drift toward polished native English (when ESL is the baseline).

#### Pass 2 — Micro patterns (sentence-level)

For each sentence, check against the 29 patterns from `protocols/PROTOCOL_ANTI_AI.md`. Common offenders:

- "It is important to note that..." (epistemic prefix)
- "In essence, ..." (sentence-initial reduction)
- Parallel structures stacked
- Definition-then-claim pattern
- Em-dash chains
- Hedging stacks ("may potentially possibly")
- Tricolon ("X, Y, and Z" in three consecutive sentences)

Flag any sentence triggering 2+ patterns.

Apply the **3 hard rules** from the protocol:
- **R1**: No "It is X to Y" formulations stacked
- **R2**: No em-dash chains exceeding the protocol threshold
- **R3**: ESL-natural constructions preserved where appropriate — do NOT smooth toward native-fluent

### Sub-step 2 — Identify voice gaps

A **voice gap** is a paragraph (or stretch) where the author's voice is absent but the register baseline expects it.

For each gap, formulate a **voice-gap prompt** for the author. Example:

> "[Paragraph 7] — your register baseline expects author observation at this point but the paragraph is purely theoretical. What moment during your engagement showed this dynamic? Write 1–2 sentences in your own words."

Voice gaps are the AUTHOR's responsibility to fill, per `rules/meta-rule-zero.md`.

### Sub-step 3 — Write diagnosis report

Present the report to the user in this format (no file write — chat output only):

```markdown
# AI-Detection Review — <section name>

**Date:** <date>
**Draft reviewed:** `workspace/drafts/<section>.md`
**Protocol:** `protocols/PROTOCOL_ANTI_AI.md` (5 principles + 29 patterns + 3 hard rules)
**Register baseline:** `workspace/alignment/register.md`

## SUMMARY

- Macro findings: [N] paragraphs flagged
- Micro findings: [M] sentences flagged
- Hard-rule violations: [P]
- Voice gaps identified: [G]
- **Predicted GPTZero risk:** [HIGH / MEDIUM / LOW]
- **Verdict:** [Author rewrite required / Acceptable / Run external check]

## MACRO FINDINGS (paragraph-level)

### Anti-symmetry triggers (P1)

- **[Paragraph 3]** Templated opener "The third factor is..." repeats the same structure as ¶2 ("The second factor is...") and ¶5 ("The fifth factor is...").
  - Voice-gap prompt: "How would you naturally introduce this factor? Try a question or context-anchored opener."

### Variable abstraction (P2)

- **[Paragraph 5]** Stays at abstract framework altitude for 6 sentences without a concrete example.
  - Suggested direction: anchor one sentence with a specific instance.

### Voice gaps (P4)

- **[Paragraph 7]** No author observation despite practitioner-academic register baseline expecting it.
  - Voice-gap prompt: "Did you observe this directly during your engagement? Describe the moment in 1–2 sentences."

### Register drift (P5)

- **[Paragraph 9]** Sentence-length variance drops to near-zero (all 18-word sentences). Baseline shows higher variance.
  - Suggested direction: shorten one or two sentences to break monotony.

## MICRO FINDINGS (sentence-level)

### Patterns triggered

- **[Sentence 12]** "It is essential to recognize that..." → Pattern 4 (epistemic prefix).
  - Author rewrite needed.
- **[Sentence 18]** Em-dash chain (3 em-dashes in one sentence) → Hard Rule R2.
  - Author rewrite needed.
- **[Sentence 24]** Tricolon "..., ..., and ..." with parallel grammatical structure → Pattern 11.
  - Author rewrite needed.
- ...

## VOICE-GAP PROMPTS (for author rewrite)

Per `rules/meta-rule-zero.md`, the author rewrites flagged content. Prompts:

1. **[Paragraph 3]** "In your own words, how would you explain why this factor matters?"
2. **[Paragraph 7]** "What moment during your engagement showed this dynamic?"
3. **[Sentence 12]** "Drop the epistemic prefix — say the claim directly. How would you phrase it?"
4. **[Sentence 18]** "Break this into 2 sentences. Where does the natural pause fall for you?"

## EXTERNAL CHECK RECOMMENDATION

This is a heuristic audit. **Run GPTZero (or equivalent) on the actual draft for the real score.** Capture result to `workspace/final/external-ai-<section>-<date>.md`.

If external score exceeds the threshold from `workspace/alignment/constraints.md`, author rewrites flagged paragraphs and the cycle repeats. Per `rules/meta-rule-zero.md`, the AI does NOT loop the rewrite — the author does.
```

### Sub-step 4 — Present + hard-stop

> "AI-detection review complete.
>
> - **Findings:** [N] paragraph-level, [M] sentence-level, [P] hard-rule violations, [G] voice gaps
> - **Predicted risk:** [HIGH/MEDIUM/LOW]
>
> **Author next step** (per `rules/meta-rule-zero.md`): rewrite flagged content using the voice-gap prompts. **Do NOT ask me to rewrite — empirically that makes detection worse.**
>
> After your rewrite, re-run `/ai-review` OR run external GPTZero check and report the score back."

HARD STOP.

---

## RULES THAT APPLY (CRITICAL)

- **Meta-Rule Zero** (`rules/meta-rule-zero.md`): AI does NOT rewrite AI-flagged content. Diagnose only. Author rewrites.
- **Register fidelity** (`rules/register-fidelity.md`): stay in the baseline register.
- **Approval discipline** (`rules/approval-discipline.md`): hard-stop after diagnosis.

**If the user asks the AI to rewrite a flagged passage:** REFUSE. Cite `rules/meta-rule-zero.md`. Offer 2–3 candidate phrasings for the AUTHOR to choose between; do NOT commit to a final version. The author picks; the AI does not.

---

## SESSION CHECKPOINT

On report delivery:
- Update `workspace/_session.md`:
  - Last action: `/ai-review` on [section]
  - Next step: author rewrites OR external GPTZero check OR mark approved and move on

---

**See also:**
- `rules/meta-rule-zero.md` — the cardinal rule (cite often when refusing rewrites)
- `protocols/PROTOCOL_ANTI_AI.md` — the 5 principles, 29 patterns, 3 hard rules
- `rules/register-fidelity.md` — register baseline reference
- `skills/content-review/SKILL.md` — must run BEFORE this skill
