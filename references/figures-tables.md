# Figures and Tables

> **When to load this file:** writing or reviewing figures, tables, captions, algorithm boxes, or in-text references to them.

This file covers presentation conventions for floats. For LaTeX-specific commands and `\ref{}` mechanics, see references/latex-conventions.md.

---

## 1. Referencing Tables and Figures

Use **present tense**.

```
Good:
Table~\ref{tab:main} shows the comparison results across all baselines.
Figure~\ref{fig:arch} illustrates the architecture of [SYSTEM].
Algorithm~\ref{alg:scheduler} describes the temperature scheduling procedure.
```

**Avoid low-information references:**

```
Weak:    The results are shown in Table~2.
Better:  Table~\ref{tab:main} shows that [SYSTEM] achieves the highest compilation success
         rate among all evaluated methods.
```

The reference should *introduce the finding*, not merely point at the float.

### Capitalization

Capitalize *Figure*, *Table*, *Section*, *Algorithm*, *Equation*, *Appendix* when they precede a reference number:

```
Good: Figure~3, Table~2, Section~4, Algorithm~1, Equation~(5), Appendix~B
Weak: figure 3, table 2, section 4, algorithm 1
```

### Abbreviation consistency

Use either *Fig.* or *Figure* consistently throughout the paper. Most ACM and IEEE proceedings use *Fig.* in captions and *Figure* in body text. Follow the venue convention; do not mix.

---

## 2. Describing Table Results

When discussing a table, state the main finding before the supporting numbers.

### Pattern

```
Table~\ref{tab:N} shows [METRIC] on [DATASET].
[METHOD] achieves [VALUE], compared with [BASELINE] at [VALUE].
The improvement over [BASELINE] is [DIFFERENCE].
```

### Example

```
Table~\ref{tab:results} shows the compilation success rates for each method. [SYSTEM] achieves
the highest rate ([VALUE]), outperforming [BASELINE] ([VALUE]) by [DIFFERENCE] percentage
points.
```

### "Respectively" rule

Use *respectively* only when the mapping is unambiguous and parallel.

```
Good: [SYSTEM], [BASELINE-A], and [BASELINE-B] achieve pass rates of [V1], [V2], and [V3],
      respectively.

Weak: [SYSTEM] and [BASELINE-A] achieve pass rates of [V1] and [V2] on benchmarks A and B,
      respectively.   <- ambiguous: which value goes with which benchmark?
```

Avoid *respectively* in long sentences with nested lists.

---

## 3. Captions

Captions should be **informative**, not labels.

```
Weak:    Results of different methods.
Better:  Compilation success rates of [SYSTEM] and baseline methods on [N] C-to-Rust
         migration benchmarks.

Weak:    System architecture.
Better:  Architecture of [SYSTEM], consisting of an error classifier, a temperature scheduler,
         and a repair generator.
```

A caption should be readable on its own — reviewers often skim captions before body text.

### Algorithm captions

```
Weak:    Algorithm for our method.
Better:  Adaptive temperature scheduling given a compiler error category and repair history.
```

### Caption length

- 1 line is fine if the figure is simple.
- 2–4 lines for figures that need axis or legend explanation.
- More than 5 lines: move detail into the body or a separate paragraph that references the figure.

### Caption position

- **Tables:** caption *above* the table (most CS venues; check the style guide).
- **Figures:** caption *below* the figure.
- **Algorithms:** caption above (with `algorithm` package) or below (depends on package).

---

## 4. Table Layout

### Use `booktabs`, not vertical lines

```latex
\usepackage{booktabs}

\begin{table}
  \caption{Compilation success rates on [DATASET].}
  \label{tab:main}
  \begin{tabular}{lrr}
    \toprule
    Method      & Pass rate (\%) & Δ vs.\ baseline \\
    \midrule
    [BASELINE]  & 51.1           & 0.0             \\
    [SYSTEM]    & 66.4           & +15.3           \\
    \bottomrule
  \end{tabular}
\end{table}
```

Rules:

- Use `\toprule`, `\midrule`, `\bottomrule` only — no vertical lines, no `\hline`.
- Right-align numeric columns; left-align text columns.
- Avoid double rules.
- Keep column headers short; explain in caption if needed.

### Cell formatting

- Use a consistent number of decimal places per column.
- Bold or shade only the best value per column, not multiple "near-best" values.
- Use `±` for standard deviation: `0.85 ± 0.02`. In LaTeX, `$0.85 \pm 0.02$`.
- For percentages, decide once: include `%` in every cell or only in the column header.

### Significant digits

- Match precision to measurement uncertainty.
- Reporting `66.354281%` when the standard deviation is 1.2% is misleading. Two decimal places (`66.35`) is usually enough.
- For wall-clock time and memory, follow `siunitx` (see references/latex-conventions.md).

