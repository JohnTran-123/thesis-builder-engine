# User Guide

A step-by-step guide for using the Thesis Builder Engine. If you're new here, start with `README.md`, then come back to this file when you're ready to start working.

---

## Before you begin

You'll need:
- Claude Code installed (CLI, VS Code extension, or web)
- This repo cloned and opened as your project
- Your thesis topic in mind
- Your school's submission requirements (page limit, citation style, AI-detection threshold)
- (Recommended) 1–5 reference theses from your school as PDFs
- (Recommended) 1–2 sample papers in the writing style you're aiming for

The recommended uploads aren't required, but the engine works significantly better with them.

---

## The workflow at a glance

```
   ALIGN  →  PLAN  →  DRAFT  →  CONTENT REVIEW  →  AI REVIEW  →  SUBMIT
```

Six phases. The engine pauses at every phase boundary and asks you to confirm before moving on. You stay in control.

**Rule of thumb:** you produce 1 section at a time, review it, fix it, then move to the next. No batching.

---

## Step 1 — ALIGN

**What it does:** sets up your project foundation. Collects your context, analyzes your reference materials, raises honest-scope questions.

**What you do:**
1. Open `QUESTIONNAIRE.md` and fill in what you can. Blanks are OK — the engine will walk you through them.
2. Drop your reference theses and sample papers (PDFs) into `workspace/inputs/`.
3. In Claude Code, say "let's align" or type `/align`.

**What you'll see:** the engine asks about any blank questions, analyzes your uploads, calibrates your writing register, and surfaces honest-scope flags (things you can't claim given your evidence).

**Output:** 4 files in `workspace/alignment/` — your project's foundation.

**Come back if:** your scope changes, your deadline shifts, your advisor changes the brief.

---

## Step 2 — PLAN

**What it does:** builds your project plan — outline, iteration roadmap, page budget, risk register.

**What you do:** type `/plan`.

**What you'll see:** the engine proposes a chapter outline based on your alignment, sequences the work into iterations, allocates page budgets, and pushes back on your project like an advisor would ("What are your research questions? What's your one-sentence contribution? What's your baseline?").

**Output:** ONE file — `workspace/plan/plan.md`. This is your single source of truth. Everything about your project lives here: outline, tasks, risks, bibliography, status.

**Come back if:** you need to revise the outline. Re-running `/plan` overwrites it; your drafts are NOT discarded.

---

## Step 3 — DRAFT (one section at a time)

**What it does:** drafts ONE section per invocation. No batching.

**What you do:** type `/execute`.

**What you'll see:** the engine picks the next 🔴 To Do task from your plan, drafts that section applying your register / citation discipline / honest-scope rules, stays within the section's page budget, and shows you the result.

**What you do next:**
- Read the draft
- Reply **"Approved"** to mark it done and move on
- Or give feedback for revision

**This is iterative.** You draft → review → approve → next section → repeat. Most theses are 15–25 sections.

---

## Step 4 — CONTENT REVIEW (after drafting each section)

**What it does:** checks the section for argument quality, logic, structure, and honest scope.

**What you do:** type `/content-review` after you've drafted a section.

**What you'll see:** the engine runs 4 checks and produces a findings list (HIGH / MEDIUM / LOW severity) with suggested directions. It does NOT rewrite for you.

**What you do next:** address the findings. The task gets marked 🟢 in your plan when done.

---

## Step 5 — AI REVIEW (after content-review)

**What it does:** audits for AI-detection patterns and identifies "voice gaps" where you need to write in your own words.

**What you do:** type `/ai-review` after content-review is complete on the section.

**What you'll see:** the engine flags patterns that look AI-shaped (parallel structures, em-dash chains, sterile prose, voice absence) and produces "voice-gap prompts" — small specific questions you answer in your own words.

**Critical rule:** the engine WILL NOT rewrite flagged content for you. Empirically, AI rewriting AI-flagged text makes detection scores worse, not better. You rewrite. The engine helps you see where, not what.

---

## Step 6 — SUBMIT

**What it does:** verifies everything is approved, builds your .docx, prompts external checks (GPTZero, Turnitin), final approval.

**What you do:** type `/presubmit` after all sections are drafted and reviewed.

