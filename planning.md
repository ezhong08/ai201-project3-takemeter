# Planning Document — TakeMeter

## 1. Community

**Community:** Steam Community gaming forums ([steamcommunity.com](https://steamcommunity.com/)), specifically the per-game discussion hubs where players post about gameplay, strategy, controversy, and meta-commentary.

**Why this community?** Gaming forums are a natural fit for a discourse-format classification task for three reasons:

- **Format diversity.** A single thread can contain one-line emotional outbursts ("bruh"), multi-paragraph mechanical breakdowns of animation-canceling systems, comparative rankings of Souls-like difficulty, and critical evaluations of developer decisions. This diversity is exactly what makes classification non-trivial — the model must learn structural signals, not just topic keywords.
- **High signal-to-noise ratio.** Gaming forums produce thousands of posts daily. Community managers and players alike need tools to separate substantive contributions (analytical breakdowns, structured criticism) from noise (venting, one-word replies). A classifier that operates on communicative _format_ rather than opinion _content_ is reusable across games, genres, and controversy cycles.
- **Public accessibility.** Steam Community discussions are publicly visible. Threads are organized by game and topic, which makes it straightforward to sample across genres (action RPG, colony sim, survival horror, competitive FPS) and discussion types without authentication barriers.

The five games selected — _Black Myth: Wukong_, _RimWorld_, _Resident Evil 4_, _007: First Light_, and _Counter-Strike 2_ — span five distinct genres and five distinct discussion dynamics (strategy Q&A, sandbox theorycrafting, casting controversy, reviewer meta-criticism, anti-cheat policy debate). This breadth ensures the dataset captures discourse variation beyond any single community's norms.

---

## 2. Labels

TakeMeter uses **four mutually exclusive labels**, each capturing the structural format of a forum post — not the topic, tone, or opinion expressed.

### `critical`

A post that evaluates some aspect of gaming — a game, a developer decision, an anti-cheat system — across multiple criteria, with the author adopting critical distance and weighing pros and cons systematically.

| Example                                                                                                                                                             | Why it fits                                                                                                                        |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| "0/10 award bait. There is only a couple of decent bosses, rest is easily spammable. Its the easiest souls like ever made"                                          | Structured as a verdict (score + criteria): boss quality, difficulty, overall value. The author is _assessing_, not just reacting. |
| "I'm not sure someone who made a 40 minute video titled 'Silent Hill f is disgusting woke propaganda' has a whole lot of valuable things to say about video games." | Evaluates a reviewer's credibility using criteria: content history, bias, argument quality. The post is organized around judgment. |

### `impressionistic`

A post centered on the author's personal experience and emotional response, prioritizing subjective feeling over structured evaluation.

| Example                                                                                                                                              | Why it fits                                                                                                                                           |
| ---------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| "this game is just as bad as sekiro it feels liek the game is not meant to be beat. do u guys watch boss moveset videos on youtubve or something???" | Organized around the author's frustration and personal struggle. The comparison to Sekiro serves the emotional expression, not a structured argument. |
| "yeah lol"                                                                                                                                           | Pure affective response. No criteria, no analysis — the post _is_ the reaction.                                                                       |

### `comparative`

A post that primarily understands its subject through comparison to other games, entries, or benchmarks — the comparison is the organizing structure.

| Example                                                                                                                                                                             | Why it fits                                                                                                        |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| "Bout pattern recognition and waiting for proper openings to do damage. Its a LOT easier than sekiro honestly but you gotta keep track. use skills at the right time and all that." | The comparison to Sekiro frames the entire post. The gameplay advice serves the comparison, not vice versa.        |
| "this is cake compared to sekiro lmfao"                                                                                                                                             | Minimalist, but the entire meaning of the post is the comparative claim. Without the comparison, there is no post. |

### `analytical`

A post that explains _how_ or _why_ something works — game mechanics, design choices, systems interactions — going beyond evaluation to build understanding.

| Example                                                                                                                                                                                                                                                  | Why it fits                                                                                                                                            |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| "Learn their patterns and use them against them. Every boss has an opening. Don't be too greedy."                                                                                                                                                        | Explains a mechanical principle (pattern recognition → openings → punish) rather than just evaluating difficulty. The author is teaching, not judging. |
| "Okay…so here's how VAC actually works. The best analog to describe it is antivirus software…only for cheats. When a piece of a cheat app's memory signature is hashed into the VAC blacklist, anyone who used it in the past…gets a VAC Ban instantly." | A systemic explanation of anti-cheat architecture. Organized around _how it works_, not whether it's good or bad.                                      |

---

## 3. Hard Edge Cases

Some posts genuinely sit at the boundary between two labels. These are not annotation errors — they reflect real ambiguity in how people communicate. The taxonomy handles these cases through the **dominant structural mode** rule: when a post blends two formats, choose the label that _organizes_ the post, not the one whose signals appear most frequently.

### Boundary: `impressionistic` vs `analytical`

**What makes it hard:** Personal experience posts that include genuine mechanical insight. The author describes their subjective journey but the journey includes design observations.

> "For me, it means trying over and over and if I fail, I try again the next day. Eventually you will get it. You just have to keep trying. It really is all about memorizing what the enemies can do and reading their attacks through animation. It can be very tough."

**Resolution:** Ask: _Is the insight in service of the story, or is the story in service of the insight?_ If the post is organized around the author's experience and the mechanical observation is part of that narrative, label it `impressionistic`. If the post uses personal experience as a launching pad for a standalone analytical argument, label it `analytical`.

### Boundary: `comparative` vs `analytical`

**What makes it hard:** Posts that compare games but also make genuine mechanical or design arguments. The comparison is the vehicle but the destination is analysis.

> "I think you're supposed to hit them with the stick a bunch, but I only just started playing yesterday. Elden Ring is the only particular Souls-like I've dove into, and frankly it IS an easy game if you take your time and actually use all the mechanics available to you. Every fight in the game can be cheesed multiple ways, even Malenia."

**Resolution:** Ask: _If you removed the comparison, would the analytical argument still stand?_ The Elden Ring post compares two games but also makes a standalone claim about difficulty being mitigated by mechanical optionality. However, the comparison is still the framing device — the argument is that Black Myth is like Elden Ring in this respect. Label it `comparative` when the comparison is load-bearing; label it `analytical` when the comparison is an aside.

### Boundary: `critical` vs `impressionistic`

**What makes it hard:** Short evaluative posts that are also emotionally charged. A post saying "0/10 award bait" is evaluative but also an emotional outburst — is it a critical assessment or an impressionistic reaction?

> "holy cow what were they thinking? all previous ada wong sound ok but RE4R omg i feel bad for the actress but you wouldn't pick rob schneider to give voice to kratos..."

**Resolution:** Look for _criteria._ If the post identifies what is being evaluated and on what grounds (even briefly), it's `critical`. Emotional language is tone, not structure. If the post is pure reaction without identifiable evaluative criteria, it's `impressionistic`.

### Process for ambiguous posts during annotation

1. Flag the post as borderline in a working note.
2. Apply the dominant-mode question for the relevant boundary.
3. Make a decision and record the reasoning in one sentence.
4. After every ~50 annotations, review all flagged posts together — consistency across similar edge cases matters more than perfect correctness on any single case.
5. If a boundary is consistently troublesome, refine the edge-case rule in `taxonomy.md`.

---

## 4. Data Collection Plan

### Sources

All posts are collected from **Steam Community per-game discussion forums**. Threads are selected to maximize format diversity: gameplay Q&A, controversy threads, review/discussion posts, and meta-commentary about the game industry.

### Target quantities

- **Minimum:** 200 total annotated posts
- **Target per label:** ~50 posts each (balanced), but the natural distribution of gaming discourse skews toward impressionistic and analytical formats
- **Minimum per label:** 10 posts for any individual label (below this, the model cannot learn meaningful patterns)

### Collection strategy

1. Select 4–6 games across different genres to avoid genre-specific discourse bias.
2. For each game, identify 3–5 active discussion threads that represent different discussion types.
3. Collect posts from each thread, sampling across the thread's lifespan to capture both early (reactive) and late (reflective) contributions.
4. Annotate posts in batches of ~50, tracking label distribution after each batch.

### What if a label is underrepresented after 200 posts?

If any label has fewer than ~15 examples after collecting 200 posts:

1. **Targeted thread selection.** Different thread types produce different discourse formats:
   - Low on `critical` → seek out review-aggregation threads, "should I buy this game" discussions, post-launch assessment threads.
   - Low on `comparative` → seek out "X vs Y" threads, franchise ranking discussions, "if you liked **_, try _**" recommendation threads.
   - Low on `analytical` → seek out mechanics-explanation threads, build/strategy guides, "how does \_\_\_ work" Q&A.
   - Low on `impressionistic` → this is unlikely (it's the most common format), but first-impressions and "my time with" threads are reliable sources.

2. **Adjust annotation priorities.** In the next collection batch, prioritize threads likely to produce the underrepresented format.

3. **Accept real distribution as a feature.** Some imbalance is natural — `comparative` posts are genuinely rarer in gaming forums because they require cross-game knowledge. A classifier that reflects real prevalence is more useful than one trained on an artificial balance. The minimum bar is: enough examples of every label for the model to learn distinguishable features (≥10–15).

### Final distribution (actual, after 212 annotations)

| Label             | Count | Notes                                                                         |
| ----------------- | ----: | ----------------------------------------------------------------------------- |
| `impressionistic` |    81 | Most common — short reactions and emotional responses dominate forum threads  |
| `analytical`      |    59 | Well-represented — gameplay discussion naturally produces mechanical analysis |
| `critical`        |    59 | Well-represented — controversy threads produce structured evaluations         |
| `comparative`     |    13 | Underrepresented but real — requires cross-game knowledge, genuinely rarer    |

The `comparative` class at 13 examples (6.1%) is the most challenging for model training and is flagged as a risk (see §7). Mitigation: during evaluation, per-label metrics for `comparative` are reported separately so low performance on this class is visible rather than hidden by a high overall accuracy.

---

## 5. Evaluation Metrics

Accuracy alone is misleading for this task because of class imbalance — a model that predicts `impressionistic` for every post would achieve 38.2% accuracy. The following metrics are used instead:

### Primary: Macro-averaged F1

**Why:** Macro F1 computes the F1 score for each label independently and then averages them, treating all four labels as equally important regardless of prevalence. A model must perform well on `comparative` (6% of data) to achieve a strong macro F1 — it cannot coast on the majority classes.

### Per-label precision, recall, and F1

**Why:** Different deployment contexts care about different labels. A community manager triaging feedback might need high `critical` recall (don't miss substantive criticism). A content curator building a "deep dives" feed might need high `analytical` precision (don't pollute the feed with shallow posts). Per-label metrics make these tradeoffs visible and actionable.

### Confusion matrix

**Why:** The most interesting failures are not random — they're systematic confusions between adjacent labels (impressionistic ↔ analytical, comparative ↔ analytical). A confusion matrix reveals _where_ the model's structural understanding breaks down and whether the errors align with the hard edge cases identified during annotation.

### Secondary: Cohen's kappa

**Why:** Kappa measures agreement with ground truth above chance level. For a 4-class problem with imbalanced data, a kappa of 0.6–0.8 represents substantial agreement; below 0.4 is poor. This provides a single interpretable number that accounts for the class distribution.

### What we are NOT using and why

- **Accuracy:** Uninformative with imbalanced classes.
- **ROC-AUC (one-vs-rest):** Less interpretable than per-label F1 for a multi-class problem where false positives and false negatives have different real-world costs.
- **Cross-entropy loss:** Useful during training but not a deployment-facing metric — it doesn't answer "is this good enough to use?"

---

## 6. Definition of Success

### What makes this classifier genuinely useful?

A discourse-format classifier is useful when it can _reliably triage_ — that is, when a community tool built on it surfaces more signal than noise. Concretely:

| Tier                        | Threshold                               | What it means                                                                                                                                                                                 |
| --------------------------- | --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Deployable**              | Macro F1 ≥ 0.80, no label < 0.70 F1     | The classifier is reliable enough to power automated features (e.g., "sort by analytical depth," "filter critical reviews only"). False positives are low enough that users trust the labels. |
| **Useful with supervision** | Macro F1 ≥ 0.70, no label < 0.60 F1     | The classifier can assist human moderators (e.g., flagging potentially analytical posts for a highlights feed) but needs human review for borderline cases.                                   |
| **Not there yet**           | Macro F1 < 0.70, or any label < 0.50 F1 | The classifier is not reliable enough for production use. Focus on: more data for the weak label, error analysis on the confusion matrix, and possible taxonomy simplification.               |

### "Good enough" for a real community tool

For this project, **macro F1 ≥ 0.75 with `comparative` F1 ≥ 0.60** is the success threshold. Rationale:

- **75% macro F1** means the model is substantially better than random or majority-class baselines across all four labels.
- **60% `comparative` F1** acknowledges that this is the hardest class (fewest examples, most subtle signals). 60% is usable — it means the model is right about `comparative` posts more often than not, and when combined with the stronger labels, the overall system still adds value.
- At this performance level, a hypothetical community tool could: (a) auto-tag posts by discourse format with reasonable accuracy, (b) enable filtering by format type, and (c) highlight analytical contributions in noisy threads — all with errors concentrated in the most ambiguous edge cases, which humans also find difficult.

### What we'd need for deployment beyond gaming

The current model is trained exclusively on gaming forum data. To deploy in a new domain (e.g., sports forums, music discussion boards), we would need:

- Domain-specific fine-tuning data (~50–100 annotated examples from the target community)
- Re-evaluation of label definitions — some communities may need different structural categories
- Validation that the four-label taxonomy transfers; if not, the taxonomy itself may need domain adaptation

---

## 7. AI Tool Plan

This project's workflow is annotation-heavy rather than implementation-heavy — there's no application code to generate, no API to design. AI tools are most useful at three specific points: stress-testing label definitions before annotation begins, assisting with the annotation workload itself, and analyzing model failures after training.

### 7.1 Label Stress-Testing

**Goal:** Before annotating 200 posts, verify that the label definitions and edge-case rules actually produce clean, consistent classifications — not just for clear-cut examples but for the posts that sit at boundaries.

**Method:**

1. Give an LLM the full taxonomy from [`data/taxonomy.md`](data/taxonomy.md), including the edge-case resolution rules from §3 of this document.
2. Prompt the model to generate **5–10 synthetic forum posts that sit at each boundary** (impressionistic↔analytical, comparative↔analytical, critical↔impressionistic, critical↔analytical) — roughly 20–40 posts total.
3. Annotate the generated posts myself _without_ looking at which boundary the model says each post targets.
4. Compare my labels against the intent behind each generated post. Score the exercise:
   - **Clean pass:** ≥90% of generated posts land unambiguously in one label and I agree with the intended boundary characterization. The taxonomy is tight enough.
   - **Definitions need work:** 70–89% — some boundaries are genuinely muddy. Refine the edge-case rules in `taxonomy.md` before annotating real data.
   - **Back to the drawing board:** <70% — the label definitions themselves need restructuring. Do not proceed with annotation until this is fixed.

**Why before annotation, not after:** Fixing label definitions after annotating 200 posts means either re-annotating everything (wasting time) or living with inconsistent labels (hurting model quality). Stress-testing the taxonomy with synthetic borderline posts costs ~30 minutes up front and can prevent hours of rework.

### 7.2 Annotation Assistance

**Decision:** An LLM will be used to pre-label batches before manual review, not to replace manual annotation.

**Tool:** Claude (via claude.ai or the API) — chosen because the taxonomy is already Claude-native (the label definitions were refined through iterative prompting) and pre-labeling with the same model family that will be used for failure analysis keeps the tooling consistent.

**Workflow for each batch of ~50 posts:**

1. Send the raw (unlabeled) posts to the LLM along with the full taxonomy and edge-case rules.
2. The LLM returns a label for each post plus a one-sentence rationale.
3. I review every pre-label manually, reading the full post text. For each post, I either:
   - **Accept** the pre-label (agree with both label and rationale).
   - **Override** the pre-label (disagree) and record my final label.
4. The final label (whether accepted or overridden) becomes the ground truth. The LLM's pre-label is _advice_, not authority.

**Tracking and disclosure:**

- Every post in the dataset carries a `pre_label` field recording the LLM's initial suggestion (or `null` if not pre-labeled).
- A `label_source` field is set to `"manual"` for posts where the pre-label was overridden, or `"llm_reviewed"` for posts where the pre-label was accepted after manual review. No post is labeled purely by the LLM without human review.
- The README and any writeup will disclose: which model was used for pre-labeling, what percentage of final labels matched the pre-label (inter-annotator agreement rate), and whether any systematic biases were observed (e.g., the LLM over-predicting `analytical` on longer posts).
- Pre-labeling metadata is stored in the dataset JSON so future readers can audit which labels were LLM-suggested vs. human-originated.

**Why pre-label instead of label from scratch:** Pre-labeling turns annotation from a _generation_ task (harder, slower, more prone to drift) into a _verification_ task (faster, more consistent). The human is still the final arbiter for every single label.

### 7.3 Failure Analysis

**Goal:** After training and evaluation, identify _why_ the model makes the errors it makes — not just count them.

**Method:**

1. After running evaluation on the test set, collect every misclassified post into a failure log: post text, true label, predicted label, and the model's confidence score.
2. Feed the full failure log to an LLM (Claude 4 or equivalent) with this prompt structure:

   > "Here are [N] misclassifications from a 4-label discourse-format classifier. The labels are critical, impressionistic, comparative, and analytical (definitions attached). For each error, explain what structural signals might have led the model to the wrong label. Then, identify patterns across the full set: are there systematic confusions? Do certain boundary types dominate? Are there surface features (post length, specific phrases, game context) that correlate with errors?"

3. The LLM returns:
   - **Per-error analysis:** A one-line hypothesis for each misclassification.
   - **Pattern summary:** Grouped findings (e.g., "8 of 12 errors are impressionistic posts predicted as analytical — the model is over-indexing on mechanical vocabulary in otherwise experiential posts").
   - **Suggested mitigations:** Concrete ideas (e.g., "add length as a feature," "oversample comparative in training," "the word 'pattern' appears in 6 false positives — the model treats it as an analytical keyword").

**What I'll look for:**

| Pattern type                    | Example                                                                                        | Why it matters                                                                                                                                 |
| ------------------------------- | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| **Boundary concentration**      | 70% of errors are on impressionistic↔analytical boundary posts                                 | The taxonomy's hardest edge case is also the model's hardest case — the definitions are sound but the boundary is inherently difficult         |
| **Surface-feature overfitting** | Posts containing "mechanic" are always predicted analytical, even when the post is comparative | The model learned a keyword, not a structure — fix with more diverse training examples or data augmentation                                    |
| **Length bias**                 | Short posts are systematically predicted impressionistic; long posts predicted analytical      | The model is using post length as a proxy for discourse format — might be valid (real analytical posts tend to be longer) or might be spurious |
| **Game leakage**                | All CS2 posts about VAC bans are predicted critical regardless of actual label                 | The model associated a topic with a label — indicates topic contamination in training data                                                     |
| **Confidence calibration**      | High-confidence errors cluster on one boundary; low-confidence errors are scattered            | If high-confidence errors are systematic, the model is confidently wrong about a specific pattern — that's the highest-priority fix            |

**How I'll verify the patterns myself:**

- For each pattern the LLM identifies, spot-check at least 5 examples manually. Read the posts. Ask: _does the LLM's explanation hold up when I look at the text myself?_
- Cross-reference patterns against the annotation decision log from §3 — do the model's errors cluster on posts I flagged as borderline during annotation? If yes, the model is struggling where I struggled, which is a good sign (the taxonomy is consistent).
- If the LLM identifies a pattern I disagree with, note it in the writeup as an unconfirmed hypothesis and explain why I rejected it.

**Why use an LLM for this instead of doing it manually:** A 32-post test set with ~10–15 errors is small enough to review manually, but an LLM brings consistency and an outside perspective — it reads every error through the same lens and can surface patterns a human might skip because of anchoring on individual posts. The LLM's output is the _starting point_ for my failure analysis, not the final word.

---

## 8. Risks and Open Questions

| Risk                                                                       | Likelihood | Mitigation                                                                                                                                               |
| -------------------------------------------------------------------------- | ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `comparative` class too small for reliable learning                        | Medium     | Report per-label metrics; consider oversampling or weighted loss during training if F1 < 0.50                                                            |
| Label drift during annotation (inconsistent criteria across batches)       | Medium     | Flag borderline posts; review all flags together after each batch; maintain an annotation decision log                                                   |
| Model overfits to game-specific vocabulary rather than discourse structure | Low        | Train/test split is stratified by label, not by game — test performance across unseen games reveals vocabulary overfitting                               |
| Four labels are insufficient for real community needs                      | Low        | The taxonomy is designed to be exhaustive (≥90% coverage). If a systematic fifth format emerges during annotation, revisit the taxonomy before training. |
| Cross-domain generalization is poor                                        | Expected   | This is a feature, not a bug — the model is explicitly domain-specific. Cross-domain transfer is future work (§6).                                       |
| LLM pre-labeling introduces systematic bias                                | Medium     | Track pre-label agreement rate; report any label-level skew (e.g., LLM over-predicts analytical); all labels are human-verified (§7.2)                   |
