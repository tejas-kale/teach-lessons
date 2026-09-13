---
type: unit
course: "[[Causal Inference through Football]]"
status: not-started
order: 14
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Repeated treatment decisions create a special problem when a variable is both changed by earlier treatment and used to choose later treatment. Longitudinal g-methods are designed for that feedback.

About 25 minutesIncludes guided exercises[[Football CI 14 — Choose a method for time-varying treatment|Exercise sheet]]

## What you will be able to do

You will be able to recognise treatment-confounder feedback and compare the parametric g-formula, marginal structural models and structural nested models.

## Evidence gap

The paper search did not find a credible football application of longitudinal g-methods. This lesson uses a constructed training-load example that is informed by the injury studies. The example is not a method attributed to those papers.

## Weekly training-load decisions

Each Monday, a coaching team chooses a high or moderate training load. Current fatigue affects the choice, because fatigued players are less likely to receive high load. Fatigue also predicts injury. Earlier training load changes current fatigue. The analyst wants the season-long effect of a sustained high-load strategy compared with a moderate-load strategy.

## Ordinary adjustment creates a conflict

If the analyst ignores weekly fatigue, later load is confounded. If the analyst adjusts for fatigue in an ordinary outcome regression, the model blocks part of the effect of earlier load that passes through fatigue. It can also create selection bias. Fatigue is both a mediator of earlier treatment and a confounder of later treatment.

This structure is called treatment-confounder feedback. The treatment and covariate history must be handled in time order.

## Define a treatment strategy

A longitudinal estimand compares complete strategies, not isolated weekly choices. One strategy might set moderate load whenever fatigue exceeds a threshold and high load otherwise. Another might always set moderate load. Eligibility, outcome horizon and the response to changing player history must be specified.

**Parametric g-formula**

Fit models for the time-varying covariates and outcome. Simulate each player forward under each strategy, then average the simulated outcomes.

**Marginal structural model**

Weight each observed treatment history by the inverse probability of receiving it. The weighted data create sequential balance for measured histories.

**Structural nested model**

Use g-estimation to find the effect that removes the association between treatment and a transformed outcome under the treatment model.

## Worked two-week history

A player receives high load in week 1 and becomes fatigued. Fatigue leads the coach to choose moderate load in week 2. A standard regression that adjusts for week-2 fatigue compares players at the same fatigue level, which blocks part of the week-1 effect. An inverse-probability weighted marginal structural model instead weights the observed two-week treatment sequence using the probabilities of week-1 treatment given baseline and week-2 treatment given the full past history.

The product weight can become large when a treatment choice is rare for a history. Stabilised weights can reduce variance. Weight truncation limits extremes but changes the estimator and introduces bias, so it must be reported.

## State the sequential assumptions

- **Sequential exchangeability.** At each week, measured history includes the common causes of that week's load and later injury.
- **Sequential positivity.** Each strategy remains possible for every history in the target population.
- **Consistency.** Weekly load levels and dynamic rules represent clear interventions.
- **Correct time order.** Covariates are recorded before the treatment decision they are meant to confound.

## Exercise 1. Find the feedback variable

Earlier minutes played increase soreness. Soreness affects later minutes and future injury. Explain the two roles of soreness.

Show a suggested answer

Soreness is a mediator of earlier minutes because earlier play changes soreness. It is also a confounder of later minutes and injury because coaches use soreness to choose later exposure and soreness predicts injury.

## Exercise 2. Diagnose positivity

Club policy forbids high load whenever fatigue exceeds 8 on a 10-point scale. Can the data estimate a strategy that assigns high load at fatigue 9?

Show a suggested answer

No. The policy creates a structural positivity failure for that history. The observed data contain no high-load outcomes at fatigue 9, so the analyst must redefine the target strategy or obtain data from a setting where that decision occurs.

## Practice

An [[Football CI 14 — Choose a method for time-varying treatment|exercise sheet of 7 questions]] accompanies this lesson. It takes about thirty minutes and carries worked answers at the end. Attempt it before moving on.

## Check yourself

Which variable creates treatment-confounder feedback? Stable player height measured once Final injury status after follow-up Weekly fatigue before training decisionsCheck answer


> [!tip]- Reveal the answer
> Correct. Weekly fatigue responds to earlier load and affects both later load and injury.

## What to remember

- Treatment-confounder feedback occurs when a covariate is changed by past treatment and confounds later treatment.
- Longitudinal estimands compare complete treatment strategies.
- The g-formula, marginal structural models and structural nested models handle the feedback in different ways.
- Sequential exchangeability and positivity must hold at each decision time.

**Primary reading.** Hernan and Robins, *What If*, Chapters 19 to 21.

If any step is unclear, ask your teaching agent to work through another football example with you.

[[Football CI 13 — Analyse an interrupted time series|Previous lesson: Analyse an interrupted time series]][[Football CI 15 — Use double machine learning for a causal effect|Next lesson: Use double machine learning for a causal effect]]
