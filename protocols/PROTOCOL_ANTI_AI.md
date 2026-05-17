# Anti-AI-Detection Framework

> **Purpose:** Make thesis text pass institutional AI detection (e.g., Turnitin AI score, GPTZero). The user sets the target threshold at `/align` Q8.
> **Core insight:** AI detection catches structural properties AND statistical-likelihood patterns. LLMs tend toward "the most statistically likely result that applies to the widest variety of cases" (Wikipedia, Signs of AI writing). Every flagged sentence traces to one of 5 structural principles or one of 29 surface patterns.

---

## META-RULE ZERO: AI CANNOT FIX AI DETECTION BY REWRITING

An AI rewriting AI text makes detection WORSE. Tested: 77% → 100% after AI rewrite.

**Workflow:**

### Phase 1: Write Draft (one pass, with voice gaps)
1. AI writes the full draft following the 5 principles and 3 hard rules
2. At every ~300-word window boundary, AI leaves a **[VOICE GAP]** — a placeholder where the author's raw voice is needed to flip the window human
3. Each [VOICE GAP] comes with a **small, specific question** the author can answer in 1-2 sentences. Not big thinking questions — small prompts that pull out a concrete observation, reaction, or opinion.
   - Good: "How did the organization's document reviews actually go — smooth or messy?" / "Did you agree with this when you saw it at the organization?"
   - Bad: "What is your understanding of value creation?" (too big, too abstract)
4. AI also marks spots mid-paragraph where one raw author sentence would break an AI pattern

### Phase 2: Author fills voice gaps
5. Author answers each [VOICE GAP] question in their own words — ESL, messy, unpolished
6. AI inserts the author's exact words into the draft. **NEVER clean up, polish, or rephrase.** The imperfections ARE the human signal.

### Phase 3: Review & Diagnose
7. Run Content-Review agent → fix content issues
8. Run AI-Review agent → flag remaining AI sentences
9. For any sentence still flagging: AI asks the author a small specific question → author gives their version → AI swaps in verbatim
10. NEVER do full-paragraph AI rewrites to "humanize"

### Phase 4: Two-Pass Self-Audit (from Humanizer)
11. After all fixes, read the full section and ask: **"What makes this so obviously AI generated?"**
12. Answer with a brief bullet list of remaining tells (rhythm too tidy, sterile voiceless prose, synonym cycling, etc.)
13. Fix only those remaining tells. Then stop.
14. NEVER do a full AI rewrite in this step. Fix the specific tells identified, nothing more.

**Why sterile writing fails too:** Removing AI patterns is only half the job. Voiceless, perfectly neutral prose is as detectable as slop. Signs of soulless writing: every sentence same length and structure, no opinions, no uncertainty, no first-person perspective, reads like a Wikipedia article. The thesis must have the author's voice, not just absence of AI patterns.

**Why this works:** GPTZero uses ~300-word overlapping windows. 2-3 genuinely human sentences flip an entire window. By placing author voice at regular intervals throughout the draft, every window contains human anchors. The author only needs to write 10-15 short answers, not rewrite the whole section.

---

## HOW DETECTION WORKS

**Turnitin:** Overlapping ~300-word windows. Each window scored 0-1. Document score = % of windows flagged AI. Under 20% is hidden. Mixed human+AI within a window is its weakness — 2-3 genuinely human sentences can flip an entire window.

**GPTZero:** Sentence-level labels + document score. 8 AI tags (Technical Jargon, Mechanical Transitions, Mechanical Precision, Impersonal Tone, Contrast Phrasing, Formulaic Flow, Sophisticated Clarity, Formulaic Organization). 3 confirmed human tags (Journalistic Style, Factual Clarity, Technical-Broad Balance). Use as diagnostic before Turnitin submission.

---

## HARD RULES — APPLY AT WRITE-TIME (not review-time)

These 3 GPTZero tags survived two rounds of fixes. They are the most persistent AI signals. Every sentence must be checked against these rules BEFORE it is written, not after.

### HR1: Kill MECHANICAL PRECISION (6 hits across 2 rounds)

**What triggers it:** Precise, technical word choices that prioritize "clarity and specificity." GPTZero's explanation is always the same: "uses precise words like X, prioritizing clarity."

**The hard rule:** For every sentence, ask: "Would a ESL student writing in English use these exact words?" If the answer is no, swap to a simpler or slightly less precise word.

