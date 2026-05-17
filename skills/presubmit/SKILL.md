# Presubmit Skill

**Trigger:** User invokes `/presubmit` (or "ready to submit", "final checks") after all chapters are drafted and reviewed.

**Required upstream artifact (per `rules/approval-discipline.md`):** Every section in `workspace/plan/plan.md` §1 Document Structure must have its corresponding `[draft]`, `[review]` (content), and `[review]` (ai) tasks marked 🟢 in `plan.md` §3.

**Output:** `workspace/final/thesis.docx`, optionally `workspace/final/thesis.pdf`, plus a checklist report.

---

## PROCEDURE

### 1. Verify prerequisites

For each chapter listed in the Document Structure section of `workspace/plan/plan.md`:

**File-existence check (drafts only):**
- Does the draft exist in `workspace/drafts/`?

**Approval-state check (primary gate):**
- Parse `workspace/plan/plan.md` §3 Activities to find the `[draft]`, `[review]` (content), and `[review]` (ai) tasks for this section.
- All three tasks MUST be marked **🟢 Reviewed/Approved**. A 🔵 Draft Completed (or earlier) means the user has not yet approved — `/presubmit` cannot ship draft-stage content.
- (Review reports are transient and not persisted as files — approval state in `plan.md` is the source of truth.)

**Section-id-to-filename convention:**
- Section "N.M" from plan.md §1 maps to filename stem `chN-sectionM` (e.g., "1.1" → `ch1-section1`).
- Whole-chapter drafts use stem `chN` if the iteration covers the whole chapter as one task.
- If your engine instance uses a different convention, document it in `workspace/README.md` "File-naming hints" section.

If any file missing OR any task not 🟢, HARD STOP and direct the user to complete the missing step. Also append a brief entry to `workspace/_session.md` Open Blockers: "/presubmit blocked at prerequisite check — N of M sections incomplete."

### 2. Run pre-submission checklist

| **CHECK** | **WHAT** | **IF FAILED** |
| :-------- | :------- | :------------ |
| TOC links | All chapter references resolve | Fix references |
| Bibliography integrity | Every citation has metadata; no `[NEEDS VERIFICATION]` remains | Verify or remove citation per `rules/no-fabricated-citations.md` |
| Page count | Within total page budget (Q6 from alignment)? | Trim or expand |
| Register consistency | Sample 3 random sections; register matches baseline from `workspace/alignment/register.md` | Re-run `/content-review` on flagged sections |
| Honest-scope disclosures | Per `rules/honest-scope.md`: validated / observation / aspirational labels in place | Author adds missing disclosures |
| Limitations section | Present, ≥ 1 page, substantive | Author writes or expands |
| Spelling / grammar | Run spell-check; flag systematic errors | Author corrects |

Present the checklist results to the user in-conversation. **No file written** — the verdict (X passed, Y flagged) feeds Step 5's final approval gate directly.

### 3. Build the .docx

Build the docx BEFORE running external checks — external tools (GPTZero, Turnitin) run against the built .docx, not the markdown drafts.

Invoke `tools/build_thesis.py` against the school template. First, extract the Bibliography section from `workspace/plan/plan.md` to a temporary text file (e.g., `workspace/final/_bibliography.txt`):

```
python tools/build_thesis.py --template path/to/school-template.docx [--order order.txt] [--bibliography workspace/final/_bibliography.txt]
```

The v1 generic builder handles:
- Markdown-to-docx conversion (`# ` through `#### ` headings + body paragraphs)
- Body formatting (font, size, line spacing) inherited from the school template
- Optional chapter-ordering file
- Optional bibliography append under a "References" heading

The v1 builder does NOT handle:
- Inline markdown (bold, italic, links) — emitted as literal text
- Tables, lists, code blocks (rich-formatted)
- Automatic TOC regeneration — open in Word and "Update Field" on the TOC manually
- Table caption auto-detection
- Footnotes / endnotes

For richer formatting, customize `tools/build_thesis.py` per the template's needs. The v1 script is intentionally simple and extensible.

If build fails, surface the error and direct user to fix template, content, or extend the script.

### 4. External measurement loop (manual — user runs externally)

The engine does NOT integrate GPTZero or Turnitin directly. The built `.docx` from Step 3 is the input. Document for the user:

> "Run the following external checks and paste the results back:
>
> 1. GPTZero (or equivalent AI detector) on `workspace/final/thesis.docx`. Save the report to `workspace/final/external-ai-<date>.md`.
> 2. Turnitin (via your institution) on the same docx. Save the report to `workspace/final/external-plagiarism-<date>.md`.
>
> Reply with the AI score and the similarity score, or paste the full reports."

When the user replies, parse the results:
- If AI score exceeds the threshold from Q8 (in `workspace/alignment/constraints.md`), flag each section above threshold for author rewrite per `rules/meta-rule-zero.md`. Re-build the .docx (Step 3) after author rewrites.
- If plagiarism score exceeds the threshold from Q8, flag each high-similarity passage. Address via `protocols/PROTOCOL_ANTI_PLAGIARISM.md` response workflow. Re-build after fixes.

### 5. Final approval gate

Present:

> "Pre-submission checklist: [X passed, Y flagged]. External checks: [AI score, plagiarism score]. Final docx: `workspace/final/thesis.docx`. Reply **'Yes'** to mark submitted, or address flagged items first."

HARD STOP.

---

## ARTIFACT CONTRACT

| **FILE** | **CONTAINS** |
| :------- | :----------- |
| `workspace/final/thesis.docx` | Assembled thesis matching school template |
| `workspace/final/thesis.pdf` | (optional) PDF export |
| `workspace/final/external-ai-<date>.md` | External AI-detection report (user-pasted) |
| `workspace/final/external-plagiarism-<date>.md` | External plagiarism report (user-pasted) |

---

## SESSION CHECKPOINT

On final approval, update `workspace/_session.md`:
- Phase: PRESUBMIT → COMPLETE
- Last action: `/presubmit` complete; submitted
- Next step: (none — workflow complete)

---

**See also:**
- `rules/no-fabricated-citations.md` — citation verification at presubmit
- `rules/honest-scope.md` — disclosure check
- `protocols/PROTOCOL_ANTI_PLAGIARISM.md` — plagiarism response
- `tools/build_thesis.py` — docx build script
