---
type: unit
course: "[[Causal Inference through Football]]"
status: not-started
order: 12
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

A synthetic control combines untreated units to reproduce one treated unit before intervention. The pre-treatment fit and placebo gaps show whether the comparison is useful.

About 20 minutesIncludes guided exercises[[Football CI 12 — Build and assess a synthetic control|Exercise sheet]]

## What you will be able to do

You will be able to define a donor pool, interpret synthetic-control weights and assess fit and placebo evidence.

## Two football applications

Breidenbach and Mitze built a separate synthetic control for each of 41 German districts that hosted professional football matches. Kleven, Landais and Saez built synthetic-country comparisons to study how tax reforms changed football-player migration. Both applications need an untreated trajectory for a treated place.

## Choose the treated unit and donor pool

The treated unit must have a clear intervention date and enough outcome history before treatment. The donor pool contains untreated units that could represent the treated unit's counterfactual. Donors exposed to the same intervention or strong spillovers should be removed.

The method chooses non-negative weights that sum to one. The weighted donor outcome should reproduce the treated unit's pre-treatment outcomes and important predictors.

Synthetic outcome at time t = sum of donor weight\_j x donor outcome\_jt

## Worked construction

A treated district's synthetic control uses three donors with weights 0.50, 0.30 and 0.20. Their infection rates in one week are 10, 14 and 8 per 100,000 people. The synthetic rate is 0.50 times 10, plus 0.30 times 14, plus 0.20 times 8, which equals 10.8.

If the treated district's rate is 15, the treated-minus-synthetic gap is 4.2. The gap is meaningful only if the weighted donors closely reproduced the district before hosting resumed.

## Pre-treatment fit is the main design check

A large post-treatment gap is not persuasive when the synthetic control missed the treated trajectory before intervention. Report the complete path and a pre-treatment fit measure such as root mean squared prediction error. Do not hide poor pre-periods.

The analyst should also test whether results depend on one donor, one predictor or the chosen pre-treatment window. A leave-one-out analysis can show whether one heavily weighted donor drives the result.

## Use placebo assignments for context

Placebo-in-space analysis reassigns treatment to each donor and calculates its gap. If many untreated donors show gaps as large as the treated unit, the observed result is not unusual. Comparisons should account for pre-treatment fit, because a donor with terrible pre-fit can generate a large meaningless post-gap.

Placebo-in-time analysis moves the intervention date to an earlier untreated period. A large effect at the fake date suggests that the model can create gaps without treatment.

## Exercise 1. Inspect the donor pool

A neighbouring district sends many supporters to the treated district and may receive returning infections. Should it remain in the donor pool?

Show a suggested answer

Probably not without a model of spillovers. The neighbour may be indirectly treated, so its outcome no longer represents an untreated counterfactual. The analyst should remove it or test how its inclusion changes the estimate.

## Exercise 2. Read a poor fit

The treated unit has a pre-treatment fit error four times larger than most placebo units. What should the analyst do?

Show a suggested answer

The analyst should not compare raw post-treatment gaps as if fit were equal. The design may be unsuitable for that unit. At minimum, report the poor fit, use post-to-pre error ratios and test alternative defensible donor or predictor choices.

## Practice

An [[Football CI 12 — Build and assess a synthetic control|exercise sheet of 7 questions]] accompanies this lesson. It takes about thirty minutes and carries worked answers at the end. Attempt it before moving on.

## Check yourself

Which result most weakens the design? Poor pre-treatment outcome trajectory fit Non-negative donor weights that sum Clearly defined intervention timing usedCheck answer


> [!tip]- Reveal the answer
> Correct. Poor pre-treatment fit weakens the claim that the synthetic path represents the missing untreated outcome.

## What to remember

- A synthetic control is a weighted combination of untreated donors.
- The donor pool must avoid treatment and important spillovers.
- Good pre-treatment fit is necessary for a credible counterfactual path.
- Placebo gaps show whether the treated gap is unusual relative to untreated units.

**Primary reading.** [Breidenbach and Mitze (2022)](https://pmc.ncbi.nlm.nih.gov/articles/PMC8411384/); [Kleven, Landais and Saez (2013)](https://eml.berkeley.edu/~saez/kleven-landais-saezMay12football.pdf); and Ruiz de Villa, Section 11.3.

If any step is unclear, ask your teaching agent to work through another football example with you.

[[Football CI 11 — Interpret an instrumental-variable estimate|Previous lesson: Interpret an instrumental-variable estimate]][[Football CI 13 — Analyse an interrupted time series|Next lesson: Analyse an interrupted time series]]
