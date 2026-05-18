# Thesis Builder Engine

An end-to-end engine for writing thesis-style documents in Claude Code. Built-in alignment, drafting discipline, anti-AI-detection mitigation, and school-template docx export.

---

## Read this first — to actually get a good thesis from this engine

Four things determine whether the engine produces a high-quality thesis or a mediocre one. Worth ~2 minutes to read before you start:

1. **Alignment is non-negotiable.** If you rush `/align` or fill `QUESTIONNAIRE.md` with vague answers, everything downstream is degraded. The 15–25 minutes you spend there is the highest-leverage time in the whole project. Treat it as the foundation, not a checkbox.

2. **Engage actively at every gate.** Yes, this engine ghostwrites — it drafts every section for you. That's the point. But ghostwriting only produces a good thesis if YOU read every output, push back on what doesn't fit, and rewrite every passage `/ai-review` flags (Meta-Rule Zero). Click "Approved" without reading and you'll ship a thesis-shaped document that doesn't actually argue what you mean. The engine drafts; the discipline makes the work yours.

3. **You author every paragraph.** Meta-Rule Zero: when AI-detection flags content, the engine REFUSES to rewrite it for you. You rewrite. Why: AI rewriting AI-flagged text empirically makes detection WORSE (77% → 100% tested). You cannot outsource your voice.

4. **Bring evidence.** The engine builds quality FROM what you provide — reference theses, sample papers, your actual research data. Without these, alignment is degraded; without alignment, everything else is degraded. The engine cannot manufacture rigor from nothing.

---

## Track record

This engine was iterated while the developer wrote his own thesis. The full workflow — from alignment through submission — was tested in production, and the resulting thesis received positive results under the institution's AI-detection threshold and review standards. The engine that ships here is the same one that produced that thesis, generalized to remove institution-specific content.

---

## WHY this exists

Thesis writers face a new constraint: schools deploy AI-detection (Turnitin AI, GPTZero) with strict thresholds. Generic AI tools — "write my literature review" — produce text that gets flagged and bounced. The conventional response is to ask AI to rewrite the flagged passages, which empirically makes detection **worse**: tested rewrite cycles take detection from 77% → 100%.

Most thesis-writing tools focus on prose generation. They don't track honest scope, don't enforce register fidelity, can fabricate citations, and skip the structural alignment phase. They produce template-shaped output that passes superficial review but fails on rigorous reading.

This engine flips it:

- **Alignment is the differentiator.** Before any drafting, you spend ~20 minutes on a 12-question intake. The engine analyzes reference theses from your school and sample papers in your target register. Quality flows from this foundation, not from prose-polish.
- **One section at a time.** Hard-stop discipline. No batching. Each section approved before the next.
- **AI never rewrites AI-flagged text.** When the engine's anti-AI audit flags content, YOU rewrite. The engine refuses to do it (Meta-Rule Zero).
- **No fabricated citations.** Every citation needs verifiable metadata. Unverified ones get `[NEEDS VERIFICATION]` flags.
- **Honest scope.** Claims get labeled (validated cite / author observation / aspirational). Limitations section is mandatory.

---

## WHAT you get

A 6-phase workflow with explicit approval gates:

```
   ALIGN  →  PLAN  →  EXECUTE  →  CONTENT-REVIEW  →  AI-REVIEW  →  PRESUBMIT
```

| Phase | What it does |
| :---- | :----------- |
| **ALIGN** | 12-question intake + reference-thesis analysis + register calibration + advisor-style honest-scope onboarding |
| **PLAN** | Outline + iteration roadmap + page budget + risk register + advisor-style pushback (RQs, contribution claim, baseline, data-leakage checks, etc.) |
| **EXECUTE** | Draft one section at a time, applying register / honest-scope / citation discipline during drafting. Plan-adherence + drift check before write. |
| **CONTENT-REVIEW** | Diagnose argument quality, logic, structure, RQ alignment, honest scope. No rewrites — diagnoses only. |
| **AI-REVIEW** | Audit against 5 anti-AI-detection principles + 29 patterns + 3 hard rules. Identifies voice gaps; produces author-rewrite prompts. No rewrites. |
| **PRESUBMIT** | Pre-submission checklist + build docx via school template + external GPTZero/Turnitin loop + final approval gate |

---

## HOW to use it

### Quickstart

```bash
git clone <this-repo> my-thesis
cd my-thesis
# Open the project in Claude Code (CLI, VS Code extension, or web)
```

Then:
1. Fill in `QUESTIONNAIRE.md` (12 questions, ~15-25 min). Blanks are OK; the engine walks you through them.
2. Drop reference theses + sample papers (PDFs) into `workspace/inputs/` (optional but strongly recommended).
3. Say "let's align" or type `/align`.

The engine guides you through the rest, one phase at a time. You confirm before moving forward.

### Your project lives in `workspace/`

`workspace/` is gitignored — your thesis stays private.

| Folder | Contains | Generated by |
| :----- | :------- | :----------- |
| `workspace/inputs/` | Reference theses + sample papers | You |
| `workspace/alignment/` | Alignment outputs (4 files) | `/align` |
| `workspace/plan/plan.md` | **Single source of truth** — outline + iterations + tasks + risks + bibliography + status | `/plan` |
| `workspace/drafts/` | Chapter / section drafts | `/execute` |
| `workspace/final/` | Built `.docx` + external check reports (user-pasted GPTZero/Turnitin) | `/presubmit` + manual |

