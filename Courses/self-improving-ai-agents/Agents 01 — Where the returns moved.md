---
type: unit
course: "[[Self-Improving AI Agents]]"
status: not-started
order: 1
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Making the model bigger stopped being the best way to spend a dollar. Three other places to spend it took over, and you control one of them completely.

About 9 minutes · Lecture 1 · Gives you the map the rest of the course is drawn on

Your leverage over an agent is uneven, and the unevenness is not random. Sometimes a better prompt fixes everything. Sometimes nothing you write helps. Occasionally a model upgrade silently solves a problem you spent a month engineering around. Which of those you are in depends on which axis of improvement your problem sits on, and there are now three axes instead of one.

## The old axis, and why it flattened

From roughly 2020 to 2024 the field had one dial. Kaplan and colleagues showed that test loss fell predictably as you increased compute, dataset size, and parameter count.[¹](#s1) The models got bigger on schedule, from BERT at 340M parameters to GPT-3 at 175B to PaLM at 540B, and each step bought measurable capability. Two capabilities arrived that nobody had planned for: few-shot learning from examples in the prompt, and chain-of-thought reasoning that only appeared above a size threshold.[²](#s2)

Whether those capabilities really "emerged" is contested, and some of the effect comes from measuring with all-or-nothing metrics like exact match. The argument does not change what you should do. What matters is that the era's answer to every capability question was to wait for the next model, and that answer has expired.

CS329A opens by saying plainly that the curve began to saturate around 2024.[³](#s3) It did not stop. Returns per unit of pre-training compute got small enough that the frontier labs went looking elsewhere, and where they went is the subject of this course.

## The three axes that replaced it

![[cf44136ff964ad3a17bd8144d763a83dc72e5536.svg]]

*Axis 3 is the one the course is named after. It is a loop rather than a separate technique. Test-time compute produces verified successes, those successes become training data, and the cheaper model then reaches on its first attempt what the expensive procedure needed many attempts to reach. DeepSeek-R1 and the o-series are this loop run at industrial scale.<a href="#s4">⁴</a>*

### Axis 1, train-time scaling

Spend compute before deployment, but on reinforcement learning against checkable rewards rather than on more next-token prediction. Math with known answers, and code with passing tests. This is where the reasoning models came from. As a practitioner you mostly consume this axis, because it reaches you as a model release. Lesson 9 covers the part you can act on.

### Axis 2, test-time scaling

Spend compute during the request. Think for longer, sample repeatedly, search over candidates, and verify before returning an answer. This axis is a property of the harness you write rather than of the weights, so it is almost entirely under your control. Lessons 2 to 7 live here, and this is where most of your leverage is.

### Axis 3, self-improvement

Close the loop between the first two. In its industrial form this means generating training data with an expensive test-time procedure and fine-tuning on the results that passed verification. In the form you are likely to use, it means an agent that accumulates verified experience in the shape of notes, reusable skills, and corrected failures, and gets better without anyone touching the weights. Lesson 9 covers it.

> [!abstract] The sentence to keep
> Every technique in this course answers one question: how do you convert compute into reliability? Pre-training answered "spend it on parameters". The current answer is "spend some on generating candidates, and spend the rest on checking them".

## Check yourself

A vendor says their agent "reasons for longer on hard problems and gets better results". Which axis is that claim about?

- [ ] Train-time scaling, because reasoning behaviour has to be learned during training first.
- [ ] Self-improvement, because the agent is adapting how it works to the problem.
- [ ] Test-time scaling, because the extra compute is being spent during the request.
- [ ] Pre-training scaling, because thinking capacity comes from the model's parameter count.

> [!tip]- Reveal the answer
> The compute is spent at inference, so this is axis 2. The three wrong answers are all true statements that answer a different question, which is exactly how these claims blur together in marketing copy. Sorting a claim onto the right axis tells you who controls the dial. You can tune axis 2 yourself, and you can only buy axis 1.

## Where verification enters, and why it never leaves

Axes 2 and 3 both assume you can tell a good output from a bad one. If you sample a hundred solutions and one of them is right, you have gained nothing unless you can find it. If you generate training data from your own model, it poisons the next model unless you can filter it.

The first lecture names this in an aside that turns out to be the whole plot. Models can produce an enormous amount of plausible-looking material cheaply, and deciding whether any of it is good requires a feedback loop. Where the feedback is crisp, as in math and code and anything rule-based, improvement compounds. Where it is not crisp, human judgement becomes the bottleneck and verification stays hard.[³](#s3)

The distance between what a model can produce and what it can recognise as correct is called the *generation-verification gap*, and Lesson 4 is about it. It reframes your problem usefully. "The model cannot do this" is a wall, and "my verifier is too weak" is a task.

This is the most important thing to carry out of Lesson 1, because it changes what you build. Most people facing a disappointing agent work on the generator by writing a better prompt, buying a better model, or adding more context. Often the generator was already producing a correct answer somewhere in its distribution, and the system threw it away.

## What an agent is made of

The course uses a plain decomposition that matches Anthropic's workflow taxonomy.[⁵](#s5) An agentic system is some arrangement of five ingredients.

<table>
<thead>
<tr>
<th>Ingredient</th>
<th>What it does</th>
<th>Covered in</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>LLM calls</strong></td>
<td>Generate candidate text or actions</td>
<td>Lesson 2</td>
</tr>
<tr>
<td><strong>Tool calls</strong></td>
<td>Touch the world through search, code execution, APIs, and databases</td>
<td>Lesson 7</td>
</tr>
<tr>
<td><strong>Verifiers</strong></td>
<td>Check an output against ground truth, such as tests, types, schemas, and executions</td>
<td>Lessons 4 and 7</td>
</tr>
<tr>
<td><strong>Judges</strong></td>
<td>Score an output when no ground truth exists</td>
<td>Lesson 5</td>
</tr>
<tr>
<td><strong>Orchestration</strong></td>
<td>Chaining, routing, running in parallel, planner and worker, evaluator and optimiser</td>
<td>Lessons 6 and 8</td>
</tr>
</tbody>
</table>

Two things follow from having this vocabulary. When a run fails you can localise the failure to an ingredient instead of blaming "the model", which is the diagnostic habit Lesson 12 formalises. And the ingredients you are most likely to be missing are the middle two, because most working agents are LLM calls plus tools plus hope.

Project hook

Does sorting failures by ingredient predict which fix works? Take fifty failed runs from one workflow, label each with the ingredient that failed, then apply the fix that label implies and see whether the failure rate drops more than a generic fix like a better prompt would deliver.

Measure Failure rate before and after, split by label. The interesting result is the label where the implied fix does not help, because that tells you the taxonomy is wrong somewhere.

## Retrieval Practice

Pick one agent workflow you actually run. Without scrolling up, write down which of the three axes it currently uses and which of the five ingredients it lacks. Two or three sentences.

> [!example]- Model answer
> The common shape is a single LLM call plus a few tools. That is axis 2, but barely, because it takes one sample and does no search. It has no verifier and no judge, and its orchestration is a prompt asking the model to be careful. That configuration has exactly one improvement lever, the prompt, which is why prompt-tuning feels like the whole job. Adding a verifier gives you a second lever and, more importantly, a measurement. Keep what you wrote, because Lesson 14 asks you to read it again.

## What you can now do

You can sort any claim, paper, or product pitch onto one of three axes, and you know which of them you control. You know that verification rather than generation is usually the binding constraint. And you have a five-part vocabulary for describing what your own agents are made of, which is what makes the rest of this course actionable.

Next is [[Agents 02 — The dial you already have|Lesson 2, the dial you already have]], where "spend more at inference" becomes a specific set of things to change.

**Your teacher is in this window.** If the three-axis split feels too neat, say so and we will take it apart, because it is a simplification and reasonable people argue about where post-training ends and test-time begins. Bring a real failing run and we can localise it against the five ingredients together.

Read one thing

1.  [Kaplan et al., *Scaling Laws for Neural Language Models*](https://arxiv.org/abs/2001.08361). Read section 1 and the three headline figures. Seeing the straight lines is the point.
2.  [Wei et al., *Emergent Abilities of Large Language Models*](https://arxiv.org/abs/2206.07682).
3.  **Primary source for this lesson:** [CS329A Lecture 1, Course Overview](https://www.youtube.com/watch?v=6YnLB0XbTnI) (Mirhoseini and Tandon, Stanford, 2026). The first thirty minutes are the argument above, delivered by the people who ran the experiments. Watch it before Lesson 2.
4.  [DeepSeek-AI, *DeepSeek-R1*](https://arxiv.org/abs/2501.12948), the clearest public description of axis 1 meeting axis 3.
5.  [Anthropic, *Building Effective Agents*](https://www.anthropic.com/engineering/building-effective-agents), the workflow taxonomy in the table above, with code.
