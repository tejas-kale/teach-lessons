---
type: unit
course: "[[Causal Inference through Football]]"
status: not-started
order: 8
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Difference in differences subtracts a comparison group's change from a treated group's change. The design is credible only when the comparison group represents the treated group's untreated trend.

About 20 minutesIncludes guided exercises[[Football CI 08 — Estimate a difference-in-differences effect|Exercise sheet]]

## What you will be able to do

You will be able to calculate a difference-in-differences estimate and explain the parallel-trends assumption in football terms.

## Matches without supporters

Scoppa used closed-door matches during the COVID-19 period to study how the absence of crowds changed player and referee behaviour. The data covered five countries and ten seasons. The design compares the change in affected matches with a change in a comparison group.

A simple pre-period and post-period comparison is not enough. Many things changed during the pandemic, including fixture congestion, training routines, travel and player fitness. Difference in differences tries to remove changes that the treated and comparison groups would have shared.

## Calculate the two differences

First calculate the treated group's change from before to after. Then calculate the comparison group's change over the same periods. Subtract the second change from the first.

DiD = (treated post - treated pre) - (control post - control pre)

## Worked calculation

Suppose the average home goal difference falls from +0.40 to +0.10 in the closed-door group. Its change is -0.30. In the comparison group, home goal difference falls from +0.35 to +0.25. Its change is -0.10. The difference-in-differences estimate is -0.30 minus -0.10, which equals -0.20 goals.

The estimate says that home goal difference fell by 0.20 goals more in the closed-door group than the contemporaneous change in the comparison group. A causal interpretation needs the assumptions below.

## Parallel trends concerns an unobserved outcome

The main assumption says that, without closed-door treatment, the treated group would have had the same average change as the comparison group. We cannot observe that untreated path after treatment. Similar trends across several pre-treatment periods can support the assumption, but they cannot prove it.

A flat pre-trend test can have low power. A researcher should also explain why the groups were exposed to comparable changes in schedules, rules and health conditions. Subject-matter reasoning is part of the design.

## Check the other assumptions

- Teams should not change behaviour before the closed-door period in anticipation of treatment.
- The composition of matches should not change in a way caused by treatment and related to outcomes.
- Outcome definitions and data collection should remain stable.
- Standard errors should account for the level of treatment assignment and repeated outcomes.

## Exercise 1. Calculate the estimate

Home yellow cards fall from 2.4 to 2.0 in treated matches and from 2.2 to 2.1 in comparison matches. Calculate the difference-in-differences estimate.

Show a suggested answer

The treated change is -0.4 cards. The comparison change is -0.1 cards. The difference is -0.4 minus -0.1, which equals -0.3 cards per home team.

## Exercise 2. Challenge parallel trends

Name one change that could affect only the closed-door group at the same time and explain why it is a threat.

Show a suggested answer

If closed-door leagues also adopted a different substitution rule while the comparison leagues did not, the rule could change fatigue, goals and cards. The estimate would combine crowd absence with the rule change unless the design separated them.

## Practice

An [[Football CI 08 — Estimate a difference-in-differences effect|exercise sheet of 7 questions]] accompanies this lesson. It takes about thirty minutes and carries worked answers at the end. Attempt it before moving on.

## Check yourself

What distinguishes this design from before and after? It randomises access to every stadium It removes every possible pandemic shock It subtracts one comparison-group outcome changeCheck answer


> [!tip]- Reveal the answer
> Correct. The second subtraction removes the comparison group's change over the same period.

## What to remember

- Difference in differences subtracts the comparison-group change from the treated-group change.
- Parallel trends concerns the treated group's unobserved untreated path.
- Pre-treatment trends support but do not prove the assumption.
- Concurrent changes that differ by group threaten the causal interpretation.

**Primary reading.** [Scoppa (2021)](https://doi.org/10.1016/j.joep.2020.102344); Angrist and Pischke, *Mastering Metrics*, Chapter 5; and Hayashi, *Econometrics*, Chapter 18.

If any step is unclear, ask your teaching agent to work through another football example with you.

[[Football CI 07 — Use fixed effects without mistaking recovery for impact|Previous lesson: Use fixed effects without mistaking recovery for impact]][[Football CI 09 — Interpret an event study with staggered timing|Next lesson: Interpret an event study with staggered timing]]
