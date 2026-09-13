---
type: unit
course: "[[Self-Improving AI Agents]]"
status: not-started
order: 13
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Long tasks fail in five specific ways. One finding explains most of the gap between what agents can do and what they do for you: they arrive as contractors rather than as maintainers.

About 10 minutes · Lecture 8 · The lesson about your unfair advantage

Lesson 12 established the reliability gap, with an hour of capability at 50 percent success and a quarter of that at 80 percent. This lesson is about what fills the gap. It is not one thing, and none of it is mysterious.

## Five ways long runs fail

The failure analysis in Lecture 8 gives a taxonomy stable enough to classify against.[¹](#s1)

<table>
<thead>
<tr>
<th>Failure</th>
<th>What it looks like in a trace</th>
<th>What helps</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Poor planning</strong></td>
<td>Each step is reasonable and the decomposition solves an adjacent problem</td>
<td>Make the plan an explicit artefact you can inspect before execution, as in Lesson 8</td>
</tr>
<tr>
<td><strong>Poor tool choice</strong></td>
<td>Reaches for the wrong instrument, or an inadequate one</td>
<td>Fewer and better-described tools, with capabilities and limits stated</td>
</tr>
<tr>
<td><strong>Reasoning and arithmetic errors</strong></td>
<td>Confident mental calculation that is quietly wrong</td>
<td>A tool. Never let it do arithmetic in its head, as Lesson 7 argues</td>
</tr>
<tr>
<td><strong>Premature abandonment</strong></td>
<td>Stops and declares itself done without meeting the goal</td>
<td>An explicit, checkable completion criterion, because it often does not know what done means</td>
</tr>
<tr>
<td><strong>Repetitive loops</strong></td>
<td>Repeats a failing action because it is still the highest-probability action</td>
<td>Loop detection, a forced strategy change, and richer error messages so the next attempt differs</td>
</tr>
</tbody>
</table>

Reasoning models reduced the repetitive-loop failure substantially compared with earlier models, and the categories persist. Four of the five have cheap structural mitigations that cost no model quality at all, which is the point of classifying rather than lamenting.

"It does not know when it is done" is worth dwelling on. A human professional carries an implicit standard of completion, and an agent has only whatever you wrote in the prompt. "Produce a good analysis" is not a completion criterion. Stating one, such as every figure tracing to a query and the three named questions being answered, removes a whole failure class for the price of a sentence.

## The finding that reframes everything, which is contractor against maintainer

On internal codebases, people without prior context, meaning contractors, take somewhere between 5 and 18 times longer than the maintainers who work there daily. Model performance tracks the **contractor** times rather than the maintainer times.[¹](#s1)

> [!abstract] The reframe
> Your agent is not a weak expert. It is a capable professional with no context, dropped into your codebase, your warehouse, and your conventions on their first morning, every single time. Most of what you experience as "the model is not good enough" is the same penalty you would impose on a talented new hire by telling them nothing.

GDPval found the matching result from the other direction. Strip context out of the prompts and win rates fall, and more tellingly, the models struggle to work out what to work on at all. As the lecture puts it, real work is context-heavy, and much of professional skill is knowing what to prioritise and where the bodies are buried. Benchmarks measure whether a smart person with no context can do the task, and they do not measure what someone embedded in the work can do.

This is good news, because context is the input you control and no benchmark can. It is also the most under-exploited lever available to you, considerably more so than any prompt-engineering technique.

![[5b174c3c310f28a1879a3aa657efb7b7648dff8a.svg]]

*The bottom bar is the whole opportunity. Nothing about the model changes between the third row and the fourth, only what it was told about your conventions, your schema quirks, your priorities, and what good looks like here.*

## Four strategies for context

More context is not automatically better, because Lesson 11 showed a reasoning chain drowning in retrieved documents. What matters is relevant context, and that needs managing.

- **Write the standing context down.** Schema quirks, naming conventions, which tables are stale, what a good output looks like, and the three things reviewers always ask. This is the highest-value document you can write, it is reusable across every agent you run, and its absence is what makes your agent a contractor.
- **Compact rather than accumulate.** Long runs collect transcript, so periodically replace history with a summary of state and decisions. That is the reason-in-documents move from Lesson 11 applied to the agent's own trace.
- **Fan out into fresh contexts.** Independent sub-tasks from Lesson 8 each get a clean window and return a short result. This is context hygiene as much as parallelism, which is why decomposition helps even when nothing runs concurrently.
- **Externalise state.** A file the agent writes progress and decisions to survives context loss, gives you something to inspect mid-run, and lets a failed run resume rather than restart. It is cheap, and it converts a catastrophic failure into a recoverable one.

## Check yourself

An agent does well on short tasks and degrades badly past about thirty minutes. What is the first thing to try?

- [ ] A stronger model, since long-horizon coherence is what separates capability tiers.
- [ ] A larger context window, so the whole run fits without any information being lost.
- [ ] A lower temperature, so the agent drifts less from its approach over many steps.
- [ ] Decompose it into checkpointed stages, each with fresh context and a verified handoff.

> [!tip]- Reveal the answer
> Degradation with length is the signature of accumulating context and cascading error, and the structural fix is cheap and available today. A larger window does not help much, because the problem is dilution rather than truncation, and reasoning quality falls over long noisy contexts well before the window fills. The first answer is the expensive move and is worth trying only after the structural one fails. Checkpointing also converts one long unreliable run into several short reliable ones, which is exactly what the METR curves say you want.

## What is actually driving the improvement

The lecture's list of capabilities behind the lengthening horizon works as a checklist for your own harness, because each one has a harness-level counterpart.

<table>
<thead>
<tr>
<th>Model capability</th>
<th>Your equivalent lever</th>
</tr>
</thead>
<tbody>
<tr>
<td>Better logical reasoning</td>
<td>Thinking budget, and an explicit planning step</td>
</tr>
<tr>
<td>Better tool use</td>
<td>Tool design and rich failure messages, from Lesson 7</td>
</tr>
<tr>
<td>Fewer repetitive behaviours</td>
<td>Loop detection, and a forced strategy change on a repeated failure</td>
</tr>
<tr>
<td>Error recovery</td>
<td>Checkpoints, and a written reflection after each failure, from Lesson 8</td>
</tr>
<tr>
<td>Goal awareness and state</td>
<td>An externalised state file, and explicit completion criteria</td>
</tr>
</tbody>
</table>

The class also raised re-planning. Strong agents plan, execute, discover the plan was wrong, and plan again. If your harness treats the plan as fixed after step one, you have engineered out the recovery behaviour that makes long tasks work at all.

Project hook

How much of the contractor penalty can context buy back? Write the standing-context document for one domain, then run the same task set with and without it in the prompt.

Measure Success rate and time-to-completion in both conditions, and the failure taxonomy split. This is the course's central claim tested on your own work, it needs no new infrastructure, and the document is worth having even if the result is null.

## Retrieval Practice

Write the first ten lines of the standing-context document for your domain, meaning the things a brilliant new colleague would need on day one and could not work out alone.

> [!example]- Model answer
> The shape that works covers which tables are authoritative and which are deprecated but not deleted, the column whose name lies about its contents, the timezone convention and where it is violated, which metric definition is official when three exist, the filter everyone forgets such as test accounts or internal orders, what the weekly numbers are actually used for and by whom, the caveat every analysis must carry, the three questions your reviewer always asks, who to ask when something looks wrong, and what done means here. None of this is in the schema, all of it is in your head, and every one of your agents is currently working without it.

## What you can now do

You can classify a long-run failure into one of five categories and name the structural fix for each. More importantly, you have the contractor and maintainer reframe, which says the gap between benchmark agents and useful agents is mostly context, and context is the input you own. The standing-context document is the most valuable thing you could write this month.

Next is [[Agents 14 — The frontier|Lesson 14, the frontier]], which closes the taught arc by looking at what is still unsolved.

**The standing-context document is worth drafting together.** It is hard to write alone precisely because the knowledge is invisible to whoever holds it, which is what makes it valuable. Being interrogated about your domain by someone who does not share your assumptions is the fastest way to get it out.

Read one thing

1.  **Primary source:** [CS329A Lecture 8, Agentic Evaluations and Long Horizon Tasks](https://www.youtube.com/watch?v=8JAqLnTaZu4) (Tandon), for the failure taxonomy and the contractor and maintainer discussion.
2.  [Kwa et al., *Measuring AI Ability to Complete Long Tasks*](https://arxiv.org/abs/2503.14499) (METR). Read the limitations section, which is where the context findings live.
3.  [Anthropic engineering blog](https://www.anthropic.com/engineering). The context-engineering posts are the closest thing to a practitioner manual for the four strategies above.

[[Agents 12 — Evals that change decisions|← Lesson 12]] · [[Agents 14 — The frontier|Lesson 14: The frontier →]]
