# AI Drafting Failure Patterns

> **When to load this file:** reviewing AI-drafted prose, spotting rationalisations during a polish pass, or auditing a near-final manuscript for telltale AI-generation signatures.

These are the failure modes that LLM drafting most often exhibits. Each entry has the failure, why it harms the paper, and the prevention rule.

---

## 1. Hallucinated Evidence

**Failure.** Inventing results, citations, datasets, baselines, hardware, or p-values.

**Why it harms.** Reviewers and editors verify claims; fabricated evidence leads to retractions and reputational damage.

**Prevention.** Use placeholders. Ask for missing information. Never produce specific numbers, citations, or system names that the user has not provided.

```
[RESULT] [DATASET] [BASELINE] [METRIC] [CITATION] [STATISTICAL TEST] [P-VALUE]
```

---

## 2. Empty Academic Prose

**Failure.** Writing formal-sounding sentences that contain no technical information.

```
Weak:    This problem is very important and has attracted much attention.
Better:  Memory-safety bugs remain a major source of crashes and vulnerabilities in systems
         software.
```

**Why it harms.** Reviewers skip filler. Empty sentences pad the page count without earning the space.

**Prevention.** Every sentence should advance a specific claim, define a term, or transition to the next claim.

---

## 3. Unsupported Novelty

**Failure.** Claiming *first*, *novel*, or *state-of-the-art* without evidence.

**Why it harms.** Asserts a position the paper cannot defend; reviewers will counterexample with prior work.

**Prevention.** Use *we propose*, *we introduce*, or *we present* instead. Reserve novelty claims for cases where the user has explicitly framed firstness, with evidence.

See references/quick-reference.md § "Words to use carefully".

---

## 4. Overclaiming

**Failure.** Saying a method "solves" a broad problem when it only improves a metric.

```
Weak:    Our method solves C-to-Rust migration.
Better:  Our method improves the success rate of LLM-based C-to-Rust migration on the
         evaluated benchmarks.
```

**Why it harms.** Reviewers will read this as inflation; honest papers scope claims to evaluation.

**Prevention.** Scope claims to the **evaluated task, benchmark, and metric**.

---

## 5. Mechanical Transitions

**Failure.** Overusing *Furthermore*, *Moreover*, *Additionally* — often as the first word of consecutive paragraphs.

**Why it harms.** Signals AI-generated prose; obscures whether the relationship is actually additive.

**Prevention.** Use connectors only when they express a clear logical relationship. Vary the connector when the relationship is the same. Sometimes the connector is unnecessary — drop it.

---

## 6. Ambiguous Pronouns

**Failure.** Using *this*, *it*, or *they* when multiple antecedents are possible.

**Why it harms.** The reader has to guess; technical sentences should not require guessing.

**Prevention.** Replace ambiguous pronouns with explicit noun phrases.

```
Weak:    The classifier categorizes errors. The scheduler maps these to temperatures.
         This improves performance.        ← what does "this" refer to?
Better:  This pipeline improves performance.
```

---

## 7. Vague Results

**Failure.** *The method performs better.*

**Why it harms.** No metric, no dataset, no baseline. Reviewers cannot evaluate the claim.

**Prevention.** Specify metric, dataset, baseline, and value when provided. Use the safe-claim patterns in references/quick-reference.md.

---

## 8. Method-Result Mixing

**Failure.** Reporting evaluation results inside the Method section without a reason.

**Why it harms.** Confuses the contribution (mechanism) with the empirical evidence; readers expect a clean separation.

**Prevention.** Keep mechanism descriptions in Method and empirical findings in Results. Forward references ("Section~5 evaluates this design") are acceptable.

---

## 9. Contribution Inflation

**Failure.** Turning small implementation details into separate contributions.

```
Weak:    - We propose the framework.
         - We implement the prototype in Python.
         - We add a logging module.
         - We add a configuration parser.
```

**Why it harms.** Diluting the contribution list makes the real contributions look smaller.

**Prevention.** Group minor details under a larger design or implementation contribution.

```
Better:  - We propose [SYSTEM], a framework that ...
         - We implement and release a [SYSTEM] prototype evaluated on [DATASET].
```

---

## 10. Semantic Drift During Rewriting

**Failure.** Changing the author's intended technical meaning while polishing.

**Why it harms.** Fundamentally violates trust: the author is no longer the author of their own claims.

**Prevention.** Preserve mechanism, scope, numbers, caveats, and claims. When unsure, leave the original; flag the unclear sentence to the user instead of guessing.

---

## 11. Over-Formalization

**Failure.** Replacing simple clear words with unnecessarily complex ones.

```
Weak:    The proposed methodology effectuates the facilitation of improved performance.
Better:  The proposed method improves performance.
```

