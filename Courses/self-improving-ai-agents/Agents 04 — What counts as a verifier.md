---
type: unit
course: "[[Self-Improving AI Agents]]"
status: not-started
order: 4
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Grading the answer against grading the working. Four years of research on that distinction, reduced to the handful of results that should change what you build.

About 10 minutes · Lecture 3 · Gives you the ORM and PRM vocabulary everyone uses and few define

Lesson 3 left you with a job, which is to build a selector that contacts reality. The research literature has worked on that job since 2021 and converged on one distinction worth internalising: **where in the trajectory you attach the score**.

<table>
<thead>
<tr>
<th></th>
<th>Outcome reward model (ORM)</th>
<th>Process reward model (PRM)</th>
</tr>
</thead>
<tbody>
<tr>
<th>Scores</th>
<td>The final answer, once</td>
<td>Every intermediate step</td>
</tr>
<tr>
<th>Label needed</th>
<td>Was the final answer right?</td>
<td>Was each step right?</td>
</tr>
<tr>
<th>Label cost</th>
<td>Cheap, and you often have it already</td>
<td>Expensive, and historically annotated by hand</td>
</tr>
<tr>
<th>Catches</th>
<td>Wrong answers</td>
<td>Wrong answers, and right answers reached by bad reasoning</td>
</tr>
<tr>
<th>Usable for</th>
<td>Ranking finished candidates</td>
<td>Ranking candidates, and pruning the search mid-generation</td>
</tr>
</tbody>
</table>

## The founding result, and its size ratio

OpenAI's 2021 GSM8K work introduced both the benchmark and the recipe.[¹](#s1) Fine-tune a generator, sample 100 solutions per problem, label each one by whether its final answer matched the known answer, and train a model to predict that label. At test time you sample and take the highest-scoring candidate. With enough verifier training data, **this beats fine-tuning the generator harder on the same data**. Below roughly a thousand labelled problems it does not, because the verifier has not learned enough to be worth consulting.

Notice what the verifier route preserves. Your base model stays general. Fine-tuning specialises the generator onto one distribution, and a verifier leaves the generator alone and adds a filter you can swap, retrain, or remove. If you maintain several workflows on one model, that difference matters more than the accuracy difference does.

One ablation from that paper deserves to be a rule of thumb. The authors compared two ways of allocating a fixed budget, a large generator with a small verifier against a small generator with a large verifier, and the first won. The intuition is the one you already have, because generating is harder than checking. It inverts a common instinct, which is to make the judge the most expensive model in the pipeline. If you have to choose, spend on the generator and keep the checker cheap.

> [!abstract] The result nobody expects
> Verifier-selected accuracy on GSM8K rose with sample count up to roughly 400 and then declined. It did not plateau, it declined. With enough candidates, some wrong but plausible answer eventually outscores the right one, because you are running an efficient search for the verifier's blind spots. Every verifier has a k beyond which it works against you, and you find that k by measuring rather than by reasoning.

## Why grading the working is better

*Let's Verify Step by Step* compared ORMs and PRMs directly, using a dataset of 800,000 human step-level labels.[²](#s2) Four findings carry over to anything you build.

- **PRMs beat ORMs and beat majority voting.** A PRM can identify correct solutions that appear in under 5 percent of samples, which is the rare-correct case where voting is structurally blind.
- **PRMs remove false positives.** Models reach right answers through wrong reasoning more often than is comfortable. Outcome supervision rewards that, and if you use it to generate training data you are selecting for it.
- **PRMs are more label-efficient** per unit of annotation despite needing more labels per solution, because each step is a separate training signal.
- **PRMs generalise further when the distribution shifts.** Plain majority voting generalises better than an ORM here, because the ORM overfits its training distribution, and the PRM beats both.

There is a cost. A PRM introduces a threshold you have to tune, and it can penalise an unconventional path that happens to be correct. Current practice combines both signals rather than choosing between them.

## Check yourself

You plan to fine-tune on your agent's successful runs. Which verifier type matters most, and why?

- [ ] An outcome model, because final correctness is the only property that your users can observe.
- [ ] A process model, because outcome filtering keeps runs that were right for the wrong reason.
- [ ] A majority vote, because agreement across samples is the cheapest available correctness signal.
- [ ] A larger judge model, because verification quality dominates the value of the training data.

> [!tip]- Reveal the answer
> This is the false-positive problem with real consequences. Outcome-filtered data contains lucky runs, meaning a right answer reached by an incoherent path, and fine-tuning on them teaches the incoherent path. The distortion compounds over rounds of self-improvement, which is the loop Lesson 9 warns about. The fourth answer raises a real consideration on the wrong axis, and the size-ratio result above argues against it.

