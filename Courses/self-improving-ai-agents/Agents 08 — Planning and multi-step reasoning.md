---
type: unit
course: "[[Self-Improving AI Agents]]"
status: not-started
order: 8
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Search over actions instead of over answers, plus the observation that most of a model's reasoning did not need to be sequential in the first place.

About 10 minutes · Lecture 5 · Where sampling becomes search

Everything so far treated a task as one shot, where you generate candidates and pick one. Real agent work is a trajectory, meaning a sequence of actions where each one changes what is possible next. Sampling k complete trajectories wastes an enormous amount, because they share prefixes and you get no way to abandon a bad path early.

The move is to search the tree of actions rather than the set of answers.

## LATS, which is Monte Carlo tree search with a language model

Language Agent Tree Search imports MCTS into agent work.[¹](#s1) It cycles through six stages: **selection, expansion, evaluation, simulation, backpropagation, and reflection**.

![[1b35bde783877e034b0167467d34d3c5bc7e11de.svg]]

*Each node is a state and each edge is an action actually executed in the environment, with its observation appended to context. Two details are worth stealing. The node value combines an LLM judge score with a self-consistency score, meaning how often that action was sampled. And after a trajectory ends, the model writes a reflection on why it succeeded or failed, which is carried into later expansions and contributes a large share of the gains.*

The distinction from earlier verifier-guided search matters. A PRM from Lesson 4 scores reasoning steps, and LATS scores the outcomes of actions taken in the world. Selection uses UCT, which is the node's value plus a bonus for nodes visited rarely relative to their parent, and that is the standard machinery for balancing exploitation against exploration.

Results were strong on multi-hop question answering and on web shopping, with no fine-tuning. Two limitations decide whether you can use it, and the lecture names both.

- **Cost.** Expansion, simulation, and backpropagation multiply the number of calls, and the paper does not analyse the cost against the benefit at all.
- **Irreversibility.** Tree search assumes you can back up. If an action sends an email, charges a card, or drops a table, there is no backing up, so search is only available where the environment is resettable or read-only.

The second constraint decides where you can use this. Read-only analysis over a warehouse is searchable and cheap, because you can branch freely and nothing is harmed. A pipeline that writes to production is not searchable unless you can run it against a sandbox or inside a transaction you can roll back. Building that sandbox is usually the enabling investment, rather than the search algorithm.

## Check yourself

Which agent workflow is the best candidate for tree search over actions?

- [ ] An agent that sends follow-up emails to customers after a support ticket is closed.
- [ ] An agent that explores a data warehouse to answer an open-ended analytical question.
- [ ] An agent that applies schema migrations to a production database on a nightly schedule.
- [ ] An agent that reconciles two ledgers and posts the adjusting entries it discovers.

> [!tip]- Reveal the answer
> Read-only exploration has many plausible paths and a cheap undo, because abandoning a query costs a few seconds. The other three take irreversible actions in the world, so searching them means actually sending, migrating, or posting down every branch you explore. The general test is whether you can abandon a branch at no cost. If you cannot, you need a sandbox before you need a search algorithm.

## Spring, and how much of that reasoning was not sequential

Reasoning models think for longer on harder problems, and longer thinking correlates with higher accuracy, which you can see in DeepSeek-R1's training curves where response length rises alongside benchmark scores. The cost is latency and money, because you wait and pay for every sequential token.

Spring starts from a sharp observation.[²](#s2) If you inspect a long reasoning trace as a dependency graph, **large parts of it are independent**. The model tries alternatives, decomposes into sub-tasks, and checks separate conditions, and it generates all of that in a strict line because a strict line is the only thing autoregression permits.

The method is a good example of manufacturing training data with models. Take reasoning traces from a strong model, use a second model to segment each trace into steps, tag each step as planning or execution, and annotate the dependencies between steps. That gives you a directed graph, which you can repack so that independent steps sit in parallel groups. Fine-tune on the repacked traces and the model learns to emit plans and their parallel executions explicitly, which the harness can then dispatch concurrently.

> [!abstract] The result that was not the goal
> The project set out to cut sequential tokens and achieved roughly a 40 percent reduction on maths. It also raised accuracy by about 3.5 points over the base distilled model, and the gains transferred to tasks it was never trained on. Being made to state a plan before executing it appears to help the model think, independently of any parallelism it wins. Structure is not only a scheduling convenience.

A second observation applies without any fine-tuning at all. **Parallelism is high early and low late.** Early on the model explores many independent directions, and towards the end it converges on one line and drives it home. That is the exploration-then-exploitation shape you should be designing your own workflows to have.

## Translating this into workflow design

You are not going to run MCTS or fine-tune a parallel planner. Take these four things instead.

- **Make the plan an artefact.** Have the agent emit an explicit plan before acting. It improves quality on its own, it lets you intervene at the cheapest possible moment, and it makes failures classifiable as a bad plan or a bad execution.
- **Fan out the independent parts.** Once a plan exists, its independent branches can run as separate agent calls, which costs less latency and gives each branch a clean context window, as Lesson 13 explains.
- **Explore early and converge late.** Sample several approaches at the start, when a branch costs little, and commit to one before the expensive execution.
- **Keep the reflection.** After a failed trajectory, have the agent write down why it failed and carry that note into the retry. It is the cheapest component of LATS and reportedly a large part of its benefit.

None of those work without cheap undo, so the prerequisite is a sandbox. A dry-run mode or a scratch schema is what unlocks everything in this lesson, and it is usually a day of work rather than a project.

Project hook

Does making the plan explicit help even when nothing runs in parallel? Run one workflow in two conditions, one where the agent acts directly and one where it must emit a plan first and then execute it, with the same model and the same token budget.

Measure Success rate, plus the share of failures attributable to the plan rather than to execution. Spring predicts the planning condition wins on quality alone, and testing that on a non-maths workload is a genuine contribution.

## Retrieval Practice

Take a multi-step workflow you run and draw its dependency graph. Which steps genuinely need the previous step's output, and which only look sequential because you wrote them in a line?

> [!example]- Model answer
> A typical finding for an analysis pipeline is that pulling three data sources, computing four metrics, and checking two data-quality conditions are all independent, and only the synthesis at the end depends on everything. Written as one prompt, the agent does them in a line and accumulates everything in one context. Written as a plan plus a fan-out, they run concurrently in fresh contexts and the synthesis step sees only clean summaries. That is a latency win and a context-hygiene win from one change, which is why it is usually the highest-value restructuring available.

## What you can now do

You can recognise when a task is searchable, meaning it has cheap undo, many paths, and checkable outcomes, and when it is not. You know why explicit planning helps even when nothing runs in parallel, and you have the exploration-early and convergence-late shape to design toward.

Next is [[Agents 09 — Buy or tune|Lesson 9, buy or tune]], the one lesson on training, reduced to the decision you might actually face.

**Bring a workflow's dependency graph.** Deciding what can fan out, and what must stay serial because of shared state, is exactly the kind of design question worth arguing about with someone who can see the whole picture.

Read one thing

1.  [Zhou et al., *Language Agent Tree Search Unifies Reasoning, Acting, and Planning*](https://arxiv.org/abs/2310.04406) (ICML 2024). Read section 3 for the six stages and the value function.
2.  **Primary source:** [CS329A Lecture 5, Planning and Multi-Step Reasoning](https://www.youtube.com/watch?v=Ml_fp9XkB8Y) (Mirhoseini), presenting LATS, *Spring* (NeurIPS 2025), and SWiRL. The Spring dependency-graph slides are the ones to look at.
3.  [Yao et al., *Tree of Thoughts*](https://arxiv.org/abs/2305.10601), the simpler ancestor. Read it first if tree search over model states is new to you.

[[Agents 07 — Tools as ground truth|← Lesson 7]] · [[Agents 09 — Buy or tune|Lesson 9: Buy or tune →]]