**Why it harms.** Inflates word count, reduces clarity, signals AI generation.

**Prevention.** When two phrasings have the same meaning, prefer the shorter, plainer one.

---

## 12. Citation Misuse

**Failure.** Using citations as decoration rather than evidence.

```
Weak:    Code generation is important [1], [2], [3].
Better:  Code generation has emerged as a major application of language models [1], [2].
```

**Why it harms.** Reviewers spot-check citations; misuse erodes credibility for the rest of the paper.

**Prevention.** Attach each citation to a specific factual claim. See references/citations.md § 8.

---

## 13. Hedge Stacking

**Failure.** Stacking hedges to dodge commitment.

```
Weak:    Our method might possibly have some effect.
Better:  Our method improves the pass rate on the evaluated benchmarks.
```

**Why it harms.** Hedge stacks are usually trying to compensate for weak evidence; reviewers see through it.

**Prevention.** Use one hedge level appropriate to the evidence (see references/statistics.md § 9 — confidence calibration).

---

## 14. Filler Openings

**Failure.** Starting sentences with weak fillers.

| Filler | Replacement |
|--------|-------------|
| It is worth noting that | (delete; state the observation) |
| As can be seen          | Table~\ref{} shows |
| It is important to note that | (delete; the importance should be implicit) |
| In order to             | To |
| Due to the fact that    | Because |
| In light of the fact that | Because |

**Why it harms.** Fillers are pure padding; they slow the reader.

**Prevention.** Strike fillers in editing.

---

## 15. List Imbalance

**Failure.** Bullet lists where one bullet is two words and another is three sentences.

**Why it harms.** Suggests the author has not fully resolved the structure of the section.

**Prevention.** Make all bullets approximately the same grammatical and informational weight. Consider whether the long bullet should become a paragraph.

---

## 16. Synonym Drift

**Failure.** Using *system*, *framework*, *tool*, *prototype*, *implementation* interchangeably for the same artefact.

**Why it harms.** Reviewers wonder whether the words refer to different things.

**Prevention.** Pick one term per concept and use it consistently. Distinguish different concepts with different terms.

---

## 17. Inconsistent Capitalization / Naming

**Failure.** Switching between *AdaTrans*, *ADA-Trans*, *AdaTRANS*, *adatrans* in the same paper.

**Why it harms.** Reviewers notice; copyeditors flag; readers lose track.

**Prevention.** Pick one canonical name (matching the artifact, repository, and bibliographic record) and apply paper-wide. Add a `\newcommand{\sysname}{AdaTrans}\xspace` macro and use `\sysname` everywhere.

---

## 18. Forward-Reference Maze

**Failure.** Long chains of "We will discuss this in Section X" without ever discussing it concretely.

**Why it harms.** Suggests the author has not finished the paper; reviewers feel led around.

**Prevention.** Forward references are fine when they point to a *specific* concrete discussion. They are not a substitute for explaining the concept now.

---

## 19. AI-Voice Tells

A non-exhaustive list of constructions that strongly signal LLM-drafted prose:

- "It is important to note that ..."
- "It is worth mentioning that ..."
- "In summary, ..." (at the end of every short paragraph)
- "Furthermore, additionally, moreover, ..." (chained at start of consecutive paragraphs)
- "delve into", "navigate", "tapestry", "underscore", "harness", "leverage" (when *use* would do)
- "It should be noted that ..."
- "On the one hand ... on the other hand ..." (when only one hand is being argued)
- "In conclusion, this paper ..." (at the start of the conclusion — almost always cuttable)
- "This research / this study / this work / this paper" repeated within a paragraph.

Strip these in editing. They make the writing sound like a press release, not a technical paper.

---

## 20. Self-Promotion

**Failure.** Inserting phrases like *cutting-edge*, *highly innovative*, *significant breakthrough*, *unprecedented*.

**Why it harms.** Reviewers read these as red flags; they prefer authors who let the evidence speak.

**Prevention.** Strike all such adjectives. State the result.

---

## Quick Audit

When you suspect AI-drafted prose, scan for:

```
[ ] Specific numbers that the user did not provide.
[ ] Specific citation numbers without an associated bib entry.
[ ] "First", "novel", "state-of-the-art" not justified by user-provided evidence.
[ ] "Furthermore" or "Moreover" at the start of consecutive paragraphs.
[ ] Bare "This is important" / "This is significant".
[ ] Synonym drift across paragraphs (system / framework / tool).
[ ] Filler openings ("It is worth noting that").
[ ] Hedge stacking ("might possibly have some").
[ ] AI-voice tells (delve into, leverage, harness).
[ ] Self-promotion adjectives (cutting-edge, unprecedented).
```

If any check fires, revise that section before marking it ready.
