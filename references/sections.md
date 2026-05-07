# Per-Section Drafting Policies

> **When to load this file:** drafting, rewriting, or reviewing a specific paper section (Abstract, Introduction, Related Work, Method, Evaluation, Threats, Discussion, Conclusion).

This file specifies what each section should contain, how to structure it, and what NOT to do. Templates appear at the end of each subsection.

---

## 1. Abstract

Use a 5-move structure by default for CS/SE/Systems abstracts unless the user or venue specifies otherwise.

```
Move 1: Context
Move 2: Problem or gap
Move 3: Approach
Move 4: Results
Move 5: Implication or impact
```

Target length: usually 150–250 words, unless the venue specifies another limit.

**Hard constraints:**

- Do not invent results.
- Do not invent datasets, baselines, statistical significance, or novelty.
- Do not include excessive background.
- Do not copy sentences verbatim from the introduction.

**Preferred behavior:**

- Start with the technical context or problem domain.
- State the gap clearly.
- Describe the method concretely.
- Report quantitative results if provided.
- End with a calibrated implication.

**Avoid empty openings:**

```
Weak:    In this paper, we study software testing.
Better:  Compiler-guided testing can expose memory-safety bugs that conventional test generation often misses.
```

`In this paper, we …` is acceptable when the sentence immediately states a concrete contribution:

```
Acceptable: In this paper, we present XYZ, a compiler-guided framework for C-to-Rust migration.
```

If results are missing, do not fabricate them. Use placeholders:

```
We evaluate the framework on [DATASET] using [METRIC].
[RESULTS: insert the main quantitative finding.]
```

### Abstract template

```
[CONTEXT]   One sentence establishing the domain or problem setting.
[PROBLEM]   One sentence identifying the limitation or gap.
[APPROACH]  Two to three sentences describing the proposed method.
[RESULTS]   One to two sentences reporting key results, if provided.
[IMPACT]    One sentence stating the implication without overclaiming.
```

### Worked example (illustrative — replace numbers with your own)

```
Automated migration of legacy C code to Rust can improve memory safety without requiring full
manual rewrites. However, existing LLM-based translators often violate Rust ownership rules,
producing code that fails to compile. We propose [SYSTEM], an error-driven adaptive framework
that uses compiler feedback to guide C-to-Rust translation. [SYSTEM] classifies compiler errors
and adjusts generation parameters according to error categories. On [N] benchmarks, [SYSTEM]
achieves a [VALUE] problem-solving rate, outperforming the strongest baseline by [DIFFERENCE]
percentage points. These results suggest that compiler feedback can provide an effective
control signal for LLM-based code migration.
```

---

## 2. Introduction

Use a funnel structure by default.

```
Paragraph 1: Motivation and broad context
Paragraph 2: Problem, gap, or limitation in current work
Paragraph 3: Proposed approach and key idea
Paragraph 4: Contributions
```

For empirical, systems, SE, PL, security, and tool papers, include an explicit contribution list by default unless the user or venue discourages it.

### Contribution list template

```
In this paper, we make the following contributions:

- We propose [SYSTEM/METHOD], a [DESCRIPTION] for [TASK].
- We design [COMPONENT/MECHANISM] that [FUNCTION].
- We implement [PROTOTYPE/TOOL] to [PURPOSE], if implementation is a contribution.
- We evaluate [SYSTEM/METHOD] on [DATASET/BENCHMARK], showing [RESULT if provided].
```

**Bullet rules:**

- Start each bullet with a clear active verb.
- Make each bullet a specific, verifiable claim.
- Avoid vague claims such as "We make several contributions."
- Avoid splitting minor implementation details into separate contributions.
- Include headline results only when provided and directly relevant.
- Do not include detailed result breakdowns in the contribution list.

**Good contribution verbs:** propose, introduce, present, design, develop, implement, evaluate, characterize, analyze, demonstrate, release.

**Avoid unsupported novelty claims:**

```
Avoid: We propose the first framework for ...
Use:   We propose a framework for ...
```

unless firstness is explicitly supported (see references/quick-reference.md § "Words to use carefully").

### Optional last-paragraph element

If the venue allows, end the introduction with a roadmap sentence:

```
The remainder of the paper is organized as follows. Section~2 reviews related work; Section~3
presents the design of [SYSTEM]; ...
```

Most modern venues consider roadmap paragraphs filler. Skip them unless clearly expected.

---

## 3. Related Work

Organize Related Work by **theme**, not by chronology, unless chronology is essential.

### Per-subsection structure

```
1. Topic sentence defining the category of prior work.
2. Discussion of representative approaches.
3. Strengths and limitations of that category.
4. Differentiation from the present work.
```

### Template