**What you'll see:**
1. The engine checks all sections are 🟢 in your plan. If anything is missing, it stops and tells you what.
2. Runs a pre-submission checklist (TOC, bibliography, page count, register, limitations).
3. Builds the .docx using your school template via `tools/build_thesis.py`.
4. Asks you to run GPTZero and Turnitin externally on the docx, paste results back.
5. Shows the final gate — reply **"Yes"** to mark submitted.

**Output:** `workspace/final/thesis.docx`.

---

## Where your files live

Your thesis content lives in `workspace/` (gitignored — stays private).

| Folder | What's in it |
| :----- | :----------- |
| `workspace/inputs/` | Your uploaded reference theses, sample papers (PDFs) |
| `workspace/alignment/` | 4 files `/align` produces — your project foundation |
| `workspace/plan/plan.md` | Your project plan — outline, tasks, risks, bibliography, status |
| `workspace/drafts/` | Section drafts (one file per section) |
| `workspace/final/` | Built .docx + external check reports |
| `workspace/_session.md` | Session state — engine updates this so you can resume across days |

The engine itself (root files + `rules/`, `skills/`, `protocols/`, `agents/`, `tools/`) you don't usually touch.

---

## 5 rules the engine enforces

1. **AI never rewrites AI text.** If AI-detection flags content, YOU rewrite — not the engine. Why: empirically tested; AI rewriting AI makes detection worse.
2. **Approval discipline.** The engine pauses at every phase boundary. It won't proceed without your "Approved" or "Proceed".
3. **Honest scope.** Claims get labeled by what they are (validated cite / author observation / aspirational). Limitations section mandatory.
4. **Register fidelity.** Stay in the writing register you chose at alignment. The engine flags drift.
5. **No fabricated citations.** Every citation points to a verifiable source. The engine won't invent sources.

---

## Common situations

### You come back after a break

Just open the project in Claude Code. The engine reads `workspace/_session.md` automatically and tells you where you left off: "Last action: X. Next step: Y. Resume?"

### Your advisor sent feedback

- If the feedback shifts your scope: re-run `/align` on the changed items
- If it's on specific sections: edit those drafts and re-run `/content-review` on each

### GPTZero flagged your draft above threshold

- Re-run `/ai-review` on the flagged sections
- The engine gives you voice-gap prompts — write 1–2 sentences each in your own words
- Re-build the docx, re-check externally
- Repeat until below threshold

### You're stuck on a section

Mark the task 🟠 Stuck in your plan. Tell the engine what's blocking you. The engine helps you work through it — but you make the calls.

### You want to revise your outline mid-project

Run `/plan` again. It overwrites the plan. Your existing drafts are NOT discarded — you map them to the new outline manually.

### Your section overran the page budget

The engine tells you. Reply with what to do: "trim", "expand the budget", or "deviate, here's why."

### You disagree with a review finding

Reply with your reasoning. The engine surfaces findings; you decide what to act on. Nothing is forced.

---

## A few "don'ts"

- **Don't ask the engine to rewrite flagged content for you.** It will refuse (Meta-Rule Zero). Ask for "candidate phrasings" instead — it'll offer 2-3 options for YOU to pick from.
- **Don't edit `workspace/_session.md` manually.** The engine relies on it for session resumption.
- **Don't try to draft multiple sections in one go.** The engine is built around one-at-a-time discipline. Trying to bypass it produces lower-quality output.
- **Don't skip content-review before ai-review.** Order matters — content issues bias AI-detection results.

---

## Tips

- **Be honest in the questionnaire.** Vague answers compound. The engine adjusts to what you tell it.
- **Upload reference theses + sample papers early.** Calibration is significantly better with them.
- **Treat every approval gate seriously.** They're intentional. Skipping them produces lower-quality output.
- **Run external GPTZero / Turnitin yourself, paste results into `workspace/final/`.** The engine doesn't run them directly — it helps you interpret the scores.
- **For long projects:** the engine handles session continuity. Just come back when you have time.

---

## What this engine doesn't do

- **Author your thesis.** Every paragraph is yours.
- **Replace your advisor.** It can simulate some advisor moves, but it's not a substitute for real supervision.
- **Run plagiarism or AI-detection scans directly.** You run those tools; the engine helps interpret results.
- **Make claims your evidence can't support.** Honest-scope rule binds.

---

## When you need help mid-session

Just ask. Say things like:
- "What should I do next?"
- "I'm stuck on §1.3 — help me work through it"
- "Show me what's pending in my plan"
- "What does my register baseline say?"

The engine reads your session state and responds with concrete next steps.