---

## 5. Figure Quality

### Format

- Use **vector** formats (PDF, EPS, SVG) for plots and diagrams.
- Use **PNG/JPEG** only for screenshots or photographs.
- Never include raster screenshots of LaTeX-rendered text — re-render as PDF.

### Resolution and font size

- Embed fonts in PDF figures (`\usepackage{pdfpages}` or matplotlib's `pdf.fonttype = 42`).
- In-figure text should be roughly the same size as the body text after the figure is placed at its target column width.
- Minimum readable font size in a 1-column IEEE/ACM figure is ~7pt; aim for 9pt.
- Avoid figures that need a magnifier to read at print resolution.

### Colour

- Use **colorblind-safe palettes** (e.g., ColorBrewer, viridis, the Wong 8-colour palette).
- Do not rely on red vs. green to distinguish categories.
- Add **shape** or **line-style** redundancy in line plots so the figure is legible in greyscale.
- Test print-to-greyscale before submission.

### Plot conventions

- Always label axes, with units.
- Use consistent y-axis bounds across related sub-plots.
- Show variance: error bars (with description in caption — SD? SE? 95 % CI?), bands, or box plots.
- Avoid 3-D bar charts.
- Avoid pie charts when a bar chart would convey the same data more precisely.

### Accessibility

- High contrast text vs. background.
- Avoid putting essential information solely in colour.
- Consider that figures may appear in printed proceedings.

---

## 6. Figure and Table Placement

Mention every float in the text *before or on the page where it first appears*.

```
Good:
"Figure~\ref{fig:arch} illustrates the overall architecture."
[figure appears on the same or next page]

Bad:
[Figure appears on page 4 but is first mentioned on page 6]
```

If LaTeX places a float far from its mention, force placement with `[t]`, `[!ht]`, or `\FloatBarrier` (from `placeins`). Do not change the citation order to match LaTeX's float placement.

---

## 7. Subfigures and Subtables

When a figure has multiple panels:

```latex
\begin{figure}
  \centering
  \begin{subfigure}{0.48\linewidth}
    \includegraphics[width=\linewidth]{plot-a.pdf}
    \caption{Pass rate vs.\ temperature.}
    \label{fig:plot-a}
  \end{subfigure}
  \hfill
  \begin{subfigure}{0.48\linewidth}
    \includegraphics[width=\linewidth]{plot-b.pdf}
    \caption{Pass rate vs.\ error category.}
    \label{fig:plot-b}
  \end{subfigure}
  \caption{Sensitivity of [SYSTEM] to (a) sampling temperature and (b) error category.}
  \label{fig:sensitivity}
\end{figure}
```

Reference the parent and the panel:

```
Figure~\ref{fig:sensitivity}(a) shows the effect of temperature.
Figure~\ref{fig:plot-a} shows the effect of temperature.
```

Pick one referencing style and use it consistently.

---

## 8. Algorithm Floats

Use `algorithm` + `algpseudocode` (algorithmicx) or `algorithm2e`:

```latex
\begin{algorithm}
  \caption{Adaptive temperature scheduling.}
  \label{alg:scheduler}
  \begin{algorithmic}[1]
    \Require Compiler error category $e$, history $H$
    \State $T \gets \mathrm{lookup}(\textit{TemperatureMap}, e)$
    \If{$\mathrm{stuck}(H, e)$}
      \State $T \gets T + \Delta$
    \EndIf
    \State \Return $T$
  \end{algorithmic}
\end{algorithm}
```

Conventions:

- One algorithm per logical procedure.
- Keep lines under 60 characters; use multiple lines rather than dense single lines.
- Use math mode for variables: `$T$`, not `T`.
- Comment lines (`\Comment{}`) for non-obvious steps only.
- Number lines for easier in-text reference: "Line~3 of Algorithm~\ref{alg:scheduler}".

---

## 9. Common Float Mistakes

| Mistake | Fix |
|---|---|
| Caption placeholder remained ("TODO caption") | Audit before submission. |
| Same colour bar in two unrelated figures | Use distinct palettes when categories differ. |
| Figure referenced as "the figure below" | Always use `Figure~\ref{}`. |
| Table missing units in column headers | Add units to header or caption. |
| Bold "best" value chosen by visual not metric | Pick one rule (highest mean) and apply uniformly. |
| Logarithmic axis without explicit log label | State the scale in the axis label and caption. |
| Y-axis truncated to exaggerate differences | Either start at 0 or note explicitly. |
| Different colour for the same method across figures | Pin one colour per method paper-wide. |
| Vector figure but rasterized text | Re-export with embedded fonts or use TikZ. |