```
Prior work on [TOPIC] has explored [GENERAL APPROACH] [CITATIONS].
X et al. [N] proposed [METHOD], which [DESCRIPTION].
This approach [STRENGTH].
However, it [LIMITATION].
In contrast, our method [DIFFERENTIATION].
```

**Hard constraints:**

- Do not invent citations.
- Do not invent author names.
- Do not claim "no one has studied X."
- Do not overstate limitations of prior work.
- Do not use dismissive language.

**Prefer respectful framing:**

```
few studies have addressed ...
is limited to ...
does not directly address ...
assumes ...
focuses primarily on ...
is less suitable for ...
```

**Avoid repetitive negative phrasing:**

```
X suffers from ...
Y suffers from ...
Z suffers from ...
```

Vary the language. Reviewers notice patterns.

### Placement

- Related Work may appear after the Introduction or before the Conclusion.
- Follow venue conventions.

---

## 4. Method / Approach / Design

Use this policy for method, system, architecture, design, algorithm, and implementation sections.

### Default structure

```
1. Overview
2. Design goals or assumptions, if relevant
3. One subsection per major component
4. Algorithmic details
5. Implementation details, if relevant
```

### Overview paragraph

- State the purpose of the system or method.
- Describe the high-level architecture.
- Reference the figure if provided.

```
Figure~\ref{fig:arch} illustrates the architecture of [SYSTEM]. The framework consists of three
components: an error classifier, a temperature scheduler, and a repair generator.
```

### Component paragraph pattern

```
The [COMPONENT] performs [PURPOSE].
It takes [INPUT] and produces [OUTPUT].
The component uses [MECHANISM] to [FUNCTION].
This design enables [BENEFIT].
```

### Algorithm reference pattern

```
Algorithm~\ref{alg:scheduler} describes the temperature scaling procedure. Given an error
category e, the algorithm selects a temperature value from the mapping table T and applies the
value to the next generation attempt.
```

**Avoid low-information references:**

```
Weak:    The algorithm is shown in Algorithm~1.
Better:  Algorithm~\ref{alg:scheduler} describes how the scheduler maps compiler error
         categories to sampling temperatures.
```

### Method-section discipline

- Keep mechanism descriptions in Method.
- Keep empirical findings in Results.
- Do not introduce new motivation or related work.
- Define every symbol/term before use; on first use, italicize with `\emph{}`.

### Venue-flavored content

- **Systems:** design goals, assumptions, architecture, implementation, deployment model, failure handling, overhead, scalability considerations. (See references/venue-guides.md)
- **PL:** syntax, semantics, type rules, inference rules, theorem statements, proof sketches.
- **Security:** threat model, attacker capabilities, assumptions, defense scope, limitations.

---

## 5. Evaluation

The evaluation structure depends on paper type.

### Default for empirical SE papers

```
1. Research Questions
2. Experimental Setup
3. Results organized by RQ
4. Threats to Validity
```

### Default for systems papers

```
1. Evaluation Goals
2. Experimental Setup
3. End-to-End Performance
4. Microbenchmarks
5. Scalability
6. Overhead
7. Ablation or Sensitivity Analysis
8. Discussion or Limitations
```

For Systems-specific reporting conventions (tail latency, throughput, warm-up/steady-state), see **references/venue-guides.md § 2**.

### Default for ML-oriented papers

```
1. Experimental Setup
2. Datasets
3. Baselines
4. Metrics
5. Main Results
6. Ablation Study
7. Sensitivity Analysis
8. Efficiency or Cost
9. Limitations
```

### Hard constraints

- Do not invent datasets, benchmark sizes, baselines, hardware, metrics, or p-values.
- Do not claim statistical significance without evidence.
- Do not use "significant" ambiguously — see references/statistics.md.

### Research Question rules

- Each RQ should be a question.
- Each RQ should be answerable with the available data.
- RQs should address distinct aspects.
- RQ1 usually addresses main effectiveness.
- Later RQs may address comparison, ablation, efficiency, robustness, or generalizability.
- State the RQ, the metric used to answer it, and the outcome together.

```
RQ1: How effective is [SYSTEM] in improving the compilation success rate of LLM-generated Rust code?
RQ2: How does [SYSTEM] compare with existing automated migration tools?
RQ3: How much does each component contribute to overall performance?
```

### Result reporting pattern

```
Table~\ref{tab:main} shows [METRIC] on [DATASET].
[METHOD] achieves [VALUE], outperforming [BASELINE] ([VALUE]) by [DIFFERENCE].
```

When statistical evidence is provided:

```
This improvement is statistically significant (p < 0.01, McNemar's test, Holm-corrected for 3
comparisons). The effect size is large (Cliff's δ = 0.62).
```

When statistical evidence is not provided:

