# Alignment Intake — 12 Questions

Fill this BEFORE invoking `/align`. The more you complete, the faster alignment runs. You can leave blanks; the AI will walk you through them during `/align`.

When done, run `/align`. The AI will read this file plus your uploaded reference materials (in `workspace/inputs/`) and produce alignment outputs in `workspace/alignment/`.

---

## 1. THESIS TYPE

What kind of thesis are you writing? (check one)

- [ ] Academic (literature-driven, theoretical)
- [ ] Application (apply X to Y at Z)
- [ ] Empirical (hypothesis testing, data analysis)
- [ ] Case study
- [ ] Mixed methods
- [ ] Other (describe):

## 2. INSTITUTIONAL CONTEXT

- School / university:
- Country:
- Department / program:
- Degree (BA, BSc, MA, MSc, PhD, etc.):

## 3. SUBJECT DOMAIN

What field of study? (e.g., management, computer science, public health, education, etc.)

## 4. THESIS SUBJECT (1-2 sentences)

What is your thesis about? Plain language is fine; the working title can change later.

## 5. CONFIDENTIALITY BOUNDARY

What can you NOT disclose? (host firm names, participant data, personal info, internal frameworks, etc.) Be specific — this becomes a hard constraint downstream.

## 6. LENGTH

- Total page or word limit:
- Per-section / per-chapter limits (if specified):

## 7. TIME

- Submission deadline (absolute date):
- Hours per week you can realistically work:
- Total weeks remaining:

## 8. SCHOOL SUBMISSION REQUIREMENTS

- Citation style (APA 7, Harvard, Chicago, IEEE, etc.):
- Language (English, native, both):
- File format (.docx, LaTeX, PDF):
- Required AI-detection threshold (if school specifies, e.g., Turnitin AI score < 20%):
- Required plagiarism threshold (e.g., Turnitin similarity < 25%):

## 9. REFERENCE THESES

Upload 1–5 reference theses from your school (PDFs) to `workspace/inputs/`. List them here so the AI knows which files to analyze:

- [filename or title 1]:
- [filename or title 2]:
- [filename or title 3]:

## 10. TARGET REGISTER + SAMPLE PAPERS

Pick the register your school expects:

- [ ] Strict formal academic (PhD-style, third person only, no author voice)
- [ ] Standard academic (most undergrad / grad theses, occasional first-person in methodology)
- [ ] Practitioner-academic mixed (case-study, internship-based, applied — first-person observation acceptable)
- [ ] Other (describe):

Provide 1–2 sample papers in your field that exemplify the register and quality you're aiming for. Place them in `workspace/inputs/` and list filenames here:

- [sample 1]:
- [sample 2]:

## 11. AUDIENCE (ADVISOR + COMMITTEE)

- Advisor name + expertise area:
- Committee size + members' expertise (if applicable):
- Any known reviewer preferences or pet peeves:

## 12. AVAILABLE EVIDENCE

What data, tools, contacts do you have for the research?

- Interviews you can conduct (with whom, how many):
- Documents / archives you have access to:
- Software / databases / tools:
- Personal observations (field engagement, work history, etc.):

---

**Once filled:** run `/align`. The AI walks you through any blanks, then produces:
- `workspace/alignment/alignment.md` — consolidated alignment record
- `workspace/alignment/constraints.md` — confidentiality + page + time + citation constraints
- `workspace/alignment/register.md` — register baseline extracted from sample papers
- `workspace/alignment/reference-thesis-analysis.md` — outline patterns extracted from reference theses

These become the inputs for `/plan` and everything after.
