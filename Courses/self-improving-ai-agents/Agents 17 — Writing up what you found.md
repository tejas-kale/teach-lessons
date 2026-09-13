---
type: unit
course: "[[Self-Improving AI Agents]]"
status: not-started
order: 17
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

One page, five parts, written whether the result was positive or null. The writeup is what turns a month of runs into something you and other people can use.

About 8 minutes · Closes the course · Produces the artefact

The temptation at the end of an experiment is to note the answer, apply it, and move on. Resist it for one afternoon. A month from now you will remember the conclusion and not the conditions under which it held, and the conditions are what determine whether it still applies.

You are not writing a paper. One page is the right length, and it has five parts.

## The five parts

1.  **The question and the prediction,** copied verbatim from your pre-registration. Copying rather than rewriting is the point, because it preserves what you actually believed before you saw the data.
2.  **The setup,** in enough detail to repeat it. The task set and its size, the model string, the conditions, the grader, and the number of runs. This is the part you will need most later and the part most likely to be skipped.
3.  **The result,** as a number with its spread, next to the noise floor from your null experiment. One small table or one chart is plenty.
4.  **What surprised you.** The gap between your prediction and the outcome, plus anything you found while reading raw runs by hand. This is usually the most valuable paragraph in the document.
5.  **What you would do next, and what you now believe.** Two or three sentences, written as a claim with its scope attached.

> [!abstract] The scope sentence
> Every finding needs its boundary stated in the same breath. Not "richer error messages help", but "richer error messages cut turns-to-fix by about a third on SQL tasks where the failure was a schema mismatch, with no measurable effect on tasks that failed for logical reasons". The second version is usable a year later by someone else. The first is a slogan, and slogans get applied where they do not hold.

## Write the null up too

A null result is a real result and it is worth the same page. It stops you repeating the attempt in six months, it stops a colleague starting the same experiment, and it is genuine evidence about your domain. The field has a publication bias against nulls, and you do not have to inherit it, because your writeup answers to you rather than to a reviewer.

The only null worth discarding is the one your experiment could not have detected, and Lesson 16's null experiment tells you which kind you have. If your noise floor was larger than any plausible effect, write down that the experiment was underpowered and what a better-powered version would need. That is a finding about your method rather than about your hypothesis, and it is still worth knowing.

## Check yourself

Which sentence belongs in a writeup as a finding?

- [ ] Ensembling three judges beat the best single judge by 11 points, against a 3-point noise floor.
- [ ] Ensembling judges works considerably better than relying on a single judge model does.
- [ ] We found that verification quality is the main constraint on this particular agent workflow.
- [ ] The ensemble approach seems promising and is worth exploring further across other teams.

> [!tip]- Reveal the answer
> The first has an effect size and a noise floor, so a reader can judge it and you can compare it against a future run. The second states a direction with no magnitude and no comparison. The third is a conclusion that the stated experiment probably cannot support on its own. The fourth is a feeling. The test to apply to your own sentences is whether someone could disagree with them using data, and only the first passes.

## Where the writeup goes

Put it in this workspace, in a `projects/` directory, named with the date and the question. Then do two more things with it.

**Write a learning record** in `learning-records/` capturing what the result changes about how you work. The writeup records what happened, and the learning record records what it means for the next decision. Those are different documents and the second one is what future sessions read.

**Show it to someone who runs agents.** This is the wisdom half of the course, and it is the half a workspace cannot supply. A result about your own workflows gets sharper the moment someone with different workflows asks why you think it generalises. `RESOURCES.md` lists places where that conversation happens, and one thoughtful reader is worth more than a hundred views.

Do this now

Write the page. Then write the one-sentence version of your finding, with its scope attached, and check whether you would still believe it if someone applied it to a workflow two steps removed from yours.

Measure If the sentence survives that test unchanged, the scope is too vague. A well-scoped finding should visibly stop applying somewhere.

## Retrieval Practice

Retrieval practice, and the close of the course

> [!example]- Model answer
> The shape of a good answer is a number attached to a decision. Something like: "coverage on my analysis tasks is 71 percent while pass@1 is 34 percent, so the gap is where my effort belongs, and the three-verifier ensemble closed about a third of it for two hours of work. What I could not have predicted is that the deterministic check contributed almost nothing, because it never fired on real failures, and the value came from the rubric judge from a different model family." Notice that the surprising part is specific to your data and could not have been read out of any paper. That is what running it buys.

## Where this leaves you

You have done the thing the original course asks its students for. A question, an experiment with a control, a measured result, and a written account of what it means, at a scale one person can carry. That is a different kind of knowledge from the fourteen lessons before it, because it is about your system rather than about the field.

The loop from here is short. A result raises the next question, and the next question is usually smaller and sharper than the first one was. That is what the project hooks are for, and there are sixteen of them left.

**Bring the writeup.** The most useful conversation in the whole course is the one about what your result does and does not license you to conclude, and it is much better had with the numbers in front of both of us. After that we pick the next question, which will be a better one than the first.

Read one thing

1.  **Primary source:** [CS329A Lecture 1, Course Overview](https://www.youtube.com/watch?v=6YnLB0XbTnI), on what the course accepts as a project result. The demand for a stated hypothesis and a described improvement is the standard this arc aims at.
2.  [Kwa et al., *Measuring AI Ability to Complete Long Tasks*](https://arxiv.org/abs/2503.14499) (METR), worth rereading as a model of scope statement. Its limitations section is more careful than most papers' results sections.
3., which holds the pre-registration template and the writeup skeleton in one page you can print.

[[Agents 16 — Running the experiment|← Lesson 16]] ·
