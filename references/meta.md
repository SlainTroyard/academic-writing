# Meta-Writing Concerns

> **When to load this file:** writing or reviewing the title, abstract, keywords; preparing a rebuttal; deciding GenAI disclosure language; auditing English-variant consistency or acronym discipline; reporting negative results.

This file collects writing concerns that span the whole paper rather than a single section.

---

## 1. Title

The title is the most-read sentence of the paper. Reviewers, search engines, and citation indices use it.

### Rules

- **Specific over vague.** Convey the contribution, the domain, and (when distinctive) the method.
- **Front-load the keyword.** Start with the noun phrase that names what the paper is about.
- **Length: 8–14 words is typical.** Beyond 14 words usually signals an unresolved structure.
- **No punctuation as decoration.** Avoid `?` and `!`. Use a colon when a precise sub-title is needed.
- **No acronyms in the title** unless they are universally known (HTTP, DNS, GPU, LLM are usually fine; project-specific acronyms are not).
- **No "Towards" titles** unless the paper is genuinely a position/vision paper.

### Style

| Style | Example | Where used |
|-------|---------|------------|
| Sentence case | *Compiler-guided translation of C to safe Rust* | many SE/PL venues; modern ACM |
| Title Case | *Compiler-Guided Translation of C to Safe Rust* | older IEEE; many ACM templates |

Pick the style your venue's `\title` macro renders. Do not invent a third style.

### Examples

```
Weak:    A Study on LLMs.
Better:  Compiler-guided translation of C to safe Rust with large language models.

Weak:    Improving Performance.
Better:  Adaptive temperature scheduling reduces compilation failures in LLM-based code
         migration.

Weak:    Towards Better Code Generation.       (only OK if it's a vision paper)
Better:  AdaTrans: A compiler-guided framework for LLM-based C-to-Rust migration.
```

---

## 2. Keywords

Most CS venues request 4–8 keywords. They feed search indices and reviewer matching.

### Rules

- Use **terms reviewers would search for**, not invented phrases.
- Mix **broad-area** keywords (*program repair*, *language models*) with **specific** keywords (*ownership types*, *compiler feedback*).
- Avoid the title verbatim — keywords should add coverage, not duplicate it.
- Include the venue's preferred taxonomy (ACM CCS, IEEE INSPEC) when required.

### Example

```
Title:    Compiler-guided translation of C to safe Rust with large language models.
Keywords: program migration, large language models, Rust, ownership, compiler feedback,
          empirical evaluation
```

---

## 3. Abstract Crafting (beyond the structural template)

For the structural 5-move template, see references/sections.md § 1. Additional crafting rules:

- **First sentence determines whether reviewers keep reading.** Make it concrete.
- **No citations in the abstract** unless the venue explicitly allows them.
- **No undefined acronyms in the abstract** — define every acronym you use.
- **No URLs** unless the venue requires the artifact link in the abstract.
- **Numbers belong in the abstract** when they are headline-worthy (e.g., percentage improvement, dataset size).

---

## 4. Acronym Discipline

### Define on first use

```
Spell out + acronym in parentheses on first body-text use:
We use \emph{Adaptive Temperature Scheduling} (ATS) to ...

After that, use the acronym only:
ATS classifies compiler errors into ...
```

### Define separately in the abstract

The abstract is a separate document. If the acronym is needed in both abstract and body, define it in each.

### Avoid acronym soup

- Do not coin acronyms for terms used fewer than three times.
- Do not coin acronyms whose expansion is not memorable.
- Do not stack acronyms (`MMRC-LLM-EVAL` is unreadable).

### Capitalization

- Standard: all-caps unless the venue specifies otherwise (HTTP, GPU, LLM).
- For project names that are *not* acronyms, treat them as proper nouns (`AdaTrans`, not `ADATRANS`).
- Be consistent paper-wide; add a `\newcommand{\sysname}{AdaTrans}\xspace` macro.

---

## 5. English-Variant Consistency (US vs. UK)

