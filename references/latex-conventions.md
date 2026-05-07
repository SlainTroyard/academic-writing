# LaTeX Writing Conventions

> **When to load this file:** writing or reviewing LaTeX source, dealing with cross-references, math mode, tables, units, equations, anonymization, or template/style issues.

This file covers correctness and consistency in LaTeX. For figure/table content rules see references/figures-tables.md.

---

## 1. Cross-References

### 1.1 Capitalize reference words

Capitalize *Figure*, *Table*, *Section*, *Algorithm*, *Equation*, *Appendix* when they precede a number.

```
Good:
Figure~3 illustrates the architecture.
Table~2 shows the main results.
Section~4 describes the experimental setup.
Algorithm~1 presents the scheduling procedure.

Weak:
figure 3 illustrates ...
table 2 shows ...
```

### 1.2 Use nonbreaking space `~` before `\ref`

Always use a nonbreaking space (`~`) between the label word and `\ref{}` to prevent line breaks at the reference.

```latex
Figure~\ref{fig:arch}
Table~\ref{tab:results}
Section~\ref{sec:method}
Algorithm~\ref{alg:scheduler}
Equation~\eqref{eq:loss}
```

### 1.3 Use `\eqref{}` for equations

`\eqref{}` produces "(3)" instead of bare "3"; preferred for equation references.

```latex
We minimize the loss in Equation~\eqref{eq:loss}.
```

Use `\ref{}` for non-equation labels.

### 1.4 Never hardcode reference numbers

```latex
% Bad: breaks on reorder
As shown in Figure 3 and Table 5...

% Good: robust to reorder
As shown in Figure~\ref{fig:arch} and Table~\ref{tab:main}...
```

### 1.5 Naming conventions for labels

Stable, semantic, prefixed:

```
fig:arch          fig:sensitivity
tab:main          tab:ablation
sec:method        sec:eval-rq1
alg:scheduler
eq:loss           eq:gradient
app:proof         app:prompts
```

Avoid generic labels like `fig:1` or `tab:results` (which clash when re-used across sections).

### 1.6 Appendix references

When an appendix exists, reference it explicitly with a label.

```latex
See Appendix~\ref{app:proof} for the full derivation.
```

Do not write "in the appendix" without a specific label when multiple appendix sections exist.

---

## 2. Math Typesetting

### 2.1 Inline vs. display

- **Inline:** `$x$`, `$f(x) = ax + b$` — for short symbols and short expressions inside prose.
- **Display:** `\[ ... \]` or `equation` environment — for important or multi-line expressions, with a label if referenced.

Do not use `$$ ... $$` (deprecated; produces inconsistent vertical spacing).

### 2.2 Variables vs. function/operator names

Always italicize variables (default in math mode) and **do not italicize** multi-letter function names. Use `\mathrm{}` or built-in commands.

```latex
% Wrong:
$loss = exp(-y * f(x))$

% Right:
$\mathrm{loss} = \exp(-y \cdot f(x))$

% Better still (built-in):
$\mathcal{L} = \exp(-y \cdot f(x))$
```

Built-in operators (typeset upright automatically): `\log, \exp, \min, \max, \sin, \cos, \det, \dim, \ker, \arg, \sup, \inf, \lim, \sum, \prod, \int`.

For custom multi-letter operators, define once:

```latex
\DeclareMathOperator*{\argmax}{argmax}
\DeclareMathOperator*{\argmin}{argmin}
\DeclareMathOperator{\softmax}{softmax}
```

Then use `$\argmax_x f(x)$`.

### 2.3 Vectors and matrices

Pick one convention and apply paper-wide.

| Convention | Vectors | Matrices |
|------------|---------|----------|
| Bold       | `\mathbf{x}` or `\bm{x}` | `\mathbf{A}` or `\bm{A}` |
| Arrow      | `\vec{x}` | usually not used for matrices |
| Plain italic | `x` | `A` (less safe; relies on context) |

`\bm` (from `bm` package) bolds Greek letters too: `$\bm{\theta}$`.

### 2.4 Spacing in math

Use `\,`, `\:`, `\;` for thin/medium/thick spacing.

```latex
$d\theta$       % usually fine
$d\,\theta$     % thin space when needed
$\int f(x)\,dx$ % conventional thin space before differential
```

Do **not** insert spaces with `\ ` (literal space) in math; use `\,` etc.

### 2.5 Numbers and units (siunitx)

Use `siunitx` for consistent number, unit, and range formatting.

