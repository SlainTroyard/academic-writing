# Reproducibility, Artifacts, and LLM-Specific Disclosure

> **When to load this file:** writing the experimental-setup section, the data-availability statement, the artifact-evaluation appendix, the LLM-evaluation methodology, or responding to a reviewer concern about reproducibility, contamination, or LLM evaluation.

This is a high-priority file for any paper that (a) uses LLMs, (b) targets an empirical SE / ML / Systems venue with an artifact track, or (c) reports stochastic experiments.

---

## 1. Reproducibility Levels

Distinguish three levels in your writing. Most venues now expect at least the first two.

| Level | Definition |
|-------|------------|
| **Repeatable** | Same authors, same setup, same data → same results. (Bare minimum.) |
| **Reproducible** | Independent group, same setup, same data → same results. |
| **Replicable** | Independent group, different data or setup → consistent conclusions. |

Use these terms strictly. A paper that releases code but uses a private dataset is reproducible but not replicable.

---

## 2. Artifact Badges

Major CS venues award artifact badges (ACM standard since 2020):

| Badge | What it certifies |
|-------|-------------------|
| **Available** | Artifact is publicly archived (e.g., Zenodo with DOI). |
| **Functional** | Artifact runs and is documented. |
| **Reusable** | Artifact runs in a clean environment with reasonable effort. |
| **Reproduced** | Independent reviewers reproduced the paper's main results. |
| **Replicated** | Different artifact achieves the same conclusions. |

Conferences with artifact tracks: ICSE, FSE/ESEC, ASE, ISSTA, OOPSLA, PLDI, POPL, OSDI, SOSP, ATC, NSDI, MLSys.

### Reporting a successful badge

```
Our artifact is publicly available on Zenodo at \url{https://doi.org/10.5281/zenodo.XXXXXXX}
and was awarded the ACM Available, Functional, and Reusable badges by the [VENUE] artifact
evaluation committee.
```

---

## 3. What Belongs in a Replication Package

Minimum viable artifact:

```
artifact/
├── README.md           # 1-page overview, hardware/software requirements, time-to-run
├── INSTALL.md          # step-by-step; Docker preferred
├── LICENSE             # explicit (MIT/Apache/BSD/GPL/...)
├── REQUIREMENTS.txt    # pinned versions
├── data/               # benchmark inputs (or a download script with checksums)
├── src/                # all code that reproduces the paper's results
├── scripts/
│   ├── run_all.sh      # single command that produces all tables and figures
│   └── ...
├── results/
│   ├── raw/            # raw outputs from each run
│   └── processed/      # aggregated numbers used in tables
└── figures/            # rendered figures used in the paper
```

Recommended additions:

- A Dockerfile or Nix flake for environment pinning.
- A `MAKEFILE` or `justfile` with named targets (`make table-1`, `make figure-3`).
- `expected_results.txt` with tolerance bounds.
- A test script that runs in <10 minutes on a small subset.

---

## 4. Dataset Documentation

For every dataset used or released, document:

```
- Source and citation.
- License (MIT, Apache, CC-BY-4.0, …).
- Number of items, size on disk.
- Splits (train/val/test) with rationale.
- Preprocessing (deduplication, decontamination, normalization).
- Anonymization, if applicable.
- Known biases / limitations.
- Date of collection.
- Version or commit hash if scraped from a moving source.
```

If you release a new dataset, follow the **datasheet for datasets** template (Gebru et al., 2021).

### Reporting template

```
We use [DATASET] (N = [SIZE], [LICENSE], cited as [CITATION]). We exclude [K] items that
[REASON]. We split the remaining [N−K] items into [TRAIN]/[VAL]/[TEST] with seed [SEED]; the
splits are released with the artifact.
```

---

## 5. Inter-Rater Agreement (Qualitative Coding)

When the paper involves human labels, taxonomic coding, or any subjective categorisation, report inter-rater agreement.

### Standard metrics

| Metric | When to use | Notes |
|--------|-------------|-------|
| **Cohen's κ** | 2 raters, categorical labels, fixed categories | Most common in SE coding studies |
| **Fleiss' κ** | 3+ raters, categorical labels | Generalises Cohen's κ |
| **Krippendorff's α** | Any number of raters, missing data tolerated, ordinal/nominal/interval | More robust; preferred for messy datasets |
| Intraclass correlation (ICC) | Continuous ratings | E.g., 1–5 Likert agreement |

