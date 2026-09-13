---
type: unit
course: "[[Self-Improving AI Agents]]"
status: not-started
order: 7
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Feedback comes from three places: the environment, an execution, or the model's own written principles. The quality of your error messages turns out to be a performance parameter.

About 10 minutes · Lecture 4 · Where verification stops being a model and becomes the world

Lessons 4 to 6 built increasingly sophisticated machinery for judging outputs with models. Lecture 4 makes the point that has to be made somewhere, which is that the best verifier is not a model at all. It is **something that happened**. Code ran or it did not. The query returned 400 rows or none. The API accepted the payload or rejected it with a reason.

The lecture organises this around three papers, separated by where the feedback comes from.

<table>
<thead>
<tr>
<th>Method</th>
<th>Feedback comes from</th>
<th>Signal quality</th>
<th>Available when</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>ReAct</strong></td>
<td>The environment, through searches, lookups, and observations</td>
<td>Grounded but noisy</td>
<td>You have tools that report state</td>
</tr>
<tr>
<td><strong>RLEF</strong></td>
<td>Execution, by running code against tests</td>
<td>Crisp, close to an oracle</td>
<td>The task compiles and runs</td>
</tr>
<tr>
<td><strong>Constitutional AI</strong></td>
<td>The model, checked against written principles</td>
<td>Weak, but always available</td>
<td>Always, which makes it the fallback</td>
</tr>
</tbody>
</table>

## ReAct, interleaving thought and action