```latex
\usepackage{siunitx}

\num{1234567}              % 1\,234\,567
\num{0.000056}             % 5.6×10⁻⁵
\SI{32}{\giga\byte}        % 32 GB
\SI{2.4}{\giga\hertz}      % 2.4 GHz
\SIrange{10}{20}{\percent} % 10–20 %
```

### 2.6 Bracket sizing

Use `\left(...\right)`, `\left[...\right]` to size delimiters automatically; or `\big`, `\Big`, `\bigg`, `\Bigg` for fixed sizes.

```latex
% Good:
$\sigma\!\left( \mathbf{W}^\top \mathbf{x} + \mathbf{b} \right)$
```

Use `\!` (negative thin space) sparingly to tighten spacing after operators.

### 2.7 Mathematical symbols to use precisely

| Symbol | Command | Meaning |
|--------|---------|---------|
| ≤, ≥ | `\leq`, `\geq` | less/greater-or-equal |
| ≠ | `\neq` | not equal |
| ≈ | `\approx` | approximately |
| ≡ | `\equiv` | identical |
| ≜ | `\triangleq` | defined to equal |
| := | `\coloneqq` (mathtools) or `\mathrel{:=}` | assignment |
| ∝ | `\propto` | proportional |
| ⟶ | `\longrightarrow` | implies / steps to |
| ⟹ | `\implies` | logically implies |
| ⊆, ⊂ | `\subseteq`, `\subset` | subset (proper or not — pick consistently) |

### 2.8 Equation labelling discipline

Label only equations that are referenced in the text.

```latex
% Good: equation is labeled and referenced
\begin{equation}
  \mathcal{L} = \sum_{i=1}^{N} \ell(y_i, \hat{y}_i)
  \label{eq:loss}
\end{equation}

We minimize the loss in Equation~\eqref{eq:loss}.
```

Unlabelled equations should use `equation*` or `align*` to avoid wasted numbers.

---

## 3. Tables

### 3.1 booktabs only

Use `\toprule`, `\midrule`, `\bottomrule` from `booktabs`. No vertical lines, no `\hline`.

```latex
\usepackage{booktabs}

\begin{tabular}{lrr}
  \toprule
  Method      & Pass rate (\%) & Δ vs.\ baseline \\
  \midrule
  [BASELINE]  & 51.1           & 0.0             \\
  [SYSTEM]    & 66.4           & +15.3           \\
  \bottomrule
\end{tabular}
```

### 3.2 Decimal alignment

Use `siunitx`'s `S` column for numeric alignment on the decimal point:

```latex
\usepackage{siunitx}
\begin{tabular}{l S[table-format=2.2] S[table-format=+1.2]}
  \toprule
  Method     & {Pass rate} & {Δ}    \\
  \midrule
  [BASELINE] & 51.10       & {--}   \\
  [SYSTEM]   & 66.35       & +15.25 \\
  \bottomrule
\end{tabular}
```

### 3.3 Multi-row / multi-column

Use `multirow` and `\multicolumn`. Keep multi-row only when necessary; flat tables are easier to scan.

### 3.4 Wide tables

For tables wider than the column:

```latex
\begin{table*}[t]   % spans both columns in two-column layout
  ...
\end{table*}
```

Or rotate with `sideways` (from `rotating`) — but only for genuinely wide content.

### 3.5 Avoid these

- Vertical lines (`|`) in column specifications.
- `\hline` (use `\midrule`).
- Mixing `\hline` and `\midrule`.
- Cells wrapped to 4+ lines (split the table).
- Tiny font to force-fit.

---

## 4. Anonymization for Double-Blind Submissions

### 4.1 The cardinal rule

You must remain anonymous. Reviewers should not be able to identify the authors from the PDF, the artifact, or any external link cited in the paper.

### 4.2 Self-citation in third person

```latex
% Good (anonymous):
Smith and Jones~\cite{smith2023priorwork} proposed a related method ...

% Bad (deanonymizes):
In our prior work~\cite{smith2023priorwork}, we proposed a related method ...
Our group~\cite{smith2023priorwork} proposed a related method ...
```

### 4.3 Anonymous repository / artifact links

Use anonymous hosting:

```latex
The artifact is available at \url{https://anonymous.4open.science/r/[ID]}.
We will release a permanent DOI on Zenodo upon acceptance.
```

Do **not**:

- Link to a personal GitHub username.
- Link to a lab webpage.
- Embed metadata in the PDF that contains your name or institution.