### Interpretation (Landis & Koch, 1977 — guideline only)

| κ | Agreement |
|---|-----------|
| < 0.20 | poor |
| 0.21–0.40 | fair |
| 0.41–0.60 | moderate |
| 0.61–0.80 | substantial |
| > 0.80 | (almost) perfect |

### Reporting template

```
Two authors independently coded all [N] artefacts using a coding schema developed iteratively
on a 10% pilot sample. Disagreements were resolved through discussion. Inter-rater agreement
on the full sample was κ = 0.78 (substantial; Cohen's κ).
```

For ≥ 3 raters, report Fleiss' κ or Krippendorff's α, and follow the same template.

---

## 6. LLM Evaluation Reproducibility

This section is critical for any paper that uses an LLM as a method, baseline, or evaluator. Reviewers at NeurIPS, ICLR, ICML, ACL, EMNLP, ICSE, FSE, ASE now routinely reject papers missing these disclosures.

### 6.1 Model identification (mandatory)

Always pin the exact model version. "GPT-4" is not a model name.

```
We use OpenAI gpt-4-0613 and gpt-4-turbo-2024-04-09 via the official API on [DATE RANGE].
```

```
We use Anthropic claude-3-5-sonnet-20241022 via the Anthropic API on [DATE RANGE].
```

```
We use the open-source Llama-3.1-8B-Instruct (Hugging Face revision [HASH]) loaded with
vLLM 0.5.4 on [HARDWARE].
```

Required information:

- Provider and model identifier with date suffix or revision hash.
- API access date range (model behaviour drifts).
- Open-source models: Hugging Face revision / commit, inference server and version.

### 6.2 Sampling parameters (mandatory)

```
Generation parameters: temperature = 0.7, top-p = 0.95, max-tokens = 2048, seed = 0
(where supported), n = 5 samples per prompt.
```

If the model exposes a seed, set it and report it. If not (e.g., hosted APIs that ignore seed), say so explicitly.

### 6.3 Prompt disclosure (mandatory)

Provide the **full** prompt template in an appendix, with all variables named:

```latex
\begin{figure}[t]
\caption{Prompt template used for [SYSTEM]. Placeholders are typeset in \textit{italics}.}
\label{fig:prompt}
\begin{lstlisting}
You are a Rust expert. Translate the following C function to safe Rust.

C source:
{C_SOURCE}

Compiler errors from previous attempt:
{COMPILER_ERRORS}

Provide only the corrected Rust code, no explanation.
\end{lstlisting}
\end{figure}
```

If the prompt was iterated:

- Disclose the iteration process (how many versions, what changed, what data was used to iterate).
- Lock the final prompt before final evaluation.
- **Do not iterate the prompt on the test set.** That is prompt-on-test contamination.

### 6.4 Data contamination / training-cutoff disclosure (mandatory for any LLM benchmark)

Any benchmark released before the model's training cutoff is suspect. Reviewers expect a contamination check.

```
[BENCHMARK] was released in [YEAR-MONTH], which is before the training-data cutoff of
[MODEL] ([YEAR-MONTH]). To assess potential contamination, we [METHOD: e.g., probe the model
for memorised solutions, search the model's pre-training corpus, run a sub-string membership
test]. We find [RESULT] and discuss the implications in Section~\ref{sec:threats}.
```

Common methods:

- Sub-string membership: ask the model to complete a unique 50-token suffix of a benchmark item.
- Solution probing: ask the model for a known solution; compare to the published one.
- Hash overlap: if the pre-training data is known, intersect file hashes.

For benchmarks released *after* the cutoff, state this and note that the comparison is contamination-free for that model.

### 6.5 LLM-as-judge (when applicable)

If you use an LLM to judge outputs (e.g., "is this Rust idiomatic?"):

- Validate the judge against human ratings on a held-out subset.
- Report agreement (κ or correlation) between the judge and humans.
- Use a different model from the one being evaluated, when possible, to avoid in-family bias.
- Disclose the full judge prompt.
- Report inter-judge variance if multiple judge models are used.

