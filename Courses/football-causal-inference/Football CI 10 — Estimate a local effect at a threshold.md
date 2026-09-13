---
type: unit
course: "[[Causal Inference through Football]]"
status: not-started
order: 10
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Regression discontinuity compares units immediately above and below a treatment threshold. The estimate is local, and its credibility depends on continuity at the cut-off.

About 20 minutesIncludes guided exercises[[Football CI 10 — Estimate a local effect at a threshold|Exercise sheet]]

## What you will be able to do

You will be able to define the running variable and cut-off, interpret a sharp regression-discontinuity estimate and assess its main diagnostics.

## Football thresholds

Reilly and Witt used the sixth and seventh position split in the Scottish Premiership to estimate how league design affected attendance. Speer compared teams close to promotion and relegation cut-offs to study later sporting and financial outcomes. These settings use a ranking or points measure that determines treatment at a threshold.

## Define the design

The running variable is the measure that determines treatment. The cut-off is the value where treatment changes. A sharp design assigns treatment deterministically at the cut-off. A fuzzy design changes the probability of treatment but allows exceptions.

Local effect = outcome limit just above cut-off - outcome limit just below cut-off

The comparison uses teams very close to the cut-off. If their untreated potential outcomes would change smoothly through the threshold, a jump in observed outcomes can be attributed to treatment.

## Worked interpretation

Suppose clubs finishing just above a promotion cut-off have average next-season revenue of 18.2 million pounds, while the fitted outcome just below the cut-off is 13.7 million pounds. The local discontinuity is 4.5 million pounds. The estimate describes clubs at the margin of promotion, not champions or clubs far below the cut-off.

The estimate can include every consequence of promotion, including broadcast income, attendance and player decisions. It is the total effect of crossing the threshold under the assignment rule.

## Check continuity and manipulation

The continuity assumption fails if another cause of the outcome jumps at the same threshold. Researchers should inspect predetermined club characteristics on both sides and test fake cut-offs where treatment does not change.

Precise manipulation is a concern when teams can sort around the threshold using information related to their potential outcomes. Football positions are earned through match results, so strategic effort is expected. The key question is whether teams can precisely control the final running variable near the threshold. Rules, tie-breakers and late-season incentives need careful discussion.

## Fit the local comparison

The researcher should plot binned outcome means and fit separate local trends on each side. A narrow bandwidth improves local comparability but uses fewer observations. A wide bandwidth adds precision but relies on stronger functional-form assumptions. High-order global polynomials can behave badly near boundaries and should not be the default.

## Exercise 1. Identify the parts

In a study of relegation, name the treatment, running variable, cut-off and local population.

Show a suggested answer

The treatment is relegation. The running variable can be final league points or rank under the exact relegation rule. The cut-off separates the final relegated and surviving positions. The local population consists of teams whose final standing is close to that boundary.

## Exercise 2. Interpret external validity

Why should an estimate for sixth versus seventh place not be applied directly to the league winner?

Show a suggested answer

The design identifies an effect for clubs near the split threshold. The league winner differs in quality, supporter expectations and schedule consequences. Applying the estimate to that club requires an additional assumption that the effect is stable far from the cut-off.

## Practice

An [[Football CI 10 — Estimate a local effect at a threshold|exercise sheet of 7 questions]] accompanies this lesson. It takes about thirty minutes and carries worked answers at the end. Attempt it before moving on.

## Check yourself

Which clubs does the estimate primarily describe? Every football club across all divisions Clubs located close to the threshold Only clubs far above the thresholdCheck answer


> [!tip]- Reveal the answer
> Correct. Regression discontinuity primarily identifies the treatment effect for units near the cut-off.

## What to remember

- The running variable determines treatment at a cut-off.
- The continuity assumption supports a local causal comparison.
- Manipulation, predetermined-variable jumps and fake cut-offs are important diagnostics.
- The estimate applies most directly to units near the threshold.

**Primary reading.** [Reilly and Witt (2021)](https://eprints.lse.ac.uk/113344/1/1527002521989393.pdf); [Speer (2023)](https://doi.org/10.1016/j.serev.2022.100003); Angrist and Pischke, Chapter 4; and Hayashi, Chapter 21.

If any step is unclear, ask your teaching agent to work through another football example with you.

[[Football CI 09 — Interpret an event study with staggered timing|Previous lesson: Interpret an event study with staggered timing]][[Football CI 11 — Interpret an instrumental-variable estimate|Next lesson: Interpret an instrumental-variable estimate]]