### 4.4 PDF metadata

Strip identifying metadata before submission:

```bash
exiftool -all= submission.pdf
```

Or set in LaTeX:

```latex
\hypersetup{
  pdftitle={Anonymous Submission},
  pdfauthor={},
  pdfsubject={},
  pdfkeywords={},
  pdfcreator={},
  pdfproducer={}
}
```

### 4.5 Acknowledgments

Comment out or anonymize:

```latex
\begin{acks}
% \thanks{ANONYMIZED for review}.
\end{acks}
```

After acceptance, fill in the real text.

### 4.6 Watch-list before submission

```
[ ] No author names in PDF text or metadata.
[ ] No institution names visible (header, footer, dataset names like "MIT-Bench", artifact links).
[ ] Self-citations are third-person.
[ ] No funding-source names that uniquely identify the lab.
[ ] No anonymous-but-distinctive screenshots or figures.
[ ] Repository URL is anonymous.4open.science (or equivalent).
[ ] Acknowledgments removed or marked anonymized.
```

---

## 5. Common LaTeX Pitfalls

### 5.1 Quotation marks

Use ` `` ` (two backticks) and ` '' ` (two apostrophes) for English-style smart quotes, not `"`.

```latex
``compiler-guided'' repair      % correct
"compiler-guided" repair        % wrong (straight quotes)
```

### 5.2 Dashes

| Mark | LaTeX | Use |
|------|-------|-----|
| Hyphen `-` | `-` | compound modifiers: *compiler-guided* |
| En-dash `–` | `--` | ranges and citation runs: *Sections~3--5*, *[1]--[4]* |
| Em-dash `—` | `---` | parenthetical: *the result---surprising though it was---was clear* |

### 5.3 Spacing after periods

LaTeX inserts extra space after a sentence-ending period. To suppress after abbreviations:

```latex
e.g.\ this case        % \ keeps normal spacing
i.e.\ as follows
et al.\ proposed
Fig.\ 3 illustrates
Dr.~Smith              % nonbreaking space, also avoids the extra-space issue
```

### 5.4 Math periods

End sentences that finish in display math with a period **inside** the equation environment for typeset balance:

```latex
... is given by
\begin{equation}
  f(x) = ax + b.
  \label{eq:linear}
\end{equation}
```

### 5.5 No double spaces, no unnecessary `\\`

LaTeX collapses multiple spaces. Do not use `\\` to force line breaks in body text — restructure the sentence instead.

### 5.6 Use `\textit{}`, `\emph{}`, `\textbf{}` correctly

- `\emph{}` — emphasis (italic in upright text, upright in italic text).
- `\textit{}` — explicit italic.
- `\textbf{}` — explicit bold.

For first introduction of a technical term, `\emph{}` is usually correct.

```latex
We propose \emph{adaptive temperature scheduling} (ATS), a method that ...
```

Do not italicize the term on subsequent uses.

### 5.7 Algorithm captions (placement)

Different `algorithm` packages place captions differently. Most place them above; verify and stay consistent.

```latex
\begin{algorithm}
  \caption{Adaptive temperature scheduling.}
  \label{alg:scheduler}
  \begin{algorithmic}[1]
    ...
  \end{algorithmic}
\end{algorithm}
```

---

## 6. Reproducible Builds

For artifact evaluation, builds should be reproducible:

```bash
# Pin TeX Live version
docker run --rm -v $PWD:/work -w /work texlive/texlive:TL2024-historic latexmk -pdf paper.tex
```

Pin all bibliography style files (`*.bst`) and document classes (`*.cls`) by including them in the source tree, not by relying on the system distribution.

---

## 7. Quick LaTeX Self-Check

```
[ ] No `\ref{}` without nonbreaking space `~` before it.
[ ] No hardcoded reference numbers ("Figure 3", "Equation 5").
[ ] All cross-reference labels use the prefix convention (fig:, tab:, sec:, alg:, eq:, app:).
[ ] booktabs (no vertical lines) for all tables.
[ ] Math operators use `\mathrm{}` or `\DeclareMathOperator`.
[ ] siunitx is used for numbers and units.
[ ] Quotes are smart (`` ''), dashes are correct (-, --, ---).
[ ] PDF metadata is stripped (double-blind only).
[ ] Self-citations are anonymized (double-blind only).
[ ] No deprecated `$$ $$` math delimiters.
[ ] All equations referenced from text use `\eqref{}`; unreferenced ones use the unstarred environment only if numbering is desired.
```
