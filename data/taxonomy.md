# Podcast Format Taxonomy

Use these four labels when annotating game reviews. Each label captures the structural format of the review — not the genre of game, review score, or writing style.

---

## Labels

### `critical`

A formal review that evaluates a game across multiple dimensions: gameplay, graphics, sound, story, technical performance, and overall value. The reviewer adopts a critical distance, weighing pros and cons systematically.

**Key signal:** Structured evaluation with clear criteria. The reviewer is assessing, not just describing or experiencing.

**Examples:** Traditional review scores, "buy/rent/skip" recommendations, launch day reviews, Game of the Year considerations, technical analyses.

---

### `impressionistic`

A review focused on the reviewer's personal experience and emotional response to the game. Less structured than critical reviews. Emphasizes how the game felt to play, moments that stood out, and the subjective journey.

**Key signal:** First-person perspective emphasizing experience over evaluation. "I felt," "I experienced," "for me." The review is more memoir than verdict.

**Examples:** Personal retrospectives, "my time with" essays, emotional response pieces, reviews that prioritize atmosphere over analysis.

---

### `comparative`

A review that primarily evaluates a game by comparing it to other games — either within a franchise, genre, or against a specific benchmark. The game is understood through its relationships to other titles.

**Key signal:** Heavy use of comparison language. "If you liked X, you'll like Y," "this is Dark Souls meets Stardew Valley," "better/worse than the previous entry.".

**Examples:** Franchise comparisons, "spiritual successor" analyses, "which game should you buy" breakdowns, genre-defining works discussed through lineage.

---

### `analytical`

A deep dive into a game's systems, mechanics, design philosophy, or cultural context. The review goes beyond evaluation to understand how the game works, why it was designed that way, and what it means. Often draws on game design theory, history, or cultural criticism.

**Key signal:** The review explains how and why the game functions, not just whether it's good. Uses design terminology, mechanical analysis, or contextual framing.

**Examples:** Design post-mortems, cultural critiques, mechanical breakdowns, "what this game says about" essays, historical contextualization.

---

## How to Handle Ambiguous Cases

Some reviews genuinely fit more than one label. That's intentional — it mirrors the real challenge of building labeled training data. Here's how to resolve common edge cases:

**Critical review with strong personal voice:** If the reviewer still systematically evaluates across criteria (even with personality), label it critical. Structure matters more than tone.

**Impressions that include score:** If there's a score but the body is primarily experiential, consider impressionistic — but if the structure is systematically evaluative, choose critical.

**Comparative review that also analyzes:** If the comparison is the primary framing device but includes deep design analysis, label it comparative. If the analysis is deeper than the comparison, choose analytical.

**Analytical piece that reaches a verdict:** Many analytical reviews still render a judgment. The question is: is the piece organized around evaluation, or around understanding? If analysis comes first and evaluation is secondary, label it analytical.

**First-impressions vs. full reviews:** A "I played the first two hours" piece that still evaluates systematically should be critical. A "here's what it felt like to play the demo" piece that doesn't evaluate across dimensions is impressionistic.

**Review that's all three:** Some reviews weave comparison, analysis, and critical evaluation together seamlessly. Pick the dominant structural mode — the one that organizes the piece. Ask: if you had to summarize the review's approach in one word, would it be "judging," "experiencing," "comparing," or "understanding"?

**When you're genuinely unsure:** Pick the label that fits the structure you'd expect if you were reading, not the label that fits the marketing language or genre tags. Review descriptions are often written to sound comprehensive or exciting — they're not trying to be structurally accurate.

---

## Valid Labels (for reference)

```
critical
impressionistic
comparative
analytical
```

These are the only four valid labels. Use exactly these strings in your labels file.
