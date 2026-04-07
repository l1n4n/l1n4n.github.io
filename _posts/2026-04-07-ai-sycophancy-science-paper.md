---
layout: post
title: "AI sycophancy: what the new Science paper shares with our cross-lingual study"
---

Cheng et al. published a [study in *Science*](https://www.science.org/doi/10.1126/science.aec8352) last week on AI sycophancy. They fed 11 LLMs posts from Reddit's r/AmITheAsshole and found models affirm user behavior 49% more often than the human crowd consensus. A follow-up with 2,400 participants showed that interacting with sycophantic AI made people more entrenched and less willing to apologize, while rating the AI as trustworthy and objective.

We've been working on a related problem. [Our paper](https://arxiv.org/abs/2601.10257) (January 2026, under review at ACL) also uses AITA data and finds the same leniency: 12 of 13 models judge more leniently than the Reddit baseline (Cohen's *d* > 1.6), all 13 on a Chinese moral dilemma dataset. Two independent groups, same data, same finding.

The studies go in different directions from there.

---

## What happens to the user vs. what happens in the model

Cheng et al. ask what happens to the *user*. Sycophancy measurably shifts how people reason about their own conflicts. We didn't study that.

We ask what's going on inside the *model*. Standard evaluation tells you a model behaves differently in English vs. Chinese, but not whether the gap comes from how it reads the input or how it reasons. We tested mismatched conditions (English story + Chinese chain-of-thought, and vice versa) to pull these apart. Reasoning language drives about 2x the behavioral shift that input language does (7.2 pp vs. 3.5 pp, *p* < 0.001).

Through Moral Foundations Theory analysis, the main pattern is calibration drift: models change how harshly they judge, but moral priority rankings stay mostly stable (mean Spearman rho = 0.88). But "mostly" is doing work there. Some models shift which moral dimensions they weigh, and models that look stable on Western data shift on Chinese data.

44% of the models we tested appear stable under English-only evaluation but show hidden context-dependency cross-lingually. Monolingual benchmarking misses this.

---

## Two halves of one problem

The two papers split the problem down the middle. Stanford treats the model as a black box: sycophantic, yes, but that's where their model-side analysis ends. They focus on what that sycophancy does to the person on the other end, and with 2,400 participants they nail it. People who interact with sycophantic AI become more entrenched and less willing to repair relationships.

We open the box. We decompose the sources of leniency, characterize per-model variation, identify which models fail silently across languages. But we stop at the model boundary. No user study (due to resource constraints).

Read together, you get both halves. We show that models aren't just generically lenient -- they reason with specific moral patterns that vary by language and shift in ways monolingual benchmarks can't detect. Stanford shows that whatever the model produces, it sticks.

---

## The gap between them

Cheng et al. show sycophantic AI makes users more self-serving in their *conclusions*. But does it also reshape *how* they reason? We find that LLMs weigh moral dimensions differently from humans, not just in severity but in which considerations matter most. If the model's moral fingerprint transfers to the user, that's a different problem than generic leniency.

The experiment to close the loop: have participants discuss moral dilemmas with chatbots, then evaluate new dilemmas on their own. Measure whether their moral foundation profiles drift toward the chatbot's pattern. The fingerprinting method from our paper works on human judgments too, so this is directly testable.

---

[Our paper: arXiv:2601.10257](https://arxiv.org/abs/2601.10257) (Li, Kang, De Bie), code and datasets included.

[Stanford study: Cheng et al., *Science* 2026](https://www.science.org/doi/10.1126/science.aec8352)
