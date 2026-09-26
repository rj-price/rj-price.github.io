---
layout: page
title: Fungi Wiki Agent - Grounded RAG
description: A citation-backed mycology assistant over a ~500K-word fungal textbook.
github: https://github.com/rj-price/fungi-wiki-agent
importance: 1
category: software
---

<br>

Fungi Wiki Agent is a question-answering assistant that answers only from a fungal biology textbook (around 500,000 words) and cites the passages it used. It follows the "LLM wiki" pattern: the textbook is turned into a curated, linked index, and the agent navigates that index to find evidence instead of relying on an embeddings search. It runs on local models through Ollama or on Claude.

<br>

## Testing the design

The agent comes with a deterministic test suite of 879 tests. Two results were more interesting than the headline accuracy:

- **Abstention is a model-family trait, not a size trait.** Whether a model correctly says "the book doesn't cover this" depended far more on which family it came from than on its parameter count.
- **Embeddings versus keyword retrieval.** I ran a comparison to check my own decision to skip embeddings. Embeddings win on retrieval alone (Hit@8 1.00 vs 0.89; MRR 0.81 vs 0.58), but the gap almost disappears end to end, because the model condenses the query and the navigation layer recovers from weak rankings.

<br>

Code and results are on [GitHub](https://github.com/rj-price/fungi-wiki-agent).