ReAct is now so absorbed into how models behave that its novelty is easy to miss.[¹](#s1) Before it there were two separate lineages. Chain-of-thought models reasoned well but were ungrounded and hallucinated confidently, and tool-using models acted without visibly reasoning. ReAct interleaves them as **thought, then action, then observation**, repeatedly, with the thought appended to the context so the next action is conditioned on it.

The gain shows up exactly where reasoning alone fails for lack of facts, which is multi-hop question answering, fact verification, and web navigation. Reasoning alone hallucinates and acting alone flails, and together the observations correct the reasoning while the reasoning directs the actions. One detail is worth keeping: ReAct alone does not always beat chain-of-thought, but ReAct combined with self-consistency does. Grounding and sampling complement each other rather than substituting for each other.

Frontier models now do this inside their chain of thought without being asked, and models like Qwen show the pattern because they were distilled on traces of this shape. That is why the paper reads as obvious today. Its structural claim is not obsolete. An interleaved trace of thought, action, and observation is far easier to interpret and debug than either half alone, which is why you should log all three.

The named failure mode is **error cascading**. One wrong observation early poisons every step after it, and the model rarely goes back to check. Lesson 13 returns to this, because it is the dominant failure of long-horizon runs.

## RLEF, and the public and private test split

RLEF trains coding agents with reinforcement learning, where the actions are code and the observations are execution feedback.[²](#s2) The loop generates a solution, runs it against public tests, feeds failures back for another attempt, and once it passes, computes the training reward from a held-out private test set.

![[a6ccbb7a946773805228f128f30576b79a2b3ef6.svg]]

*The separation is the whole trick. Tests the model can see become part of the problem statement and get satisfied specifically, and tests it cannot see measure whether the solution generalises. Any agent that iterates against a check will eventually satisfy the check rather than the intent behind it.*

That separation is the transferable idea, and it has nothing to do with RL. **Whenever your agent iterates against a check, hold some checks back.** If it retries until the assertions pass, the assertions have become the specification and you have no evidence about anything they do not cover. This is Goodhart's law with a tighter loop, and the fix costs nothing. Split your checks, show half, and score on the other half.

> [!abstract] Your error messages are a hyperparameter
> RLEF's measured finding is about repair behaviour. With execution feedback in the loop, errors fall turn over turn and the model's edits become targeted repairs, meaning it fixes the part that broke. Without the loop, later attempts are not reliably better, because it rewrites instead of repairing. The feedback text itself, such as "execution timeout", is what makes that difference, separately from the binary pass or fail used as the training reward. An agent handed "AssertionError" has almost nothing to work with. The same agent handed the failing input, the expected value, the actual value, and a stack trace can usually fix the bug on the next turn. If you wrap tools for agents, the richness of the failure path is doing more work than your prompt is. One caveat about how far this generalises. CodeContests problems are short, at a few lines each. Whether binary pass or fail is sufficient there while richer traces become necessary on larger problems was raised in the lecture as a student's conjecture, and the lecturer agreed it was plausible and untested. Treat it as a good hypothesis about your own codebase rather than as a result.

## Check yourself

Your agent retries a data-cleaning task until all your validation checks pass. What does a clean run now tell you?

- [ ] The data is clean, since every validation rule you wrote is now satisfied by the output.
- [ ] Nothing at all, because agents that iterate cannot produce trustworthy output of any kind.
- [ ] The agent has improved, since fewer retries are needed than on the previous comparable run.
- [ ] Only that those checks pass. You have no evidence about anything they fail to cover.

> [!tip]- Reveal the answer
> The checks stopped being an independent measurement the moment they became a target the agent optimises against. It may have dropped the offending rows, coerced a type to silence a complaint, or special-cased the failing key. Hold back a validation set the agent never sees and score there instead. The second answer overstates the case, because iterating against checks is good and you should do it. You simply cannot also use those checks as your evidence.

## Constitutional AI, when there is no world to consult

Sometimes nothing can be executed. Constitutional AI handles that case by replacing human preference labels with a written **constitution** of explicit principles, and having the model critique and revise its own output against them.[³](#s3) In the supervised stage the model responds, critiques itself against a principle, revises, and then trains on the revisions. In the RL stage a preference model built from the constitution scores responses and the policy optimises against it.

Two things are worth taking from it. The mechanism is self-critique made specific, because with a constitution of 16 written principles the model is not asked whether something is good, it is asked whether the output violates a named principle. That is the same move as replacing a rating scale with a binary checklist in Lesson 5, and it works for the same reason. And the results are presented honestly as a trade between helpfulness and harmlessness rather than as a free win, with the constitution being where you write the trade down.

The lecture adds a caution from the follow-on literature. Getting a model to critique itself is harder than it looks, because models are overconfident about what they do not know, and a consensus of other models often critiques better. That is Lesson 5's independence argument arriving from another direction. Where you can afford it, prefer a critic from another model family over self-critique.

The version for your work: a constitution is an explicit written statement of what a good output must satisfy in your domain. A house style for analyses, a set of statistical practices, the caveats a report must always carry. Once written down it works as both a critique prompt and an eval rubric. Most teams keep this knowledge in reviewers' heads, where an agent cannot reach it.

## Design rules for agent-facing tools

Here is the lecture reduced to what to change.

- **Return rich failures**, meaning expected against actual, the offending input, and the stack trace. Prefer a 400-word error to a boolean.
- **Make tools report what they did**, such as rows written, files changed, or bytes read, so the agent has an observation to reason about. A tool that returns "OK" gives it nothing.
- **Fail loudly rather than degrading quietly.** A tool that returns an empty result on error teaches the agent that its approach worked.
- **Split checks into shown and held-out**, which stops being optional the moment anything iterates.

Two more rules matter once you are debugging rather than building. Write the constitution down for domains with no oracle, and use the same document as your critique prompt and your eval rubric. And log thought, action, and observation separately, because when a run goes wrong you need to know whether the reasoning was bad or the observation was, and those need completely different fixes.

Project hook

How much does failure-message richness actually buy? Take one tool your agent calls often and prepare three versions of its error path: a bare boolean, a one-line message, and a full trace with expected and actual values. Run the same task set through each.

Measure Success rate and turns-to-fix under each version. This is the cleanest controlled experiment in the whole course, because the only thing that changes is a string, and it is a strong Arc V candidate for that reason.

## Retrieval Practice

Take a tool your agents call regularly. What does it return on failure? Rewrite that failure message the way you would want an agent to receive it, then say what the agent could now do that it could not before.

> [!example]- Model answer
> A typical before is "Error: query failed". A good after is the failing SQL, the database's own error text, the line and column, the schema of the tables referenced, and where it is cheap, a hint such as "column user_id not found, the table has userId". The agent goes from guessing at a rewrite, and often rewriting the whole query and losing the correct parts, to making a one-token fix. The general principle decides nearly every case: return whatever a competent colleague would need in order to fix it without asking you a follow-up question.

## What you can now do

You can classify a feedback source as environmental, execution-based, or self-critique, and you know their relative strength. You have the public and private split as a defence against your own agent gaming your checks, and a concrete rule for what tools should return. Rich failures are the cheapest performance improvement in this course.

Next is [[Agents 08 — Planning and multi-step reasoning|Lesson 8, planning and multi-step reasoning]], and the question of who decides what the steps are.

**Send a tool wrapper.** Tool interface design is unglamorous, consequential, and easy to review. Paste one in and we will rewrite its failure paths.

Read one thing

1.  [Yao et al., *ReAct: Synergizing Reasoning and Acting in Language Models*](https://arxiv.org/abs/2210.03629). Read it for the trace examples, which are still the clearest illustration of grounded reasoning.
2.  **Read this one:** [Gehring et al., *RLEF: Grounding Code LLMs in Execution Feedback*](https://arxiv.org/abs/2410.02089). The public and private split, and the discussion of how much feedback detail is enough, apply directly even if you never touch RL.
3.  [Bai et al., *Constitutional AI: Harmlessness from AI Feedback*](https://arxiv.org/abs/2212.08073). Skim the method and read the results on the helpfulness and harmlessness trade.
4.  [CS329A Lecture 4, Learning from Feedback with Tools and Code](https://www.youtube.com/watch?v=Lxh9RF5S-K0), the source for this lesson.

[[Agents 06 — Inference architectures|← Lesson 6]] · [[Agents 08 — Planning and multi-step reasoning|Lesson 8: Planning and multi-step reasoning →]]
