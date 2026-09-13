---
type: unit
course: "[[Causal Inference through Football]]"
status: not-started
order: 7
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Fixed effects remove stable differences within a panel unit. They do not remove every changing shock, and they do not prevent regression to the mean after a poor run.

About 20 minutesIncludes guided exercises[[Football CI 07 — Use fixed effects without mistaking recovery for impact|Exercise sheet]]

## What you will be able to do

You will be able to state what team-season fixed effects remove, explain the Ashenfelter dip and identify remaining time-varying confounding.

## The apparent recovery after dismissal

De Paola and Scoppa studied coach dismissals in Serie A. A simple before-and-after comparison often shows better results after a new coach arrives. The improvement may look persuasive because dismissal usually follows an extreme run of poor results. However, unusually bad results tend to move back towards a team's usual level even without intervention.

## Fixed effects use changes within the same unit

A team-season fixed effect gives each team in each season its own intercept. It absorbs characteristics that do not change during that team-season, including much of the squad's underlying quality, the club's broad resources and its season objective. Calendar effects can also absorb shocks common to all teams at a given time.

Points\_it = team-season effect\_i + time effect\_t + beta dismissal\_it + controls\_it + error\_it

The dismissal coefficient is identified from changes within a team-season after accounting for the included time patterns and controls. The comparison is not driven by permanent differences between a wealthy title contender and a newly promoted club.

## The Ashenfelter dip is a selection pattern

Treatment often begins after an outcome has fallen unusually far. If the outcome later returns towards its normal level, a before-and-after analysis assigns that recovery to treatment. This pattern is called the Ashenfelter dip in programme evaluation.

Coach dismissal is a clear example. A board reacts to recent defeats, but defeats contain both persistent weakness and temporary bad luck. The temporary part can reverse. A suitable comparison must have a similar pre-treatment path and face a similar post-treatment schedule.

## Worked example

A club usually earns 1.4 points per match. It earns 0.4 points per match over five unusually poor matches, dismisses the coach and then earns 1.3 points per match. The before-and-after change is +0.9. If a comparable club with the same temporary slump would also have returned to 1.3 without dismissal, the causal effect is zero.

A team-season fixed effect accounts for the 1.4-point long-run level, but it does not by itself account for the temporary shock that both caused dismissal and later reversed.

## List the threats that still change over time

- Opponent quality and home-away composition may differ before and after dismissal.
- Injuries, suspensions and player returns can cause both the board's decision and future points.
- The board may anticipate dismissal, and players may react before the recorded treatment date.
- Serial correlation within teams affects uncertainty and requires suitable standard errors.

## Exercise 1. Sort the variables

Which variables are absorbed by a team-season fixed effect, and which need separate handling: stadium capacity, current opponent strength, season-long ownership model and an injury to the first-choice goalkeeper?

Show a suggested answer

Stable stadium capacity and a stable ownership model are absorbed within that team-season. Current opponent strength and a new goalkeeper injury change over time, so the fixed effect does not remove them.

## Exercise 2. Build a better comparison

Suggest a comparison that would help separate coach impact from recovery after a slump.

Show a suggested answer

Compare dismissed teams with retained-coach teams that had similar recent form, season position, squad strength and opponent schedules. Inspect several pre-treatment matches to confirm that the paths were similar before the decision. The comparison still relies on measured causes, but it addresses the dip more directly than a simple before-and-after design.

## Practice

An [[Football CI 07 — Use fixed effects without mistaking recovery for impact|exercise sheet of 7 questions]] accompanies this lesson. It takes about thirty minutes and carries worked answers at the end. Attempt it before moving on.

## Check yourself

What do team-season fixed effects remove? Every changing performance shock completely Stable team-season characteristics from comparisons All measurement error in outcomesCheck answer


> [!tip]- Reveal the answer
> Correct. Team-season fixed effects remove characteristics that remain stable within that team and season.

## What to remember

- Fixed effects remove stable unobserved differences within the chosen panel unit.
- They do not remove time-varying common causes.
- A poor run before treatment can produce an apparent recovery through regression to the mean.
- The comparison should reproduce both the level and pre-treatment path of treated teams.

**Primary reading.** [De Paola and Scoppa (2012)](https://doi.org/10.1177/1527002511402155); Wooldridge, *Introductory Econometrics*, Chapters 13 and 14; and Hayashi, *Econometrics*, Chapter 17.

If any step is unclear, ask your teaching agent to work through another football example with you.

[[Football CI 06 — Compare weighting, standardisation and AIPW|Previous lesson: Compare adjustment estimators]][[Football CI 08 — Estimate a difference-in-differences effect|Next lesson: Estimate a difference-in-differences effect]]
