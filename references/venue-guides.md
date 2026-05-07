# Venue-Specific Guides

> **When to load this file:** when the user names a target venue or sub-area (SE, Systems, ML/AI, PL, Security), or when section conventions differ from the general policies.

This file complements references/sections.md. The general policies still apply; this file lists what each sub-area emphasises and the section vocabulary specific to that area.

---

## 1. Empirical Software Engineering (ICSE, FSE, ASE, ISSTA, EMSE, TSE, TOSEM)

### Expected sections

```
Introduction
Background (often combined with Related Work)
Approach / Methodology
Experimental Setup
  - Research Questions
  - Datasets / Benchmarks / Subjects
  - Baselines
  - Metrics
  - Implementation
Results (organised by RQ)
Discussion
Threats to Validity
Conclusion
```

### What reviewers look for

- **Explicit RQs** that map one-to-one to sub-sections in Results.
- **Threats to Validity** in Wohlin's four categories (Internal / External / Construct / Conclusion). See references/sections.md § 6.
- **Statistical rigour:** non-parametric tests + effect size + multiple-comparison correction. See references/statistics.md.
- **Replication package** with artifact-track badges.
- **Honest discussion** of when the method does *not* work.

### Common reviewer concerns

- "Why these baselines and not [X]?"
- "Is the benchmark too small / non-representative?"
- "Have you corrected for multiple comparisons?"
- "Is statistical significance the same as practical significance?"
- "What's the inter-rater agreement on the qualitative coding?"
- "Has the method been run multiple times to assess variance?"

### Common SE-specific terms to use precisely

| Term | Meaning |
|------|---------|
| Bug / defect / fault / failure | A bug is a developer-perceived issue; a fault is a code error; a failure is an observable misbehaviour. Distinguish. |
| Mutant | A program with one syntactic mutation. |
| Mutation score | Killed mutants / total non-equivalent mutants. |
| Coverage | State the **type**: line, branch, MC/DC, mutation. Coverage alone is not adequacy. |
| Fix-inducing commit | The commit that introduced the bug. Use SZZ if computed automatically. |
| Repository / project / system | Avoid mixing. Pick one term per study. |

---

## 2. Systems (OSDI, SOSP, ATC, NSDI, EuroSys, ASPLOS, MLSys)

### Expected sections

```
Introduction
Background / Motivation
Design / Architecture (often the largest section)
Implementation
Evaluation
  - Methodology / Setup
  - End-to-End Performance
  - Microbenchmarks
  - Scalability
  - Overhead
  - Sensitivity / Ablation
Discussion
Related Work
Conclusion
```

### What reviewers look for

- **Concrete design goals** stated upfront, then evaluated against.
- **Tail latency**, not just average latency.
- **Steady-state vs. warm-up** distinction.
- **Direct comparison** with the strongest existing system, not strawmen.
- **Ablation** that shows which design choices matter.
- **Production-style workloads** (or a clear justification for synthetic ones).

### Latency reporting

Always report:

- **P50 (median):** typical user experience.
- **P99 / P99.9 (tail):** SLA-relevant.
- **Mean** is *not enough* for systems work.

```
Under load X, [SYSTEM] sustains a median latency of [V_p50] ms and a P99 latency of [V_p99]
ms, compared with [BASELINE]'s [B_p50] ms and [B_p99] ms.
```

### Throughput reporting

```
At saturation, [SYSTEM] sustains [N] requests per second on [HARDWARE], a [Δ%] improvement
over [BASELINE].
```

State the workload, the hardware, the duration, and the steady-state criterion.

### Warm-up and steady state

- Discard the first $T$ seconds of measurement (justify $T$).
- Report measurement window length.
- Report the statistical method used to detect steady state (often a moving-window CV threshold).

```
We discard the first 60 s of each run as warm-up. Steady state is detected as a 10-second
moving-window coefficient of variation below 5%.
```

### Reporting variance