```
We use [JUDGE_MODEL] as the evaluator. On a 100-item validation subset, [JUDGE_MODEL]'s
ratings agreed with two independent human raters at κ = 0.71 (substantial). Cases with
disagreement were resolved by majority vote among the three.
```

### 6.6 Cost reporting

Reviewers increasingly request cost transparency.

```
The full evaluation required [N_API_CALLS] API calls, totalling [N_INPUT_TOKENS] input
tokens and [N_OUTPUT_TOKENS] output tokens. Total API cost: USD [AMOUNT] at [DATE] pricing.
```

For self-hosted models:

```
Inference required [HOURS] GPU-hours on [HARDWARE]. Estimated cost: USD [AMOUNT] at
[CLOUD] on-demand pricing on [DATE].
```

### 6.7 Variance across LLM runs

Even at temperature 0, LLMs can produce different outputs across calls due to backend non-determinism. Always report:

- Number of independent runs $N \geq 3$ (≥ 5 preferred).
- Mean ± SD or median + IQR for every reported metric.
- A statistical test that respects the paired structure across runs.

```
Each prompt was sampled [N=5] times with different sampling seeds (where supported). Reported
results are mean ± SD across the 5 runs. Statistical comparisons use the Wilcoxon signed-rank
test on per-benchmark mean scores.
```

---

## 7. Compute Reporting

Required by NeurIPS / ICML / ICLR; recommended elsewhere.

### Hardware

```
All experiments were conducted on [GPU model, count] GPUs ([GPU memory] each), [CPU model],
[RAM]. Training required approximately [X] GPU-hours per run ([Y] runs total).
```

### Software

```
Our implementation uses PyTorch 2.4.0 + CUDA 12.4. Reproduction instructions and pinned
versions are provided in the artifact.
```

### Carbon disclosure (optional but increasingly common)

```
Estimated CO2 emissions for the full experimental campaign: [X] kg CO2-eq, computed using
the methodology of Lacoste et al. [N].
```

---

## 8. Reproducibility Statement

A standalone "Reproducibility" subsection (or appendix) helps reviewers locate the relevant disclosures.

### Template

```
\section*{Reproducibility Statement}

We provide the full source code, datasets, configuration files, and trained models at
\url{https://anonymous.4open.science/r/[ID]} (anonymous version) and will release a permanent
DOI on Zenodo upon acceptance.

\textbf{Datasets.} See Section~\ref{sec:setup} and the artifact's \texttt{data/README.md} for
sources, licenses, and preprocessing scripts.

\textbf{Models and parameters.} See Section~\ref{sec:setup}; full hyper-parameter sweeps and
prompt templates are in Appendix~\ref{app:prompts}.

\textbf{Compute and cost.} See Appendix~\ref{app:compute} for hardware, runtime, and cost.

\textbf{LLM contamination.} See Section~\ref{sec:threats} for our contamination assessment.

\textbf{Variance.} All headline results are reported as mean and standard deviation across
[N] independent runs with seeds \{...\}; raw per-run results are in the artifact.
```

---

## 9. Honest-Reporting Principles

- Do not report only the best run; report the distribution.
- Do not iterate prompts on the test set; lock prompts before final evaluation.
- Do not exclude data points that hurt the result without a pre-specified rule.
- Do not change the metric after seeing the data.
- Do not cherry-pick benchmarks where the method wins; report all benchmarks tried.
- Disclose every external library, model, dataset, and external API used.

See references/statistics.md § 10 (HARKing / p-hacking) and § 11 (negative results) for more.

---

## 10. Quick Reproducibility Self-Check

```
[ ] Code, data, scripts, and configs are released or scheduled for release.
[ ] Random seeds are set and listed.
[ ] Hardware, software, and runtime are documented.
[ ] Dataset license, source, and preprocessing are documented.
[ ] If LLMs are used: model+date pinned, prompts disclosed, contamination assessed, cost reported, variance reported.
[ ] If LLM-as-judge: validated against humans with κ.
[ ] Inter-rater agreement reported for all human coding.
[ ] Anonymous artifact link works for double-blind review.
```
