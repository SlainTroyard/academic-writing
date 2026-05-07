# Statistical Reporting

> **When to load this file:** writing or reviewing any sentence that uses the words *significant*, *effect*, *p-value*, *confidence interval*, *correlation*, *correction*, *power*, *sample size*; reporting comparisons across methods/runs/seeds; choosing a test.

This file is the most important reference for empirical SE/AI papers. Reviewers reject papers for missing effect sizes, missing corrections, or sloppy use of *significant* — even when the headline result is correct.

---

## 1. The Three-Pillar Rule

Every comparison should report all three of:

1. **A point estimate** (mean, median, success rate, …).
2. **An uncertainty estimate** (CI, SD, IQR — pick what fits the distribution).
3. **An effect size** when comparing groups (Cohen's *d*, Cliff's δ, Vargha–Delaney Â₁₂).

A statistical-test result (e.g., a p-value) is **not a substitute** for any of these. p < 0.05 with n = 10 000 and a 0.1 % effect is not a useful finding.

```
Bad:  [SYSTEM] is significantly better than [BASELINE] (p < 0.05).
OK:   [SYSTEM] achieves [V₁] vs. [V₂] for [BASELINE] (Δ = [V₁−V₂], 95% CI [LO, HI]),
      a large effect (Cliff's δ = [δ]). The difference is statistically significant
      (p < [α], [TEST], Holm-corrected for [k] comparisons).
```

---

## 2. Choosing a Statistical Test

| Data design                                              | Test                              | Notes                                                |
|----------------------------------------------------------|-----------------------------------|------------------------------------------------------|
| Paired binary outcomes (same items, two methods)         | **McNemar's test**                | Common for "did the system solve this benchmark?"   |
| Paired continuous, approximately normal                  | Paired *t*-test                   | Check normality; small N often fails the assumption. |
| Paired continuous, non-normal or ordinal                 | **Wilcoxon signed-rank test**     | Default for SE empirical work.                       |
| Independent groups, non-normal                           | **Mann–Whitney *U* test**         | Equivalent: Wilcoxon rank-sum.                       |
| Independent groups, normal                               | Independent *t*-test              |                                                      |
| ≥ 3 groups, continuous                                   | ANOVA (normal) / Kruskal–Wallis   | Follow with post-hoc + correction (§ 4).             |
| ≥ 3 groups, paired (e.g., methods × benchmarks)          | Friedman test                     | Follow with post-hoc Nemenyi or Wilcoxon + correction.|
| Categorical × categorical                                | χ² (large N) / Fisher's exact (small N) |                                                |
| Correlation, normal                                      | Pearson's *r*                     |                                                      |
| Correlation, ranked or non-normal                        | **Spearman's ρ** or Kendall's τ   | Default for SE.                                      |

**Decision rules:**

1. Are the data **paired** (same benchmark / same input under different methods)? Almost always yes in SE method comparisons. Use a paired test.
2. Are the data **normally distributed** (Shapiro–Wilk p > 0.05, or visual QQ-plot)? Usually no with success counts and runtimes. Use a non-parametric test.
3. Are there **≥ 3 methods**? Use Friedman or Kruskal–Wallis first; only do pairwise tests if the omnibus test rejects.

**Default for SE method-comparison tables:** Wilcoxon signed-rank (paired, non-parametric) with Holm correction.

---

## 3. Effect Sizes

A statistical test answers "is there a difference?". An effect size answers "how big is it?". Always report an effect size for any comparison the paper draws conclusions from.

### Cohen's *d* (continuous, parametric)

For two groups with means $\mu_1, \mu_2$ and pooled SD $s$:

$$
d = \frac{\mu_1 - \mu_2}{s}
$$

Common interpretation thresholds (Cohen, 1988):

| |d| | Magnitude |
|------|-----------|
| < 0.2  | negligible |
| 0.2–0.5 | small |
| 0.5–0.8 | medium |
| > 0.8  | large |

### Cliff's δ (non-parametric)

For two groups, Cliff's δ is the proportion of pairs (one from each group) where the first dominates, minus the proportion where the second dominates:

