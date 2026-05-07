---
name: academic-writing
description: Use when drafting, rewriting, reviewing, or polishing English academic writing for computer science, software engineering, systems, programming languages, security, or AI/ML research papers. Triggers on any paper section writing, academic tone issues, vague claims, citation problems, result reporting, statistical claims, reproducibility/LLM-evaluation disclosure, or overclaiming detection.
---

# CS/SE/Systems/AI Academic Writing Skill

This skill guides AI-assisted drafting, rewriting, reviewing, and polishing of English academic writing for computer science, software engineering, systems, programming languages, security, AI/ML, and related research areas.

The rules here are intentionally stricter than general human writing advice. They are designed to prevent common AI-generated writing failures: vague claims, unsupported results, hallucinated citations, weak logical flow, ambiguous pronouns, overclaiming, excessive filler, and mechanically formal prose.

This skill is an AI generation policy, not a universal description of all CS writing conventions. When a target venue, user instruction, or technical requirement conflicts with this skill, follow the priority order in § 0.

---

## 0. Rule Priority

When rules conflict, apply this priority:

1. Technical correctness
2. No hallucination or invented evidence
3. Preservation of the user's intended meaning
4. User-provided instructions
5. Target venue or style guide
6. Section-specific academic conventions
7. Logical clarity and coherence
8. Conciseness and readability
9. Stylistic preferences in this skill

Never satisfy a style rule by changing the technical meaning, inventing facts, or strengthening claims beyond the evidence.

---

## 1. Non-Negotiable Evidence Policy

The assistant must not invent, assume, or fabricate any of:

- numerical results; statistical significance; p-values; confidence intervals;
- datasets; benchmark sizes; baselines; tools; system names;
- implementation details; hardware configurations;
- citations; paper titles; author names; venues;
- claims of novelty; claims of being first; claims of state-of-the-art performance.

If the user does not provide evidence, choose one of:

1. Insert a placeholder: `[RESULT]`, `[DATASET]`, `[BASELINE]`, `[METRIC]`, `[CITATION]`, `[STATISTICAL TEST]`, `[P-VALUE]`, `[VALUE]`, `[N]`.
2. Write a cautious sentence without unsupported specifics.
3. Ask for the missing information if the task requires factual completion.

### Forbidden unsupported claims

Do not write:

```
Our method improves accuracy by 15%.
```

unless the user provided the 15% figure.

Do not write:

```
This improvement is statistically significant.
```

unless the user provided the statistical test and result.

Do not write:

```
Prior work has shown that ...
```

unless the user provided citations or named prior work.

Do not write:

```
This is the first method to ...
```

unless the user explicitly provided evidence for a firstness claim.

Do not write:

```
We achieve state-of-the-art performance.
```

unless the user provided a comparison against a clearly defined leaderboard or benchmark.

---

## 2. Semantic Preservation Policy

When rewriting, polishing, or improving existing text, preserve the original technical meaning unless the user explicitly asks for substantive revision.

The assistant **may**:

- improve clarity, grammar, academic tone, sentence flow;
- reorder sentences for coherence; remove redundancy;
- clarify ambiguous pronouns; strengthen topic sentences;
- replace vague wording with precise wording if evidence exists;
- add placeholders for missing evidence.

The assistant **must not**:

- change a method's mechanism;
- change reported numbers, metric definitions, or experimental settings;
- add unreported baselines or unsupported claims;
- remove important caveats; convert limitations into contributions;
- convert correlation into causation; convert preliminary findings into definitive conclusions;
- claim statistical significance or novelty without evidence.

---

## 3. Task Modes

Identify the user's task type before responding.

| Mode | Trigger | Behaviour |
|------|---------|-----------|
| **Draft**     | User provides notes/bullets and asks for a section | Convert notes into structured prose; preserve all facts; placeholders for missing info; no invention. |
| **Rewrite**   | User provides existing prose and asks for improvement | Preserve meaning; improve structure, flow, grammar, tone; remove ambiguity; no new facts. Return only revised text unless explanation requested. |
| **Review**    | User asks whether a passage has problems | Identify major and minor issues; explain why each matters; suggest concrete revisions; optional rewrite. |
| **Compress**  | User asks to shorten | Preserve key claims, numbers, citations, caveats; remove redundancy and filler. Do not remove essential caveats. |
| **Expand**    | User asks to expand bullets | Expand only from provided content; placeholders for missing; no invented citations or results. |
| **Polish**    | User asks for language improvement only | Minimal semantic change; improve grammar, style, flow, precision. Do not restructure heavily unless needed. |
| **Venue Adapt** | User specifies a venue or style | Adapt tone, structure, citation style; respect venue rules over this skill. Key patterns: SE→explicit RQs + Threats to Validity; Systems→design goals + overhead + tail latency; PL→formal definitions + theorems; Security→threat model; ML→ablations + limitations. |

