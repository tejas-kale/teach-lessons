---
type: unit
course: "[[Self-Improving AI Agents]]"
status: not-started
order: 12
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Three benchmarks that ask genuinely different questions, about how long a task is, how good the output is against an expert, and how well-sourced it is. Plus the reliability gap hiding inside the headline numbers.

About 11 minutes · Lecture 8 · The lesson your third stated goal was about

The lecture opens by asking who in the room runs evaluations for their project, and nobody answers. That is the state of practice, and it is why "the agent seems better lately" is the most common performance claim in the field.

The three benchmarks below are worth knowing as **three distinct question types** rather than as leaderboards, because each maps onto a decision you might have to make.

## METR, which asks how long a task is and how reliably it is done

METR's time-horizon work anchors difficulty in a unit you already understand, which is **how long the task takes a skilled human**.[¹](#s1) It uses around 170 tasks spanning one second to eight hours, with completion times recorded from professionals with roughly five years of experience. Fit a curve of model success rate against human task duration and you can read off the duration at which the model succeeds half the time.

The headline is the doubling. It went from about two seconds for GPT-2 in 2019, to minutes around GPT-4, to roughly 59 minutes for the frontier models of early 2025, with a doubling time near seven months. That number is quoted everywhere.

> [!abstract] The number that is not quoted
> Ask for 80 percent reliability instead of 50 percent and the same models drop to roughly 15 minutes. That is a fourfold collapse from the same measurements. The 80 percent curve is also doubling, and it sits far below, so any deployment decision made from the 50 percent number is being made from a horizon four times longer than the one you would actually experience.

![[f70a0ad1d82debaa004a3038398f30f78e9bb162.svg]]

*Both curves double at a similar rate and are separated by a large multiple. The distinction matters because the two describe different products. An agent that succeeds half the time is one you supervise, and an agent that succeeds four times in five is one you can leave alone for a while. Progress on capability does not automatically deliver progress on reliability, and reliability is what decides whether something can be deployed.*

This is Lesson 3's distinction between pass@1 and pass^k wearing different clothes, which is why that pairing is in the eval playbook. Whenever you see an agent capability claim, ask what success threshold it was measured at. A demo is a pass@k result presented as a pass^k product.

## GDPval, which asks whether the output is good enough against a professional

