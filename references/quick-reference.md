# Quick Reference

> **When to load this file:** when scanning for a specific fix — a weaker word's replacement, a precise verb for a function, a safe vs. unsafe claim pattern. This is the lookup table.

---

## 1. Common Weak Expressions and Replacements

| Weak expression          | Problem            | Better expression                                               |
| ------------------------ | ------------------ | --------------------------------------------------------------- |
| very good                | vague              | substantially improves / achieves higher accuracy               |
| many methods             | vague              | prior methods / several methods [CITATIONS] / N methods         |
| our method is better     | vague              | our method outperforms [BASELINE] on [METRIC]                   |
| this is important        | vague pronoun      | this limitation is important                                    |
| it can be seen that      | filler             | Table N shows that                                              |
| in this paper, we study  | weak opening       | we propose / we investigate / we evaluate                       |
| significant improvement  | ambiguous          | statistically significant improvement / substantial improvement |
| the algorithm is shown   | low information    | Algorithm N describes how ...                                   |
| suffers from             | potentially harsh  | is limited by / does not address / assumes                      |
| solves the problem       | overclaiming       | addresses / mitigates / reduces                                 |
| it is worth noting that  | filler opening     | (state the observation directly)                                |
| as can be seen           | filler             | Table N shows / Figure N shows                                  |
| in order to              | wordy              | to                                                              |
| due to the fact that     | wordy              | because                                                         |
| in light of the fact that| wordy              | because                                                         |
| performs well            | vague              | achieves [VALUE] on [METRIC]                                    |
| obviously                | unsupported        | (delete; if it is obvious, do not say so)                       |
| naturally                | unsupported        | (delete or replace with a reasoned argument)                    |
| basically                | colloquial         | (delete)                                                        |
| pretty / quite / really  | colloquial         | (delete or replace with a quantitative claim)                   |
| various / several / some | vague              | three / N / specific list with citations                        |
| robust to noise          | unsupported        | tolerates [NOISE TYPE] up to [LEVEL] without [DEGRADATION]      |
| achieves significant gain| ambiguous          | achieves [VALUE] over [BASELINE] (Cliff's δ = ...)              |
| outperforms by a wide margin | unscoped       | outperforms by [DIFFERENCE] on [METRIC] / [DATASET]             |

---

## 2. Preferred Academic Verbs

| Function     | Verbs                                               |
| ------------ | --------------------------------------------------- |
| Contribution | propose, introduce, present, develop, design        |
| Analysis     | analyze, examine, investigate, characterize         |
| Evaluation   | evaluate, measure, compare, assess, validate        |
| Result       | achieve, reduce, improve, increase, outperform      |
| Evidence     | show, demonstrate, indicate, suggest, reveal        |
| Limitation   | limit, constrain, assume, require, omit             |
| Mechanism    | enable, allow, support, guide, control              |
| Comparison   | outperform, exceed, match, lag behind, improve upon |
| Definition   | define, denote, refer to, formalize, specify        |
| Replacement  | replace, substitute, supersede                      |
| Approximation| approximate, estimate, bound                        |
| Causation    | cause, lead to, result in, induce                   |

---

## 3. Words to Use Carefully

Use these only when justified:

```
novel
first
state-of-the-art / SOTA
optimal
guaranteed
significant
robust
general
scalable
efficient
lightweight
comprehensive
unprecedented
groundbreaking
revolutionary
```

For each, check whether evidence supports the claim:

| Adjective | Evidence required |
|-----------|-------------------|
| scalable     | results across increasing workloads showing acceptable degradation |
| efficient    | runtime / memory / cost / complexity evidence vs. a comparable baseline |
| robust       | evidence across perturbations, settings, or failure modes |
| comprehensive | broad coverage with a defined scope statement |
| optimal      | proof or a precise optimization context |
| significant  | statistical test + correction + p-value (or large effect size with CI) |
| novel        | clear differentiation from named prior work |
| first        | survey of prior work plus a precise definition of "first to do X" |
| state-of-the-art | leaderboard or comparison table at submission time on a clearly-defined benchmark |

---

## 4. Safe Claim Patterns

```
Our method improves [METRIC] on [DATASET].
The results suggest that [INTERPRETATION].
This design reduces [PROBLEM] by [MECHANISM].
Compared with [BASELINE], [METHOD] achieves [DIFFERENCE].
Within the evaluated setting, [METHOD] [CLAIM].
On the [DATASET] benchmark, [SYSTEM] outperforms [BASELINE] by [DIFFERENCE]
  (Cliff's δ = [δ], p < [α], Wilcoxon signed-rank, Holm-corrected).
We do not claim [BROAD CLAIM]; the evaluation is restricted to [SCOPE].
```

---

## 5. Unsafe Claim Patterns

Avoid unless explicitly supported:

```
Our method solves [BROAD PROBLEM].
This is the first work to [CLAIM].
The method is optimal.
The system is fully robust.
The framework generalizes to all settings.
The improvement is statistically significant.        ← without test + p
Prior work has ignored this problem.
Our method works in all real-world cases.
This is a state-of-the-art result.                   ← without leaderboard
The method is universally applicable.
```

---

## 6. Hedge Strength by Evidence

| Evidence strength       | Verbs                                         | Example                                    |
| ----------------------- | --------------------------------------------- | ------------------------------------------ |
| Direct measured         | achieves, reduces, increases, outperforms     | The method **achieves** 66.4 % pass rate.  |
| Strong interpretation   | indicates, demonstrates, shows                | Table 3 **shows** that …                   |
| Moderate interpretation | suggests, provides evidence that              | These results **suggest** that …           |
| Limited evidence        | may indicate, appears to, is consistent with  | The trend **appears to** support …         |
| Speculation             | might, could, it is plausible that            | A future extension **could** explore …     |

Match the verb to evidence strength. Do not hedge directly observed results; do hedge interpretations and generalizations.

---

> **Note:** These are compact openers for quick lookup. For full section-specific structure, constraints, and templates, see references/sections.md.

## 7. Section-Opening Templates

### Abstract opener (5 moves)

```
[CONTEXT] [Domain or problem setting]
[PROBLEM] [Limitation or gap]
[APPROACH] [Method]
[RESULTS] [Numbers if available]
[IMPACT] [Calibrated implication]
```

### Introduction opener

```
[Domain context: 1–2 sentences]
[Why the problem matters: 1–2 sentences]
[Limitation of current state: 1–2 sentences]
```

### Related-Work paragraph opener

```
Prior work on [TOPIC] has explored [APPROACH] [CITATIONS].
```

### Method overview opener

```
Figure~\ref{fig:arch} illustrates the architecture of [SYSTEM]. The framework consists of
[N] components: [LIST].
```

### Evaluation opener

```
We evaluate [SYSTEM] to answer the following research questions:
RQ1: ...
RQ2: ...
RQ3: ...
```

### Conclusion opener

```
This paper addressed [PROBLEM]. We presented [METHOD/SYSTEM], which [CORE IDEA].
```

---

## 8. Result-Reporting Templates

### With statistical evidence

```
[METHOD] achieves [VALUE] on [DATASET], compared with [BASELINE]'s [VALUE]
(Δ = [DIFF], 95% CI [LO, HI]). The improvement is statistically significant
(p < [α], [TEST], [CORRECTION] for [k] comparisons; Cliff's δ = [δ], [MAGNITUDE]).
```

### Without statistical evidence

```
On [DATASET], [METHOD] achieves [VALUE], compared with [BASELINE]'s [VALUE].
The results show a consistent improvement over the evaluated baselines.
```

### With variance across runs

```
Across 5 independent runs (seeds 0–4), [METHOD] achieves a mean of [VALUE]
(SD = [σ]); [BASELINE] achieves [VALUE] (SD = [σ]).
```

### For null results

```
On [DATASET], [METHOD] does not outperform [BASELINE] (p = [P], [TEST];
Cliff's δ = [δ], [MAGNITUDE]). We discuss possible reasons in Section~\ref{sec:discussion}.
```

---

## 9. Threats-to-Validity Templates

```
A potential threat to internal validity is [THREAT].
We mitigate this threat by [MITIGATION].
However, [REMAINING LIMITATION].

A potential threat to external validity is [THREAT].
We mitigate this threat by [MITIGATION].
Nevertheless, the results may not generalize to [SCOPE].

A potential threat to construct validity is that [METRIC] is a proxy for [INTENDED CONCEPT].
We supplement [METRIC] with [SECONDARY MEASURE] to triangulate.

A potential threat to conclusion validity is the small sample size of [N] benchmarks.
We mitigate this threat by reporting effect sizes and confidence intervals alongside p-values.
```

---

## 10. Compact Style Cheat Sheet

```
Verbs: prefer the most precise verb available.
Voice: active "we" for the authors' actions; passive for procedures.
Tense: present for facts and contributions; past for completed actions and prior authors.
Pronouns: explicit nouns over bare "this"/"that"/"it" when ambiguous.
Numbers: digits for measurement and comparison; words for small counts in prose.
Hedges: match strength to evidence; do not hedge measured numbers.
Citations: integrated, with author name when the cited work is the subject.
Lists: parallel grammar, comparable weight per bullet.
Connectors: only when the relationship is real.
"Significant": only with test + p + correction; or replace with "substantial" / "meaningful".
"First / Novel / SOTA": only when justified.
```
