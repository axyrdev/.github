# Axyr

**The deterministic AI Change Safety Layer.** *Proof, not vibes.*

Axyr reads an AI-generated change, follows the data, and refuses to call data loss
safe — deterministically, with evidence, before the merge. No LLM sits in the verdict
loop: the same input always yields a byte-identical result, which is what lets a
verdict be trusted enough to block a merge.

We hold ourselves to one rule: **what can't be proven stays `UNKNOWN` — never dressed
up as `SAFE`.** We don't claim "zero false negatives" or "zero false positives."

- 🌐 **Website:** https://axyr.dev
- 📚 **[AI Code Disasters](https://github.com/axyrdev/ai-code-disasters)** — an open,
  sourced reference of documented incidents where AI coding agents broke production,
  each reduced to the property that failed and how to prevent it.

Open-source projects live here. The detection engine is proprietary and runs
server-side — it is not published.
