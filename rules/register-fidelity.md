# Register Fidelity

## The Principle

Stay in the register chosen at `/align`. Do not drift toward over-polished native-English prose, blog tone, or marketing register.

Register is the load-bearing anti-AI-detection signal. Drift breaks it.

## Register Options (set at alignment, Q10)

| **REGISTER** | **CHARACTERISTICS** | **TYPICAL THESIS SHAPE** |
| :----------- | :------------------ | :----------------------- |
| Strict formal academic | Third-person only, no author voice, dense theory-cite chains, hedged claims | PhD theses in management, economics, hard sciences |
| Standard academic | Mostly third-person, occasional first-person in methodology | Most undergrad / grad theses |
| Practitioner-academic mixed | Formal voice + first-person author observations | Case-study, internship-based, applied theses |
| Other | User-defined | Specified at `/align` |

## Anti-Drift Checks (primarily used by `/ai-review`)

`/ai-review` runs all of these checks against the register baseline. `/content-review` may incidentally flag obvious drift but its primary job is argument/logic/structure — drift detection is delegated to `/ai-review`.

| **DRIFT SYMPTOM** | **ACTION** |
| :---------------- | :--------- |
| Sentences sound polished-native when register is ESL | Flag for author rewrite |
| Author voice appears in a strict-formal register | Flag for removal |
| Marketing / blog tone ("seamlessly leverage synergies") | Flag for removal |
| Inconsistent register across sections | Flag the divergent section for normalization |
| Sentence-length monotony (all medium length) | Flag for variation |
| Definition-then-claim pattern repeated identically across paragraphs | Flag as templated structure |

## Calibration at `/align`

`/align` asks the user for 1–2 sample papers in the target register (Q10). Samples are **strongly recommended but not required**. `/align` analyzes:

- Sentence-length distribution
- First-person frequency
- Definition density
- Theory / cite ratio
- Hedging frequency ("may", "appears", "suggests")
- Connective patterns (transitions between paragraphs)

These become the **register baseline** stored at `workspace/alignment/register.md`. Subsequent reviews check against it.

If no sample papers are provided, the baseline is a generic profile based on the user's Q10 register choice (see `skills/align/references/register-calibration.md` edge cases). The engine flags this as DEGRADED and recommends uploading samples for higher-fidelity calibration.

## What This Looks Like in Drafting

When `/execute` drafts a section, it:

1. Loads the register baseline.
2. Drafts sentences matching the baseline's distribution.
3. Does NOT polish toward native-English ideal.
4. Preserves the author's register characteristics if drafting from author notes.

After drafting, `/content-review` and `/ai-review` flag drift.

---

**See also:**
- `protocols/PROTOCOL_ANTI_AI.md` — register patterns and AI-detection
- `skills/align/references/register-calibration.md` — calibration methodology
- `skills/align/SKILL.md` — produces the register baseline
