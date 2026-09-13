---
type: unit
course: "[[Self-Improving AI Agents]]"
status: not-started
order: 14
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Three bottlenecks the field knows it has, what "non-verifiable" actually means, and a way of reading the next paper that outlasts any particular result.

About 9 minutes · Lecture 9 · The lesson designed to expire slowly, and the source of your project questions

The final lecture organises the open problems around the self-improvement loop you have been building since Lesson 1, which generates candidates, verifies them, and trains or selects on what survives. Each of the three bottlenecks is a place that loop stalls, and each has a version that stalls your workflows at a much smaller scale. Arc V turns one of them into a project, so read this lesson with a pen.

## Bottleneck 1, diversity collapse

Run the loop with one model and it stops paying after a handful of iterations. The diagnosis is a **lack of diversity**. Pre-training data is diverse because thousands of people wrote it over decades, and a single model sampling its own outputs produces variations on one voice. Raising the temperature does not fix this, because it perturbs wording rather than approach.

The proposed remedy is multiple specialised agents, meaning several generators fine-tuned differently plus critic agents, exchanging and summarising each other's answers over rounds of debate.[¹](#s1) Measured by embedding dissimilarity, responses stay diverse across iterations where single-agent fine-tuning collapses, and accuracy keeps climbing.

> [!abstract] The version you can run
> The lecture calls it the poor man's version, and it delivers most of the benefit: use different models. Different families have genuinely different failure modes, so their errors are closer to independent, which is the property Lesson 5 needed for verifier ensembles and Lesson 10 needed for candidate clustering. Diversity of mechanism keeps turning out to be the scarce resource.

## Bottleneck 2, verification again

Verification has been the binding constraint since Lesson 3, and the frontier version is sharper. In theorem proving, models produce invalid proofs and then, asked to check them, confidently declare them valid. LLM-as-judge fails outright here, because the judge shares the generator's blind spots.

DeepSeek-Math-V2 answers by adding a layer, a **meta-verifier** that reviews the verifier's critique.[²](#s2) Do the issues it identified actually exist? Does the score follow from those issues? This catches the failure that makes weak judges dangerous, which is fabricated criticism, meaning a judge inventing plausible-sounding faults. Humans annotate the meta-verification layer at first, and once it learns, the labelling automates. Verifier and generator then improve each other in a loop.

![[6ae03c737916269df620247f3479c14664662e00.svg]]

*The generalisable recipe, as the lecture states it, is to get LLM verifiers to identify issues without reference solutions, add a meta-verification layer to suppress hallucinated issues, and give the generator an incentive to maximise quality rather than merely to pass. This still only applies in domains where verification is tractable at all.*

The practitioner version is cheaper than it sounds. When a judge rejects an agent's output, run a second pass asking whether the criticism is specific and whether it is true. False rejections cost as much as false acceptances and are much harder to notice, because an agent thrashing against an incorrect critique looks exactly like an agent failing at a hard task.

## Bottleneck 3, where the problems come from

The least-discussed constraint is that training data for the loop needs prompts, and those are curated by humans. Improving at olympiad problems needs olympiad experts to write them, and as models approach and pass expert level, that supply becomes the ceiling.

