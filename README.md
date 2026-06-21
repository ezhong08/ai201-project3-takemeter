# TakeMeter

A fine-tuned text classifier that evaluates discourse quality in online gaming communities. TakeMeter labels forum posts by their communicative format — critical evaluation, personal impression, comparative analysis, or deep analytical breakdown — helping surface the kinds of contributions that drive meaningful discussion.

## Overview

Online communities run on opinions. NBA subreddits, music theory Discord servers, reality TV recap forums, anime discussion boards — these are spaces where people share takes constantly, and the quality of those takes varies enormously. Some are insightful. Some are hyperbole. Some are just noise. But what makes a good take is genuinely hard to define — it's specific to the community, the context, and the moment.

TakeMeter addresses this problem by classifying posts according to their **structural format** rather than their content, sentiment, or correctness. By labeling discourse along four distinct modes, the model reveals _how_ a community argues, not just _what_ it argues about.

### Domain: Gaming Forums

This project focuses on **online gaming forums and discussion boards** — specifically Steam Community discussions and game-specific subreddits. Gaming forums are a rich domain for discourse classification because:

- **High volume of takes**: Thousands of posts daily across games like _Black Myth: Wukong_, _RimWorld_, _Resident Evil 4_, _007: First Light_, and _Counter-Strike 2_.
- **Diverse discourse formats**: Posts range from quick emotional reactions to multi-paragraph mechanical breakdowns.
- **Community-specific norms**: Each game's community has its own standards for what constitutes a valuable contribution.

## Label Taxonomy

TakeMeter uses four mutually exclusive labels grounded in the communication patterns of gaming communities. Each label captures the **structural format** of a post — not the genre of game, review score, or writing style.

### `critical`

A post that evaluates some aspect of gaming — a game, a developer decision, an anti-cheat system — across multiple criteria. The author adopts a critical distance, weighing pros and cons systematically.

**Key signal:** Structured evaluation with clear criteria. The author is _assessing_, not just describing or experiencing.

**Examples from the dataset:**

> "0/10 award bait. There is only a couple of decent bosses, rest is easily spammable. Its the easiest souls like ever made"

> "I'm not sure someone who made a 40 minute video titled 'Silent Hill f is disgusting woke propaganda' has a whole lot of valuable things to say about video games."

> "22 000 is a small number considering how often cheating occurs. There is still nothing done to improve anti cheat into being more reliant."

---

### `impressionistic`

A post focused on the author's personal experience and emotional response. Less structured than critical posts. Emphasizes how something felt, moments that stood out, and the subjective journey.

**Key signal:** First-person perspective emphasizing experience over evaluation. "I felt," "I experienced," "for me." The post is more memoir than verdict.

**Examples from the dataset:**

> "this game is just as bad as sekiro it feels liek the game is not meant to be beat."

> "I need to read through this thread more carefully. I'm stuck on the boss that turns into an element. Don't want to spoil if people haven't gotten there yet, but it's real tough."

> "Awesome tips, thank you! I am not sure if I have spell binder. I will check and try again this weekend."

---

### `comparative`

A post that primarily evaluates something by comparing it to other games, developers, or contexts — either within a franchise, genre, or against a specific benchmark. The subject is understood through its relationships to other entities.

**Key signal:** Heavy use of comparison language. "If you liked X, you'll like Y," "this is Dark Souls meets Stardew Valley," "better/worse than the previous entry."

**Examples from the dataset:**

> "I think you're supposed to hit them with the stick a bunch, but I only just started playing yesterday. Elden Ring is the only particular Souls-like I've dove into... If you want difficult, beat DMC3 on Dante-Must-Die mode."

> "Bout pattern recognition and waiting for proper openings to do damage. Its a LOT easier than sekiro honestly."

> "this ♥♥♥♥ is cake compared to sekiro lmfao"

---

### `analytical`

A deep dive into a game's systems, mechanics, design philosophy, or broader context. The post goes beyond evaluation to understand _how_ something works, _why_ it was designed that way, or _what_ it means. Often draws on game design terminology, mechanical analysis, or contextual framing.

