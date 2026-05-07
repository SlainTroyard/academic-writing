# Citations

> **When to load this file:** integrating citations, choosing citation style, handling preprints, fixing `et al.` mistakes, or reviewing reference integrity.

---

## 1. Citation Integrity

**Do not invent citations.** Do not fabricate author names, paper titles, venues, years, or DOI strings.

If citations are missing, use placeholders:

```
[CITATION]   for an unknown reference
[1]          for a numbered slot
```

Or ask the user for the references. When the user provides a `.bib` file, prefer the keys defined there; never invent a key.

---

## 2. Citation Integration

Integrate citations into sentence flow. Do not append citations as a list of disembodied numbers.

```
Weak:    Many methods have been proposed [1][2][3][4].
Better:  Several methods have been proposed for C-to-Rust migration [1]–[4].

Weak:    [5] showed that LLMs can generate code.
Better:  Wei et al. [5] showed that LLMs can generate syntactically correct code.
```

**Author-led patterns are usually clearer:**

```
Smith and Jones [7] proposed a type-directed repair method.
Chen et al. [8] introduced a benchmark for evaluating code translation systems.
```

Use author-led when the cited work is the *subject* of the sentence; use parenthetical citations when the cited work merely *supports* a general claim.

```
Compiler-guided repair improves precision over signature-based heuristics [6], [7].
```

---

## 3. Citation Style

Follow the target venue.

**Numeric (IEEE, most ACM):**

```
Smith et al. [5] proposed ...
Prior work has explored this problem [1]–[4].
```

**Author–year (some ACM, springer):**

```
Smith et al. (2023) proposed ...
Prior work has explored this problem (Smith et al., 2023; Chen et al., 2024).
```

Do not mix styles unless the user asks. Once you commit to a style at the top of the paper, use it everywhere — including the appendix.

---

## 4. Citation Placement

Place citations at natural pause points (end of clause or sentence; never mid-noun-phrase).

```
Good: Compiler-guided repair has been widely studied in automated program repair [6], [7].
Good: Prior work on static analysis has shown that ownership violations can be detected
      before runtime [8].
```

Avoid dangling citation clusters with no context:

```
Weak:    This is useful [1], [2], [3], [4].
Better:  Prior work has used compiler feedback to localize and repair program errors [1]–[4].
```

**Range vs. comma list:**

- Use ranges (`[1]–[4]`) for **consecutive** numbers.
- Use comma lists (`[1], [3], [5]`) for non-consecutive numbers.
- Do not write `[1]-[2]` — use the en-dash `–` (LaTeX `--`).

---

## 5. Author Name and `et al.` Usage

Follow the venue's citation style for author counts in text citations.

| Author count | In-text form                       | Parenthetical form            |
|--------------|------------------------------------|-------------------------------|
| 1 author     | Smith [5] proposed ...             | (Smith, 2023)                 |
| 2 authors    | Smith and Jones [5] proposed ...   | (Smith & Jones, 2023)         |
| 3+ authors   | Smith et al. [5] proposed ...      | (Smith et al., 2023)          |

Do **not** use `et al.` for papers with exactly two authors.

```
Wrong:   Smith et al. [5] proposed ... (when the paper has two authors)
Correct: Smith and Jones [5] proposed ...
```

**Typographic rules:**

- `et al.` always ends with a period (it abbreviates *et alii*).
- Do not write `et al` without the period.
- Do not italicize `et al.` unless the venue style guide requires it (most CS venues do not).
- The period in `et al.` does not end the sentence: write `Smith et al. [5] showed …`, not `Smith et al [5]. showed …`.

---

## 6. arXiv and Preprint Citations

Prefer the **peer-reviewed published version** over a preprint whenever both exist.

Before citing an arXiv preprint, check whether a venue-published version has appeared.

**arXiv citation format when no published version is available:**

```
A. Smith and B. Jones, "Title of the paper," arXiv preprint arXiv:2301.12345, 2023.
```

**Constraints:**

- Do not cite an arXiv preprint to support an established fact when peer-reviewed alternatives exist.
- Do not claim a preprint result is "proven" or "established."
- Label preprints explicitly when the venue permits: `arXiv preprint` or `under review`.
- Update citations to the published version once the paper appears at a venue.

For SE venues (ICSE, FSE, ASE, ISSTA), arXiv preprints are accepted as evidence of concurrent or contemporaneous work but are not treated as peer-reviewed baselines.

For ML venues, the same rule applies: cite NeurIPS/ICML/ICLR papers when both an arXiv preprint and a conference version exist.

---

## 7. Self-Citation

For double-blind submissions:

- Refer to your own prior work in the third person: "Smith et al. [12] proposed …" — not "Our prior work [12] proposed …".
- Do not redact entries from the bibliography unless the venue instructs you to.
- Do not cite an anonymous preprint URL; the venue will reject it.

For non-anonymous submissions:

- Self-citation is normal but should be justified by content, not volume.
- Avoid citing the same prior paper of yours in every paragraph.

---

## 8. Citation Misuse Patterns to Avoid

**Decoration, not evidence:**

```
Weak:    Code generation is important [1], [2], [3].
Better:  Code generation has emerged as a major application of language models [1], [2].
```

Each citation should support a specific factual claim in the immediately preceding clause.

**Citing surveys for primary results:**

```
Weak:    LLMs can generate Rust code [survey].
Better:  Recent code-generation models can produce Rust output that compiles in [X]% of
         cases [primary paper].
```

When a survey is appropriate (e.g., to establish a research area), cite it explicitly as a survey: "as surveyed by [N], …".

**Citing the wrong paper:**

- Verify each citation actually contains the claim attributed to it.
- Do not transitively cite ("X cites Y, so I'll cite Y") without reading Y.
- For LLMs as drafters: never accept an LLM-suggested citation without checking the paper.

**Inflated citation counts:**

- Do not pad with citations that were not actually consulted.
- Reviewers can usually tell.

---

## 9. BibTeX Hygiene

When the user maintains a `.bib` file:

- Use stable, descriptive keys (e.g., `wei2022chain` rather than `bib1`).
- Provide DOI when available; reviewers cross-check.
- Capitalize protected words in titles using braces: `{R}ust`, `{LLM}`.
- Do not change keys on every paper revision — broken `\cite{}` calls slip through.

---

## 10. Quick Self-Check for Citations

Before submitting:

```
[ ] Every citation supports a specific claim in the surrounding sentence.
[ ] No bare [1] or [CITATION] placeholders remain.
[ ] et al. is used correctly (not for 2 authors; period after "al.").
[ ] arXiv preprints have been replaced with venue versions where possible.
[ ] Citation style is consistent throughout (numeric or author–year, not both).
[ ] Range notation uses en-dash (--), not hyphen.
[ ] Self-citations are anonymized for double-blind submissions.
[ ] No citations to retracted or withdrawn papers.
```
