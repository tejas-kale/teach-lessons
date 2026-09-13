---
type: unit
course: "[[Self-Improving AI Agents]]"
status: not-started
order: 9
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

The whole train-time lecture, reduced to the decision you might actually face, plus one honest result about what reinforcement learning does and does not buy.

About 10 minutes · Lectures 5 and 6 · The lesson that mostly tells you not to

This is the course's most technical territory and the least of it applies to you directly. Take it in the order that matters, which is the loop, then the honest result, then the failure conditions, and only then the decision.

## The loop, in one sentence

> [!abstract] If you keep one thing from this lesson
> Train-time scaling means taking the model's own outputs, filtering them cleverly, and fine-tuning on what survives. Test-time compute produces the candidates, verification does the filtering, and the fine-tune turns an expensive procedure into a cheap one. That is the entire self-improvement loop the course is named after.

It has a hard precondition, which is that it only runs where **verification is possible**. The o-series data shown in Lecture 6 makes this vivid. Win rates over a non-thinking model exceed 50 percent in programming, data analysis, and mathematical calculation, which are domains where you can close the loop, and they are markedly weaker in personal writing and editing, where you cannot. If your domain has no verifier, this axis is largely closed to you, which is why Lessons 4 to 7 came first.

## STaR, bootstrapping reasoning from almost nothing