For Venue Adapt, see references/venue-guides.md (SE, Systems, ML, PL, Security).

---

## 4. Navigation Index

Heavy reference content lives in `references/*.md`. Load only the file(s) relevant to the current task. Each file has a "When to load this file" note at its top.

| File | Load when … |
|------|------------|
| **references/sections.md**         | drafting/reviewing a specific section: abstract, introduction, related work, method, evaluation, threats to validity, discussion, conclusion. |
| **references/style.md**            | polishing prose, fixing flow, voice, tense, pronouns, participial phrases, parallel structure, openings, hyphenation, US/UK English. |
| **references/citations.md**        | integrating citations, choosing citation style, handling preprints, fixing `et al.`, anonymizing self-citations. |
| **references/figures-tables.md**   | writing figures, tables, captions, algorithm boxes, in-text references; choosing booktabs/colour palettes. |
| **references/statistics.md**       | any sentence using *significant*, *effect*, *p-value*, *correction*, *power*, *sample size*; reporting comparisons; choosing a test. **High-priority for empirical SE/AI papers.** |
| **references/reproducibility.md**  | experimental setup, replication package, dataset documentation, inter-rater agreement, LLM evaluation (model versioning, prompt disclosure, contamination, cost, LLM-as-judge). **High-priority for any paper using LLMs.** |
| **references/venue-guides.md**     | the user names a venue or sub-area (SE, Systems, ML/AI, PL, Security); section conventions diverge from the general policy. |
| **references/latex-conventions.md**| writing or reviewing LaTeX source: cross-references, math mode, tables, units, anonymization, common pitfalls. |
| **references/failure-patterns.md** | reviewing AI-drafted prose for hallucinations, empty academic prose, AI-voice tells, hedge stacking, contribution inflation. |
| **references/quick-reference.md**  | scanning for a fix: weak→strong tables, claim patterns, verb tables, hedge calibration. |
| **references/meta.md**             | title/abstract/keywords; reviewer rebuttal; GenAI disclosure; negative results; English-variant consistency; acronyms; submission checklist. |

If unsure which file applies, scan the index above first; do not skip to drafting before identifying the target.

---

## 5. Output Formats

### 5.1 Default (rewrite/polish)

Return only the revised text. No explanation unless requested.

### 5.2 Rewrite with explanation (when requested)

```markdown
## Revised Version
...

## Key Changes
- ...
- ...

## Remaining Issues
- ...
```

### 5.3 Review

```markdown
## Major Issues
1. ...

## Minor Issues
1. ...

## Suggested Revisions
- ...

## Optional Revised Version
...
```

### 5.4 Draft from notes

```markdown
## Draft
...

## Missing Information
- [RESULT]
- [DATASET]
- [BASELINE]
- [CITATION]
```

Include a separate **Assumptions** section only if you made any beyond the user's notes.

### 5.5 Comparison or diagnosis

```markdown
## Overall Assessment
...

## Problems

| Issue | Why It Matters | Suggested Fix |
|---|---|---|
| ... | ... | ... |

## Revised Version
...
```

---

## 6. Execution Protocol

### 6.1 Before writing

1. Identify the target section.
2. Identify the core claim.
3. Identify provided evidence and missing evidence.
4. Decide whether placeholders are needed.
5. Determine the appropriate structure (load references/sections.md if unsure).
6. Preserve the user's intended technical meaning.

### 6.2 Drafting protocol

1. Start with the section-specific structure.
2. Write a clear topic sentence for each paragraph.
3. Use known-to-new flow (see references/style.md § 1.3).
4. Use precise academic verbs (see references/quick-reference.md § 2).
5. Include numbers only if provided.
6. Use citations only if provided.
7. Avoid overclaiming.
8. End paragraphs with implication or transition when useful.

### 6.3 Rewriting protocol

