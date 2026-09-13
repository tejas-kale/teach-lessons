---
type: unit
course: "[[Self-Improving AI Agents]]"
status: not-started
order: 3
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Your agent probably produced the right answer already and then discarded it. The distance between those two facts has a name, and measuring it takes an afternoon.

About 9 minutes · Lecture 2 · The idea the next four lessons all serve

Lesson 2 assumed something without saying so, which is that after drawing k samples you can pick the good one. Drop that assumption and the picture changes completely.

Coverage, or pass@k, is what your system could deliver with a perfect oracle at the end. What it actually delivers is coverage minus whatever your selection mechanism gets wrong. That subtraction is the *generation-verification gap*, and on most real workloads it is the dominant term.[³](#s3)

![[870e82c0ecba58945bd27c73b0d67cc42f78760a.svg]]

*A schematic of the pattern reported in Lecture 2 across MATH and GSM8K. The selection curves flatten after roughly 10 to 50 samples while coverage keeps climbing. Every sample beyond the plateau costs money and delivers nothing unless you fix the selector, and harder benchmarks widen the gap.*

## Why the obvious selectors plateau

**Majority voting**, also called self-consistency, takes the most common answer.[⁴](#s4) It works well when the model is usually right and occasionally slips, because then the errors are idiosyncratic and the correct answer is the most common one. It fails exactly where you need help. On the hardest problems, Lecture 2 reports correct answers appearing once or twice in ten thousand samples, and a rare correct answer is invisible to a vote by construction. Majority voting suppresses noise, and on hard problems the correct answer is the outlier.

This also explains a failure you have probably seen. An agent becomes more confidently wrong when you ask it to check its work by trying several times, because you have built a machine for reinforcing its most common error.

**Reward-model ranking**, or best-of-n with a learned scorer, does better, because it can in principle recognise a rare correct answer without needing it to be popular. But learned reward models inherit the generator's blind spots, degrade when the input drifts away from their training distribution, and are trained on preferences that only approximate correctness. In the MATH results the gap to true coverage stays wide.

The general lesson is that **selectors which only look at the text are weak**. Both failing methods judge answers by comparing them to each other or to a learned prior, and neither one contacts reality.

## Check yourself

You raise k from 10 to 200 with majority-vote selection and accuracy barely moves. What has most likely happened?

- [ ] The model has reached its capability ceiling and cannot solve the remaining items.
- [ ] The sampling temperature is too low, so the extra draws are near-duplicates.
- [ ] The tasks are too easy, so extra samples have nothing left to contribute here.
- [ ] The selector has saturated, so newly-covered answers are generated and discarded.

> [!tip]- Reveal the answer
> You cannot tell these apart from the accuracy number alone, which is the point. Measure coverage separately, using an oracle or careful manual grading on a small slice. If coverage rose while accuracy stayed flat, the bottleneck is selection and buying a better model would be spending money on the wrong thing. If coverage stayed flat too, then the first or second answer is live. Two numbers, entirely different diagnoses.

## Where verification is easy, and where it is not

The gap closes when you have something that contacts ground truth. Lecture 2 lists the domains where that is cheap, and they share a structure: **checking is a fundamentally easier problem than producing**.

<table>
<thead>
<tr>
<th>Domain</th>
<th>Verifier</th>
<th>Why it is easy</th>
</tr>
</thead>
<tbody>
<tr>
<td>Formal maths</td>
<td>Proof checker such as Lean or Coq</td>
<td>Validity is mechanically decidable</td>
</tr>
<tr>
<td>Code</td>
<td>Unit tests, type checker, compiler</td>
<td>Writing a test costs far less than writing the program</td>
</tr>
<tr>
<td>Kernel and compiler generation</td>
<td>Differential testing, meaning run both and compare outputs</td>
<td>A reference implementation already exists</td>
</tr>
<tr>
<td>Format and language translation</td>
<td>Round-trip and equivalence checks</td>
<td>The source itself is the oracle</td>
</tr>
<tr>
<td>Data pipelines, meaning yours</td>
<td>Schema checks, row counts, invariants, reconciliation against a known total</td>
<td>You already write these and simply do not feed them back to the agent</td>
</tr>
<tr>
<td>Analysis, writing, strategy</td>
<td>None</td>
<td>No oracle exists. This is where the gap lives, and Lessons 5 and 6 are about it.</td>
</tr>
</tbody>
</table>

The fifth row should interest you most. Data work is unusually rich in cheap partial verifiers that practitioners already write and already trust, and then use only as an alarm for humans after the fact rather than as a signal the agent can iterate against. Turning an assertion you already have into a check the agent can see gives you more leverage per hour of effort than anything else in this course.

> [!abstract] Verifiers make mistakes too
> Lecture 2 is candid about this. Unit tests with poor coverage pass bad code, and automated grading of MATH answers was spot-checked by hand at roughly 97 to 98 percent agreement, which is good but not perfect. A weak verifier does not just fail to help. Under heavy sampling it becomes a target, because drawing a thousand samples is an efficient search for whatever your check does not examine. Lesson 5 makes this precise.

## The reframe worth stealing

When an agent underperforms, the instinctive move is to work on the generator with a better prompt, a better model, or more context. The gap says to measure first, because two different problems produce identical symptoms.

- **Low coverage.** Even the best of k attempts is wrong, so this is a real capability problem. You need a better model, better tools, or a better decomposition, which Lesson 8 covers.
- **High coverage and low accuracy.** The answer is in there and you are picking the wrong one. Nothing about the generator needs to change, and you should build a verifier, which Lessons 4 to 7 cover.

The second case is far more common than practitioners assume, and it is also the cheaper one to fix. "The model cannot do this" is a wall, and "my selector is weak" is a task.

Project hook

How wide is the gap across your own task types, and does its width predict which intervention pays? Measure coverage and pass@1 on three different workflows, then spend one week improving the selector on the widest-gap workflow and the narrowest, and see which improvement is larger.

Measure Gap width per workflow, then accuracy gained per hour of work invested. This is the strongest candidate for a full project in Arc V, because the measurement is cheap and the prediction is falsifiable.

## Retrieval Practice

Design the smallest experiment that would tell you which of the two problems one of your own workflows has. Be concrete about what you run, what you record, and what each outcome implies.

> [!example]- Model answer
> Take 20 to 30 real tasks with known-good outcomes and run each one ten times. Record two numbers: how often the first run succeeded, which approximates what you ship today, and how often any of the ten succeeded, which is coverage. If coverage is much higher, you have a selection problem and your next build is a verifier. If coverage is barely above the first number, extra sampling is wasted and you need capability, meaning a stronger model, better tools, or a different decomposition. The whole thing costs a few hundred calls and an afternoon of grading. Almost nobody does it, which is why almost everybody guesses.

## What you can now do

You can name the generation-verification gap, explain why majority voting and reward-model ranking both plateau, and run a two-number diagnostic that tells you whether to invest in the generator or the selector. That diagnostic is the backbone of the evaluation work in Lessons 12 and 13.

Next is [[Agents 04 — What counts as a verifier|Lesson 4, what counts as a verifier]], which separates outcome from process reward models and sharpens the taxonomy.

**Push back on this one.** The gap framing is powerful enough to be over-applied. Some tasks really are limited by capability, and hunting for a verifier there wastes weeks. If you suspect one of yours is, describe it and we will argue it through.

Read one thing

1.  **Primary source:** [CS329A Lecture 2, Test-Time Compute Scaling](https://www.youtube.com/watch?v=-Ggc37xLj_Y), the section contrasting coverage with majority voting and reward-model selection. The gap is stated most plainly there.
2.  [Wang et al., *Self-Consistency Improves Chain of Thought Reasoning*](https://arxiv.org/abs/2203.11171). Read it as the strongest case for majority voting, then note the conditions it needs.
3.  [Brown et al., *Large Language Monkeys*](https://arxiv.org/abs/2407.21787), section 5. The sample sets are published, so you can look at what "correct once in ten thousand" actually looks like.

[[Agents 02 — The dial you already have|← Lesson 2]] · [[Agents 04 — What counts as a verifier|Lesson 4: What counts as a verifier →]]
