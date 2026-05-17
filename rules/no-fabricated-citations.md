# No Fabricated Citations

## The Principle

Every citation in the bibliography MUST refer to a real, verifiable source. The AI is **prohibited** from inventing references, authors, years, page numbers, or quotes.

This rule is non-negotiable. Fabricated citations are academic misconduct. If submitted, they can result in thesis rejection or degree revocation.

## Common AI Failure Modes (FORBIDDEN)

- "Smith (2019) argues that..." where Smith (2019) does not exist
- Plausible-sounding journal articles with invented DOIs
- Real author + invented publication date / journal
- Real source + invented page numbers
- Real source + paraphrased "quotes" that were never said verbatim

## Required Verification

For any citation the AI proposes or includes:

1. **The source must be retrievable.** The AI must have read the source (via web fetch, file upload, or database query) OR mark the citation `[NEEDS VERIFICATION]`.
2. **Quotes must match verbatim.** Any quoted text must match the source exactly. If quoting from memory, paraphrase instead and remove quote marks.
3. **Page numbers verified.** Do not invent page numbers for paginated sources. Use "n.p." for unpaginated digital sources.
4. **Author names spelled correctly.** Verify against the source itself, not memory.

## Required Metadata (per citation)

The AI tracks for each citation:

- Where the source was accessed (URL, file path, citation database)
- Whether full-text was read vs. abstract only
- Confidence level (`high` / `medium` / `[NEEDS VERIFICATION]`)

This metadata lives in the Bibliography section of `workspace/plan/plan.md` — each entry includes the source pointer + verification status alongside the citation itself.

## At `/presubmit`

The pre-submission checklist includes a verification pass:

- Every citation in the bibliography has a verifiable source
- No `[NEEDS VERIFICATION]` markers remain
- Quotes match sources (spot-check at minimum)
- DOIs / URLs resolve

If verification cannot be completed for a citation, the cited claim must be **REMOVED or REPHRASED** to not require the citation. The thesis cannot ship with unverified citations.

## When the Research Assistant Agent Adds Citations

The `agents/research-assistant.md` agent is bound by this rule. It must:

- Cite only sources it has actually read (via search / fetch / database)
- Mark unverified candidates as `[NEEDS VERIFICATION]` and surface them to the user
- Provide source-access metadata for every citation

The user reviews and approves before any citation enters the Bibliography section of `workspace/plan/plan.md`.

---

**See also:**
- `rules/honest-scope.md` — disclosure of unvalidated claims
- `protocols/PROTOCOL_ANTI_PLAGIARISM.md` — citation practice and Turnitin response
- `agents/research-assistant.md` — bibliography-management workflow
