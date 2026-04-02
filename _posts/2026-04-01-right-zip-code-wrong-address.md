---
layout: post
title: "Right zip code, wrong address: why you can't just add knowledge to an LLM"
---

I spent the last couple of weeks reading into how people combine knowledge graphs with LLMs. Knowledge graphs store facts as triples like (France, capital, Paris), structured and updatable. LLMs know a lot but hallucinate and go stale. Plenty of work tries to bridge the two: you can convert triples to text and put them in the prompt ([KAPING](https://arxiv.org/abs/2306.04136)), fine-tune with projected KG embeddings ([ConceptFormer](https://arxiv.org/abs/2504.07624), [TEA-GLM](https://arxiv.org/abs/2408.14512)), inject KG entries as extra attention key-value pairs ([KBLAM](https://arxiv.org/abs/2410.10450)), or just edit the model's weights ([ROME](https://arxiv.org/abs/2202.05262), [MEMIT](https://arxiv.org/abs/2210.07229)).

I kept coming back to a simpler question: what if you just take a KG vector for "France," project it into the LLM's activation space, and add it to the hidden state? No fine-tuning, no weight editing, just vector addition on a frozen model.


It doesn't work. We tried 300 configurations on [Pythia-1.4B](https://arxiv.org/abs/2304.01373) with factual fill-in-the-blank questions from [LAMA T-REx](https://arxiv.org/abs/1909.01066) (baseline: 20% Hit@1). Five injection methods, five layers, four strengths, three positions. Every one degraded performance. Best result: ~14%. The KG vectors performed no better than random noise of the same magnitude.

![Injection pipeline](/assets/figures/fig1_injection_pipeline.png)

---

## KG embeddings are noise to the LLM, but the LLM knows the facts anyway

We measured representational similarity ([CKA](https://arxiv.org/abs/1905.00414)) between KG embeddings and Pythia's hidden states. [TransE](https://papers.nips.cc/paper/2013/hash/1cecc7a77928ca8133fa24680a88d2f9-Abstract.html), [RotatE](https://arxiv.org/abs/1902.10197), and sentence embeddings all look identical to random noise at every layer. Three fundamentally different external representations, same result.

![CKA comparison](/assets/figures/fig2_cka_comparison.png)
*Left: all external embeddings overlap with the random baseline; the lines are inseparable. Right: Pythia's inter-layer CKA exceeds 0.95 through layers 4–22, confirming the metric can detect real similarity. KG embeddings aren't slightly misaligned. They're orthogonal.*

The interesting part: the LLM *does* encode relational structure internally. A linear probe on Pythia's hidden states classifies relation type (capital of? native language of?) at 65% on a 15-way task (chance: 6.7%). The facts are there. The geometry is completely different.

---

## Alignment is easy to learn, and useless

We trained an MLP projector: KG vector → Pythia activation space. With 10 examples we hit 0.93 cosine similarity to Pythia's own representations. The alignment problem is trivially solvable. Injecting these aligned vectors still doesn't improve accuracy.

We tracked why. An injected perturbation, whether carefully aligned or random noise, gets amplified (~1.85×) and rotated until its direction is essentially random by the final layer.

![Perturbation tracking](/assets/figures/fig4_perturbation_tracking.png)
*Perturbation magnitude grows (left) and direction randomizes (right) through the network. The aligned vector (blue) and random noise (orange) reach nearly the same endpoint by layer 23. The transformer treats them identically; alignment washes out.*

Alignment buys tolerance, not benefit. The model can handle an aligned perturbation at higher strength before collapsing, but accuracy never rises above the clean baseline. Both aligned and random perturbations end up in the same place by the output layer.

A static KG vector, the same "France" whether you're asking about its capital or its language, can't target the specific internal features the model reads for each prediction. Right zip code, wrong address.

---

*[Part 2]({% post_url 2026-04-02-five-heads-in-layer-12 %}): a learned encoder gets 96% on a frozen LLM, and we trace where in the model the signal goes.*