Review reports (content + AI) are **transient** — presented in-conversation, not saved as files. Approval state lives in `plan.md`.

---

## PRINCIPLES (5 rules the engine enforces)

These bind every operation. They are not optional.

1. **Meta-Rule Zero — AI never rewrites AI text.**
   When AI-detection flags content, YOU rewrite. The engine refuses. Why: AI rewriting AI compounds detection (77% → 100% in tested cases). Your voice is the only reliable human signal.

2. **Approval discipline.**
   Every phase has explicit hard-stops. The engine pauses and waits for "Approved" or "Proceed." No silent advance.

3. **Honest scope.**
   Claims labeled by mode: **validated** (cite a source) / **observation** (author saw it) / **aspirational** (not yet validated). Limitations section is mandatory and gets adequate page allocation.

4. **Register fidelity.**
   Stay in the register calibrated at alignment from your sample papers. The engine flags drift toward generic, over-polished, or marketing prose.

5. **No fabricated citations.**
   Every citation must be verifiable. Unverified candidates carry `[NEEDS VERIFICATION]` flags until you confirm them. The engine refuses to invent sources.

---

## SOP — Standard Operating Procedure

Day-to-day, the engine works in a simple rhythm: you invoke a skill → engine produces output (a draft, a review report, an outline) → you read it → reply **"Approved"** or give feedback → move to the next task. Each thesis section typically takes 3–5 hours of work spread across drafting, content-review, AI-review, and author rewrite — but you only spend short focused sessions at a time, and the engine handles continuity.

Multi-week or multi-month projects are explicitly supported. The engine writes session state to `workspace/_session.md` after every approval, so when you come back tomorrow (or next month), it greets you with: *"Last action: drafted §1.2. Next step: run /content-review on §1.2. Resume?"* — and you're back in flow.

The **`USER_GUIDE.md`** walks through:

- **Step-by-step usage of each of the 6 phases** — what you do, what you'll see, what to reply at each gate
- **Where your files live** — workspace folders explained in plain language
- **Common situations** — resuming after a break, integrating advisor feedback, recovering from a GPTZero flag, revising your outline mid-project, getting unstuck on a section
- **Five "don'ts"** — the most common mistakes the engine guards against
- **Tips** — small habits that significantly improve output quality
- **What this engine doesn't do** — honest scope on the tool itself

If you're new to the engine, read it after `README.md` and before you fill `QUESTIONNAIRE.md`. About 10 minutes to read end-to-end.

For the 12-question intake form, see **`QUESTIONNAIRE.md`**.

---

## Who this is for

- Undergraduate and graduate students writing thesis-style documents
- Any field of study (engine is domain-agnostic; you provide the content)
- Any institutional template (the engine adapts to your school's docx template)
- Submitting under any AI-detection threshold

**You need:** comfort typing in a chat interface (Claude Code) and editing markdown files.

**You don't need:** software-development experience, LaTeX, specialized writing tools beyond Word/Pages.

---

## What this engine does NOT do

- **Author your thesis.** Every paragraph is yours.
- **Replace your advisor.** It simulates some advisor moves but is not a substitute.
- **Run plagiarism / AI-detection scans directly.** You run GPTZero / Turnitin externally; the engine helps interpret results.
- **Make claims your evidence cannot support.** Honest-scope rule binds.
- **Rewrite AI-flagged content for you.** Meta-Rule Zero. You rewrite.
- **Handle complex markdown in the docx build.** v1 builds chapters + paragraphs from headings; richer formatting needs post-build editing in Word.

---

## Honest limitations

- Tested primarily against application-style theses. Empirical / PhD / creative theses may need adjustments.
- The build script (`tools/build_thesis.py`) is a generic markdown-to-docx assembler. Institution-specific features (footnotes, automatic ToC regeneration, custom heading styles) may need script customization.
- The engine doesn't bundle GPTZero or Turnitin — you run them externally.
- Reference-thesis extraction works best on standard academic thesis structure.
- Register calibration depends on having sample papers; without them, the engine uses a generic profile (degraded but functional).

---

## Architecture (for maintainers)

| Layer | Files | Role |
| :---- | :---- | :--- |
| Dispatcher | `CLAUDE.md` (~134 lines) | AI entry point; routes to skills/rules/protocols/agents |
| Rules | 5 files in `rules/` | Always-loaded global constraints |
| Skills | 6 in `skills/` + 2 alignment references | One procedure per workflow phase |
| Protocols | 3 in `protocols/` | Long-form references loaded on demand |
| Agents | 3 in `agents/` | Research-assistant (literature work), Content-Reviewer (audit, spawned by `/content-review`), AI-Detection Reviewer (audit, spawned by `/ai-review`) |
| Tools | `tools/build_thesis.py` | Generic markdown-to-docx builder |

23 engine files total. Workspace template gitignored.

---

## License

MIT. See `LICENSE`.

## Contributing

v1 release. Issues welcome at the repo's issue tracker. PRs deferred until v1 has community feedback.