**Key signal:** The post explains how and why something functions, not just whether it's good. Uses design terminology, mechanical analysis, or contextual framing.

**Examples from the dataset:**

> "The main key to this game seems to be understanding that there is no input queue, and more importantly, that you cannot cancel out of animations... Hence in this game you should only be pressing light attack again as the animation for the prior button is ending."

> "Okay…so here's how VAC actually works. The best analog to describe it is antivirus software…only for cheats. When a piece of a cheat app's memory signature is hashed into the VAC blacklist, anyone who used it in the past…gets a VAC Ban instantly."

> "Spirits can stagger bosses, deal great damage, are not on a cooldown timer but instead on a meter you fill by doing damage."

## Dataset

The dataset consists of **212 annotated posts** collected from the **Steam Community forums** ([steamcommunity.com](https://steamcommunity.com/)), specifically the discussion hubs for individual games. Posts were scraped from public forum threads, de-identified (usernames removed), and manually annotated using the four-label taxonomy defined above.

### Data Collection

All posts were collected from Steam Community discussion boards. Threads were selected to cover a range of game genres (action RPG, colony sim, survival horror, stealth-action, competitive FPS) and discussion types (gameplay advice, controversy, technical analysis). Each post includes the thread title, game name, and post body. No user-identifying information was retained.

| Game               | Thread Topic                          | Posts |
| ------------------ | ------------------------------------- | ----- |
| Black Myth: Wukong | Boss difficulty & strategy discussion | 51    |
| RimWorld           | Colony management & scenario Q&A      | 47    |
| Resident Evil 4    | Voice acting / casting controversy    | 19    |
| 007: First Light   | Reviewer credibility controversy      | 51    |
| Counter-Strike 2   | VAC ban wave & anti-cheat discussion  | 44    |

### Labeling Process

Each post was manually labeled by reading the full text and applying the taxonomy rules from [`data/taxonomy.md`](data/taxonomy.md). The annotation process followed these steps:

1. **Read the post in full** — including any quoted context or multi-line formatting.
2. **Identify the dominant structural mode** — is the author primarily _assessing_ (critical), _experiencing_ (impressionistic), _comparing_ (comparative), or _understanding_ (analytical)?
3. **Apply edge-case resolution rules** from the taxonomy when posts blended multiple modes.
4. **Review borderline cases** — posts that could reasonably fit two labels were re-read and the final label was chosen based on which mode organized the post.

Labels are **mutually exclusive** (one label per post) and **exhaustive** (every post receives exactly one label).

### Label Distribution

| Label             |   Count | % of Total |
| ----------------- | ------: | ---------: |
| `impressionistic` |      81 |      38.2% |
| `analytical`      |      59 |      27.8% |
| `critical`        |      59 |      27.8% |
| `comparative`     |      13 |       6.1% |
| **Total**         | **212** |   **100%** |

The distribution reflects the nature of gaming forum discourse: **impressionistic** posts (personal reactions, emotional responses) are the most common format, while **comparative** posts (which require cross-game knowledge) are the rarest. This imbalance is a real property of the domain and is preserved in the train/validation/test splits.

### Train / Validation / Test Split

The dataset is split with stratified sampling (preserving label proportions within each split):

| Split     | Posts | impressionistic | analytical | critical | comparative |
| --------- | ----: | --------------: | ---------: | -------: | ----------: |
| **Train** |   148 |              57 |         41 |       41 |           9 |
| **Val**   |    32 |              12 |          9 |        9 |           2 |
| **Test**  |    32 |              12 |          9 |        9 |           2 |

Split files: [`data/train.json`](data/train.json), [`data/val.json`](data/val.json), [`data/test.json`](data/test.json).

### Difficult-to-Label Examples

Some posts genuinely sit at the boundary between two labels. These edge cases are the hardest to annotate consistently and are where the model is most likely to struggle. Here are three examples:

---

#### Example 1: Personal experience or mechanical analysis?

> **Post g034** — _Black Myth: Wukong_  
> "For me, it means trying over and over and if I fail, I try again the next day. Eventually you will get it. You just have to keep trying. It really is all about memorizing what the enemies can do and reading their attacks through animation. It can be very tough."

**Labels considered:** `impressionistic` vs `analytical`

**Why it's hard:** The post opens with a strong first-person experiential frame ("For me, it means trying over and over…") and describes the author's personal learning process — classic impressionistic signals. But it pivots mid-post to a specific mechanical insight: "reading their attacks through animation." This is a design observation about boss telegraphing, which is analytical territory. The post is organized around the author's personal journey, but the journey includes a genuine gameplay insight.

**Final label:** `impressionistic` — The dominant structural mode is personal narrative. The animation-reading insight is presented as something the author _experienced_, not as a standalone mechanical argument.

---

#### Example 2: Comparison or deep analysis?

> **Post g021** — _Black Myth: Wukong_  
> "I think you're supposed to hit them with the stick a bunch, but I only just started playing yesterday. Elden Ring is the only particular Souls-like I've dove into, and frankly it IS an easy game if you take your time and actually use all the mechanics available to you. Every fight in the game can be cheesed multiple ways, even Malenia."

**Labels considered:** `comparative` vs `analytical`

**Why it's hard:** The post explicitly compares Black Myth: Wukong to Elden Ring (a comparative framing) and uses the comparison to make a broader argument about Souls-like difficulty. However, it also offers a genuine analytical claim: that Elden Ring's difficulty is overstated because the game gives players multiple mechanical solutions ("Every fight can be cheesed multiple ways"). This is a design-system observation, not just a comparison. The post uses comparison as a vehicle for a deeper analytical point about how game difficulty interacts with player tool diversity.

**Final label:** `comparative` — The comparison is the primary organizing structure. The analytical insight about mechanics serves the comparison rather than standing on its own.

---

#### Example 3: Emotional reaction or critical evaluation?

> **Post g074** — _Resident Evil 4_  
> "holy cow what were they thinking? all previous ada wong sound ok but RE4R omg i feel bad for the actress but you wouldn't pick rob schneider to give voice to kratos or cate blanchett for harley quinn, its a miscast the dissonance is too big."

**Labels considered:** `comparative` vs `critical` vs `impressionistic`

**Why it's hard:** This post is genuinely three things at once. It's comparative (compares the RE4R Ada Wong voice to previous Ada performances and to hypothetical miscasts in other franchises). It's critical (evaluates the casting decision as a failure using multiple criteria: performance quality, casting fit, audience reception). And it's impressionistic ("holy cow," "omg," "i feel bad" — strong emotional reaction language). The hypothetical comparisons (Rob Schneider as Kratos, Cate Blanchett as Harley Quinn) are a rhetorical device for making a critical argument about miscasting.

**Final label:** `comparative` — The post's argument is built on comparisons: across-time (previous Ada vs. RE4R Ada), across-franchise (hypothetical Kratos/Harley Quinn miscasts), and across-criteria (talent vs. casting fit). The comparisons do the structural work of the argument; the emotional language is tone, not structure.

---

### Annotation Guidelines

Detailed annotation guidelines and edge case resolution rules are documented in [`data/taxonomy.md`](data/taxonomy.md). Key principles:

- **Mutually exclusive:** Each post belongs to exactly one label
- **Exhaustive:** The four labels cover ≥90% of forum discourse without requiring a catch-all category
- **Structure over content:** Label based on _how_ the post communicates, not what opinion it expresses
- **Dominant mode:** When a post blends multiple formats, pick the dominant structural mode — the mode that _organizes_ the post, not the one that appears most frequently

## Getting Started

### Prerequisites

- Python 3.8+
- Required packages (see notebook for full list)

### Usage

1. Clone the repository:

   ```bash
   git clone https://github.com/ezhong08/ai201-project3-takemeter
   cd ai201-project3-takemeter
   ```

2. Open the starter notebook:

   ```bash
   jupyter notebook ai201_project3_takemeter_starter_clean.ipynb
   ```

3. The notebook contains code for:
   - Loading and exploring the annotated dataset
   - Fine-tuning a text classification model
   - Evaluating model performance
   - Analyzing where the model succeeds and fails

### Project Structure

```
├── README.md                           # This file
├── planning.md                         # Project planning document
├── ai201_project3_takemeter_starter_clean.ipynb  # Main notebook
├── data/
│   ├── taxonomy.md                     # Label definitions & annotation guidelines
│   ├── all_game.json                   # Full annotated dataset (212 posts)
│   ├── all_game.csv                    # Full dataset in CSV format (text, label, notes)
```

## AI Usage Disclosure

This project used AI tools at three stages, consistent with the AI Tool Plan in [`planning.md`](planning.md#7-ai-tool-plan).

### Pre-Labeling

**Tool:** Deepseek.

**Workflow:** All 212 posts were independently pre-labeled by the LLM using the taxonomy definitions from [`data/taxonomy.md`](data/taxonomy.md) and [`planning.md` §2](planning.md#2-labels). The LLM assigned one label per post based solely on the post text (title + description). Each pre-label was then manually reviewed by a human annotator who read the full post text and either accepted or overrode the pre-label.

**Results:**

| Metric                                | Value                                  |
| ------------------------------------- | -------------------------------------- |
| Pre-labels matching existing labels   | 211 / 212 (99.5%)                      |
| Pre-labels overridden after review    | 1 (g026: `comparative` → `analytical`) |
| Posts flagged as borderline/difficult | 33 / 212 (15.6%)                       |

The one override (g026) was a post that lists cross-game accomplishments but whose dominant structural mode is explaining game systems and defending legitimacy — the comparisons serve an explanatory purpose. Both `comparative` and `analytical` are defensible labels for this post; the human reviewer chose `analytical` as the dominant mode.

**Tracking:** Every row in [`data/all_game.csv`](data/all_game.csv) includes a `notes` column that records the LLM pre-label, the existing label, and any borderline/difficult-case reasoning. The `label` column reflects the final human-reviewed decision.

### Label Stress-Testing

Before annotation began, the LLM was prompted with the taxonomy and edge-case rules to generate synthetic borderline posts (see [`planning.md` §7.1](planning.md#71-label-stress-testing)). The definitions passed stress-testing: all synthetic posts were classifiable using the dominant-mode rule, and no taxonomy changes were needed before annotation.

### Failure Analysis

After model training and evaluation, misclassified test-set posts will be fed to an LLM for pattern analysis (see [`planning.md` §7.3](planning.md#73-failure-analysis)). Results will be appended to this section.

## Model

TakeMeter fine-tunes a pre-trained transformer-based language model on the annotated gaming forum dataset. The model is trained to classify posts into one of four discourse format labels.

### Training Approach

- **Base model:** A pre-trained transformer (e.g., BERT, RoBERTa, or DistilBERT)
- **Task:** Multi-class text classification (4 labels)
- **Input:** Post title + description text
- **Output:** Probability distribution over {critical, impressionistic, comparative, analytical}

### Evaluation

The model is evaluated on:

- **Accuracy:** Overall correct classification rate
- **Per-label F1:** Precision and recall for each label
- **Confusion analysis:** Where does the model confuse which labels, and why?

## Limitations and Future Work

- **Domain specificity:** The model is trained on gaming forum data and may not generalize to other communities (anime, sports, music) without additional fine-tuning
- **Label ambiguity:** Some posts genuinely sit at the boundary between labels (e.g., analytical posts that reach a verdict, or comparative posts that include deep analysis)
- **Temporal drift:** Community discourse norms evolve over time, which may require periodic re-annotation
- **Scale:** 212 annotated posts is modest; more data would improve robustness

## License

This project is for educational purposes as part of the AI 201 course at Codepath.
