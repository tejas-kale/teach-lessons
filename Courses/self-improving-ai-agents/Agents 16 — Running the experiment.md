---
type: unit
course: "[[Self-Improving AI Agents]]"
status: not-started
order: 16
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

The control condition is the whole experiment. Here is how to build one, and the four ways a small agent experiment quietly lies to you.

About 11 minutes · Produces a result you can defend

You have a question, a prediction, and a falsifier. This lesson is about the part where you collect evidence without fooling yourself, which is harder with agents than with most systems because the thing you are measuring is stochastic, expensive, and eager to please.

## The control is the experiment

An agent experiment without a control produces a number that means nothing, because you have no idea what the number would have been anyway. The control is the same everything, minus the one thing you are testing.

Getting that "same everything" right is most of the work. Four things must be held constant between your A and B conditions, and each one has bitten someone.

- **The task set.** The identical tasks in both conditions, in the same order, with no additions after you have seen early results.
- **The model and its settings.** The same model version, temperature, and thinking budget. A provider silently updating a model mid-experiment is a real hazard, so record the exact model string with every run.
- **The grader.** The same grading procedure applied to both conditions, ideally without knowing which condition it is grading.
- **The token budget,** where the intervention could plausibly change it. If your treatment gets more compute as a side effect, you have measured compute rather than your idea.

That last point catches more experiments than the others combined. Comparing "one sample" against "five samples and pick the best" is not a test of your selection method, it is a test of whether five is more than one. If you want to know whether your selector is any good, compare it against a random pick from the same five candidates. Lesson 6's chart is built exactly this way, which is why it is convincing.

## Four ways a small experiment lies

<table>
<thead>
<tr>
<th>The lie</th>
<th>How it happens</th>
<th>The defence</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Noise looks like signal</strong></td>
<td>30 tasks, a 4-point difference, and a rerun would move it more than that</td>
<td>Run each condition several times and report the spread, not just the mean</td>
</tr>
<tr>
<td><strong>The grader has an opinion</strong></td>
<td>An LLM judge prefers the longer, more confident output that your treatment happens to produce</td>
<td>Grade blind, and check the judge against your own labels on a sample</td>
</tr>
<tr>
<td><strong>The tasks were chosen after the fact</strong></td>
<td>You added three tasks because the first set "was not representative"</td>
<td>Fix the task set in the pre-registration and never touch it again</td>
</tr>
<tr>
<td><strong>You stopped when it looked good</strong></td>
<td>Checking results as they arrive and stopping at the favourable moment</td>
<td>The stopping rule from Lesson 15, decided in advance and honoured</td>
</tr>
</tbody>
</table>

The first one deserves a number. With 30 tasks and a success rate near 50 percent, the standard error on your estimate is around 9 percentage points, so a 5-point difference between conditions is well inside the noise. You have three ways out, and they are all cheap: use more tasks, run each task several times so you are averaging over sampling noise as well, or pick an intervention large enough that the effect exceeds the noise. The third is why the tool-error hook from Lesson 7 is a good first project, because the effect is usually large.

> [!abstract] Paired comparison, which you should almost always use
> Run both conditions on the same tasks and compare per task, rather than comparing two averages. Task difficulty varies enormously and it is the largest source of variance in your measurement. Pairing removes it, because you are asking how often B beats A on the same problem rather than whether B's mean is higher. This routinely turns an ambiguous result into a clear one at no extra cost.

## The run itself

1.  **Build the harness on three tasks first.** Get one end-to-end pass working on a tiny slice before you scale, because every bug you find here costs minutes rather than a full run.
2.  **Log everything, in a form you can re-analyse.** One row per run, with the task id, condition, model string, timestamp, the full output, the grade, and the token counts. You will want to ask a question you did not anticipate, and only the log can answer it.
3.  **Run a null experiment.** Run condition A against condition A, with nothing changed. Any difference you see is your noise floor, and any real effect has to beat it. This costs one extra run and is the single most convincing thing in a small writeup.
4.  **Then run the real thing,** to the stopping rule you wrote down, without looking at aggregate results until you are finished.