```
The results show a consistent improvement over the evaluated baselines.
```

For details on tests, effect sizes, multiple-comparison corrections, and CIs, see **references/statistics.md**.

### Reproducibility for ML/AI venues

NeurIPS, ICML, ICLR, AAAI now require explicit reproducibility information (see references/reproducibility.md):

- Compute reporting (GPU model, count, memory, runtime)
- Variance across N independent runs with different seeds
- Code and data availability statement
- LLM evaluation parameters (model + version + date, temperature, prompt template)
- Data contamination / training-cutoff disclosure
- Cost reporting (USD or tokens) when LLMs are used

---

## 6. Threats to Validity

Use Threats to Validity by default for empirical SE papers. The taxonomy below follows Wohlin et al., *Experimentation in Software Engineering* (Springer, 2012).

### Standard categories

```
Internal Validity
Threats related to implementation bugs, measurement errors, uncontrolled variables, or
experimental procedure. Did the change in the dependent variable actually result from the
manipulated variable?

External Validity
Threats related to generalizability across datasets, languages, systems, domains, or users.

Construct Validity
Threats related to whether metrics measure the intended concept.

Conclusion Validity
Threats related to statistical analysis, sample size, variance, multiple comparisons, or
inference reliability.
```

### Pattern

```
A potential threat to [VALIDITY TYPE] is [THREAT].
We mitigate this threat by [MITIGATION].
Nevertheless, [REMAINING LIMITATION].
```

### Common SE-specific threats to remember

- **Internal:** subject selection bias; tool/version drift; non-deterministic LLM sampling without seed control.
- **External:** small benchmark size; one programming language only; one model family only.
- **Construct:** proxy metric (e.g., compilation success ≠ correctness); annotator bias.
- **Conclusion:** small N for statistical tests; multiple comparisons not corrected; effect size not reported.

### Equivalent sections at other venues

For systems, security, ML, or PL papers, the equivalent section may be:

- Limitations
- Discussion
- Reproducibility
- Ethical Considerations
- Artifact Availability
- Measurement Bias
- Deployment Limitations

---

## 7. Discussion

The Discussion section should **interpret** results rather than repeat them.

### Recommended structure

```
1. What the results mean
2. Why the method worked or failed
3. Practical implications
4. Limitations
5. Future directions, if relevant
```

**Good discussion behavior:**

- Explain mechanisms behind results.
- Discuss unexpected findings.
- Identify boundary conditions.
- State limitations honestly.
- Avoid introducing new experiments unless clearly labeled.
- Distinguish exploratory observations from confirmatory tests.

**Avoid mere repetition:**

```
Weak:    As shown in Table~3, our method achieves 85.58%.
Better:  The improvement suggests that compiler feedback provides a useful signal for guiding
         generation, especially when errors correspond to local repair actions.
```

---

## 8. Conclusion

The conclusion should **summarize**, not introduce.

### Default structure

```
1. Restate the problem.
2. Summarize the approach.
3. Summarize key findings.
4. State implication or future work.
```

**Hard constraints:**

- Do not introduce new technical claims.
- Do not introduce new citations.
- Do not introduce new results.
- Do not copy the abstract verbatim.
- Do not overclaim.

For empirical papers, include key quantitative findings if provided. For theoretical/conceptual/design papers, summarize the main theorem, insight, design principle, or implication.

### Conclusion template

```
This paper addressed [PROBLEM].
We presented [METHOD/SYSTEM], which [CORE IDEA].
Our evaluation shows that [KEY RESULT, if provided].
These findings suggest that [CALIBRATED IMPLICATION].
Future work will [FUTURE DIRECTION], if relevant.
```

### Worked example (illustrative — replace numbers with your own)

```
This paper addressed the challenge of improving LLM-based C-to-Rust migration. We presented
[SYSTEM], an error-driven framework that adapts generation behavior using compiler feedback.
On [N] benchmarks, [SYSTEM] achieved a [VALUE] problem-solving rate and outperformed the
strongest baseline by [DIFFERENCE] percentage points. These findings suggest that compiler
feedback can serve as an effective control signal for code migration.
```

---

## 9. Other common sections (brief)

### Background

- Cover only what the reader needs to follow the rest of the paper.
- Avoid textbook-level expansions.
- Cite primary sources, not surveys, when possible.
- Define every term you will reuse later.

### Acknowledgments

- Funding sources (grants, institutions).
- Specific people (anonymize for double-blind submissions).
- Computing resources / cloud credits if non-trivial.
- Do not acknowledge "the authors of all related work."

### Declarations / Ethics

- Conflicts of interest.
- IRB / ethics-board approval if human subjects.
- Data-use compliance if proprietary data.
- AI-tool usage disclosure (see references/meta.md § Generative-AI disclosure).