The proposal is a model that **generates its own tasks** alongside solving them, with the proposer rewarded for producing problems of the right difficulty.[³](#s3) There is zero reward if the solver never succeeds, and none for trivial problems either, so it targets the band where the solver sometimes succeeds and sometimes fails. Proposed tasks are validated by execution before entering training, and a buffer of past tasks is sampled from to sustain diversity. Results reached the state of the art on coding benchmarks with no human-curated prompts, and transferred to maths.

You have met that difficulty rule twice already, in DAPO's dynamic sampling in Lesson 9 and in the advice that an eval set you pass every time teaches you nothing. The principle is the same at every scale, which is that **learning happens where outcomes are mixed**.

## Check yourself

Which principle recurs across all three of this lecture's bottlenecks?

- [ ] Larger models absorb synthetic training data better and generalise further from it.
- [ ] Progress needs variation, in approaches, in critiques, and in problem difficulty.
- [ ] Human annotation remains the ultimate bottleneck on every self-improvement loop.
- [ ] Verification is the binding constraint on every domain the loop can be applied to.

> [!tip]- Reveal the answer
> Diversity collapse is variation lost among generators. Meta-verification restores meaningful variation in critiques, because a judge that approves everything or fabricates faults carries no signal. Task proposal manufactures variation in difficulty. A homogeneous system stops learning, whatever the dimension. The first and third answers are true statements from the lecture, and the fourth is the theme of the whole course but does not explain the third bottleneck.

## What "non-verifiable" actually means

Asked which domains resist this machinery, the lecturers separate two cases that call for different responses.

<table>
<thead>
<tr>
<th></th>
<th>Examples</th>
<th>Why it blocks the loop</th>
<th>What to do</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Slow verification</strong></td>
<td>Scientific discovery, chip design simulation, wet-lab chemistry</td>
<td>Ground truth exists but takes days, and RL needs thousands of iterations</td>
<td>Build cheap proxies and validate them against the slow signal periodically</td>
</tr>
<tr>
<td><strong>Genuinely subjective</strong></td>
<td>Creative writing, taste, strategy</td>
<td>No reward function exists, and any modelled one gets gamed</td>
<td>Keep a human in the loop, and use rubrics only for the decomposable parts</td>
</tr>
</tbody>
</table>

Most of your work is the first kind rather than the second. "We cannot evaluate this" usually means evaluating it takes a week and involves three people, which is a proxy-design problem rather than an impossibility. That distinction is worth pressing whenever someone claims their domain is unmeasurable.

The open bet named at the end is worth watching. Push self-improvement hard in verifiable domains and see how far the resulting capability transfers to domains you cannot verify. SWiRL's cross-tool generalisation from Lesson 9 and the task-proposal work's transfer from coding to maths are both early evidence that it does.

## How to read the next paper

The specific results above will age and this will not. For any new agent technique, ask four questions.

- **Which axis is it on?** Train-time, test-time, or the loop between them, from Lesson 1. This tells you immediately whether you can adopt it or must wait to buy it.
- **What is the verifier, and how good is it?** Every method has one even when it is not named, and if the paper is vague here, that is where its results are weakest.
- **Is the reported metric coverage or delivery?** pass@k, majority@k, and pass@1 are different claims, as Lesson 3 showed, and the gap between them is where enthusiasm goes to die.
- **What does it assume about the environment?** Cheap undo, fast feedback, known ground truth. These assumptions decide whether it applies to your work far more than the headline number does.

Two more questions are worth asking once the first four pass. What is the cost frontier, because accuracy at an unstated cost is not a result and you should assume a missing cost is a bad one. And which of the three bottlenecks does it address, because if the answer is none of them, it is worth asking what the technique is really for.

Project hook, and the shortlist for Arc V

All three bottlenecks have cheap practitioner versions, and each is a candidate project. Does using two model families instead of one raise your cluster diversity and your final accuracy? Does a meta-verification pass reduce false rejections in your judge? Does curating your eval set toward the sometimes-passes band make it better at predicting which change helps?

Measure Take the hook you have found most tempting across the last fourteen lessons into Lesson 15, where it gets cut down to something you can finish. The third question above is the cheapest, and the second is the most likely to produce a surprise.

## Retrieval Practice

Retrieval practice, and the closing exercise for Arc IV

> [!example]- Model answer
> If the arc worked, your answer has moved from "it is a prompt and some tools" to something like "it is test-time only, single-sample, and has no verifier, so coverage is probably well above pass@1 and I have never measured it". The change is usually one of three: run the two-number test from Lesson 3, write the standing-context document from Lesson 13, or build the three-verifier ensemble from Lesson 5. All are days of work rather than weeks. The two-number test should come first, because the other two are answers and it tells you which question you have.

## Where this leaves you

You have the map of three axes, the constraint that verification rather than generation binds, the levers in the, the measurement discipline in the, and a reading frame for whatever comes next. That is what a practitioner needs from a research course.

What lessons cannot give you is **wisdom**, meaning what happens when you run this on your data, under your constraints, in front of your colleagues. That comes from doing it and arguing about it with people who also do it. `RESOURCES.md` lists communities worth joining, and the evals discussion in particular gets sharper when other people's numbers are in the room.

Arc V is where you do it. Next is [[Agents 15 — Choosing a question you can finish|Lesson 15, choosing a question you can finish]].

**The taught arc ends and the teaching does not.** The most useful next session is not another lesson, it is running the Lesson 3 diagnostic on a real workflow and working out together what the numbers mean. Bring results and we will decide what to build.

Read one thing

1.  [Subramaniam et al., *Multiagent Finetuning: Self Improvement with Diverse Reasoning Chains*](https://arxiv.org/abs/2501.05707), the diversity-collapse result and its remedy.
2.  [DeepSeek-AI, *DeepSeekMath-V2: Towards Self-Verifiable Mathematical Reasoning*](https://arxiv.org/abs/2511.20835), meta-verification, open-sourced.
3.  [Zhao et al., *Absolute Zero: Reinforced Self-play Reasoning with Zero Data*](https://arxiv.org/abs/2505.03335), the self-proposed curriculum.
4.  **Primary source:** [CS329A Lecture 9, Future Research Areas](https://www.youtube.com/watch?v=AyO6wyu4DEg) (Tandon and Mirhoseini). The closing discussion of verifiable against non-verifiable domains is the most useful ten minutes.

[[Agents 13 — Long horizons and context|← Lesson 13]] · [[Agents 15 — Choosing a question you can finish|Lesson 15: Choosing a question →]]