$$
\delta = \frac{\#(x_1 > x_2) - \#(x_1 < x_2)}{n_1 \cdot n_2}
$$

Interpretation thresholds (Romano et al., 2006):

| |δ| | Magnitude |
|------|-----------|
| < 0.147 | negligible |
| 0.147–0.33 | small |
| 0.33–0.474 | medium |
| ≥ 0.474 | large |

### Vargha–Delaney Â₁₂ (non-parametric, common in SE)

Â₁₂ is the probability that a random sample from group 1 exceeds a random sample from group 2:

$$
\hat{A}_{12} = \frac{R_1 / n_1 - (n_1 + 1) / 2}{n_2}
$$

(where $R_1$ is the sum of ranks of group 1).

Interpretation:

- 0.5  = no difference
- 0.56 = small effect
- 0.64 = medium effect
- 0.71 = large effect

Â₁₂ is widely used in search-based SE and mutation-testing papers.

### Reporting templates

```
Cohen's d = 0.74 (medium effect).
Cliff's δ = 0.62 (large effect).
Vargha–Delaney Â₁₂ = 0.78 (large effect; [SYSTEM] outperforms [BASELINE] in 78% of paired
comparisons).
```

---

## 4. Multiple-Comparison Correction

When you run more than one test on the same data, the chance of a false positive grows fast. **Always correct** unless the paper explicitly tests a single pre-registered hypothesis.

### Common corrections

| Method | When to use | Conservativeness |
|--------|-------------|------------------|
| **Bonferroni** | Few comparisons (≤ 5), strict family-wise error control | Most conservative |
| **Holm–Bonferroni** | General-purpose family-wise error control | Less conservative than Bonferroni; usually preferred |
| **Benjamini–Hochberg (BH)** | Many comparisons (e.g., per-benchmark tests), false-discovery-rate control | Less conservative; appropriate when some false positives are tolerable |

### Reporting templates

```
Statistical significance is reported after Holm correction for [k] pairwise comparisons.
p-values below report adjusted p-values; α = 0.05.
```

```
Per-benchmark p-values were adjusted using the Benjamini–Hochberg procedure to control the
false-discovery rate at q = 0.05.
```

### Common mistake

Running pairwise tests *before* the omnibus test (e.g., 6 pairwise *t*-tests across 4 methods, no ANOVA/Friedman). Reviewers will flag this. Always:

1. Run the omnibus test (Friedman, Kruskal–Wallis, ANOVA).
2. Only if it rejects, run post-hoc pairwise tests.
3. Apply correction to the post-hoc p-values.

---

## 5. Confidence Intervals

A 95 % confidence interval on a mean / median / success-rate is more informative than a p-value alone.

### Bootstrap CI (default for non-parametric problems)

For a statistic $T$ computed on a sample, a 95 % bootstrap CI is the 2.5 / 97.5 percentile of $T$ over $B$ resampled datasets ($B \geq 1000$, often $B = 10000$).

```
[SYSTEM] achieves a pass rate of [V] (95% bootstrap CI: [LO, HI]; B = 10000).
```

### Wilson interval (for proportions)

For pass-rate / success-rate metrics with $n$ trials and $k$ successes, the Wilson score interval is more accurate than the normal approximation, especially for small $n$ or $k$ near 0/100 %.

```
[SYSTEM] solved 138 of 208 benchmarks (66.3%, 95% Wilson CI: [59.7%, 72.5%]).
```

### When to prefer CI over p-value

- Always, when reporting a single comparison.
- The CI conveys both effect size and uncertainty in one go.
- Reviewers at ICSE/ASE/FSE increasingly request CIs over bare p-values.

---

## 6. Power Analysis and Sample Size

For a planned experiment, an *a priori* power analysis estimates the sample size needed to detect an effect of a given size with probability $1 - \beta$ (typically 0.8) at significance level $\alpha$ (typically 0.05).

### Reporting (when applicable)

```
A power analysis (G*Power 3.1; two-tailed Wilcoxon signed-rank; α = 0.05; 1 − β = 0.80;
small-to-medium effect, d = 0.4) suggested a minimum of [N] paired benchmarks. We evaluate
on [N+M] paired benchmarks.
```

### Post-hoc power

Avoid post-hoc power analyses on already-collected data. They are mathematically equivalent to the p-value and add no information.

### Practical rule for SE experiments

- For paired Wilcoxon at α = 0.05 with medium effect (Â₁₂ ≈ 0.64), aim for $n \geq 30$ paired benchmarks.
- For independent groups with medium effect, aim for $n \geq 50$ per group.
- These are rough; do a real power analysis when feasibility is in question.

---

## 7. Variance Across Runs (Stochastic Methods)

LLM sampling, randomized algorithms, and search-based methods are stochastic. Report variance.

### Minimum reporting

- Number of runs $N$ (≥ 3, ideally 5–10 for ML, 30+ for stochastic search).
- Random seed values (in appendix or supplementary).
- **Mean ± SD** or **median + IQR** across runs.
- One statistical test that explicitly accounts for paired runs.

```
We report results across 5 independent runs with seeds {0, 1, 2, 3, 4}. Mean and standard
deviation are reported in Table~\ref{tab:variance}.
```

### Aggregation across stochastic baselines

- Compare median-of-runs vs. median-of-runs (or mean-of-runs vs. mean-of-runs), not best-of-runs.
- Reporting only the best run is **cherry-picking**.
- Reporting only the mean while the variance is huge is misleading.

---

## 8. The "Significant" Word — Strict Discipline

| Situation | Allowed wording |
|-----------|-----------------|
| Statistical test, evidence provided | "statistically significant (p < α, [TEST], [CORRECTION])" |
| Effect size large, no test | "substantial / large improvement (Cliff's δ = …)" |
| Practically meaningful, justified | "practically meaningful: reduces failures by 18 percentage points" |
| Vague, no evidence | **Forbidden.** Use a precise word. |

### Practical vs. statistical significance

- **Statistical significance:** the effect is unlikely under the null hypothesis.
- **Practical significance:** the effect is large enough to matter in deployment.
- A 0.1 % improvement can be statistically significant with $n = 10^6$ and operationally irrelevant.
- A 15 % improvement may be operationally critical even if $n$ is too small for statistical significance.

State both, separately:

```
[SYSTEM] reduces compilation failures by 18.4 percentage points, which is operationally
meaningful for a CI pipeline. The improvement is statistically significant (p < 0.001,
Wilcoxon signed-rank, Holm-corrected).
```

---

## 9. Hedging and Confidence Calibration

Match language to evidence strength.

| Evidence strength       | Verbs                                        |
| ----------------------- | -------------------------------------------- |
| Direct measured result  | achieves, reduces, increases, outperforms    |
| Strong interpretation   | indicates, demonstrates, shows               |
| Moderate interpretation | suggests, provides evidence that             |
| Limited evidence        | may indicate, appears to, is consistent with |
| Speculation             | might, could, it is plausible that           |

Do not hedge **directly observed** numerical results.

```
Wrong:  Our method may achieve 85.6% accuracy.   (when we measured 85.6%)
Right:  Our method achieves 85.6% accuracy.
```

Hedge **interpretations** of why the number arose.

```
These results suggest that compiler feedback provides a useful signal for guiding generation.
```

---

## 10. Pre-Registration, HARKing, and p-Hacking

Even without formal pre-registration, follow these principles:

- **Distinguish exploratory from confirmatory analyses.** Mark exploratory analyses as such; do not present them as planned hypothesis tests.
- **Do not HARK** (Hypothesize After Results are Known). Do not present a post-hoc subgroup finding as if it were the original hypothesis.
- **Avoid p-hacking:**
  - Do not try multiple tests and report only the one that crossed α.
  - Do not change the metric after seeing the results.
  - Do not change the inclusion/exclusion criteria after seeing the results.
- **Report all comparisons you ran**, including null results.
- If the paper has a registered protocol or pre-registered hypotheses, cite the registration.

---

## 11. Negative and Null Results

If your method does **not** improve on a baseline, report it. Reviewers value honest negative results.

### Templates

```
On benchmark [X], [SYSTEM] did not outperform [BASELINE] (p = 0.42, Wilcoxon signed-rank,
n = [N]). The effect size was negligible (Cliff's δ = 0.04). We discuss possible reasons in
Section~\ref{sec:discussion}.
```

```
We initially expected [HYPOTHESIS], but the data did not support it (Â₁₂ = 0.51, 95% CI
[0.46, 0.56]). We treat this as an exploratory observation rather than a confirmatory finding.
```

Do **not**:

- Drop the negative result silently from the paper.
- Reframe a non-significant result as "trending toward significance."
- Reframe a negative finding as a positive "limitation" to obscure it.

---

## 12. Quick-Reference Cheat Sheet

```
[ ] I named the test, the test statistic, the corrected p-value, and the correction method.
[ ] I reported an effect size with magnitude interpretation.
[ ] I reported a CI (or SD/IQR) for every key estimate.
[ ] I corrected for multiple comparisons; I named the correction.
[ ] I distinguished statistical from practical significance.
[ ] I reported variance across runs / seeds when the method is stochastic.
[ ] I used "significant" only when "statistically significant (test, correction, p)" is true.
[ ] I did not run pairwise tests without a passing omnibus test.
[ ] I did not change my hypothesis after seeing the data.
[ ] I reported null / negative findings honestly.
```
