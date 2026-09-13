---
type: unit
course: "[[Self-Improving AI Agents]]"
status: not-started
order: 5
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Most of your work has no unit test. The state of the art there is not a better judge. It is several mediocre ones, weighted properly, with the worst ones removed first.

About 11 minutes · Lecture 3 · The most directly usable technique in the course

Formal proofs and unit tests are wonderful and irrelevant to most of what you do. Is this analysis sound? Is this summary faithful to the source? Did the agent pick a sensible modelling approach? None of these has an oracle, and the usual fallback of asking a strong model whether the answer is good gives you a *weak verifier*, meaning one that correlates with correctness and is reliably imperfect.

The Weaver line of work stops trying to fix that.[¹](#s1) It accepts that every available verifier is weak and combines many of them into one strong signal, which spends inference compute on verification rather than on more generation.

## The pipeline: score, weight, select

![[96de43c53fd2d56ddb7b33830040c232cfd15902.svg]]

*Three steps, in order. Filtering first is not optional, because a verifier below a quality floor does not merely add noise, it degrades the ensemble. This is why "ask three models and take the majority" so often disappoints. There is no filtering, no weighting, and no check that the three are looking at different things.*

### Step 1, filter

With even a small labelled set you can measure each verifier's agreement with truth and remove the ones that are barely better than chance on your data. A verifier that is excellent on a public benchmark may be useless on your domain, so measure on your own tasks.

### Step 2, weight

Naive averaging helps, but not reliably, and adding verifiers can make it worse. Learned weights help consistently. If you have labels, logistic regression over verifier scores is enough. If you do not, weak supervision in the Snorkel tradition estimates each verifier's accuracy from the structure of their agreements and disagreements, with no labels required.[²](#s2) Weaver reports strong results using around 1 percent of the training labels.

Weak supervision assumes verifiers make **independent** errors. That assumption fails when your ensemble is one model prompted three ways, because those errors are almost perfectly correlated and you gain almost nothing. Diversity of mechanism is what buys signal. A rubric judge, a tool check, and a trained reward model disagree in different directions, and two prompts of the same model do not.

### Step 3, select

Take the weighted-best candidate. The reported effect is large. On hard benchmarks such as GPQA-Diamond and MMLU-Pro, accuracy moved from roughly 40 percent to over 70 percent. A Llama-3.1-8B generator with a pool of verifiers of 8B parameters or smaller reached about what a 70B model reaches under majority voting, and the 70B-class version approached a frontier reasoning model. These are end-to-end accuracies rather than coverage.[¹](#s1)

> [!abstract] The cost problem, and its answer
> Running twenty verifiers over a hundred candidates is expensive. Weaver's answer is distillation. Train the ensemble once, then compress it into a single small model. A distilled verifier of roughly 400M parameters kept about 97 percent of the pool's accuracy at a fraction of a percent of its inference cost. If you build a good judging ensemble, you can eventually stop paying for it.

## Check yourself

Your judging ensemble is three prompts of the same frontier model. Why does it underperform its promise?

- [ ] Three verifiers is too few, so the estimated weights are noisy and unstable.
- [ ] The model is too capable, so it scores every plausible candidate near the ceiling.
- [ ] Their errors correlate, so extra verifiers add cost without adding new information.
- [ ] Prompted judges cannot output the calibrated probabilities that weighting requires.

> [!tip]- Reveal the answer
> Independence is the load-bearing assumption. Three prompts of one model share training data, blind spots, and stylistic preferences, so when the model is wrong all three are wrong together and you have paid three times for one opinion. The fix is diversity of mechanism, meaning a different model family, a rubric that examines a different property, and at least one check that is not an LLM. The fourth answer describes a real nuisance, but it is not the reason here.

## Known biases of LLM judges

Before you deploy any of this, know how the individual pieces fail. These are established results, mostly from the MT-Bench work, and they are not subtle.[³](#s3)

<table>
<thead>
<tr>
<th>Bias</th>
<th>What happens</th>
<th>Cheap mitigation</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Position</strong></td>
<td>In a pairwise comparison the first answer shown wins more often than it should</td>
<td>Evaluate both orderings and keep only the consistent verdicts</td>
</tr>
<tr>
<td><strong>Verbosity</strong></td>
<td>Longer answers score higher regardless of quality</td>
<td>Constrain length, and score specific claims rather than overall impression</td>
</tr>
<tr>
<td><strong>Self-preference</strong></td>
<td>Models favour text from their own family</td>
<td>Judge with a different family from the generator</td>
</tr>
<tr>
<td><strong>Style over substance</strong></td>
<td>Confident, well-formatted and wrong beats hesitant and right</td>
<td>Make the judge cite evidence before it scores</td>
</tr>
<tr>
<td><strong>Poor calibration</strong></td>
<td>Scores cluster at 4 and 5 out of 5, so the scale carries no information</td>
<td>Use binary criteria, meaning a checklist of yes-or-no items that you then sum</td>
</tr>
</tbody>
</table>

The last row quietly ruins most homegrown eval suites. "Rate this analysis from 1 to 10" produces a number with almost no variance and no meaning. "Does it state its assumptions? Does every figure trace to the source data? Does it name a limitation?" produces three facts you can act on. **Decompose the judgement until each part is nearly objective**, which is the practical residue of the PRM idea from Lesson 4.

A caution from the lecture: multi-agent verification by rubric prompting alone sometimes performed worse than plain majority voting. A rubric is not automatically an improvement, and it has to be validated against labels like anything else.

## Build one this week

Here is the smallest version that works, for one workflow you own.

1.  **Label 30 to 50 real outputs yourself, good or bad.** This is the whole cost, and there is no substitute for it, because everything downstream is calibrated against these labels.
2.  **Write three or four independent checks.** Aim for real diversity: one deterministic check such as a schema, a total, or a recomputation, one rubric checklist from a different model family, and one that inspects the reasoning rather than the answer.
3.  **Score your labelled set with each check.** Now you know each one's precision and recall, and you can drop anything near chance.
4.  **Fit weights.** Logistic regression on 50 points is entirely adequate and beats your intuition about which check to trust.
5.  **Re-measure the gap.** Rerun the Lesson 3 diagnostic. Selected accuracy should have moved toward coverage, and if it did not, your checks are correlated with each other or with the generator's errors.

That is an afternoon of work, and it converts "the agent seems better lately" into a number. It also gives you a component you reuse three more times, because the same ensemble is your eval harness in Lesson 12, your selector in Lesson 6, and your training-data filter in Lesson 9.

Project hook

How much of the ensemble's benefit comes from weighting rather than from simply having more checks? Build the pool, then compare three selection rules on the same candidates: unweighted majority, learned weights from labels, and weak-supervision weights with no labels.

Measure Selected accuracy under each rule, plus the pairwise error correlation between your verifiers. The correlation number explains the accuracy number, and reporting both is what makes this a result rather than an anecdote.

## Retrieval Practice

Sketch a three-verifier ensemble for a workflow you run, with one deterministic, one rubric, and one reasoning-focused. For each one, name the failure it would miss.

> [!example]- Model answer
> Here is a worked example for agent-written SQL analysis. The deterministic check confirms the query runs, row counts fall within expected bounds, and totals reconcile to a known aggregate, and it misses a correctly executing query that answers the wrong question. The rubric judge asks whether the join keys are right, whether the date filter is stated, and whether nulls are handled, and it misses subtle errors in business logic it has no context for. The reasoning check has a different model read the stated intent alongside the query and report mismatches, and it misses anything that requires looking at the actual data. All three miss the case where the underlying table is stale, which tells you the fourth check to write. That is what this exercise is for.

## What you can now do

You can build a judge you can defend. Filter, weight, then select. Insist on independence of mechanism. Prefer binary criteria to rating scales. Design against a named list of biases. And you know the escape hatch when the ensemble gets expensive, which is to distil it.

Next is [[Agents 06 — Inference architectures|Lesson 6, inference architectures]], where the pieces from Lessons 2 to 5 get assembled, along with one powerful operation you have not met yet.

**Bring your rubric.** Judge prompts fail in specific and fixable ways, and a second pair of eyes on the wording is worth more than another paper. Paste one in and we will take it apart against the bias table.

Read one thing

1.  **Primary source:** [CS329A Lecture 3, Robust Verification](https://www.youtube.com/watch?v=p7TdPUcPoik), final section, presenting *Weaver: shrinking the generation-verification gap with weak verifiers* from Mirhoseini's lab, NeurIPS 2025. The model and distilled-verifier checkpoints are open source.
2.  [Ratner et al., *Snorkel: Rapid Training Data Creation with Weak Supervision*](https://arxiv.org/abs/1711.10160), the label-model machinery Weaver borrows. You may know it already, and the reframing onto verifiers is what is new.
3.  [Zheng et al., *Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena*](https://arxiv.org/abs/2306.05685). Read section 3 on biases and treat it as a design checklist rather than as a paper.

[[Agents 04 — What counts as a verifier|← Lesson 4]] · [[Agents 06 — Inference architectures|Lesson 6: Inference architectures →]]