GDPval asks the other question, which is not how long but how good.[²](#s2) It sources real work from professionals with a decade or more of experience across 44 occupations and nine sectors, including brochures, consultation reports, competitive landscapes, audit reconciliations, and itineraries, and scores model output by its **win rate against the expert's work** in blind pairwise comparison. Around 70 percent of tasks require working with reference files and most are multimodal.

Two findings deserve your attention.

**The trend is roughly linear rather than exponential.** Win rates went from around 12 percent in 2024 to the mid-40s for the strongest 2025 models. That is real progress and a very different shape from METR's doubling, which is a useful corrective to extrapolating from "an hour now" to "a week by next year". The two benchmarks measure different things and disagree about the trajectory.

**The failure modes are mundane and familiar.** The dominant category is instruction following, and the specific pattern named in the lecture is worth writing down. Models **promise to consult the reference data and then do not**, substituting something plausible from their own weights. Formatting errors come next. In a breakdown of one strong model, roughly half of the outputs were acceptable but sub-par, around a fifth were genuinely good, and about 29 percent were bad or unacceptable.

## Check yourself

Which finding should most change how you supervise an agent working from your reference files?

- [ ] Models claim to have consulted the reference data but substitute their own recollection.
- [ ] Win rates against professionals have climbed steadily from around 12% to the mid-40s.
- [ ] The reliability horizon is roughly four times shorter than the headline capability horizon.
- [ ] Roughly 70% of the benchmark's tasks require the model to interact with reference files.

> [!tip]- Reveal the answer
> It is the one that stays invisible unless you check. A wrong answer you can catch, and a fabricated answer wearing the sentence "as shown in the attached file" defeats the check you were relying on. The operational consequence is concrete, which is to log which files were actually read and assert that the cited ones appear in that log. The third answer is the most important finding overall, but it changes your deployment decision rather than your supervision of a specific run.

## DeepScholarBench, and three axes for research output

DeepScholarBench evaluates generative research synthesis by asking for the related-work section of a recent paper, and it is refreshed monthly with new arXiv papers, which neatly sidesteps contamination.[³](#s3) Its value here is the **decomposition**, which transfers directly to any research or analysis output.

<table>
<thead>
<tr>
<th>Axis</th>
<th>Asks</th>
<th>Finding</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Knowledge synthesis</strong></td>
<td>Is it coherent, and does it capture the key facts?</td>
<td>Systems write well and consistently miss key facts</td>
</tr>
<tr>
<td><strong>Retrieval quality</strong></td>
<td>Are the sources relevant, important, and comprehensive?</td>
<td>The weakest axis, because foundational sources are routinely missed</td>
</tr>
<tr>
<td><strong>Verifiability</strong></td>
<td>Do the citations actually support the claims?</td>
<td>Varies widely, and fluent synthesis trades off against verifiability</td>
</tr>
</tbody>
</table>

No existing system exceeds about 19 percent overall, so this is a benchmark with enormous headroom. Its most sobering result is that even when handed the correct sources, systems extract only around 50 percent of the key facts. The bottleneck is not only finding things, it is reading them properly.

Notice the failure the third axis catches, which is output that is fluent, coherent, well-cited, and whose citations do not support the claims. Synthesis quality and verifiability trade off against each other, and human reviewers are badly placed to catch it because good prose suppresses scrutiny. If an agent produces written analysis for you, verifiability needs its own check with sampled claims traced back to sources, because reading it will not reveal the problem.

## What to build for yourself

These benchmarks are orientation rather than evidence about your workload. What transfers is their method.

- **Anchor difficulty in human time.** Asking how long a task would take a competent colleague is intuitive, always available, and tells you immediately what to delegate, because the METR data says models are strongest under a few hours and decline sharply beyond that.
- **Always report two reliability thresholds.** What can it do half the time, and what can it do four times in five? The second number is your deployment decision and the first is your research direction.
- **Score against a human baseline rather than a rubric.** "Better than what a colleague would produce" is a judgement you can actually make, and a score out of ten is not.
- **Decompose quality into axes that fail independently.** For analysis, ask whether the synthesis is right, whether the sourcing is complete, and whether the claims trace. A single score averages these into uselessness.

Two habits complete the method. Classify every failure rather than recording that something failed, because both benchmarks derive most of their value from failure-mode analysis and the has the taxonomy. And check the reference-data claim specifically, because asserting that cited files appear in the read log is cheap and catches the field's most common instruction-following failure.

One point is worth carrying back to the buy-or-tune decision from Lesson 9. GDPval found that retry loops helped, because trying n times with self-correction produced meaningful cost and speed improvements over an unaided expert. Test-time compute shows up again as the cheapest available lever, now measured against professional work rather than against benchmarks.

Project hook

How wide is your own reliability gap, and does it narrow with the interventions this course taught? Measure the 50 percent and 80 percent horizons on your task set, apply one intervention such as a verifier ensemble or richer tool errors, and measure both again.

Measure Both horizons before and after, in units of human minutes. A technique that moves the 80 percent horizon is worth far more than one that moves the 50 percent horizon, and almost nobody reports the difference.

## Retrieval Practice

Design the eval you would actually run for one workflow, with the human-time anchor, the two reliability thresholds, the axes, and the one failure mode you most suspect. Be specific enough to start on Monday.

> [!example]- Model answer
> Here is a worked example for an agent drafting weekly analysis notes. Anchor the tasks by how long they take you, binned at 15 minutes, an hour, and half a day. For thresholds, run 25 real tasks five times each and report both "succeeded at least once" and "succeeded all five times", expecting the second to be far worse and treating it as the honest number. Use three binary axes: is the conclusion right, does every figure trace to a query, and are the caveats present. The failure to suspect is the agent claiming to have used the latest data extract while actually using a stale one, which you check by asserting that the read log contains the file the note cites. Setting this up takes a day, and it will tell you more than every benchmark in this lesson combined, because it is about your data.

## What you can now do

You can read an agent capability claim and ask the question that deflates it, which is at what success rate. You have three question types to choose between when designing an eval, a human-time anchor for difficulty, and the specific knowledge that instruction-following failures around reference data are the field's most common and least visible defect.

Next is [[Agents 13 — Long horizons and context|Lesson 13, long horizons and context]], which explains why the reliability gap exists at all.

**This is the lesson to act on.** Building the eval is a day's work and it changes every decision after it. Bring a workflow and we will design the task set, the thresholds, and the grading criteria together, then argue about what the first results mean.

Read one thing

1.  **Read this one:** [Kwa et al., *Measuring AI Ability to Complete Long Tasks*](https://arxiv.org/abs/2503.14499) (METR). Read the comparison between the 50 percent and 80 percent horizons, which is the single most decision-relevant chart in agent evaluation.
2.  [OpenAI, *GDPval*](https://openai.com/index/gdpval/), the task suite and win-rate methodology, with a subset on Hugging Face. Read the failure-mode section.
3.  [Stanford, *DeepScholarBench*](https://arxiv.org/abs/2508.20033), the three-axis decomposition and a rare unsaturated benchmark.
4.  [CS329A Lecture 8, Agentic Evaluations and Long Horizon Tasks](https://www.youtube.com/watch?v=8JAqLnTaZu4) (Tandon), the source for this lesson.

[[Agents 11 — Deep research agents|← Lesson 11]] · [[Agents 13 — Long horizons and context|Lesson 13: Long horizons and context →]]
