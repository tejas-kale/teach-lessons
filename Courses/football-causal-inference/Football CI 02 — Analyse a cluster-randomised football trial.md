---
type: unit
course: "[[Causal Inference through Football]]"
status: not-started
order: 2
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Football interventions are often assigned to whole teams. The analysis must respect that assignment and distinguish the effect of assignment from the effect of adherence.

About 20 minutesIncludes guided exercises[[Football CI 02 — Analyse a cluster-randomised football trial|Exercise sheet]]

## What you will be able to do

You will be able to identify the experimental unit, explain why outcomes within a team are related, and interpret an intention-to-treat estimate.

## The football study

Al Attar and colleagues studied whether adding ten minutes of FIFA 11+ exercises after training reduced injuries. Both groups completed the usual twenty-minute FIFA 11+ programme before training. The researchers assigned 21 amateur men's teams, rather than individual players, to the two programmes. Ten teams with 160 players received the added exercises, and 11 teams with 184 players did not.

The team was the unit of random assignment because players train together. If players within one team had received different programmes, they could have copied exercises or changed the shared training session. Team assignment reduced this contamination.

## Random assignment creates the comparison

Random assignment makes the assigned groups exchangeable on average before treatment. Measured and unmeasured causes of injury should have similar distributions across the two groups, apart from chance differences. The design therefore identifies the effect of assignment without requiring an observational adjustment model.

Randomisation does not guarantee identical groups in one small trial. With only 21 teams, chance imbalances can occur. A baseline table helps readers understand the realised groups, but a significance test on every baseline variable does not validate or invalidate randomisation. The assignment procedure, allocation concealment and loss to follow-up are more important.

## The independent unit is the team

Players within the same team share coaching, pitches, match schedules and training loads. Their injuries are therefore correlated. An analysis that treats 344 players as 344 unrelated experimental units will usually produce standard errors that are too small. The analysis must account for clustering through a cluster-level comparison, generalised estimating equations, a mixed model or another suitable method.

The number of clusters often matters more for precision than the total number of players. Adding more players to the same teams gives less independent information than adding comparable teams.

## Worked interpretation

The experimental group reported 26 injuries over the season, while the control group reported 82. The paper used generalised estimating equations and an intention-to-treat analysis. The estimate compares teams according to their assigned programme, even if some players missed sessions.

This is the effect of offering and assigning the added post-training programme under the adherence observed in the trial. It is not the effect that every player would experience under perfect adherence. A per-protocol effect would answer that second question, but adherence is no longer random and would require additional assumptions and methods.

## Exercise 1. Identify the units

A league randomises 30 clubs to receive either a concussion training course for all coaches or the usual guidance. Researchers record knowledge scores for 12 coaches per club. Name the unit of assignment, the unit of observation and a suitable level for uncertainty.

Show a suggested answer

The club is the unit of assignment, and each coach is an observed unit. The analysis must account for 30 independent club allocations and for correlated scores among coaches in the same club. Treating 360 coaches as independently randomised would misstate the design.

## Check the main assumptions

- **Allocation.** The random sequence must be generated and concealed before teams enter their groups.
- **Interference.** Teams should not share enough training information to change one another's outcomes.
- **Outcome measurement.** Injury definitions and exposure recording should be applied in the same way in both groups.
- **Missing data.** Team or player dropout should not break the assigned comparison without careful handling.

## Exercise 2. Choose the estimand

A coach asks, "Does the programme work when players actually complete every session?" Explain why the trial's intention-to-treat estimate does not answer the question directly.

Show a suggested answer

Intention to treat compares assignment to the programme, not perfect completion. Players who complete every session may differ in fitness, motivation or injury risk from players who do not. A comparison based on completion loses the original randomisation and needs an explicit per-protocol analysis.

## Practice

An [[Football CI 02 — Analyse a cluster-randomised football trial|exercise sheet of 7 questions]] accompanies this lesson. It takes about thirty minutes and carries worked answers at the end. Attempt it before moving on.

## Check yourself

Which unit received the random assignment? Each recorded football injury event Each individual amateur football player Each participating amateur football teamCheck answer


> [!tip]- Reveal the answer
> Correct. The team received the random assignment, so the analysis must account for clustering by team.

## What to remember

- Random assignment identifies the effect of assignment when the trial is conducted and analysed as designed.
- The analysis must respect the unit that received the random assignment.
- Intention to treat estimates the effect of assignment under observed adherence.
- A per-protocol effect requires further assumptions because adherence is not random.

**Primary reading.** [Al Attar et al. (2017)](https://pubmed.ncbi.nlm.nih.gov/28939307/); Hernan and Robins, *What If*, Chapters 2 and 9; and Sweet, *Experimentation for Engineers*, Chapters 1 and 2.

If any step is unclear, ask your teaching agent to work through another football example with you.

[[Football CI 01 — Define a causal question and estimand|Previous lesson: Define a causal question and estimand]][[Football CI 03 — Use a causal diagram to choose controls|Next lesson: Use a causal diagram to choose controls]]