STaR is the simplest complete instance of the loop, and it is worth understanding because it exposes every assumption the loop makes.[¹](#s1) Start with a handful of worked examples. Few-shot prompt the model over a set of problems whose answers you know. Keep the generated reasoning chains that reached the right answer, fine-tune on those, and repeat.

The wrinkle is what to do about the failures, which carry no signal. STaR's answer is rationalisation. Hand the model the correct answer, ask it to explain how to get there, and then train on the explanation with the hint removed. This pulls harder problems into the training set rather than discarding them.

The students immediately found the flaw, and it is a good one. Given the answer, a model can produce a fluent rationale that does not actually derive it, and you then fine-tune on confident nonsense. STaR applies no filter at this step, and later work adds a process reward model there. It is the Lesson 4 false-positive problem arriving exactly where it does the most damage.

Three limits generalise well beyond STaR.

- **It cannot exceed the base model's reach.** If a problem class is entirely outside what the model can ever stumble into, no amount of bootstrapping finds it. You amplify existing capability rather than creating it.
- **It plateaus**, and iterations stop paying after a few rounds.
- **Final-answer correctness is an imperfect proxy for reasoning quality.** You keep some invalid reasoning that reached the right answer, and you discard some valid reasoning that did not.

## The honest result about reinforcement learning

DeepSeek-Math is famous for GRPO, which drops PPO's learned critic and computes the advantage from a group of sampled answers instead, as the reward minus the group mean divided by the group's spread.[²](#s2) That keeps fewer model copies in memory, so RL scales further, and it took a 7B model past 50 percent on MATH, which at the time was a size class ahead of expectations.

The result you should carry is a different one. Their RL improved **majority@k** and did not improve **pass@k**.

![[a13b8bb06004d47222be6917007400d0249654dc.svg]]

*A schematic of the DeepSeek-Math finding. Coverage, meaning the set of problems the model can solve at all, barely moved. What moved was how often the correct answer is the one produced. In Lesson 3's language, RL did not raise the ceiling here, it moved pass@1 up toward a ceiling that already existed.*

That should recalibrate what you expect a fine-tune to buy. It is a reliability instrument rather than a capability instrument, because it makes the model produce its good answer more often. That is genuinely valuable, and it is also the same thing a good verifier buys you at a small fraction of the effort. That comparison is the heart of the decision below.

## Check yourself

Your agent solves a task correctly in 3 of 10 runs. What does the DeepSeek-Math result suggest about fine-tuning it?

- [ ] It would fail, because three successes in ten is far too weak a signal to train on.
- [ ] It would fail, because fine-tuning improves coverage rather than the consistency you need.
- [ ] It could help, but so would a verifier, at far less cost, since the capability is present.
- [ ] It would help most, because a stronger policy is the only route to reliable single runs.

> [!tip]- Reveal the answer
> Three in ten means the capability exists and is surfaced unreliably, which is exactly what RL improves and exactly what selection improves. A fine-tune costs weeks and a verifier costs an afternoon, so try the verifier first and reserve the fine-tune for when you have a stable task, a real eval, and a per-call cost worth optimising. The second answer inverts the finding, because RL moved consistency rather than coverage.

## Why RL runs break, and the law underneath

Scaling GRPO naively to a larger model fails in specific ways, which DAPO set out to fix.[³](#s3) Entropy collapses, so the model becomes over-confident and stops exploring, training destabilises, and response length grows without bound. Two of its fixes are worth knowing.

- **Asymmetric clipping.** PPO's symmetric clip treats increases and decreases the same way, which caps how much a low-probability token can be promoted and quietly kills exploration. Allowing larger upward moves keeps entropy alive.
- **Dynamic sampling.** Oversample, then discard groups where every sample was correct or every sample was wrong. Those groups produce zero advantage after normalisation and contribute nothing to the gradient, so keeping only the mixed groups preserves an effective batch size.

Dynamic sampling is a special case of the law running through the whole lecture: **you need a distribution of rewards**. All-correct teaches nothing, all-wrong teaches nothing, and learning happens on the band in between.

This applies directly to your eval sets, which is where you will actually use it. A benchmark your agent passes every time, or fails every time, contains no information about your next change. The tasks worth keeping are the ones that sometimes work. When you curate an eval set, deliberately over-weight that band and keep a few from each end only as guardrails against regression.

## Two more findings that transfer

**Data curation beat the algorithm.** DeepSeek-Math's largest gains came before any RL, from curating maths content out of web crawl rather than relying on arXiv, and from starting with a code-pretrained model, which improved both reasoning and tool use. The unglamorous input work outperformed the clever optimiser, which is the most reliable pattern in applied machine learning and has not stopped being true.

**Filter for process rather than outcome, if you are doing RL.** SWiRL found that training on process-filtered trajectories, meaning ones whose steps were judged sound, beat outcome-filtered trajectories for RL, because outcome filtering keeps only problems the model already solves and teaches it nothing new.[⁴](#s4) For supervised fine-tuning the preference reverses, because SFT is imitation and incorrect final answers actively hurt. SWiRL also generalised across tools and domains, since a model trained on maths with a calculator improved at multi-hop question answering with a search tool. It learned how to work in steps and invoke a tool, not one specific tool.

## The decision

<table>
<thead>
<tr>
<th>Symptom</th>
<th>Do this</th>
<th>Not this</th>
</tr>
</thead>
<tbody>
<tr>
<td>The capability is there but surfaces unreliably</td>
<td>Verifier and selection, from Lessons 4 to 6</td>
<td>Fine-tune</td>
</tr>
<tr>
<td>Wrong output format or house style</td>
<td>Prompt first, then few-shot examples</td>
<td>Fine-tune</td>
</tr>
<tr>
<td>Missing domain knowledge</td>
<td>Retrieval and better context</td>
<td>Fine-tune</td>
</tr>
<tr>
<td>The capability is genuinely absent</td>
<td>Stronger model, better tools, decomposition</td>
<td>Fine-tune, because it amplifies rather than creates</td>
</tr>
<tr>
<td>All of the above are solved and per-call cost now dominates</td>
<td>Now consider distilling your working pipeline into a smaller model</td>
<td>None</td>
</tr>
</tbody>
</table>

Fine-tuning is right when you have a **stable task**, a **trustworthy eval**, **thousands of verified examples**, and a cost or latency problem that a smaller model would solve. In practice the last item is the real trigger, because you fine-tune to make something cheap that already works rather than to make something work. It is also worth knowing that at the frontier labs RL remains a small fraction of total training compute. It is growing, and it is nowhere near parity, and if it were the easy win it would not be.

Project hook

Does the consistency-not-capability finding hold for prompt-level changes too? Take one workflow and measure pass@k and majority@k before and after a substantial prompt improvement, rather than after a fine-tune.

Measure Both curves before and after. If prompting also moves majority@k without moving pass@k, then prompt engineering and RL are buying the same thing, which is a useful and slightly deflating result to be able to state with numbers.

## Retrieval Practice

Explain to a colleague who wants to fine-tune on your agent's successful runs why that is probably premature, and what the two cheaper experiments are.

> [!example]- Model answer
> There are three points to make. Training on outcome-filtered successes keeps runs that were right for the wrong reasons and teaches the reasoning that produced them. The DeepSeek-Math result suggests the gain would be consistency rather than new capability, and a verifier buys the same consistency in an afternoon rather than a month. And successes are by definition the problems the model already solves, so they carry the least information. The two cheaper experiments are the two-number test from Lesson 3, which tells you whether this is a selection problem or a capability problem, and a three-verifier ensemble from Lesson 5 to close whatever gap it reveals. If both are done and per-call cost is still the binding constraint, fine-tuning becomes a reasonable conversation.

## What you can now do

You can state the self-improvement loop in one sentence, say what RL actually buys, which is consistency rather than coverage, name the conditions under which training runs collapse, and apply the reward-distribution law to the eval sets you curate. You also have a defensible answer to "should we fine-tune this?", which is usually no, for reasons you can state.

Next is [[Agents 10 — The submission budget|Lesson 10, the submission budget]], where the loop meets the fact that you can only ever check a handful of the candidates you generate.

**If a fine-tune is genuinely on the table**, meaning you have the eval, the data, and the cost pressure, that deserves working through properly rather than from a table. Bring the numbers.

Read one thing

1.  [Zelikman et al., *STaR: Bootstrapping Reasoning With Reasoning*](https://arxiv.org/abs/2203.14465). Short, and the clearest exposition of the loop's assumptions.
2.  **Read this one:** [Shao et al., *DeepSeekMath*](https://arxiv.org/abs/2402.03300). Read the data-curation section and the discussion of majority@k against pass@k, and skim the GRPO derivation.
3.  [Yu et al., *DAPO: An Open-Source LLM Reinforcement Learning System at Scale*](https://arxiv.org/abs/2503.14476), the failure modes of scaled RL stated plainly.
4.  [Goldie et al., *SWiRL: Synthetic Data Generation and Multi-Step RL for Reasoning and Tool Use*](https://arxiv.org/abs/2504.04736), the cross-tool generalisation result.
5.  [CS329A Lecture 6, Train-Time Scaling and Scaling RL](https://www.youtube.com/watch?v=yVnmHSAy3ck) (Tandon), the source for this lesson.

[[Agents 08 — Planning and multi-step reasoning|← Lesson 8]] · [[Agents 10 — The submission budget|Lesson 10: The submission budget →]]
