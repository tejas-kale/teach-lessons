---
type: unit
course: "[[Self-Improving AI Agents]]"
status: not-started
order: 6
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Generate, critique, rank, fuse. Four operations you can stack into layers, and an ensemble of open models that beat the frontier at pass@1 by doing so.

About 10 minutes · Lecture 2 · The design pattern to copy today

The pieces have arrived separately so far. Sample more, from Lesson 2, and select better, from Lessons 3 to 5. The Archon work treats their combination as an architecture design problem, meaning a small set of operations arranged in layers and searched over for a given budget.[¹](#s1) It is the most immediately copyable idea in the course, and it contains one real surprise.

## Four operations

<table>
<thead>
<tr>
<th>Operation</th>
<th>Signature</th>
<th>What it does</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Generate</strong></td>
<td>prompt to n candidates</td>
<td>Repeated sampling, optionally across several different models</td>
</tr>
<tr>
<td><strong>Critic</strong></td>
<td>candidate to written critique</td>
<td>States strengths and weaknesses, which later steps read</td>
</tr>
<tr>
<td><strong>Rank</strong></td>
<td>n candidates to an ordered list</td>
<td>Prompted ordering by quality, which is cheap and better than random</td>
</tr>
<tr>
<td><strong>Fuse</strong></td>
<td>k candidates to 1 answer</td>
<td>Show a model all k answers and ask it to write one</td>
</tr>
</tbody>
</table>

Three of those are familiar, and the fourth is the surprise.

## Fusion beats oracle selection

There are four ways to turn five candidate answers into one, and you would probably rank them in this order. Pick one at random. Ask a model to rank them and take the top. Use an oracle to pick the genuinely best one. Or fuse, meaning hand all five to a model and ask it to write a single answer informed by all of them.

Ranking beats random and the oracle beats ranking, as you would expect. Fusion beats the oracle.[¹](#s1)

![[b51a7c65ae718baf58d9d1bd27cd2d58cdd44ef0.svg]]

*A schematic of the pattern reported in Archon. An oracle is bounded by the best answer present in the pool, and fusion is not, because it can take the setup from one candidate, the correction from a second, and the caveat from a third. Filtering first, by ranking and keeping the top few before fusing, did better still.*

This should reorganise how you think about multiple samples. Selection is a lossy operation, because you generated five answers' worth of information and threw four fifths of it away. Fusion is not lossy, because it reads everything. It also sidesteps the hardest problem in Lesson 3, since the fuser never has to identify which candidate is correct. It only has to write a good answer while aware of all the attempts.

This is why "try again, and here is your previous attempt" often beats "sample three times and pick one" in practice. You were doing crude fusion already. The lesson is to do it deliberately, by keeping the candidates, showing them all, and asking explicitly for synthesis rather than for a choice.

## Depth helps

The second finding is that these operations **stack**. Generate, critique, rank, and fuse, then critique, rank, and fuse again, then fuse once more at the end. More layers gave better accuracy, in a way the authors compare to depth in a neural network. The best configurations mixed different models in the generation layer so that the pool contained genuinely different failure modes, which is the independence argument from Lesson 5 again.

Two ordering constraints were worth enforcing. A critic goes before a ranker or a fuser, because the critique is the input that makes those steps better. And a unit-test generator is always followed by an evaluator. The search over configurations used Bayesian optimisation against a held-out set, maximising accuracy per inference call.

> [!abstract] The headline
> An optimised architecture over open models beat the leading closed models of the time on pass@1 by an average of around 14 points across instruction-following, reasoning, maths, and coding. Notice the metric. After all the sampling and fusing, the system returns one answer, so the user sees a single response and the architecture is invisible.

## Check yourself

Why can fusion beat an oracle that always picks the best available candidate?

- [ ] It can combine partial merits of several candidates into an answer none of them contained.
- [ ] It sees the candidates in sequence, which gives the model more reasoning tokens to work with.
- [ ] It avoids the verifier bias that causes an oracle to prefer longer, more confident candidates.
- [ ] It re-samples from the model, which raises coverage above the level of the original pool.

> [!tip]- Reveal the answer
> The oracle's ceiling is the best candidate in the pool, and fusion has no such ceiling because it composes. That is why it helps most on multi-part tasks, where different candidates get different parts right, and why it helps least on single-fact questions, where an answer is simply right or wrong and there is nothing to compose. The third answer describes a bias that an oracle cannot have by definition.

## Generated tests as a verifier

Archon also treats unit-test generation and evaluation as first-class operations, in the spirit of CodeMonkeys. The model writes tests for the problem alongside the solution, and those tests then filter the candidates. The unusual variant is evaluating a candidate against a test without executing it, by asking a model to reason about whether the answer satisfies the stated property.

This works because of the asymmetry from Lesson 3. Stating that a string with an odd number of brackets must be rejected is far easier than writing a correct bracket-matching parser. Specifications are cheaper than implementations, and a specification is a verifier.

Generalise this past code. Before your agent produces an analysis, ask it to write down the three properties a correct answer must have, such as a total that must reconcile, a sign that must be negative, or a category that must appear. Then check the output against its own stated properties. You have manufactured a verifier out of a second prompt, and because it was written before the answer existed, it cannot be rationalised to fit.

## What to build

You do not need Bayesian architecture search. You need the layers, in ascending order of effort.

<table>
<thead>
<tr>
<th>Level</th>
<th>Architecture</th>
<th>Cost</th>
<th>When to use it</th>
</tr>
</thead>
<tbody>
<tr>
<td>0</td>
<td>Single call</td>
<td>1x</td>
<td>Cheap, easy, reversible tasks</td>
</tr>
<tr>
<td>1</td>
<td>Generate n, then fuse</td>
<td>n+1x</td>
<td>The default upgrade. Almost always worth it and trivial to implement.</td>
</tr>
<tr>
<td>2</td>
<td>Generate n, critique, rank, fuse the top m</td>
<td>About 2n x</td>
<td>When candidates vary in quality and you can afford the critique pass</td>
</tr>
<tr>
<td>3</td>
<td>Level 2 with mixed model families in the generate layer</td>
<td>About 2n x</td>
<td>When failures are systematic rather than random, because different models have different blind spots</td>
</tr>
<tr>
<td>4</td>
<td>Stacked level-2 blocks with a final fuse</td>
<td>4n x and up</td>
<td>High-stakes offline work that tolerates latency, such as reports, migrations, and one-off analyses</td>
</tr>
</tbody>
</table>

Levels 3 and 4 make sense precisely when the work is offline. Nobody waits eight seconds for a chat reply, and everybody will wait ten minutes for a monthly report that is right. Sort your agent workloads by how much latency they tolerate and you will find the ones that deserve depth.

Project hook

Does fusion beat oracle selection on your tasks, and on which kind? Run the four strategies from the chart on the same candidate pool, using your own labels as the oracle, and split the results by whether the task has one part or several.

Measure Win rate for each strategy, split by task type. The prediction is that fusion's advantage grows with the number of separable parts in the answer, and a task type where fusion loses would be a genuinely useful finding.

## Retrieval Practice

Pick one offline workflow you own and write out its level-2 architecture concretely. What generates, how many candidates, what the critic is told to look for, and what the fuser is asked to produce.

> [!example]- Model answer
> Here is a worked example for a monthly analysis writeup. Generate five drafts at temperature 0.8, two of them from a second model family. Run one critic pass per draft, told specifically to check that every number traces to a query in the appendix, that stated caveats match the data's real limitations, and to list anything asserted without support. Do not tell it to "critique this", because that produces prose about tone. Rank the drafts by number of unsupported claims, ascending. Then fuse the top three drafts along with their critiques, asking for one report that keeps the strongest framing and every caveat any draft raised. The critic's specificity is what makes the difference, because a vague critic makes the architecture cost more and do nothing.

## What you can now do

You have four composable operations and evidence that stacking them works. You know fusion usually beats selection, that mixing model families raises the ceiling, and that generated specifications are a verifier you can always afford. Level 1, which is generate n and fuse, is a one-hour change to any workflow you own.

Next is [[Agents 07 — Tools as ground truth|Lesson 7, tools as ground truth]], where feedback stops coming from models and starts coming from the world.

**Worth designing together.** Architecture choice depends on your latency budget, your cost ceiling, and your failure shape, and your teacher does not know any of those until you say them. Describe a workflow and we will pick a level and write the critic prompt properly.

Read one thing

1.  **Primary source:** [CS329A Lecture 2, Test-Time Compute Scaling](https://www.youtube.com/watch?v=-Ggc37xLj_Y), final third, presenting *Archon: an architecture search framework for inference-time techniques* from Stanford. The fusion-beats-oracle chart is the one to look at.
2.  [Anthropic, *Building Effective Agents*](https://www.anthropic.com/engineering/building-effective-agents), the same operations written as engineering patterns, with the evaluator and optimiser loop spelled out in code.
3.  [Wang et al., *Mixture-of-Agents Enhances Large Language Model Capabilities*](https://arxiv.org/abs/2406.04692), layered fusion across model families and the closest independent replication of the depth result.

[[Agents 05 — Judges you can trust|← Lesson 5]] · [[Agents 07 — Tools as ground truth|Lesson 7: Tools as ground truth →]]
