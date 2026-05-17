# Anti-Plagiarism Protocol

> **Purpose:** Actionable checklist and rules to keep the thesis plagiarism-safe while maintaining academic rigor.
> **Turnitin similarity target:** typically < 25% (institutional norms vary — check your school's policy and set the threshold at `/align` Q8). Aim stricter than the institutional ceiling when possible.

---

## 0. HOW TURNITIN SIMILARITY DETECTION ACTUALLY WORKS

### 0.1 The Matching Process

1. Turnitin breaks your text into phrases and assigns unique IDs to each phrase (excluding common function words like "and," "the")
2. These phrases are compared against a database of **seven trillion+ possible matches** including:
   - Billions of current and archived web pages
   - Repository of all previously submitted student work (including other [institution] theses)
   - Thousands of periodicals, journals, and publications
3. The similarity score = **(total matched words / total words in submission) x 100%**
4. Turnitin uses NLP to detect both exact matches AND close paraphrases that retain the same sentence structure

### 0.2 What Is and Isn't Excluded

**Instructor-configurable exclusions (NOT automatic):**
- Quoted text (text within quotation marks)
- Bibliography/reference list
- Small matches (e.g., matches under 8 words or under a certain %)
- Specific sources

**Important:** These exclusions depend on your instructor's settings. Do NOT assume your reference list will be excluded. If it isn't excluded, a 5-page reference list could add 3-5% to your similarity score by itself.

### 0.3 Why Properly Cited Paraphrases Still Get Flagged

Turnitin matches **text**, not citations. A proper citation does not prevent a match if the wording is too close to the source. If your paraphrase retains the same sentence structure but swaps a few synonyms, Turnitin's NLP can still identify the structural similarity.

This means: even if you cite (Author, Year) correctly, the passage can still be highlighted as a match if the wording is too similar to the original. The fix is genuine paraphrasing (restructure the sentence, not just swap words).

### 0.4 Common Triggers for High Similarity in Thesis Writing

| Trigger | Why It Happens | How to Manage |
|:--------|:--------------|:-------------|
| **Literature reviews** | Describing others' research tends to stay close to original phrasing | Write from understanding, not from the source text open in front of you |
| **Common academic phrases** | "The purpose of this study is to," "the results indicate that" appear in thousands of documents | Rephrase standard phrases in your own voice |
| **Framework/model descriptions** | Describing established frameworks using the original author's terminology | Paraphrase the description, cite the author, keep only essential technical terms verbatim |
| **Self-plagiarism** | If you published a conference paper or earlier draft that's in Turnitin's database | Not relevant here, but be aware |
| **Properly cited direct quotes** | Even with quotation marks and proper citation, Turnitin still highlights matching text | Limit direct quotes to 1-2 per chapter, max 2 lines each |
| **Standard methodology language** | Research methods use conventional phrasing matching many existing documents | Rewrite method descriptions in your own words; use active voice |

---

## 1. CITATION DENSITY TARGETS BY SECTION

### 1.1 Benchmark Basis

| Benchmark Thesis | Ch1 Citations/Page | Ch1 Unique Sources | Notes |
|:-----------------|:-------------------|:-------------------|:------|
| [example firm]          | ~1/page            | ~25                | Lowest density among A-grades |
| [example firm]            | ~2-3/page          | ~25                | Strong quantitative backing |
| [reference template]            | ~2/page            | ~50                | Highest source diversity |
| [example firm]          | ~3/page            | ~60                | Lit-review-heavy (5-chapter) |

### 1.2 Our Targets (v2 outline)

| Section | Target Density | What Counts as a Citation | Minimum Unique Sources |
|:--------|:---------------|:--------------------------|:-----------------------|
| **Introduction** (~8-10pp) | 1-2 citations/page | Published statistics, government policies, industry reports justifying rationale | 5-8 unique sources |
| **Ch1: Theoretical Framework** (~35-40pp) | **2+ citations/page** | Academic papers, textbooks, named frameworks, definitions | **20+ unique sources**, **40+ citation instances** |
| **Ch2: Current Situation** (~30-35pp) | 1-2 formal citations/page + the organization data every paragraph | Internal docs, named frameworks, interview notes, observed processes, internal metrics (see 1.3 below) | **8+ the organization data references per major section**, 5+ external benchmarks |
| **Ch3: Recommendations** (~20-25pp) | 1 citation/page (lighter) | Back-references to Ch1 theory + Ch2 findings; industry best-practice examples | **Every recommendation** cites at least 1 Ch1 source + 1 Ch2 finding |
| **Conclusion** (~2-3pp) | 0-1 citations/page | Minimal; summarizes thesis findings | None required |

### 1.3 What Counts as an the organization Data Citation in Ch2

Each of these is a valid, citable reference in Ch2. Use them liberally:

- **Named internal frameworks:** "the organization's [Framework Name]" -- cite as (the organization internal document, Year)
- **Internal process descriptions:** "According to the organization's deal sourcing workflow..."
- **Observations from field engagement:** "During the author's field engagement (Month-Month Year), it was observed that..."
- **Named people (by role):** "According to the Investment Associate responsible for..."
- **Internal metrics/KPIs:** "the organization's internal reporting shows that in 2025, the portfolio..."
- **Internal documents:** Meeting notes, investment memos, playbooks -- cite as (the organization, Document Title, Year)
- **Organizational charts/structures:** (the organization organizational chart, Year)

**Rule:** Every factual claim about the organization in Ch2 must be traceable to one of these categories. If you cannot name the source type, the claim is unsupported.

---

## 2. PER-CHAPTER CITATION TARGETS (Specific Numbers)

### Ch1: Theoretical Framework (v2 outline)

| Section | Min Unique Citations | Min Citation Instances | Notes |
|:--------|:---------------------|:-----------------------|:------|
| §1.1 [domain] and Value Creation (~10-12pp) | 8 | 16 | Definitions need 2-3 sources each, activity taxonomy needs lit backing |
| §1.2 Effectiveness and System Design (~6-8pp) | 6 | 12 | System Design grounded in academic equivalents (Drucker, Taleb, Weick, Lewin) |
| §1.3 AI Tools as Intervention (~8-10pp) | 8 | 14 | AI lit + structural constraints + adoption frameworks |
| §1.4 the organization Overview (~5-7pp) | 2 | 4 | Mostly factual; the organization sources + 1-2 industry context |
| **Ch1 Total** | **20+** | **40+** | |

### Ch2: Current Situation at [Your Organization]

| Section | Min the organization Data Refs | Min External Benchmarks | Notes |
|:--------|:-------------------|:------------------------|:------|
| §2.1 Current Activities, deep (~12-15pp) | 12 | 3 | Every process described must name its source |
| §2.1 Current Activities, light (~2-3pp) | 4 | 1 | Brief but still sourced |
| §2.2 Evaluation of Effectiveness (~10-14pp) | 10 | 4 | Framework application must reference §1.2 System Design components by name + the organization evidence |
| §2.3 Strengths, Limitations, Root Causes (~5-7pp) | 6 | 2 | Root causes need specific evidence, not general claims |
| **Ch2 Total** | **32+** | **10+** | |

### Ch3: Recommendations

| Section | Min Ch1 Back-Refs | Min Ch2 Back-Refs | Min New Sources | Notes |
|:--------|:-------------------|:-------------------|:----------------|:------|
| §3.1 Recommendations by Activity (~12-15pp) | 6 | 6 | 4 | Each rec: theory basis (Ch1) + root cause (Ch2) + tool evidence (new) |
| §3.2 Implementation Roadmap (~3-5pp) | 1 | 2 | 1 | Grounded in the organization capacity from Ch2 |
| §3.3 Conditions for Success (~2-3pp) | 2 | 2 | 1 | Adoption factors from Ch1, the organization constraints from Ch2 |
| §3.4 Lessons for Regional Firms (~2-3pp) | 2 | 1 | 2 | regional context sources |
| **Ch3 Total** | **11+** | **11+** | **8+** | |

---

## 3. PARAPHRASING RULES

### 3.1 The 5-Word Rule

**No 5 or more consecutive words copied from any source.** If you find yourself using a phrase verbatim:
- Restructure the sentence
- Replace with synonyms where meaning is preserved
- Change passive to active voice (or vice versa)
- Split the idea across two sentences with your own connector logic

**Exception:** Technical terms and proper nouns do not count (e.g., "artificial intelligence," "resource-based view," "system design," "dynamic capabilities").

### 3.2 The Analytical Layer Rule

Never restate a source without adding interpretation. Every citation must be followed by YOUR analytical contribution:

| Bad (restatement) | Good (analysis added) |
|:-------------------|:----------------------|
| "According to Author (Year), AI improves efficiency." | "Author (Year) argues that AI improves efficiency through process automation. In the context of your domain, this efficiency gain is most relevant in deal sourcing, where..." |
| "Value creation involves strategic guidance (Author, Year)." | "While Author (Year) frames value creation broadly as strategic guidance, the organization's practice reveals a more granular reality: the firm distinguishes between..." |

### 3.3 Definition Synthesis Protocol

When introducing a key concept ([domain], value creation, effectiveness, AI tools):

1. **Cite Definition A** (Author 1, Year): state it
2. **Cite Definition B** (Author 2, Year): state it
3. **Compare:** note overlap and divergence between A and B
4. **State YOUR working definition:** "For the purposes of this thesis, [concept] is defined as..." This must be your own synthesis, not a copy of either source

**Minimum:** 2 source definitions before your own. Ideal: 3.

### 3.4 Framework Application Protocol

When using a named framework (System Design, TOE, RBV, etc.):

1. **Cite the original author** who proposed the framework
2. **Explain the framework's components** (paraphrased, not copied from source)
3. **Cite 1-2 authors who have applied it** in a related context
4. **State explicitly how YOU will apply it:** "This thesis adapts [Framework] by focusing on [specific dimensions] because [reason tied to the organization context]"

**Never:** Present a framework description and move on. Always bridge to YOUR application.

### 3.5 The "Close the Source" Method

When you catch yourself paraphrasing too closely:

1. Read the source passage and understand the point
2. **Close the source** (minimize the tab, turn the page)
3. Write what you REMEMBER the point being, in your own words
4. Check your version against the original for accuracy
5. The natural forgetting creates natural paraphrasing

This is the most reliable method for avoiding structural similarity. Turnitin catches sentence-structure copying even when individual words are changed.

---

## 4. SELF-CHECK METHOD

### 4.1 Paragraph-Level Check (Run While Writing)

For EVERY paragraph, answer these three questions:

- [ ] **Source check:** Can I point to where each factual claim comes from? If not, add citation or delete claim.
- [ ] **Originality check:** Is there at least one sentence in this paragraph that is MY analysis, not a restatement of a source? If the entire paragraph is source summary, add interpretation.
- [ ] **Voice check:** Does this sound like the author's own voice writing about their subject, or does it sound like a textbook? If textbook, rewrite with specificity.

### 4.2 Section-Level Check (Run After Completing Each Section)

- [ ] **Argument ownership:** Is the section's main argument MY logic connecting sources, or am I just reporting what sources say in sequence? The section must advance MY thesis, not survey the field.
- [ ] **Citation coverage:** Count citations per page. Does it meet the density target from Section 1.2 above?
- [ ] **Source diversity:** Am I over-relying on one source for multiple consecutive paragraphs? No source should dominate more than 2 consecutive paragraphs.
- [ ] **Cross-reference integrity:** Does this section reference earlier/later sections where promised? (Ch2 references §1.2 framework? Ch3 references §2.3 findings?)

### 4.3 Red Flag Patterns (Immediate Fix Required)

| Red Flag | What It Signals | Fix |
|:---------|:----------------|:----|
| **3+ paragraphs with no citations** | Potential plagiarism zone or unsupported claims | Add sources or mark as original analysis with clear "the author observes..." framing |
| **A passage that sounds like one specific source** | Close paraphrase risk | Use the "close the source" method (§3.5): rewrite from memory, then verify |
| **Long block quotes (>2 lines)** | Lazy writing, Turnitin flag | Paraphrase and cite; reserve direct quotes for definitions only, max 1-2 per chapter |
| **Identical sentence structure across paragraphs** | AI-generated pattern (also a plagiarism concern) | Vary: start with different parts of speech, alternate short/long sentences |
| **Generic claims with no company specificity** | Could be from any source about any company | Replace with the organization-specific data point, name, date, or observation |
| **Definitions without synthesis** | Single-source copying risk | Apply the Definition Synthesis Protocol (Section 3.3) |
| **Framework description without application** | Structural copying of textbook content | Apply the Framework Application Protocol (Section 3.4) |

### 4.4 Turnitin Similarity Score Management

**Target:** < 25% overall similarity

| Similarity Range | Assessment | Action |
|:-----------------|:-----------|:-------|
| < 15% | Excellent | No action needed |
| 15-25% | Acceptable | Review flagged passages, ensure all are properly cited |
| 25-30% | Warning zone | Rewrite passages with highest match %; check for missed paraphrasing |
| > 30% | Unacceptable | Major rewrite required; likely structural copying issue |

**Common Turnitin triggers to manage:**
- Reference list entries (expected; check if instructor has exclusion enabled)
- Table of contents, headings (expected; usually excluded)
- Methodology descriptions using standard phrasing: rewrite in your own words
- Literature review passages that follow source structure too closely: use "close the source" method
- Copied framework descriptions: always paraphrase
- Common academic phrases that appear in thousands of documents: rephrase them

---

## 5. SOURCE USAGE RULES

### 5.1 Primary Sources: the organization Internal

**Format:** (the organization, [Document type or title], [Year])

Examples:
- (the organization, Investment Process Handbook, 2024)
- (the organization, Internal Performance Report, 2025)
- (Author's field observation, [engagement period])
- (Interview with [Role], the organization, [Date])

**Rules:**
- Do NOT include confidential documents in the reference list with full titles if the organization objects. Use generic descriptors: "Internal company document"
- First-person observations are valid primary data: "During field observation period ([engagement period]), the author observed that..."
- Always specify the time period for observations and data
- Named individuals should be referenced by role, not personal name, unless permission granted: "the Portfolio Manager" not "Mr. Nguyen"

**In Reference List:** Group under "Internal and Primary Sources" section:
```
[Your Organization] (2024). Investment Process Documentation. [Internal document].
[Your Organization] (2025). Portfolio Performance Report Q4. [Internal document].
Author's fieldwork notes, January-April 2026.
```

### 5.2 Academic Sources: Harvard/APA Style

**In-text citation format (Harvard):**
- Single author: (Smith, 2020)
- Two authors: (Smith and Jones, 2021)
- Three+ authors: (Smith et al., 2022)
- Direct quote: (Smith, 2020, p. 45)
- Multiple sources for one claim: (Smith, 2020; Jones, 2021; Nguyen, 2023)

**Reference list format:**
```
Gompers, P. and Lerner, J. (2001). The Venture Capital Revolution.
    Journal of Economic Perspectives, 15(2), pp. 145-168.
Kaplan, S.N. and Stromberg, P. (2009). Leveraged Buyouts and Private
    Equity. Journal of Economic Perspectives, 23(1), pp. 121-146.
```

**Rules:**
- Prefer peer-reviewed journal articles and academic books
- Conference papers and working papers acceptable but note status
- Wikipedia, Investopedia, general blogs are NOT acceptable as academic sources
- Minimum recency: at least 30% of sources from last 5 years (2021-2026)

### 5.3 Industry Reports and Benchmarks

**Format:** (Organization, Year) or (Organization, Report Title, Year)

Examples:
- (McKinsey Global Institute, 2023)
- (PwC, [Country/Region] Venture Capital Report, [Year])
- (World Bank, Global Economic Prospects, 2024)

**Rules:**
- Acceptable for market data, industry statistics, trend claims
- Not a substitute for academic sources in Ch1 theoretical sections
- Always note the methodology if citing specific numbers: "According to PwC's survey of 200 firms..."
- Check that the report is publicly available or cite as restricted access

### 5.4 Regional Government Documents

**Format:** (Issuing body, Document number, Year)

Examples (replace with your country's actual document format):
- (Government of [Country], [Decision / Act Title and Number], [Year])
- (Ministry of [Department], [Decree Number / Type], [Year])
- (Central Bank of [Country], [Circular Number / Type], [Year])

**Rules:**
- Use the official document number with whatever type prefix your country's legal system uses (e.g., Decree, Decision, Circular, Order, Regulation, Act, Statutory Instrument)
- Provide the issuing date and issuing body
- If citing specific articles/clauses: e.g., "Article 5, Clause 2 of [Decree Number]"
- Non-English titles can be provided alongside English translation in brackets
- These are strong credibility signals: use them in Introduction (rationale) and Ch1 (regional context)

---

## 6. QUICK-REFERENCE CHECKLIST (Print and Use While Writing)

### Per Paragraph:
- [ ] At least one citation OR clearly marked as author's original analysis
- [ ] No 5+ consecutive words from any source
- [ ] Analytical layer present (not just restating a source)
- [ ] If Ch2: at least one the organization-specific element (name, date, metric, process, observation)

### Per Page:
- [ ] Citation density meets section target (see Section 1.2)
- [ ] No single source dominates the entire page
- [ ] Mix of citation types (academic + data + observation as appropriate)

### Per Section:
- [ ] Main argument is YOURS, supported by sources (not source's argument restated)
- [ ] Definition Synthesis Protocol followed for key terms (Section 3.3)
- [ ] Framework Application Protocol followed for any named model (Section 3.4)
- [ ] No red flag patterns (Section 4.3)
- [ ] Cross-references to other chapters present where needed

### Per Chapter (Before Submission):
- [ ] Total citation count meets target
- [ ] Unique source count meets target
- [ ] No block quotes longer than 2 lines
- [ ] Every factual claim traceable to a source
- [ ] Read aloud: does it sound like YOUR voice with academic discipline?

---

## 7. EMERGENCY FIXES: Common Problems and Quick Solutions

| Problem | Quick Fix |
|:--------|:----------|
| "I wrote 2 pages with no citations" | Go back sentence by sentence. Each factual claim gets a source. Personal analysis gets framed as "the author argues/observes that..." |
| "This paragraph sounds exactly like the source" | Close the source. Write what you REMEMBER the point being. Then check accuracy. The natural forgetting creates natural paraphrasing. |
| "I can't find an academic source for this claim" | Either (a) find one on Google Scholar with different keywords, or (b) reframe as your original observation: "Based on the author's analysis of the organization's operations..." |
| "My Ch2 reads like a company brochure" | Add evaluation. After every description, ask: "So what? Is this effective? Compared to what? What does the System Design framework say?" |
| "Turnitin flagged my literature review" | Rewrite flagged passages from memory (§3.5). Use different sentence structures. Add more of your own connecting logic between cited points. |
| "My reference list inflated my similarity score" | Check if your instructor enabled reference list exclusion. If not, this is expected and can be explained. |

---

*This protocol is a living document. Update citation counts after completing each chapter to track actual vs. target.*
