---
type: unit
course: "[[Self-Improving AI Agents]]"
status: not-started
order: 2
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Sampling the same model repeatedly turns a weak model into a strong one, and it does so predictably enough to budget for. Here are the two ways to spend inference compute and how to choose between them.

About 10 minutes · Lecture 2 · Answers: should I upgrade the model or the harness?

Here is the result that reorganised the field. Take Llama 3 8B, which is small and open and unimpressive. Ask it the same hard problem 250 times instead of once, then use a verifier to pick the correct answer out of the pile. On several math and coding benchmarks that arrangement beats a single call to a much larger frontier model.[¹](#s1)

Nothing about the small model changed. There was no fine-tuning and no better prompt. The correct answer was already in its output distribution, and sampling once threw it away.

> [!abstract] The reframe
> Stop treating a model as a function that returns an answer. Treat it as a distribution you can draw from, and treat your harness as the thing that decides how many draws to take and which one to keep. Model quality sets the distribution, and you set everything else.

## Coverage has a scaling law

*Coverage*, usually written pass@k, is the fraction of problems solved by at least one of k samples. The Large Language Monkeys paper found that coverage grows with k as an approximate power law, and that the fit holds across model families and across three orders of magnitude of parameter count, from 70M to 70B.[¹](#s1) On SWE-bench, DeepSeek-V3 with a thousand samples solved more issues than single calls to the frontier models of the day.

Why a power law, when the per-problem arithmetic is exponential? For one problem with per-sample success probability *p*, pass@k is 1−(1−p)<sup>k</sup>, which saturates quickly. Aggregating across a benchmark only produces a power law if the distribution of *p* across problems has a long tail of very hard items, and empirically it does. That tail is why sampling keeps paying off long after you would expect it to stop.

Two consequences matter to you more than the curve does.

- **You can budget in advance.** Fit the curve on a slice of your own workload and you can answer "how many samples do I need to reach 90 percent?" with a number instead of a guess.
- **The gains come from the tail.** Easy problems are solved on the first try and extra samples buy nothing there. The value is concentrated in the hard items, which is where per-sample cost is highest, so sampling uniformly across your workload wastes most of the money.

## Two ways to spend: parallel and sequential

Repeated sampling is only one shape. The other is *sequential revision*, where the model drafts an answer, looks at it again, and improves it over several turns. Snell and colleagues studied both, and more usefully, studied when each one wins.[²](#s2)

![[a886b44f78e833c108ef558a2ee672b3d8fa69fd.svg]]

*The trade is exploration against refinement. Parallel draws are independent, so they explore different approaches and parallelise well, and fifty samples cost close to one sample in wall-clock time. Sequential revision keeps one approach and polishes it, which costs fewer tokens but runs in series, and it only works if each revision sees real feedback.*

### Which one to reach for

Snell and colleagues binned problems by difficulty and found a clean rule. **Easier problems favour sequential revision**, because a roughly right draft is close to a correct one and needs polishing rather than replacing. **Harder problems favour parallel exploration**, because the first approach is probably the wrong approach, and revising a wrong plan does not produce a right one.[²](#s2) The best mix at the very hardest end is genuinely unresolved, so treat anyone who states it confidently with suspicion.

"Difficulty" here is measured the only way available to you. Run pass@1 on a sample of your own tasks and bin by the result. You do not need a theory of difficulty, you need a histogram. This is the cheapest useful measurement in the course, and Lesson 12 builds on it.

## Check yourself

Your agent writes an analysis that is structurally right but keeps making small errors in the final numbers. Where should the extra inference budget go?

- [ ] Sequential revision, since the chosen approach is sound and only needs correcting.
- [ ] Parallel sampling, since drawing more attempts raises the coverage of correct answers.
- [ ] A larger frontier model, since small numerical errors indicate weak model capability.
- [ ] A longer system prompt, since the errors show the instructions are underspecified.

> [!tip]- Reveal the answer
> Structurally right and locally wrong is the signature of an easy or medium problem, where the draft is already near the answer. Revision is the efficient spend, ideally with a tool that can check the arithmetic, which Lesson 7 covers. Parallel sampling would buy you fifty analyses with fifty different small errors and no way to tell them apart.

## The honest limit, where a bigger model still wins

Test-time compute does not substitute for capability. Snell and colleagues compared spending tokens at inference against spending them on pre-training, and the answer flips with difficulty. For easy and medium problems, extra inference compute beats a bigger model. For the hardest problems, the larger and more heavily pre-trained model still wins even at generous inference budgets.[²](#s2) Mirhoseini reports the same pattern from her lab, where small open models keep getting more useful as you apply more test-time compute, but the hard tail still belongs to the frontier.

There is an asymmetry worth naming. Pre-training is paid once by someone with a datacentre, and inference is paid every time by you. Most people cannot choose to pre-train, and almost everyone can choose to sample five times, which is why this axis is the practical one even where it is not the theoretically better one.

> [!abstract] The catch, which is the next two lessons
> All of this assumes you can identify the good sample. Coverage is what your model could achieve with a perfect verifier. What you actually ship is coverage minus your selection error, and that subtraction is usually brutal. Lesson 3 measures it.

## Turn the dial this week

<table>
<thead>
<tr>
<th>Dial</th>
<th>How to turn it</th>
<th>What it costs</th>
</tr>
</thead>
<tbody>
<tr>
<td>k, the sample count</td>
<td>Run the same task 5 to 10 times at temperature 0.7 to 1.0 and measure how often any run succeeds</td>
<td>Tokens, linearly. It parallelises, so latency barely moves.</td>
</tr>
<tr>
<td>Revision turns</td>
<td>Add a pass that says "check your work against X, then produce a final answer"</td>
<td>Latency, in series. Nearly worthless unless X carries real feedback.</td>
</tr>
<tr>
<td>Thinking budget</td>
<td>Raise the reasoning-effort or thinking-tokens setting on a reasoning model</td>
<td>The laziest version of this lesson, and often the best first try.</td>
</tr>
<tr>
<td>Difficulty routing</td>
<td>Make a cheap single call first, and escalate to sampling only when a check fails</td>
<td>Engineering time. Usually the largest cost saving available.</td>
</tr>
</tbody>
</table>

One measurement makes all of this real. For a workflow you own, run each task ten times and record how often at least one run succeeded against how often the first run succeeded. The gap between those two numbers is your available headroom, and it is almost always wider than people expect.

Project hook

Does the difficulty rule hold on your workload? Bin your own tasks by pass@1, then spend an identical token budget two ways on each bin, once as parallel samples and once as sequential revisions. The published rule predicts sequential wins in the easy bin and parallel wins in the hard bin.

Measure Success rate per bin at matched token cost. This is a clean replication with a control, it runs on one workflow, and a result that contradicts the paper is more interesting than one that confirms it.

## Retrieval Practice

From memory: why does coverage across a benchmark follow a power law when the per-problem arithmetic is exponential, and what does that tell you about where your sampling budget goes?

> [!example]- Model answer
> Per problem, pass@k is 1−(1−p)k, which saturates fast. A power law over the whole benchmark only appears if p has a long tail toward zero, meaning many easy items and a heavy tail of very hard ones. In practice the first sample resolves the easy mass, and every sample after that is spent almost entirely on the hard tail. That is an argument for routing, where you detect the hard cases and spend there, rather than for raising k across the board.

## What you can now do

You can state the difference between parallel and sequential test-time compute, choose between them from the shape of your failures, and give a number for the headroom sampling would buy on your own workload. You also know the limit, which is that this dial does not manufacture capability the model lacks, and on the hardest tail a better model still wins.

Next is [[Agents 03 — Coverage is not accuracy|Lesson 3, coverage is not accuracy]], where the headroom you just measured meets the reason most people never collect it.

**Bring a workload.** Tell your teacher what one of your agent tasks looks like and we can work out what k is worth trying, whether revision or sampling fits your failure shape, and what the cheapest possible verifier would be.

Read one thing

1.  **Start here:** [Brown et al., *Large Language Monkeys: Scaling Inference Compute with Repeated Sampling*](https://arxiv.org/abs/2407.21787). Read section 3 and the coverage figures. It is short and empirical, and it is the paper the rest of this arc argues with.
2.  [Snell et al., *Scaling LLM Test-Time Compute Optimally Can Be More Effective Than Scaling Model Parameters*](https://arxiv.org/abs/2408.03314), the source of the difficulty-binned comparison.
3.  [CS329A Lecture 2, Test-Time Compute Scaling](https://www.youtube.com/watch?v=-Ggc37xLj_Y) (Mirhoseini), the source for this lesson including the KernelBench and SWE-bench results.

[[Agents 01 — Where the returns moved|← Lesson 1]] · [[Agents 03 — Coverage is not accuracy|Lesson 3: Coverage is not accuracy →]]