The null experiment in step three is worth insisting on. It is the closest thing to a free lunch in experimental design, it takes one afternoon, and it converts "B scored 4 points higher" into either "and the noise floor is 9 points, so this is nothing" or "and the noise floor is 1 point, so this is real". Almost nobody does it.

## Check yourself

Your treatment beats the control by 6 points on 40 tasks. What is the most useful next step?

- [ ] Write it up, since a 6-point improvement on 40 real tasks is a substantial practical gain.
- [ ] Run both conditions again unchanged, to find out whether 6 points exceeds the noise floor.
- [ ] Extend the task set to 200 tasks, so the estimate becomes considerably more precise.
- [ ] Try a stronger variant of the treatment, since the direction of the effect is now established.

> [!tip]- Reveal the answer
> You do not yet know whether 6 points is anything. A repeat run costs the same as the original and answers the only question that matters. The third answer is not wrong but it is five times the cost for the same information, and the fourth builds on a foundation you have not tested. Note that a repeat run also catches the ugliest failure, which is a bug that made your two conditions differ in some way you did not intend.

## When the result is null

Most first experiments produce no effect, and this is normal rather than a failure. What separates a useful null from a wasted month is whether you can tell the difference between three cases.

- **The intervention does nothing here.** This is a real finding, it is worth writing down, and it stops you and your colleagues from repeating the attempt.
- **The experiment could not have detected it.** Your noise floor was larger than any plausible effect. That is a design problem rather than a finding, and the null experiment tells you which one you are in.
- **Something was broken.** The treatment did not actually apply, the grader was miscalibrated, or the conditions differed in an unintended way. Read ten runs from each condition by hand before concluding anything, every time.

Reading raw runs by hand is not optional and it is where most real findings come from. The aggregate tells you whether something moved, and only the transcripts tell you why. Budget an hour for it before you look at any summary statistic, and expect to find at least one thing that changes how you read the numbers.

Do this now

Build your harness on three tasks, then run the null experiment before the real one. Record your noise floor as a number in the pre-registration file, next to the prediction you made in Lesson 15.

Measure The spread of condition A against itself. If your predicted effect size from Lesson 15 is smaller than this number, stop and redesign now, because the experiment as planned cannot answer your question.

## Retrieval Practice

Write down your control condition in one sentence, then list what is held constant between it and the treatment. Then name the most likely way your two conditions differ that you did not intend.

> [!example]- Model answer
> A worked example for the tool-error project: the control is the same agent, model, temperature, task set, and grader, with the bare error string that the tool returns today, and the treatment changes only that string. What differs unintentionally is that richer errors are longer, so the treatment condition uses more context and possibly more turns, which means you may be measuring context length rather than error quality. The fix is to report token counts alongside success, and ideally to add a third condition with an error message that is equally long but uninformative, such as padded with irrelevant schema. That third condition turns a suggestive result into a convincing one.

## What you can now do

You can build a controlled comparison, hold the four things constant that matter, measure your own noise floor before trusting any difference, and tell an uninformative null apart from an informative one. That is the whole of small-scale experimental practice, and it is most of what separates a result from an anecdote.

Next is [[Agents 17 — Writing up what you found|Lesson 17, writing up what you found]], which is shorter than this one and matters more than it sounds.

**Send the log, not the summary.** When results come in, the interesting conversation is about the individual runs where the conditions disagreed. Those are where the mechanism shows itself, and they are much easier to read with two people.

Read one thing

1.  **Read this one:** [Brown et al., *Large Language Monkeys*](https://arxiv.org/abs/2407.21787), read this time for its experimental design rather than its finding. Note how carefully it separates coverage from selection, which is the paired-comparison discipline in practice.
2.  [Snell et al., *Scaling LLM Test-Time Compute*](https://arxiv.org/abs/2408.03314), a model of matched-budget comparison. The difficulty binning is what makes its claims survive scrutiny.
3. in this workspace, whose metric definitions and failure taxonomy you will need while grading.

[[Agents 15 — Choosing a question you can finish|← Lesson 15]] · [[Agents 17 — Writing up what you found|Lesson 17: Writing up →]]
