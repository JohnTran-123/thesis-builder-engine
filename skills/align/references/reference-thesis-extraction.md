# Reference Thesis Extraction — Methodology

Methodology for analyzing reference theses (e.g., advisor-recommended exemplars from the user's school) and extracting structural patterns. Loaded on demand by `skills/align/SKILL.md` during Sub-step 3.

---

## INPUTS

- 1–5 reference theses (PDFs or text) in `workspace/inputs/`
- These should be theses from the user's school / department / similar program

## OUTPUT

`workspace/alignment/reference-thesis-analysis.md` — extracted structural patterns + recommended outline.

---

## WHAT TO EXTRACT (per reference thesis)

### 1. Chapter / section hierarchy

For each thesis:
- Total chapter count
- Section depth (Ch.Section.Subsection levels)
- Naming conventions (e.g., "Chapter 1: Literature Review" vs. "Chapter 1: Theoretical Foundation")
- Front matter sections (abstract, acknowledgments, lists)
- Back matter sections (references, appendices)

### 2. Page distribution

- Total page count
- Pages per chapter
- Front matter vs body matter ratio
- Bibliography length

### 3. Argument structure per chapter

Identify what each chapter does:
- Ch1: typically theory / literature review
- Ch2: typically context / case description / methodology
- Ch3: typically analysis / findings
- Ch4: typically discussion / recommendations (if 4-chapter)
- Ch5: conclusion (if 5-chapter)

Note variations across reference theses. The common pattern across multiple references is the safest template for the user.

### 4. Section opener patterns

Sample 5–10 section opening sentences. Catalog:
- **Direct claim**: "X is the most significant factor..."
- **Scaffold**: "This section presents..."
- **Question**: "Why does X matter?"
- **Context**: "Following the discussion in §2.1..."

ESL academic writing tends toward scaffold; native-fluent toward direct. Note the dominant pattern across the references.

### 5. Table / figure conventions

- Caption format ("Table 1.1:" vs. "Table 1:" vs. "Table I.")
- Position relative to text (above table, below)
- Bridging text patterns ("As shown in Table 1.1, ...", "Table 1.1 summarizes ...")

### 6. Bibliography format

- Citation style (APA, Harvard, Chicago, etc.)
- Per-entry format example (verbatim from a reference thesis)
- Sorting (alphabetical, by appearance, by section)

### 7. Common transitions (across reference theses)

Top 10 transitional phrases used between paragraphs. These transitions are register-appropriate by definition (they appear in the user's school's accepted theses).

### 8. Limitations and honest-scope conventions

How do reference theses handle their Limitations section? How do they discuss generalizability? This sets the bar for the user's own Limitations.

---

## OUTPUT FORMAT (reference-thesis-analysis.md)

```markdown
# Reference Thesis Analysis

**Sources analyzed:** [list of files in workspace/inputs/]
**Date:** [date]

## Common outline shape

What every reference thesis does:
- [N] chapters
- Ch1 = [pattern, e.g., "Theoretical foundation; ~25-30 pages"]
- Ch2 = [pattern, e.g., "Case description and methodology; ~30-35 pages"]
- Ch3 = [pattern, e.g., "Analysis and findings; ~35-40 pages"]
- Front matter: [list]
- Back matter: [list]

## Variation across references

Where reference theses differ (typically minor):
- [specific differences and which to follow]

## Recommended outline for THIS USER

Based on the common shape + the user's thesis type (Q1) + subject (Q4) + length (Q6):
- Ch1 = [proposed scope, page budget]
- Ch2 = [proposed scope, page budget]
- Ch3 = [proposed scope, page budget]
- ...

## Section conventions

- Naming style: [pattern]
- Caption format: "Table N.M:" or similar
- Bridging text: [pattern]
- Section openers: [direct / scaffold / question — dominant pattern]

## Citation format

- Style: [APA / Harvard / etc.]
- Example entry: [verbatim from a reference thesis]

## Top transitional phrases

1. ...
2. ...
3. ...

## Limitations conventions

How reference theses handle their Limitations section:
- Typical length: [X pages]
- Typical structure: [enumerated / narrative]
- Common limitations disclosed: [list]

## Anti-patterns to avoid

Things some reference theses did poorly (don't replicate these):
- [example]
- [example]
```

---

## EDGE CASES

- **Only 1 reference thesis:** lower-confidence baseline. Flag and ask user to add a second.
- **Reference theses span different programs or supervisors:** the user may need to pick which is closest. Surface the discrepancy.
- **No reference theses uploaded:** write `reference-thesis-analysis.md` with the note: "No reference theses provided. Recommended outline uses generic structure for Q1 type. Strongly recommend uploading 1–3 reference theses to `workspace/inputs/` and re-running `/align`."
- **Reference theses substantially exceed the user's page budget:** flag that the user's thesis must be more compressed, and `/plan` will allocate tighter page budgets per section.

---

**See also:**
- `skills/align/SKILL.md` — invokes this methodology
- `skills/plan/SKILL.md` — uses the recommended outline downstream
- `agents/research-assistant.md` — for heavy multi-thesis analysis
