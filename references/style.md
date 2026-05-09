# Sentence- and Paragraph-Level Style

> **When to load this file:** polishing prose, fixing flow, resolving ambiguous pronouns, repairing tense/voice, or reviewing language quality.

This file covers paragraph coherence (§ 1) and sentence-level style (§ 2). It does NOT cover claim language or hedging — see references/quick-reference.md and the main SKILL.md.

---

## 1. Paragraph-Level Coherence

### 1.1 Topic-sentence-first paragraphs (default)

```
Sentence 1: Core claim
Sentence 2: Evidence or explanation
Sentence 3: Mechanism or reasoning
Sentence 4: Implication
Sentence 5: Transition, if needed
```

This is the default AI drafting policy, not an absolute rule. Other paragraph structures may be appropriate for proofs, definitions, narratives, or venue-specific styles.

### 1.2 Single focus per paragraph

Each paragraph should develop one core claim.

Start a new paragraph when introducing:

- a new claim;
- a contrast;
- a new component;
- a new result;
- a new limitation;
- a new research question.

Avoid paragraphs that mix:

- motivation and method details;
- method and results;
- related work and contribution;
- multiple unrelated limitations.

### 1.3 Known-to-new flow

Each sentence should connect to an existing concept before introducing a new one.

```
Sentence 1 introduces concept A.
Sentence 2 uses A and introduces B.
Sentence 3 uses B and introduces C.
```

Do not force mechanical object-to-subject chaining if it harms naturalness.

**Good:**

```
The Rust compiler enforces strict ownership rules.
These rules prevent many classes of memory-safety errors.
Such errors are common in legacy C programs and can lead to crashes or vulnerabilities.
These risks motivate automated tools for migrating C code to safer languages.
```

**Avoid abrupt topic jumps:**

```
The compiler enforces strict ownership rules.
We also need type checking.
Errors often occur in practice.
The solution requires multiple approaches.
```

### 1.4 Transition discipline

Use logical connectors only when they express a real relationship.

```
Cause–effect: because, since, therefore, thus, consequently
Contrast:     however, whereas, in contrast, conversely
Addition:     furthermore, moreover, additionally
Example:      for example, for instance, specifically
Summary:      in summary, overall, ultimately
Emphasis:     notably, importantly
```

**Avoid overusing** *Furthermore / Moreover / Additionally* when the relationship is not actually additive. Stack-of-furthermores is a common AI tell.

### 1.5 Paragraph length guidance

- 3–7 sentences is typical for CS papers.
- One-sentence paragraphs are acceptable for emphasis but rare.
- Walls of >10 sentences usually hide multiple sub-claims; split.

---

## 2. Sentence-Level Style

### 2.1 Voice

Use **active voice** with "we" by default for the authors' contributions, design decisions, and experiments.

```
We propose [SYSTEM], an error-driven adaptive framework.
We implemented the prototype in Python.
We evaluated the method on [N] benchmarks.
```

Avoid excessive repetition of "we." Alternate with system or component subjects.

```
We propose [SYSTEM], an error-driven adaptive framework.
[SYSTEM] classifies compiler errors into four categories.
The scheduler maps each category to a sampling temperature.
```

**Passive voice is acceptable when:**

- the actor is irrelevant;
- describing experimental setup;
- describing standard procedures;
- emphasizing the object rather than the actor.

```
The dataset was split into training and test sets.
```

### 2.2 Tense

| Context                    | Preferred tense | Example                                     |
| -------------------------- | --------------- | ------------------------------------------- |
| General facts              | Present         | Rust enforces ownership rules.              |
| Paper contributions        | Present         | We propose a framework.                     |
| Method behavior            | Present         | The classifier extracts error types.        |
| Figures and tables         | Present         | Table~2 shows the results.                  |
| Specific completed actions | Past            | We collected the dataset.                   |
| Observations               | Past            | We observed that the error rate decreased.  |
| Prior authors' actions     | Past            | Smith et al. proposed a repair method.      |
| Existing relevance         | Present perfect | Several studies have explored this problem. |
| Future work                | Future          | Future work will investigate deployment.    |

**Related Work pattern:**

```
Smith et al. [5] proposed X.
Their method achieves Y but assumes Z.
```

### 2.3 Pronoun discipline

Prefer explicit nouns when the antecedent is distant, ambiguous, or important.

Avoid bare *This* or *That* when the referent may be unclear.

```
Weak:    This is important.
Better:  This distinction is important.
         This limitation motivates our design.
         This result suggests that compiler feedback is useful.
```

Pronouns are acceptable when the antecedent is clear:

```
The classifier extracts error categories. It then maps each category to a repair strategy.
```

If multiple antecedents exist, repeat the noun.

### 2.4 Participial phrases (-ing)

Avoid `-ing` participial phrases when they obscure agency, causality, or temporal order.

> **AI overuse warning:** LLM-generated prose disproportionately produces trailing participial phrases ("..., making it difficult to...", "..., thereby reducing...") and "by + -ing" constructions. These are grammatically legal but become an AI-voice tell when overused. Actively vary sentence structures: prefer finite clauses, prepositional phrases, or separate sentences. If more than one participial phrase appears per paragraph, rewrite at least one.

```
Weak:    LLMs often lack effective mechanisms, failing to parse compiler messages.
Better:  LLMs often lack effective mechanisms for parsing compiler messages.

Weak:    We designed [SYSTEM], leveraging compiler feedback to improve performance.
Better:  We designed [SYSTEM] to leverage compiler feedback for adaptive temperature selection.

Weak:    The method classifies errors, mapping each to a strategy, thereby improving repair rates.
Better:  The method classifies errors and maps each to a strategy. This mapping improves repair rates.
```