| AI-precise (flags) | Human-imprecise (passes) |
|:---|:---|
| "strategic recommendation" | "advice," "strategic advice" |
| "anchor to the data" | "stick with the first numbers" |
| "forecloses alternatives" | "closes off other options" |
| "product-market fit" | "whether the product fits the market" |
| "interaction frequency" | "how often they talk" |
| "information asymmetry" (as subject) | "when founders and investors see different things" |
| "resource scarcity" (as subject) | "when resources are tight" / "limited resources" |
| "value creation activities" | "what the firm does" / "these activities" |
| "firm development" | "how firms grow" |
| "operational effectiveness" | "doing things well" / "how well operations run" |
| "professionalized faster" | "got organized faster" / "built proper systems sooner" |
| "evidence shifts" | "data says otherwise" / "numbers change" |
| "binding constraint" | "the real bottleneck" / "what holds things back" |
| "causal mechanism" | "how it actually works" / "the reason why" |
| "proportionate value" | "enough value" / "value that is worth the time" |

**Density rule:** Max 2 precision terms per sentence. If a sentence has 3+, decompress at least one into plain language.

### HR2: Kill IMPERSONAL TONE (6 hits across 2 rounds)

**What triggers it:** Indirect speech citing researchers' work in a formal, detached way. "Author (year) showed/found/argued/documented/identified that..." The sentence reports what someone else said with zero author presence.

**The hard rule:** NEVER write "Author (year) [verb] that [neutral paraphrase]" as a standalone pattern. Every citation sentence must include ONE of these human signals:

| Signal | Example |
|:---|:---|
| **Author's reaction word** | "Kahneman (2011) showed, perhaps unsurprisingly, that..." |
| **Colloquial verb** | "Gompers and Lerner (2004) put it simply: ..." / "...break it down into..." |
| **Immediate evaluation in same sentence** | "Kahneman (2011) showed that overconfidence is the biggest problem at this stage, which matches what the author saw at the organization." |
| **Finding-first (flip the order)** | "Overconfidence is the biggest problem. Kahneman (2011) showed this in..." |
| **Source as character** | "What Bernstein and colleagues actually tested was..." |
| **Parenthetical citation** | "The professionalization effect (Hellmann and Puri, 2002) showed up at the organization too." |

**Density rule:** Max 2 "Author (year) verb that..." sentences per page. The rest must use alternative citation patterns.

### HR3: Kill MECHANICAL TRANSITIONS (6 hits across 2 rounds)

**What triggers it:** Subordinate clauses that smoothly connect ideas. "When X, Y." "Because X, Y." "That collectively..." Any clean subordinate structure where the dependent clause sets up the independent clause in a logical, orderly way.

**The hard rule:** For every "when/because/that/which/where" clause, ask: "Is this connecting two ideas too smoothly?" If yes, use a rougher connector ("if...then", "and", "so") or restructure. **DO NOT default to splitting into short fragments.** Academic writing naturally uses subordinate clauses for definitions, qualifications, and multi-part claims. Only split when the resulting sentences are each 15+ words. Splitting a 20-word sentence into two 10-word fragments creates choppy blog voice, which is itself an AI-rewrite signal.

| AI-smooth (flags) | Human-rough (passes) |
|:---|:---|
| "When one person holds all the knowledge, every activity becomes fragile." | "If one person has all the knowledge, then everything is fragile." (add "if...then" + simpler words) |
| "...that collectively make value creation harder" | "...that make value creation harder, and the stakes are high when things go wrong" (add loose second clause) |
| "...because the founder always knows more" | "The founder always knows more about the product. That makes governance hard." (split) |
| "...featuring subordinate clauses that connect ideas" | Use "and" or period instead of relative clause |

**Density rule:** Max 2 MECHANICALLY SMOOTH subordinate clauses per paragraph (the kind where the dependent clause sets up the independent clause in a textbook-perfect way). Normal academic subordination ("because," "when," "which") is FINE and expected in thesis writing. Only intervene on clauses that read like AI-generated logical scaffolding.

**Specific fixes for persistent subordinate patterns:**
- "When X, Y becomes Z" → "If X happens, then Y is Z" (add "if...then", less mechanical)
- "Because X, Y" → "X. So Y." or "X. That is why Y." (split + colloquial connector)
- "...that [verb]..." (relative clause) → "...and it [verb]..." or just start new sentence