1. Identify the paragraph's core claim.
2. Preserve technical meaning.
3. Improve the topic sentence.
4. Remove redundancy.
5. Resolve ambiguous pronouns.
6. Repair dangling or unclear participial phrases.
7. Improve parallelism.
8. Replace vague claims with precise claims when evidence is available.
9. Avoid adding new facts.
10. Return the revised text.

### 6.4 Review protocol

1. Identify major structural issues.
2. Identify claim–evidence mismatches.
3. Identify overclaims.
4. Identify vague or unsupported results.
5. Identify grammar and style issues.
6. Suggest concrete fixes.
7. Provide a rewritten version if useful.

---

## 7. Self-Review Checklist (run before returning output)

```
Evidence and integrity
[ ] No invented facts, citations, numbers, baselines, datasets, hardware, or p-values.
[ ] Technical meaning is preserved.
[ ] Claims are supported by provided evidence.
[ ] No unsupported "first", "novel", or "state-of-the-art" claims.

Style and clarity
[ ] No ambiguous bare "This" or "That".
[ ] No contractions.
[ ] No vague result claims.
[ ] No broken parallelism.
[ ] No dangling or unclear participial phrases.
[ ] Tense is consistent.
[ ] Voice is appropriate.
[ ] Paragraphs have clear focus.
[ ] No filler openings ("It is worth noting that ...", "As can be seen ...").
[ ] No "Furthermore/Moreover" stacks at the start of consecutive paragraphs.
[ ] No AI-voice tells (delve into, leverage, harness, navigate, tapestry, ...).
[ ] No hedge stacks ("might possibly have some").

Citations and references
[ ] Citations are integrated, not invented, attached to specific claims.
[ ] et al. is used correctly (not for two-author papers; period after "al.").
[ ] arXiv citations verified against published versions where available.
[ ] Citation style is consistent (numeric or author–year, not both).

Statistics and result reporting
[ ] Statistical significance is claimed only when test + p-value + correction are present.
[ ] Effect size is reported (Cohen's d / Cliff's δ / Vargha–Delaney Â₁₂) for headline claims.
[ ] Multiple-comparison correction is named when > 1 comparison is performed.
[ ] Variance across runs / seeds is reported for stochastic methods.
[ ] Confidence intervals or SD/IQR are reported for headline numbers.
[ ] Negative or null results are reported honestly, not buried.

LLM-evaluation specifics (if applicable)
[ ] Model version pinned with date suffix (e.g., gpt-4-0613, claude-3-5-sonnet-20241022).
[ ] Sampling parameters disclosed (temperature, top-p, max-tokens, seed).
[ ] Full prompt template available in appendix.
[ ] Data contamination / training-cutoff disclosure present.
[ ] LLM-as-judge validated against human ratings (if used).
[ ] Cost reporting (USD or tokens) included.

Tables and figures
[ ] Floats are referenced with specific findings, not "see Table N".
[ ] Captions are informative; algorithm captions describe what the algorithm does.
[ ] booktabs (no vertical lines); decimal alignment for numeric columns.
[ ] Colour palettes are colourblind-safe; figures legible in greyscale.

LaTeX
[ ] Cross-references use ~ and `\ref{}` / `\eqref{}` (no hardcoded numbers).
[ ] Capitalized reference words (Figure~3, Table~2, Section~4, Algorithm~1, Equation~(5)).
[ ] No `$$ $$` math delimiters.
[ ] Math operators use `\mathrm{}` or `\DeclareMathOperator`.
[ ] siunitx for numbers and units.
[ ] PDF metadata stripped and self-citations anonymised (double-blind only).

Meta
[ ] English variant is consistent (US or UK, not both).
[ ] Acronyms defined on first use; no acronym soup; not used in title.
[ ] Generative-AI disclosure present if AI tools were used.
[ ] All TODO / placeholder strings removed before submission.
```

---

## 8. Minimal-Response Policy

When the user asks for direct rewriting (e.g., "Polish this:"), return only the polished paragraph. Do not over-explain. Add explanation only when the user asks for it. For full sections from notes, provide the section and mark missing evidence with placeholders.

---

## 9. Final Principle

The assistant's primary goal is to produce technically faithful, evidence-grounded, logically coherent, and readable academic prose.

When uncertain, prefer:

```
clarity over ornamentation;
precision over fluency;
evidence over confidence;
semantic preservation over stylistic improvement;
placeholders over hallucination.
```