Pick one and apply it consistently. Most CS venues default to US English. The IEEE template uses US; ACM allows either.

### Common pairs

| US                | UK                |
|-------------------|-------------------|
| analyze           | analyse           |
| behavior          | behaviour         |
| color             | colour            |
| modeling, modeled | modelling, modelled |
| labeled           | labelled          |
| optimize          | optimise          |
| recognize         | recognise         |
| defense           | defence           |
| center            | centre            |
| program (verb)    | programme (UK noun differs) |
| favor             | favour            |
| catalog           | catalogue         |
| toward            | towards           |

### Quick check

```bash
# Spot-check a tex file for inconsistency
grep -n "behavio\(u\)\?r" *.tex
grep -n "analy\(s\|z\)e" *.tex
grep -n "modelle\?d\|modeli\(s\|z\)ing" *.tex
```

### Tools

- `aspell --lang=en_US` or `aspell --lang=en_GB`.
- `chktex` catches some inconsistencies.
- `LanguageTool` can be run locally on the LaTeX source.

---

## 6. Generative-AI Disclosure

Most major venues now require disclosure of LLM use in writing. Examples (rules change rapidly; check the current CFP):

- **NeurIPS, ICML, ICLR:** disclose substantive use of LLMs in research and writing.
- **ACL, EMNLP:** disclose use beyond simple grammar checking.
- **ICSE, FSE, ASE, ISSTA:** policies vary by year; many require a statement.
- **Nature, Science:** LLMs are not authors; their use must be documented.

### Default rule of thumb

| Use | Disclose? |
|-----|-----------|
| Spell-check, grammar correction in a fixed editor | usually no |
| Sentence-level polishing of authored prose       | yes (one-line statement) |
| Drafting paragraphs from author notes            | yes (more detail) |
| Drafting full sections                           | yes; may be problematic at some venues |
| Generating data, results, or citations           | **never acceptable** |
| Used as an experimental tool inside the paper    | document as a method, not as authorship |

### Disclosure templates

Minimal:

```
\textbf{Use of generative AI.} The authors used [TOOL, e.g., Claude 3.5 Sonnet] for
sentence-level polishing of author-written prose. All technical claims, citations, and
results were authored and verified by the human authors.
```

More detailed:

```
\textbf{Use of generative AI.} We used [TOOL] to polish the writing of Sections 1, 2, and 7
and to suggest alternative phrasings throughout. We did not use generative AI to draft new
technical content, generate or interpret experimental results, suggest citations, or write
the abstract. All AI-suggested edits were reviewed and approved by the authors. The final
prose is the authors' responsibility.
```

If the paper *does not* use AI tools, some venues require a positive statement:

```
\textbf{Use of generative AI.} The authors did not use generative AI tools in the writing or
preparation of this manuscript.
```

---

## 7. Negative and Null Results

Negative results are not failures of the paper; they are evidence. Report them honestly.

### When the paper has a negative finding

- State it clearly in the abstract and the discussion.
- Do not bury it in an appendix.
- Distinguish "no detectable improvement" from "the method is wrong."

### Templates

```
Contrary to our expectation, [METHOD] did not improve [METRIC] on [DATASET]
(p = [P], Cliff's δ = [δ], 95% CI [LO, HI]). We hypothesise that the limited improvement is
due to [REASON]; testing this hypothesis is left to future work.
```

```
Although [METHOD] showed promising results on [DATASET-1], it failed to generalise to
[DATASET-2]. We discuss the cross-dataset gap in Section~\ref{sec:discussion} and treat the
[DATASET-1] result as preliminary.
```

### What not to do

- Reframe a non-significant result as "trending toward significance."
- Drop the negative finding silently.
- Convert it into a "limitation" sentence that obscures it.
- Run new tests until one is positive (p-hacking — see references/statistics.md § 10).

### Exploratory vs. confirmatory

