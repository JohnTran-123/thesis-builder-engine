# Register Calibration — Methodology

Methodology for analyzing sample papers and producing a register baseline. Loaded on demand by `skills/align/SKILL.md` during Sub-step 4.

---

## INPUTS

- One or more sample papers (PDFs or text) in `workspace/inputs/`
- The user's register choice from Q10:
  - Strict formal academic
  - Standard academic
  - Practitioner-academic mixed
  - Other (user-defined)

## OUTPUT

`workspace/alignment/register.md` — register baseline.

---

## METRICS TO EXTRACT (per sample paper)

### 1. Sentence-length distribution

For each sentence in a representative section (e.g., one body chapter, 5+ pages):

- Word count per sentence
- Mean and median
- Range (shortest, longest)
- Variance / distribution shape (uniform / skewed / bimodal)

**Native-English academic prose** typically has mean 18–25 words, with high variance.
**ESL academic prose** often clusters in a narrower band (15–22 words) with lower variance.

### 2. First-person frequency

Count sentences where:
- "I", "we", "my", "our", "the author", "the authors" appear
- Excluding direct quotes

Report as percentage of total sentences.

| **Register** | **Expected first-person %** |
| :----------- | :-------------------------- |
| Strict formal | 0% |
| Standard academic | < 5% (mostly in methodology) |
| Practitioner-academic | 5–15% (distributed across sections) |

### 3. Definition density

Count sentences that explicitly define a term:
- "X is defined as Y"
- "By X, we mean Y"
- "X refers to Y"
- "In this thesis, X means Y"

ESL academic writing often has higher definition density (~10–15% in theory chapters) — the writer foregrounds clarity over native-fluent assumption.

### 4. Theory-cite ratio

Ratio of citations to claims:
- Cites per page in body sections
- Cites per page in theory / lit-review sections

| **Register** | **Theory cites/page** | **Body cites/page** |
| :----------- | :-------------------- | :------------------ |
| PhD-style | 3–5 | 1–2 |
| Standard undergrad/grad | 2–3 | 0.5–1 |

### 5. Hedging frequency

Count instances of hedging language:
- "may", "might", "could", "appears", "suggests", "tends to", "in general"
- "It is possible that", "Evidence indicates", "One could argue"

Hedging is a register marker. ESL academic prose often hedges more than native-fluent; PhD-style hedges most heavily.

### 6. Transition patterns

Identify the transitional phrases used between paragraphs:
- "Building on this," "In addition," "However," "By contrast," "This raises the question of," "From a different angle,", etc.

Catalog the top 5–10 transitions. Downstream `/execute` reuses them as voice anchors.

### 7. Voice-anchor sentences

Pick 5–10 sentences from the samples that exemplify the register exactly. These become VOICE ANCHORS — `/execute` references them when drafting, to keep the register fixed.

---

## COMPARING TO TARGET REGISTER

After extracting metrics, position them against the user's chosen register:

| **REGISTER** | **TYPICAL PROFILE** |
| :----------- | :------------------ |
| Strict formal | 0% first-person, mean sentence 22–28 words, hedging frequency HIGH, definition density MEDIUM, theory-cite ratio HIGH |
| Standard academic | < 5% first-person, mean sentence 18–25 words, hedging frequency MEDIUM, definition density MEDIUM, theory-cite ratio MEDIUM |
| Practitioner-academic | 5–15% first-person, mean sentence 15–22 words, hedging frequency MEDIUM-LOW, definition density HIGH, theory-cite ratio LOW-MEDIUM |

If the user's sample papers do not match the user's stated target register, surface the discrepancy:

> "You said 'strict formal' but your sample paper has 8% first-person frequency, suggesting 'practitioner-academic'. Which actually matches what your advisor expects?"

---

## OUTPUT FORMAT (register.md)

```markdown
# Register Baseline

**Target register declared:** [from Q10]
**Sample papers analyzed:** [list]
**Date:** [date]

## Quantitative profile (from samples)

| Metric | Value |
| :----- | :---- |
| Mean sentence length | [N] words |
| Sentence-length range | [min]–[max] words |
| First-person frequency | [X]% |
| Definition density | [Y]% of sentences |
| Theory-cite ratio | [Z] cites/page |
| Hedging frequency | [P] instances/page |

## ESL vs native signals

[Diagnose whether the samples are ESL-flavored or native-fluent, and which the user should aim for.]

## Common transitional phrases (top 10)

1. ...
2. ...
3. ...

## Voice-anchor sentences (verbatim, used by /execute as drafting reference)

1. "[sentence from sample]"
2. "[sentence from sample]"
...

## Anti-patterns to avoid

- Drift toward polished native English (when ESL is the baseline)
- Marketing / blog tone ("seamlessly leverage synergies")
- Author voice in strict-formal register
- Sentence-length monotony (all medium-length)
```

---

## EDGE CASES

- **Only 1 sample paper:** profile is less reliable. Flag this and recommend the user add a second.
- **Sample papers in different registers:** ask the user which to prioritize, or average with explicit weighting.
- **No sample papers:** use generic register profile based on Q10 declaration. Mark `register.md` with: "Calibration DEGRADED — no samples provided. Add 1–2 sample papers to `workspace/inputs/` and re-run `/align` Sub-step 4 for full-fidelity baseline. Drafting and review still possible against the generic profile, with higher revision burden expected." This is consistent with `rules/register-fidelity.md` — samples are strongly recommended, not required.
- **Sample papers in different languages:** if multilingual, calibrate per language separately (relevant for bilingual abstract requirements).

---

**See also:**
- `rules/register-fidelity.md` — using the baseline downstream
- `protocols/PROTOCOL_ANTI_AI.md` — register patterns and AI detection
- `skills/align/SKILL.md` — invokes this methodology