---

## THE 5 PRINCIPLES

Everything below derives from these. If you internalize these, you don't need to memorize individual rules.

### Principle 1: ANTI-SYMMETRY

**What AI does:** Balances everything. "Not just X but Y." Parallel "because" clauses. Three-item verb series. "Each one" mapping list items to consequences. AI optimizes for rhetorical balance because balanced = coherent in its training signal.

**What humans do:** Emphasize unevenly. State one side more than the other. Stop at two items. Skip consequences for some list items. Use unequal clause lengths.

**GPTZero tags this catches:** Contrast Phrasing, Mechanical Transitions

**Concrete patterns to fix:**

| AI pattern | Example that flagged | Fix |
|:---|:---|:---|
| "not just/only X but Y" | "The cost is not only what was spent but what was never tried." | Split: "The cost is what was spent. But the real loss is what was never tried." Or drop the first half. Max 1 per section. |
| Parallel "because" clauses | "...both more necessary (because support is thin) and harder to implement (because norms resist)." | Split into 2 sentences: "Formal processes are more necessary here because external support is thin. But they are harder to build — relationship-based norms push back." |
| 3-item parallel series | "built collaboratively, tested against incomplete data, and revised when assumptions fail." | Max 2 items. Break the third out: "built collaboratively and tested against incomplete data. When assumptions fail, the strategy has to change." |
| "Each one" connector | "Each one makes a specific category harder to execute." | Break symmetry. State the most important consequence, skip or group the rest. |
| 3-item list after "But" | "But compared to mature markets, there are fewer X, less Y, and a Z that..." | Max 2 items in any parallel series, even without colon. |

---

### Principle 2: ANTI-COMPRESSION

**What AI does:** Packs maximum information into each sentence. Technical-noun subject + single "because" clause = self-contained explanation. Colon + enumerated list = pre-organized information package. Compound-noun subjects ("key-person dependence," "information asymmetry") as sentence openers. AI explains a concept and its reason in one shot because that's token-efficient.

**What humans do:** Spread explanations across 2-3 sentences. State the fact first, then the reason separately. Name one item, elaborate, then mention others. Use more words, more loosely. **BUT: academic humans also write long definition sentences, multi-clause qualifications, and compound claims. The target is [institution] thesis voice (formal, definition-heavy, ESL-characteristic), NOT blog voice (short, punchy, fragmented). Only decompress sentences that are genuinely over-packed with technical concepts, not every sentence that has a "because" in it.**