- Use **coefficient of variation** (CV = SD / mean) for runtime metrics.
- Report ≥ 3 trials for any micro-benchmark, ≥ 5 for headline numbers.
- Use error bars or shaded bands on plots.

### Common systems-specific terms to use precisely

| Term | Meaning |
|------|---------|
| Latency vs. response time | Latency = network traversal; response time = end-to-end. Don't conflate. |
| Bandwidth vs. throughput | Bandwidth = capacity (link); throughput = achieved rate (system). |
| Goodput | Useful throughput, excluding retransmissions / overhead. |
| Tail latency | High percentiles (P99, P99.9, P99.99). |
| QPS / RPS / TPS | Queries / Requests / Transactions per second. State which. |

---

## 3. ML / AI (NeurIPS, ICML, ICLR, AAAI, AISTATS, COLT)

### Expected sections

```
Introduction
Related Work (sometimes after Method)
Method (sometimes called "Approach")
  - Preliminaries / Notation
  - Problem Formulation
  - Proposed Approach
Theoretical Analysis (when relevant)
Experiments
  - Setup
  - Datasets
  - Baselines
  - Main Results
  - Ablations
  - Sensitivity
Limitations / Broader Impact
Conclusion
```

### What reviewers look for

- **Reproducibility checklist** (NeurIPS now mandates one).
- **Compute disclosure**, **variance across seeds**, **hyperparameter sweeps**.
- **Ablation studies** justifying each design choice.
- **Limitations** as a separate sub-section.
- **Broader impact** statement (NeurIPS).
- **Generative-AI disclosure** (see references/meta.md).

### Reporting templates

```
We train [MODEL] for [N] epochs on [DATASET] using AdamW (lr = [α], β = [β], weight decay
= [λ]) with batch size [B]. Each model is trained 5 times with different random seeds; we
report mean ± standard deviation.
```

### Mandatory reproducibility items (per NeurIPS / ICML / ICLR)

- Code release plan.
- Dataset access plan.
- Compute reporting (GPU type, count, hours).
- Hyperparameter ranges and search method.
- Significance over multiple runs.
- Limitations.

See references/reproducibility.md for full LLM-evaluation checklist.

### Common ML-specific terms to use precisely

| Term | Meaning |
|------|---------|
| Train / val / test | Three disjoint splits. Don't tune on test. |
| Few-shot / zero-shot / fine-tuned | State explicitly which. |
| In-context / weight-update | LLM literature distinction. Use precisely. |
| SOTA | Use only when literally true on a clearly-defined leaderboard at submission time. |

---

## 4. Programming Languages (POPL, PLDI, OOPSLA, ICFP, ECOOP)

### Expected sections

```
Introduction
Overview / Example
Formal Development
  - Syntax
  - Semantics (operational or denotational)
  - Type System
  - Theorems and Proofs
Implementation
Evaluation (when applicable)
Related Work
Conclusion
```

### What reviewers look for

- **Formal definitions** before usage.
- **Theorem statements** for soundness, progress, preservation, completeness.
- **Mechanization** (Coq, Agda, Isabelle, Lean) when claimed proofs are non-trivial.
- **Worked example** early in the paper (Section 2 often).
- **Comparison with prior calculi** at the type-system level, not just empirically.

### Theorem template

```
\begin{theorem}[Type Soundness]
  \label{thm:soundness}
  If $\Gamma \vdash e : \tau$ and $e \longrightarrow^* e'$, then either $e'$ is a value or
  there exists $e''$ such that $e' \longrightarrow e''$.
\end{theorem}

\begin{proof}[Proof sketch]
  By induction on the typing derivation. The key cases are application and pattern match,
  which use Lemmas~\ref{lem:substitution} and \ref{lem:canonical-forms}.
  Full proof is in the Coq development at [URL].
\end{proof}
```

### Mechanization disclosure

```
All theorems in Section~\ref{sec:formal} are mechanized in Coq 8.18 (5,200 lines, including
proof tactics). The development is publicly available at [URL].
```

