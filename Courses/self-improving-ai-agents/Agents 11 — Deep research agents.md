---
type: unit
course: "[[Self-Improving AI Agents]]"
status: not-started
order: 11
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Retrieval that happens when the reasoning needs it, and a note-taking step between the document and the prompt. Two changes separate a research agent from a RAG pipeline.

About 10 minutes · Lecture 7 · The architecture closest to analysis work

A reasoning model with a knowledge gap does something observable, which is that it hedges. The uncertainty surfaces as vocabulary, with words like perhaps, alternatively, and likely scattered through the reasoning chain. Because reasoning is sequential, a guess made early propagates, so every later step inherits it and the final answer carries confidence it has not earned.

That is the problem Search-o1 addresses, and its diagnosis is the useful part.[¹](#s1) **The hedging is a signal, and it tells you when to go and look something up.**

## Three architectures, in order of goodness

<table>
<thead>
<tr>
<th></th>
<th>Retrieval timing</th>
<th>What enters the prompt</th>
<th>Fails when</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Classic RAG</strong></td>
<td>Once, up front</td>
<td>The top k documents</td>
<td>Later steps need information you could not have known to fetch</td>
</tr>
<tr>
<td><strong>Agentic RAG</strong></td>
<td>Whenever the model emits a search</td>
<td>Retrieved documents, whole</td>
<td>Documents are long and noisy, so the reasoning drowns in them</td>
</tr>
<tr>
<td><strong>Search-o1</strong></td>
<td>Whenever the model emits a search</td>
<td>An extracted summary of each document</td>
<td>The extraction step drops something that mattered</td>
</tr>
</tbody>
</table>

Most people have already made the step from the first row to the second, which is to let the model decide when to retrieve, mid-reasoning, as often as it needs. A single up-front retrieval is fine for asking what the weather was and hopeless for anything where step three's question depends on step two's answer.

Most people have not made the step from the second row to the third, and that is where the gains are.

## Reason in documents, which means take notes rather than paste

![[d4e3e249f7a8308e8826a5e0d5a2544c4c9cc7e3.svg]]

*A sub-routine sits inside the retrieval tool. Given the current query, the reasoning so far, and a retrieved document, it extracts only the relevant content and returns that. What enters the main context is a note rather than a paste. The lecture's comparison is exact, because a researcher reading twenty references does not staple them into the draft, they take notes.*

This matters more than it sounds, because long-context reasoning degrades. Dropping ten documents into a reasoning chain assumes the model reasons as well over 40,000 tokens of mixed-relevance text as over a focused prompt, and it does not. Extraction is a **context-quality** intervention, and Lesson 13 explains why that dominates almost everything else in long runs.

A student asked the sharp question, which is whether this is just prompting the model to summarise before continuing. Largely yes, and the lecturer's answer is worth keeping. It is a workaround for a limitation that should disappear as long-context reasoning improves. That is honest and it does not make the technique less useful today. Several patterns in this course are scaffolding around a current weakness, and knowing which weakness a pattern serves tells you when to retire it.

The reported effect is measured in an unusual way. The frequency of uncertainty markers in the reasoning chains drops substantially, because the model stops hedging once it stops guessing.

## Check yourself

Your research agent retrieves well but produces vague, hedged conclusions. What is the most likely cause?

- [ ] Its search queries are too broad, so retrieved documents are only loosely on topic.
- [ ] It retrieves too rarely, so most reasoning steps proceed without any grounding at all.
- [ ] Whole documents are entering context, so relevant content is diluted by irrelevant content.
- [ ] The base model is too weak to synthesise findings across several independent sources.

> [!tip]- Reveal the answer
> Retrieving well and concluding vaguely is the signature of good search feeding a drowning context. All four are real failure modes, and you tell them apart by reading the trace. If the right document was fetched and the right sentence is sitting unused in the context, the problem is dilution and an extraction step fixes it. This is also why logging retrieved content separately from reasoning, as Lesson 7 recommends, pays for itself.

## Knowing what you do not know

The lecture flags the genuinely hard part, which the paper only partly handles. **How does the model know what to look up?** Detecting your own knowledge gap is exactly what models are bad at, because they are systematically overconfident. Aggregate the log-probabilities of an answer and you will find a model 80 percent confident on questions it gets right half the time. Uncertainty markers in a reasoning chain are a usable proxy precisely because the calibrated signal is not available.

The practical answer offered is not clever, and it is better for that. **Decide in advance which parts of a question you will not trust the weights for.** Identify the key entities, dates, figures, and definitions, and fetch those on principle rather than waiting for the model to feel unsure. In your domain you already know which facts must come from the warehouse and which the model may supply from memory, so encode that rather than delegating it to the model's introspection.

## Building one for analysis work

- **Retrieve mid-reasoning, repeatedly.** One up-front fetch caps you at questions you could fully specify before starting, which excludes most of the interesting ones.
- **Put an extraction step inside every retrieval tool** so it returns notes rather than payloads. This applies to query results and file reads as much as to web search, and a tool that returns 5,000 rows should return a characterisation of them.
- **Pin the facts that must be fetched.** A list of entity types the agent must always look up beats hoping it notices its own ignorance.
- **Treat hedging as a metric.** Counting uncertainty markers per run is a crude but useful proxy for grounding, it costs one regular expression, and you can watch it move when you change retrieval.

One habit underlies all four. Do not trust stated confidence, because models are overconfident and will defend wrong answers, and confidence expressed in prose is not evidence.

The lecture also points at Search-R1, which learns the search behaviour with reinforcement learning rather than eliciting it by prompting. It is the same architecture reached by a different route, and a clean illustration of the Lesson 9 rule: prompt-elicited behaviour first, and trained behaviour only once the pattern is stable and the cost justifies it.

Project hook

Does the extraction step help on your data, and where does it start dropping things that matter? Run one research workflow twice, once pasting whole tool outputs into context and once passing them through an extraction step, and grade both against the same rubric.

Measure Answer quality, tokens consumed, and the count of uncertainty markers per run. Then read the cases where extraction lost, because those tell you what your extraction prompt is throwing away and that is the transferable finding.

## Retrieval Practice

For one analysis task you would delegate to an agent: which facts must never come from the model's weights, and what would the extraction step return instead of the raw tool output?

> [!example]- Model answer
> Here is a worked example for a competitor pricing analysis. The facts that must be fetched are current prices, product names and SKUs, dates of changes, anything with a number in it, and the definitions of internal metrics, all of which the model will invent plausibly. The things that may come from the weights are what a price elasticity is, how to structure the comparison, and standard caveats about sample size. The extraction step, instead of returning a 200-row result set, returns something like "42 SKUs matched, price range 4.99 to 89.99, 6 changed in the last 30 days and listed below, 3 rows have a null category". That is a characterisation the reasoning can use, with the raw data still available on request. The last clause matters, because extraction should narrow what enters context rather than destroy access to it.

## What you can now do

You can distinguish classic RAG, agentic RAG, and reason-in-documents, and say what each one fails at. You know that hedging vocabulary is a usable uncertainty proxy where calibrated confidence is not available, that models are overconfident by default, and that the fix for a drowning context is a note-taking step rather than a bigger window.

Next is [[Agents 12 — Evals that change decisions|Lesson 12, evals that change decisions]], which begins the arc that tells you whether any of this worked.

**The extraction prompt is the whole trick here.** Too aggressive and it drops the sentence that mattered, too lenient and you have pasted the document. It is worth tuning against real retrievals rather than in the abstract.

Read one thing

1.  **Read this one:** [Li et al., *Search-o1: Agentic Search-Enhanced Large Reasoning Models*](https://arxiv.org/abs/2501.05366). The reason-in-documents module takes up a page of the paper and is the most directly copyable idea in this lesson.
2.  [Jin et al., *Search-R1*](https://arxiv.org/abs/2503.09516), the same behaviour acquired by RL rather than by prompting.
3.  [CS329A Lecture 7, Self-Improvement and Deep Research Agents](https://www.youtube.com/watch?v=Uni9dqyuuDM), final third, the source for this lesson including the discussion of model overconfidence.

[[Agents 10 — The submission budget|← Lesson 10]] · [[Agents 12 — Evals that change decisions|Lesson 12: Evals that change decisions →]]