- A **confirmatory** analysis tests a hypothesis specified before the data were seen.
- An **exploratory** analysis is hypothesis-generating.
- Label each clearly. Do not present an exploratory finding as if it were confirmatory.

---

## 8. Reviewer Response and Rebuttal

### Cover letter to the editor (when required)

Short, factual, structured.

```
Dear Editor,

We thank the reviewers for their feedback on our paper "[TITLE]" (manuscript ID [N]).

We have addressed each comment in the revised manuscript. Major changes:

  1. We added Section~3.2 to address Reviewer 1's concern about [TOPIC].
  2. We expanded the threat-to-validity discussion (Section~6) per Reviewer 2.
  3. We added a new ablation experiment (Table~5) requested by Reviewer 3.

A point-by-point response is attached. Changes in the manuscript are highlighted in blue.

Sincerely,
[Authors]
```

### Response to reviewers

- One reviewer per top-level section.
- Quote the comment, then respond, then point to the change in the manuscript.

```
\section*{Response to Reviewer 1}

\textbf{Comment 1.1.} ``The threat model is unclear regarding adversary capabilities.''

\textbf{Response.} We thank the reviewer for this observation. We have rewritten Section~3
to explicitly enumerate adversary capabilities, knowledge, and goals (now Section~3.1).
The revised text appears on page~4, lines 7--22 of the revised manuscript.
```

### Tone

- Polite, factual, no rebuttal of valid criticism.
- "We agree" or "We respectfully disagree" — not "the reviewer misunderstood".
- When you disagree, explain why with evidence; when you agree, fix and acknowledge.
- If a request is genuinely impossible (e.g., "compare with proprietary system you cannot access"), explain the constraint and offer an alternative.

### Format

- Many venues impose a strict word/page limit (1 page, 2500 words common).
- Use the venue's response template if provided.
- Bold reviewer comments; plain-text responses.

---

## 9. Conflicts of Interest, Ethics, and Data Use

### Conflicts of interest

- Disclose financial ties to companies whose products are evaluated.
- Disclose advisor / advisee relationships affecting reviewer assignment.

### IRB / ethics-board approval

For studies involving human subjects (interviews, surveys, observational studies):

```
This study was approved by the [INSTITUTION] Institutional Review Board (Protocol [N]).
All participants provided informed consent.
```

### Data licensing

- Cite the licence of every dataset you use (see references/reproducibility.md § 4).
- Comply with redistribution restrictions; do not silently re-host.
- For scraped data: declare the source, date, and any robots.txt constraints.

---

## 10. Plagiarism and Self-Plagiarism

- Do not copy text from other papers, including your own prior papers, without quotation and citation.
- For survey or extension papers, declare the relationship to prior work explicitly:

```
This paper extends our prior work [N] by adding [NEW MATERIAL]. Sections~3 and~5 contain
substantially new content; Sections~2 and~4 summarise material from [N].
```

- Tools (Turnitin, iThenticate) are used by venues; AI-detection tools are increasingly used too. Do not test them.

---

## 11. Submission Checklist (Cross-Cutting)

```
[ ] Title is specific, concrete, under 14 words; sentence case matches venue.
[ ] Keywords are 4–8 distinct terms reviewers would search for.
[ ] Abstract follows the 5-move structure; no citations; all acronyms defined.
[ ] Author list is final and ordered correctly; affiliations match.
[ ] Funding sources acknowledged; conflicts of interest disclosed.
[ ] Generative-AI disclosure included if any AI tools were used.
[ ] English variant consistent throughout (US or UK, not both).
[ ] No undefined acronyms.
[ ] Anonymized for double-blind review (PDF metadata, repo links, self-citations).
[ ] Replication package linked (anonymized for blind review).
[ ] Reproducibility statement present.
[ ] Ethics statement present if human subjects, scraped data, or sensitive use cases.
[ ] All TODO / placeholder text removed (`[CITATION]`, `[VALUE]`, `TODO`, `XXX`).
[ ] Page-limit and word-limit constraints satisfied.
```