If you have *unmechanized* paper proofs, say so explicitly.

### Operational vs. denotational semantics

- **Operational (small-step / big-step):** `e ⟶ e'` reduction relations. Standard for type-system papers.
- **Denotational:** `⟦e⟧ = …` mathematical objects. Common for verification and logic.
- Pick one and stick with it; do not switch frameworks mid-section.

### Standard PL theorems

| Theorem | Statement (informal) |
|---------|----------------------|
| **Progress** | A well-typed term is a value or can step. |
| **Preservation** | A step preserves typing. |
| **Soundness** | Progress + Preservation. |
| **Completeness** | Every fact derivable in the semantics is derivable in the type system. (For decision procedures.) |
| **Determinism** | Each term reduces in at most one way. |

State assumptions explicitly (e.g., "in a call-by-value semantics with no exceptions").

---

## 5. Security and Privacy (S&P / Oakland, USENIX Security, CCS, NDSS)

### Expected sections

```
Introduction
Background
Threat Model
System / Approach
Implementation
Evaluation
  - Effectiveness
  - Performance / Overhead
  - Robustness / Adversarial Robustness
Discussion
Limitations / Ethical Considerations
Related Work
Conclusion
```

### What reviewers look for

- **Formal threat model** with explicit attacker capabilities.
- **Defence scope** stating what is *out of scope*.
- **Adversarial evaluation**, not just benign-input evaluation.
- **Responsible disclosure** for vulnerabilities (with timeline and CVE if assigned).
- **Ethical considerations** for human-subjects, datasets, or measurement studies.

### Threat-model template

```
\subsection{Threat Model}

\textbf{Attacker capabilities.} The adversary can [observe / modify / inject / execute] ...
The adversary controls [ASSETS] but does not have [RESTRICTED CAPABILITIES].

\textbf{Attacker knowledge.} The adversary knows [SYSTEM ARCHITECTURE / SECRETS / NOTHING]
and has [BLACK-BOX / WHITE-BOX] access to [TARGET].

\textbf{Attacker goals.} The adversary aims to [GOAL 1, GOAL 2, ...].

\textbf{Out of scope.} We do not consider [PHYSICAL ATTACKS / SIDE-CHANNEL X / SOCIAL
ENGINEERING / SUPPLY-CHAIN COMPROMISE].

\textbf{Trust assumptions.} We assume [TPM / SGX / TLS endpoint / honest user] is trusted.
```

### Responsible disclosure

```
We disclosed the vulnerabilities described in Section~\ref{sec:findings} to the affected
vendors on [DATE]. Three vendors acknowledged the report within 30 days; two have released
patches as of [DATE]. CVEs CVE-202X-NNNNN, CVE-202X-MMMMM have been assigned. We share our
proof-of-concept exploits only with vendors and reviewers under embargo.
```

### Common security-specific terms to use precisely

| Term | Meaning |
|------|---------|
| Vulnerability vs. exploit | A vulnerability is a flaw; an exploit weaponises it. |
| Confidentiality / Integrity / Availability | The CIA triad — name the property. |
| Adversary vs. attacker | Synonyms; pick one and stick with it. |
| TCB (trusted computing base) | What you trust must be enumerated. |
| Defence-in-depth | A layered strategy; not a single mechanism. |

---

## 6. Cross-Venue Notes

### Section-name aliases

| Concept | Aliases across venues |
|---------|------------------------|
| Method | Approach, Design, Algorithm, System, Methodology |
| Evaluation | Experiments, Empirical Study, Results |
| Threats to Validity | Limitations, Discussion, Validity |
| Reproducibility | Artifact, Replication, Availability |

### When venue conventions and the general policy conflict

The general policy in `SKILL.md § 0` always defers to the **target venue's** style guide. If a venue requires a 100-word abstract, do not write 250. If a venue forbids RQs, do not insist.