## Getting step labels without humans

The 800,000-label dataset took an enormous annotation effort, which puts PRMs out of reach for most teams. Math-Shepherd removed that constraint with a method you can implement in an afternoon.[³](#s3) It **defines the quality of a step as its potential to reach a correct final answer**.

![[86ace82b9d58a9bb38691d52c4bf3c40c68c6139.svg]]

*Rollout-based step labelling. You need only final-answer ground truth, which you often already have, plus the compute to complete each candidate step a few times. Math-Shepherd found that around four rollouts per step was enough, and that the simpler hard label performed about as well as the soft one.*

Two failure modes are worth stating, because the lecture's students found them immediately. On genuinely hard problems no rollout succeeds, so every step scores zero and you get no signal at all, which means the method is blind exactly where you need it. And a wrong step that happens to be recoverable gets labelled correct, because the rollouts route around it. More rollouts reduce both problems at linear cost.

Notice the shape here, because it recurs for the rest of the course. You **spend inference compute to manufacture supervision you do not have**. That is the self-improvement loop from Lesson 1 in its most concrete form, and it needs exactly one thing, which is an outcome you can check.

## Translating this to work you actually do

You are unlikely to train a PRM. The transferable ideas are structural.

- **Attach checks to steps rather than only to the end.** An agent running a five-stage data pipeline should be checked after each stage on schema, row count, and key uniqueness, not only on the final chart. This is a PRM made of assertions, it costs nothing, and it turns a silent 40-minute failure into a 30-second one.
- **Watch for right answers reached by wrong reasoning** whenever you use agent output as training data, as examples, or as a template, because outcome checks alone will let it through.
- **Find your verifier's ceiling.** Sweep k, plot selected accuracy, and find where it turns over. Ship below that point.
- **Keep the checker cheap and the generator strong**, which follows from the size-ratio result and saves a lot of money.

One more technique is worth having. When you have a checkable outcome and want to know which intermediate choice went wrong, re-run from step k a few times and see how often the run recovers. That is rollout labelling used for debugging rather than for training, and it localises a failure faster than reading the transcript does.

Project hook

Where is your verifier's turnover point, and what does it look like past that point? Sweep k from 1 to a few hundred on one workflow with a real check attached, plot selected accuracy, and then read the wrong answers that beat the right ones after the peak.

Measure Selected accuracy against k, plus a hand-written taxonomy of what the winning wrong answers exploited. The taxonomy is the actual result, because it tells you which blind spot to close first.

## Retrieval Practice

From memory: name the two things a PRM catches that an ORM does not, and the two things that make PRMs expensive. Then name one step of a workflow you own that is currently unchecked.

> [!example]- Model answer
> A PRM catches a right answer reached by wrong reasoning, which is the false-positive case, and it catches rare correct solutions that appear in a tiny minority of samples, which voting cannot see. It also allows pruning during generation, which an ORM structurally cannot do. The costs are step-level labels, historically human unless you use rollout labelling, and a new threshold to tune, along with the risk of penalising an unusual but valid path. The last part of the prompt is the one to act on this week.

## What you can now do

You can use the terms ORM and PRM correctly, explain why process supervision is worth its cost, generate step labels without annotators, and you know two facts that run against instinct: verifier quality degrades past a critical k, and the generator should be the larger model.

Next is [[Agents 05 — Judges you can trust|Lesson 5, judges you can trust]], for everything with no ground truth at all, which is most of your work.

**Ask about the step-check idea.** Turning existing pipeline assertions into checks the agent can see is the cheapest win in this course, but the design details decide whether it works. What the agent sees on failure determines whether it recovers or thrashes, and that is worth talking through on a real pipeline.

Read one thing

1.  [Cobbe et al., *Training Verifiers to Solve Math Word Problems*](https://arxiv.org/abs/2110.14168) (2021), the origin of GSM8K and of the verifier recipe.
2.  **Read this one:** [Lightman et al., *Let's Verify Step by Step*](https://arxiv.org/abs/2305.20050). It is the clearest statement of why process supervision wins, and the false-positive argument is worth the read on its own.
3.  [Wang et al., *Math-Shepherd: Verify and Reinforce LLMs Step-by-step without Human Annotations*](https://arxiv.org/abs/2312.08935), the rollout labelling method.
4.  [CS329A Lecture 3, Robust Verification](https://www.youtube.com/watch?v=p7TdPUcPoik), the source for this lesson, where the four-paper progression is laid out in order.

[[Agents 03 — Coverage is not accuracy|← Lesson 3]] · [[Agents 05 — Judges you can trust|Lesson 5: Judges you can trust →]]