**Permitted uses (sparingly):**

```
Dynamically adjusting temperature is essential.        (gerund subject — no alternative)
The module processes messages containing ownership errors.  (defining participial — concise)
```

### 2.5 Parallel structure

Maintain parallel grammar in lists and comparisons.

```
Weak:    The method is efficient, accurate, and has high reliability.
Better:  The method is efficient, accurate, and reliable.

Weak:    We aim to improve accuracy, reducing latency, and ensure safety.
Better:  We aim to improve accuracy, reduce latency, and ensure safety.
```

### 2.6 Sentence openings

Prefer clear noun subjects when possible. Avoid long front-loaded modifiers.

```
Weak:    To address the challenge of memory safety in legacy systems, and to improve the
         efficiency of migration, this paper proposes ...
Better:  This paper proposes a compiler-guided migration framework to improve memory safety
         in legacy systems.
```

Short infinitive openings are acceptable in isolation:

```
To address this limitation, we propose a compiler-guided repair strategy.
```

> **AI overuse warning:** LLM-generated text frequently opens consecutive sentences or paragraphs with "To + infinitive" ("To mitigate...", "To evaluate...", "To address..."). One or two per section is natural; more becomes repetitive. Vary openings with noun subjects, temporal phrases, or contrastive connectors.

### 2.7 Formal register

Avoid colloquial or vague expressions.

```
Weak:    The method gets better results.
Better:  The method achieves higher accuracy.

Weak:    We do experiments.
Better:  We conduct experiments.

Weak:    The results are very good.
Better:  The results show a substantial improvement.
```

Avoid weak/colloquial uses of: *get, do, make, have, take, come, go*.

**Do not rewrite these verbs when they are technically precise:**

```
The function takes a string as input.
The model has 7B parameters.
The algorithm makes no assumptions about input order.
The performance gain comes from reduced synchronization.
```

### 2.8 Contractions

Do not use contractions in academic prose.

```
Forbidden: it's, don't, can't, won't, isn't, doesn't
Use:       it is, do not, cannot, will not, is not, does not
```

### 2.9 Possessive forms

Possessive `'s` is acceptable in CS writing.

```
Acceptable: Rust's ownership model
            the method's performance
            the compiler's error messages
```

Avoid chains of possessives.

```
Weak:    the model's training's convergence
Better:  the convergence of model training
```

Use `of` when the noun phrase is long.

### 2.10 Technical noun phrases

Do not decompose established CS terms.

```
Acceptable: compiler error message
            memory safety violation
            type checking algorithm
            code migration tool
            ownership rule violation
            static analysis technique
            automated program repair
            test case generation
```

Decompose novel or overly long noun stacks when they are not established terms.

```
Weak:    compiler error message context analysis algorithm
Better:  an algorithm for analyzing the context of compiler error messages

Weak:    small sample size medical image segmentation improvement measures
Better:  measures for improving medical image segmentation with small sample sizes
```

**Guideline:**

- Keep recognized CS terms as noun phrases.
- Decompose novel combinations of more than three nouns.
- Prefer readability over compactness.

### 2.11 Sentence length

- Prefer 15–25 words on average.
- Vary length: long sentences for nuance, short for emphasis.
- Anything over 35 words usually hides a sub-clause that should be its own sentence.

### 2.12 Hyphenation in compound modifiers

Hyphenate multi-word adjectives that precede a noun:

```
compiler-guided repair
data-driven analysis
memory-safety bug
state-of-the-art performance
end-to-end latency
C-to-Rust migration
```

Do **not** hyphenate when the same words follow the noun:

```
the repair is compiler guided
analysis that is data driven
```

Do **not** hyphenate adverbs ending in `-ly`:

```
Wrong: a fully-trained model
Right: a fully trained model
```

### 2.13 Number formatting

Use words for numbers under 10 in non-quantitative prose; use digits for measurements, comparisons, and any number 10 or above:

```
We ran three experiments.
The model has 7B parameters.
We compared 12 baselines.
The pass rate improved by 15.2 percentage points.
```

For numerical separators and units, prefer LaTeX `siunitx` (`\num{1234567}`, `\SI{32}{\giga\byte}`) — see references/latex-conventions.md.

### 2.14 American vs. British English

Pick one and apply it consistently. Most CS venues default to **American English** (color, behavior, analyze, modeled). British (colour, behaviour, analyse, modelled) is acceptable when the venue specifies it. Do not mix.

Common pairs to watch:

```
US                 UK
analyze            analyse
behavior           behaviour
modeling/modeled   modelling/modelled
optimize           optimise
defense            defence
center             centre
```

See references/meta.md § English consistency for the full checklist.

### 2.15 Oxford comma

Pick one policy per paper and stay consistent. ACM and IEEE styles do not mandate one; most authors use the Oxford comma in CS.

```
With Oxford:    accuracy, latency, and memory usage
Without Oxford: accuracy, latency and memory usage
```

### 2.16 Em-dashes and punctuation variety

Em-dashes (---) are grammatically valid but less common in CS papers than in journalism or humanities writing.

> **AI overuse warning:** LLM-generated prose inserts em-dashes frequently as a default structuring device, often where a colon, semicolon, period, or parenthetical comma would be more natural. Prefer these alternatives; reserve em-dashes for occasional parenthetical asides where commas would be ambiguous.

```
AI default:  The framework has three phases --- generation, repair, and validation.
Prefer:      The framework has three phases: generation, repair, and validation.

AI default:  The borrow checker --- which enforces ownership rules --- rejects the code.
Prefer:      The borrow checker, which enforces ownership rules, rejects the code.
```

If a paper already avoids em-dashes, do not introduce them.