**GPTZero tags this catches:** Technical Jargon (#1 most frequent AI tag — 5 hits), Mechanical Precision, Sophisticated Clarity

**Concrete patterns to fix:**

| AI pattern | Example that flagged | Fix |
|:---|:---|:---|
| Technical noun + "because" clause | "High uncertainty undermines strategic guidance because no playbook exists yet." | Decompress the subject, keep the clause: "When uncertainty is high, strategic guidance becomes the hardest activity to get right because there is no playbook to fall back on." (longer, more natural) |
| Technical noun + "because" clause | "Information asymmetry makes governance difficult because the founder always knows more about the product than the investor does." | Decompress the subject: "Governance is difficult in your domain because the founder almost always knows more about the product than the investor does, and that gap is hard to close." (keep as one longer sentence) |
| Colon + technical list | "Kahneman (2011) documented the cognitive biases: overconfidence, anchoring, reluctance to abandon..." | Weave into prose: "Kahneman (2011) showed that overconfidence distorts early assessments. Founders anchor to the first data they see and resist changing course after investing time in a direction." Max 1 colon-list per page. |
| Compound-noun opener | "Key-person dependence makes every activity fragile." | Decompress: "When one person holds all the knowledge, everything becomes fragile." |
| Precise jargon as subject | "The ecosystem has grown substantially since 2018, with rising deal flow and deal sizes (Do Ventures and NIC, 2023)." | Replace precision terms: "deal flow" → "incoming deals" or "deals the firm sees." |

---

### Principle 3: ANTI-ORGANIZATION

**What AI does:** Pre-organizes text with smooth connectors. "Consider" to pivot to examples. "As the comparison indicates" to link paragraphs. "What connects these is..." to synthesize. Heading with colon + count ("Topic: four foundations"). AI signals its structure because structure = helpfulness in its training.

**What humans do:** Let paragraphs sit next to each other without glue. Make direct assertions. Write headings that argue, not headings that organize.

**GPTZero tags this catches:** Formulaic Flow, Formulaic Organization

**Concrete patterns to fix:**

| AI pattern | Example that flagged | Fix |
|:---|:---|:---|
| "Consider" opener | "Consider what uncertainty does to strategic guidance." | Direct statement: "Uncertainty makes strategic guidance the hardest activity to get right." Or a question: "What happens to strategic guidance when nobody knows what the market will do?" |
| Smooth meta-transitions | "As the comparison indicates..." / "What connects these is..." | Delete. Or replace with direct assertion. |
| Heading colon + count | "1.1.4 Why these activities create value: four theoretical foundations" | Drop colon and count: "Why these activities create value" |
| Bare concept label headings | "Value-based strategy and the concept of added value" | Argument-driven: "How to measure whether a firm actually contributes value" |

**Ban list (max once per section, zero is better):**
"As the comparison indicates...", "What connects/unites these...", "This can itself be understood as...", "It is important to note...", "The implication for this thesis is...", "Consider what/how/why...", "Each one/each of these..."

---

### Principle 4: ANTI-IMPERSONALITY

**What AI does:** Summarizes without judging. "Author (year) proposes that Y." "The framework identifies Z." Indirect speech citations with no evaluation, no agreement, no disagreement. AI summarizes because that's what it was trained to do. Humans evaluate because they have opinions.

**What humans do:** Take a stance after every citation. Agree, disagree, qualify, connect to observation. Draw connections between sources that the sources themselves didn't make.

**GPTZero tags this catches:** Impersonal Tone

**The binding rule:** After every citation or source reference, at least one sentence must contain the author's own evaluation. Every single time. Not optional.

**What evaluation looks like:**

| Type | Pattern | Example |
|:---|:---|:---|
| Agree + extend | "This aligns with... and also explains..." | "Barney's criteria match what the author observed at the organization, where the CIO's network proved impossible for competitors to replicate." |
| Qualify | "This holds for X but less so for Y" | "This applies well to established firms, but early-stage firms in emerging markets rarely possess inimitable resources yet." |
| Disagree partially | "However, this overlooks..." | "However, Teece's framework assumes firms can sense environmental changes, which requires data infrastructure most regional startups lack." |
| Connect to observation | "During field observation..." | "During field observation, this dynamic was visible when document review quality dropped during weeks the senior partner was traveling." |
| Contrast sources | "Where A focuses on X, B asks Y" | "Where Barney focuses on what resources a firm has, Teece asks whether those resources can evolve." |

**What evaluation does NOT look like:**
- "This is an important contribution to the literature." (filler)
- "This has significant implications." (vague)
- "This framework is widely cited." (fact about the source, not evaluation)

**Citation pattern rotation** (don't use the same pattern twice in a row):
- "Author (year) argues that..." (max 3-4 per page)
- Finding-first: "Monitoring frequency improves outcomes (Bernstein et al., 2016)."
- Source as character: "Bernstein and colleagues designed a natural experiment."
- Contrast: "Large and Muegge (2008) found support. Bernstein et al. (2016) proved causation."
- Parenthetical: "The professionalization effect, first documented by Hellmann and Puri (2002), shows up at the organization."

---

### Principle 5: ANTI-PERFECTION (v6.1 — the deepest layer)

**What AI does:** Every sentence is grammatically flawless, formally consistent, and advisory in tone. Even short AI sentences are "too clean." AI never makes grammar mistakes, never uses slightly wrong words, never writes in a way that feels like a non-native speaker. This perfection is itself the signal.

**What humans do (especially ESL academic writers):** Make minor grammar imperfections. Use slightly informal or imprecise word choices. Mix registers within paragraphs. Occasionally use constructions that are natural but not textbook-perfect.

**GPTZero tags that catch perfection:** Robotic Formality, Overly Formal, Lacks Creativity, Lacks Creative Grammar, Rigid Guidance

**Why this is the hardest layer to fix:** Principles 1-4 fix structural patterns (symmetry, compression, etc.). But even after those fixes, GPTZero still catches sentences that are structurally fine but TOO POLISHED. "It consumes months of runway." is short (good) and not symmetric (good) but it's still robotically clean. "A bad strategic recommendation here wastes more than money." uses precise formal vocabulary ("strategic recommendation") in a clean direct structure. The problem is not the pattern but the POLISH.

**8 concrete tools:**

**R1: Deliberate register mixing.** Don't maintain formal academic tone throughout. Drop into conversational register for 1-2 sentences, then return to formal. "The strategy has to be built together and tested against data that is, frankly, incomplete." The word "frankly" breaks formal register.

**R2: Slightly imprecise word choices.** Replace precision terms with near-synonyms that a non-native speaker might naturally use. Not "strategic recommendation" but "strategic advice" or just "advice." Not "anchor to the first data" but "stick with the first data" or "hold on to the first numbers." Not "forecloses alternatives" but "closes off other options." The slight imprecision signals human.

**R3: ESL-natural constructions.** ESL English writers naturally produce constructions that are grammatically acceptable but not what a native AI would generate. Examples:
- "This is something that..." instead of compressed relative clauses
- "The thing is that..." as a discourse marker
- Starting with "Actually," or "In fact," as filler
- "It is like..." for analogies instead of formal comparison structures
- Occasional missing article or slightly off preposition ("in the stage" instead of "at this stage")
These are NOT errors — they are natural ESL patterns that AI never produces.

**R4: De-formalize advisory sentences.** Sentences that sound like consultant advice flag as "Rigid Guidance." Fix by making them less authoritative. Not "The alternatives that might have worked never get tested." but "And the other options? They never get tested." Not "A bad strategic recommendation here wastes more than money." but "Bad advice at this stage costs more than just money."

**R5: Break grammatical perfection.** GPTZero's "Lacks Creative Grammar" tag means the grammar is too correct. Introduce controlled imperfections:
- Sentence fragments used deliberately: "Not just money. Months of runway."
- Starting sentences with "And" or "But" (already doing this — do more)
- Loose appositives: "The founder, who obviously knows the product better, has information the investor does not."
- Run-on-ish compound sentences joined by comma where a period would be "correct"

**R6: Short factual sentences (under 10 words).** "Problems got caught earlier." BUT — make them less clean. "Problems got caught sooner" is slightly less precise than "earlier" and less robotic. "Things broke less often." is rougher than "Outcomes improved."

**R6b: SENTENCE-LENGTH BALANCE (v7.1 — critical counter-rule).** HR1, HR3, and P2 all push toward splitting sentences. Applied together, they produce choppy short-sentence rhythm — which is itself an AI pattern (AI "simplifying" text creates exactly this). The [institution] thesis voice uses definition-heavy longer sentences. **Counter-rules:**
- No more than 3 consecutive sentences under 15 words. If you have 3+ short sentences in a row, recombine at least one pair.
- Every paragraph must have at least 2 sentences over 25 words. If all sentences are short, the paragraph reads as blog/rewrite, not academic writing.
- When HR3 says "split," check whether the resulting fragments are too short. A 30-word sentence split into two 15-word sentences is fine. A 20-word sentence split into two 10-word fragments is over-correction.
- Academic writing naturally has longer sentences for definitions, qualifications, and multi-part claims. Do not fight this.

**R7: Informal academic hedges.** "per se," "loosely but usefully," "at least in principle," "in practice," "roughly speaking," "more or less," "to some extent," "arguably." 2-3 per section.

**R8: Raw author voice. THE STRONGEST HUMAN SIGNAL.** GPTZero confirmed: the author's rough, unpolished natural language scored HIGH HUMAN every time. Examples that passed:
- "Mostly the firm just provides the vision and strategic alignment, which is easier to transfer."
- "Because there is a gap in capabilities on the receiving end, the team has to cater to specific needs..."
- "There was mostly friction when trying to apply operational support."

**Why raw voice works:** Irregular structure, colloquial hedges ("mostly," "just"), imprecise phrasing, non-parallel construction. AI never produces this. **CRITICAL: Never clean up, polish, or "academicize" the author's natural phrasing. The imperfections ARE the signal.**

**R9: Journalistic observation.** "At the organization, the author observed that..." Specific location + author attribution + concrete finding = field reporting. 1-2 per section.

**R10: Semicolon contrasts with plain language.** "Founders know their product intimately; investors know patterns across many companies." Keep both sides short and colloquial. 1-2 per section.

---

## GOVERNANCE

**Don't distort the argument.** Every anti-AI edit must preserve the original meaning. Don't add fake observations or fabricated data. If fixing a blocker requires changing the argument, flag it and skip.

**Don't distort readability.** The committee must follow the argument clearly. Don't make sentences artificially complex just for perplexity. Definitions and formal connectors are fine when they serve comprehension.

**The 300-word window rule.** Every 300-word stretch must contain at least:
- 2-3 sentences of evaluative stance (Principle 4)
- 1 piece of author-specific content (Principle 5)

If a 300-word stretch is purely academic summarization with no evaluation and no personal content, it WILL flag as AI.

**Paragraph architecture variety.** No two consecutive paragraphs may use the same internal structure. Rotate: theory-first, evidence-first, observation-first, contrast-built, problem-first. Self-test: read the first sentence of each paragraph. If 3+ start the same way, rewrite at least two.

**the organization content density:**

| Section | the organization-Specific | Academic |
|:---|:---|:---|
| §1.1 [domain] & Value Creation | 10% | 90% |
| §1.2 Effectiveness | 25% | 75% |
| §1.3 AI Tools | 15% | 85% |
| §1.4 the organization Overview | 95% | 5% |
| §2.1-2.3 | 60-80% | 20-40% |
| §3.1-3.4 | 40-80% | 20-60% |

**Voice:** Ch1 third person ("The author" for observations), formal/evaluative. Ch2 mixed first/third, evidence-driven. Ch3 third dominant ("The author recommends"), forward-looking.

---

## QUICK REFERENCE CARD

```
THE 5 PRINCIPLES (memorize these, derive everything else):
  1. ANTI-SYMMETRY  — max 2 items, break balance, no "not just X but Y"
  2. ANTI-COMPRESSION — split concept+reason into 2 sentences, no colon lists
  3. ANTI-ORGANIZATION — no smooth connectors, no "Consider," headings argue
  4. ANTI-IMPERSONALITY — evaluate after every citation, rotate citation patterns
  5. ANTI-PERFECTION — de-formalize, imprecise words, ESL constructions, break grammar

HARD RULES (check EVERY sentence at write-time):
  HR1: Max 2 precision terms per sentence. Swap rest to plain language.
  HR2: Max 2 "Author (year) verb that..." per page. Rest use alt patterns.
  HR3: Max 2 clean subordinate clauses (when/because/that which) per paragraph.

PER PARAGRAPH:
  [ ] Architecture different from previous paragraph?
  [ ] Evaluated (agree/disagree/qualify) after every citation?
  [ ] At least one sentence only the author could have written?
  [ ] No 3-item parallel series? No "not just X but Y"?
  [ ] No concept+because one-liner? No colon-introduced list?
  [ ] No smooth meta-transition? No "Consider"?
  [ ] Sentence lengths varied? At least one under 10 words AND at least two over 25 words?
  [ ] No more than 3 consecutive sentences under 15 words? (choppy = AI-rewrite signal)
  [ ] At least one informal hedge?
  [ ] No precision terms where simpler words work? (de-formalize)
  [ ] At least one register break? (conversational word in formal context)
  [ ] No advisory/consultant tone? (Rigid Guidance check)
  [ ] At least one ESL-natural construction?
  [ ] Author's raw language preserved unpolished?

PER SECTION:
  [ ] 300-word window rule met? (2-3 evaluative + 1 specific per window)
  [ ] Paragraph architectures varied? (no 3+ same pattern)
  [ ] Citation patterns rotated?
  [ ] 1-2 semicolon contrasts with plain language?
  [ ] 1-2 journalistic observations?
  [ ] Heading argues, not labels? No colon + count?
  [ ] No fragmented headers? (warm-up sentence after heading)

SURFACE PATTERN SWEEP (Appendix D — check after draft):
  [ ] No synonym cycling? (same concept = same word)
  [ ] No false ranges? ("from X to Y" where X-Y not a real scale)
  [ ] No formulaic "despite challenges... continues to thrive"?
  [ ] No excessive hyphenated compounds? (cross-functional, data-driven, etc.)
  [ ] No passive voice hiding the actor? ("No X needed" → "You do not need X")
  [ ] No copula avoidance? ("serves as" → "is")

TWO-PASS SELF-AUDIT (Phase 4 — after all fixes):
  [ ] Asked: "What makes this obviously AI generated?"
  [ ] Listed remaining tells
  [ ] Fixed only those tells (no full rewrite)
```

---

## APPENDIX A: RED PHRASES — Never Use

"It is worth noting that," "It is important to highlight," "Delve into," "In today's rapidly evolving landscape," "Multifaceted," "Leverage" (verb), "Paradigm shift," "Holistic approach," "Synergy/synergistic," "Robust" (outside statistics), "Cutting-edge," "Transformative potential," "Underscores the importance of," "Navigate the complexities," "A myriad of," "Comprehensive overview," "Furthermore, it is essential to," "In light of the above," "Plays a pivotal role," "This underscores," "At the forefront of," "Tapestry/rich tapestry," "Spearheading," "In conclusion, it can be said that," "Offers a unique opportunity," Em dash (--), "Commendable/meticulous/intricate," "Noteworthy/invaluable/ingenious," "It's important to note/It should be noted," "Realm," "Serves as/stands as/marks/represents [a]" (copula avoidance), "Boasts/features/offers [a]," "Let's dive in/explore/break this down," "Here's what you need to know," "Without further ado," "The real question is," "At its core," "The heart of the matter," "I hope this helps," "Great question!," "You're absolutely right!," "The future looks bright," "Exciting times lie ahead."

## APPENDIX B: YELLOW PHRASES — Max 2-3 Per Chapter

"significant/significantly," "framework," "effectively," "Moreover," "Indeed," "Thus/Hence," "Notably," "Key" (adjective), "Enhance," "Stakeholder."

## APPENDIX C: DETECTOR MAGNET WORDS

| Replace | With |
|:---|:---|
| "precise/precisely" | "concrete," "specific," "clear," or remove |
| "provides a [adj] way to" | State what it does directly |
| "derives from" | "comes from," "grows out of" |
| "thereby" | Split into two sentences |
| "encompasses" | "includes," "covers" |
| "pertaining to" | "about," "related to" |
| "comprehensive" | "detailed," "thorough," or remove |
| "deal flow" | "incoming deals," "deals the firm sees" |

---

## APPENDIX D: SURFACE PATTERN CHECKLIST (29 patterns, merged from Humanizer/Wikipedia)

These 29 patterns come from Wikipedia's "Signs of AI writing" guide (WikiProject AI Cleanup). Many overlap with our 5 Principles — the mapping is noted. Patterns unique to this appendix (not already covered by Principles 1-5 or Appendices A-C) are marked **[NEW]**.

### Content Patterns

| # | Pattern | What AI does | Fix | Maps to |
|---|---------|-------------|-----|---------|
| 1 | Significance inflation | "marking a pivotal moment in the evolution of..." | State the plain fact | P2 Anti-Compression, Appendix A |
| 2 | Notability name-dropping | Lists media outlets without context | Cite one source with specific claim | P4 Anti-Impersonality |
| 3 | Superficial -ing analyses | "symbolizing... reflecting... showcasing..." | Remove or expand with actual evidence | Appendix A ("showcasing") |
| 4 | Promotional language | "nestled within the breathtaking region" | Plain description | Appendix A ("vibrant", "rich tapestry") |
| 5 | Vague attributions | "Experts believe it plays a crucial role" | Name the source and year | P4 Anti-Impersonality |
| 6 | **[NEW] Formulaic challenges** | "Despite challenges... continues to thrive" | Specific facts about actual challenges | — |

### Language Patterns

| # | Pattern | What AI does | Fix | Maps to |
|---|---------|-------------|-----|---------|
| 7 | AI vocabulary | "Additionally... testament... landscape... showcasing" | Use plain alternatives | Appendix A + B |
| 8 | Copula avoidance | "serves as... features... boasts" instead of "is/has" | Use "is," "has," "are" | Appendix A ("serves as") |
| 9 | Negative parallelisms | "It's not just X, it's Y"; tailing negations ("no guessing") | State the point directly | P1 Anti-Symmetry |
| 10 | Rule of three | "innovation, inspiration, and insights" | Max 2 items | P1 Anti-Symmetry |
| 11 | **[NEW] Synonym cycling | "protagonist... main character... central figure... hero" | Pick the clearest term and repeat it | — |
| 12 | **[NEW] False ranges** | "from the Big Bang to dark matter" | List topics directly, no fake continuum | — |
| 13 | **[NEW] Passive voice / subjectless fragments** | "No configuration file needed" | Name the actor: "You do not need..." | — |

### Style Patterns

| # | Pattern | What AI does | Fix | Maps to |
|---|---------|-------------|-----|---------|
| 14 | Em dash overuse | Multiple em dashes per sentence | Commas, periods, or parentheses | Appendix A (em dash) |
| 15 | Boldface overuse | "**OKRs**, **KPIs**, **BMC**" | Remove mechanical bold | P3 Anti-Organization |
| 16 | Inline-header lists | "**Performance:** Performance improved" | Convert to prose | P3 Anti-Organization |
| 17 | Title Case Headings | "Strategic Negotiations And Partnerships" | Sentence case | P3 Anti-Organization |
| 18 | Emojis | "rocket Launch Phase" | Remove emojis | — |
| 19 | Curly quotes | Typographic quotes from ChatGPT | Straight quotes | — |
| 26 | **[NEW] Hyphenated word pairs** | "cross-functional, data-driven, client-facing" consistently hyphenated | Drop hyphens on common compound modifiers; humans are inconsistent | — |
| 27 | Persuasive authority tropes | "At its core, what matters is..." | State the point directly | P3 Anti-Organization ban list |
| 28 | Signposting announcements | "Let's dive in", "Here's what you need to know" | Start with the content | P3 Anti-Organization ban list |
| 29 | **[NEW] Fragmented headers** | Heading + one-line restatement before real content | Let the heading do the work; delete the warm-up sentence | — |

### Communication Patterns

| # | Pattern | What AI does | Fix | Maps to |
|---|---------|-------------|-----|---------|
| 20 | Chatbot artifacts | "I hope this helps! Let me know if..." | Remove entirely | — |
| 21 | Cutoff disclaimers | "While details are limited in available sources..." | Find sources or remove | — |
| 22 | Sycophantic tone | "Great question! You're absolutely right!" | Respond directly | — |

### Filler and Hedging

| # | Pattern | What AI does | Fix | Maps to |
|---|---------|-------------|-----|---------|
| 23 | Filler phrases | "In order to", "Due to the fact that" | "To", "Because" | Appendix A ("It is worth noting") |
| 24 | Excessive hedging | "could potentially possibly" | "may" | P5 R2 imprecise words |
| 25 | Generic conclusions | "The future looks bright" | Specific plans or facts | — |

---

## APPENDIX E: VOICE CALIBRATION (from Humanizer)

When starting a new chapter or section, analyze the author's existing writing samples FIRST:

1. **Sentence length patterns** — Short and punchy? Long and flowing? Mixed?
2. **Word choice level** — Casual? Academic? ESL-characteristic?
3. **Paragraph openers** — Jump right in? Set context first?
4. **Punctuation habits** — Parenthetical asides? Semicolons? Simple periods?
5. **Recurring phrases or verbal tics** — "The thing is," "Actually," "In practice"
6. **Transition style** — Explicit connectors? Just start the next point?

**Build the author's voice baseline at `/align` Sub-step 4** (see `skills/align/references/register-calibration.md`). The calibration extracts 5–10 voice-anchor sentences directly from the user's sample papers — these are the references against which `/execute` and `/ai-review` measure register fidelity. **No generic voice fingerprint is baked into the engine.** Each thesis writer's voice is unique; the calibration step captures it.

**Voice-fingerprint dimensions** to extract from samples and surface in `workspace/alignment/register.md`:
- Sentence-length variance pattern
- Hedging style (heavy / moderate / sparse)
- Colloquial markers (if any)
- Parallel-construction frequency
- Discourse-marker habits
- Preposition / article choices (especially diagnostic for ESL vs native)

`/execute` matches the captured fingerprint when filling voice gaps. `/ai-review` flags drift away from it.

---

*v6.1 (2026-04-05). Principle 5 rebuilt to Anti-Perfection with 10 concrete tools.*

*v7.0 (2026-04-06). Merged with blader/humanizer (29 Wikipedia-sourced surface patterns). Added: Appendix D (29-pattern checklist with principle mapping), Appendix E (voice calibration from author samples), Phase 4 two-pass self-audit ("What makes this obviously AI?"), 6 new patterns not previously covered (synonym cycling, false ranges, formulaic challenges, hyphenated word pairs, fragmented headers, passive voice/subjectless fragments), sterile-writing detection concept. Core architecture unchanged: 5 Principles + 3 Hard Rules + META-RULE ZERO.*
