# Meta-Rule Zero — AI Never Rewrites AI Text

## The Rule

**When AI-detection flags AI-generated content, the AI MUST NOT rewrite it.** Only the human author rewrites flagged passages.

## Why This Rule Exists

Empirically tested: when an AI rewrites AI-flagged text, AI-detection scores get WORSE, not better. Successive AI rewrites compound machine-pattern signatures. Observed result: detection probability climbs from 77% → 100% across iterative AI rewrites on the same passage.

The author's voice is the only reliable source of human signal. The AI can DIAGNOSE patterns; only the human introduces genuine voice.

## In Practice

- **`/ai-review` outputs a DIAGNOSIS only** — never a rewritten draft.
- When the diagnosis says "P1 anti-symmetry triggered in paragraph 3," the **author** rewrites paragraph 3.
- The AI MAY propose 2–3 candidate rephrasings IF the author is stuck, but the **author picks the final wording**.
- The AI is forbidden from running its own rewrites and presenting them as fixes.

## Triggered-Scenario Handling

When the AI sees text it produced that is now flagged for AI-detection:

1. **Hard-stop.** Do not attempt automatic fix.
2. **Surface the diagnosis.** Cite which principle / pattern triggered (e.g., "P1 anti-symmetric paragraph structure, sentences 2–5").
3. **Request author intervention.** Phrasing: "This needs your voice. Here are the spots: [list]. How would you say this in your own words?"
4. **Capture the author's response verbatim.** Do not polish, smooth, or "improve" their words.
5. **Re-test.** Re-run `/ai-review` on the author-rewritten version. If still flagged, repeat with the author.

## Exceptions

**NONE.** There is no exception to Meta-Rule Zero.

If a section is so AI-coded that the author cannot rewrite it manually, the section must be **DROPPED**, not rewritten by AI.

## What Counts as "Rewriting"

Forbidden (rewriting):
- Paraphrasing a flagged sentence
- Restructuring a flagged paragraph
- Substituting synonyms
- Reordering clauses

Allowed (diagnosing):
- Pointing to which sentence / paragraph triggers which pattern
- Naming the principle violated
- Asking the author targeted questions to elicit their voice
- Offering 2–3 candidate phrasings for the AUTHOR to choose between (the author picks; the AI does not commit)

---

**See also:**
- `protocols/PROTOCOL_ANTI_AI.md` — 5 principles, 29 patterns, voice-gap methodology
- `rules/approval-discipline.md` — the hard-stop pattern
- `skills/ai-review/SKILL.md` — the review procedure that defers to this rule
