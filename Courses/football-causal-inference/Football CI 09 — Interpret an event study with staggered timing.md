---
type: unit
course: "[[Causal Inference through Football]]"
status: not-started
order: 9
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

An event study estimates effects before and after treatment relative to an omitted period. Staggered treatment dates require care because already-treated units can become invalid controls.

About 20 minutesIncludes guided exercises[[Football CI 09 — Interpret an event study with staggered timing|Exercise sheet]]

## What you will be able to do

You will be able to read event-time coefficients, use leads to inspect pre-treatment patterns and explain the problem with conventional two-way fixed effects under staggered timing.

## Football matches and district infections

Breidenbach and Mitze studied whether hosting professional football matches affected COVID-19 infections in German districts. Districts hosted matches at different times. The outcome could change before, immediately after and several weeks after a match, so one post-treatment coefficient would hide the timing.

## Event time places units on a common clock

Calendar time records the date. Event time records periods relative to treatment. Event time -2 means two periods before treatment, 0 means the treatment period and +3 means three periods after treatment. An event-study regression includes indicators for these relative periods and omits one pre-treatment period as the reference.

Outcome\_it = unit effect\_i + date effect\_t + sum of event-time effects + error\_it

Lead coefficients describe periods before treatment. If they differ from zero, the treated and comparison outcomes may already have been separating, or units may have anticipated treatment. Lag coefficients describe the path after treatment relative to the omitted period.

## Worked interpretation

Suppose week -1 is the reference. The estimates are 0.01 at week -3, 0.03 at week -2, 0.10 at week 0, 0.25 at week +1 and 0.08 at week +2. The small leads are compatible with similar pre-treatment movement, although uncertainty must be considered. The effect appears largest one week after hosting and then falls.

The pattern is descriptive until the design supports parallel untreated trends, no anticipation and limited spillovers between neighbouring districts.

## Staggered timing changes the comparisons

In a conventional two-way fixed-effects model, a district treated early can later serve as a comparison for a district treated later. If treatment effects persist or vary over time, that comparison is contaminated. The weighted average can even use negative weights and produce a coefficient outside the underlying cohort effects.

A safer analysis estimates effects for each treatment cohort and calendar period using not-yet-treated or never-treated units, then aggregates those effects transparently. The researcher should show which comparison group supports each estimate.

## Use pre-treatment coefficients carefully

A non-significant lead does not prove parallel trends. The estimate may be imprecise. Researchers should inspect the size and pattern of leads, use joint tests, and explain whether the assignment process makes a common untreated trend plausible.

## Exercise 1. Read an event path

Leads become steadily more positive from week -4 to week -1. What concern does this create?

Show a suggested answer

The outcome was already trending differently before treatment. The post-treatment coefficients may extend that earlier divergence rather than measure a treatment effect. The analyst should reconsider the comparison group, trend specification and assignment process.

## Exercise 2. Choose valid controls

District A hosts in week 4 and District B hosts in week 8. Can District A serve as an untreated control for B in week 7 if hosting effects last six weeks?

Show a suggested answer

No. District A is still affected by its earlier treatment in week 7. Using it as an untreated comparison would contaminate B's estimate. A not-yet-treated or never-treated district is preferable.

## Practice

An [[Football CI 09 — Interpret an event study with staggered timing|exercise sheet of 7 questions]] accompanies this lesson. It takes about thirty minutes and carries worked answers at the end. Attempt it before moving on.

## Check yourself

What can a large pre-treatment lead indicate? Earlier divergence or treatment anticipation Guaranteed delayed treatment impact later Perfectly parallel untreated outcome trendsCheck answer


> [!tip]- Reveal the answer
> Correct. A pre-treatment lead can indicate an earlier trend difference or anticipation.

## What to remember

- Event time describes periods relative to treatment.
- Leads inspect pre-treatment patterns and possible anticipation.
- Lags describe how the estimated effect changes after treatment.
- Staggered timing requires comparison groups that are untreated at each cohort's treatment time.

**Primary reading.** [Breidenbach and Mitze (2022)](https://pmc.ncbi.nlm.nih.gov/articles/PMC8411384/); Wooldridge, *Introductory Econometrics*, Chapters 13 and 14; and Hayashi, Chapters 17 and 18.

If any step is unclear, ask your teaching agent to work through another football example with you.

[[Football CI 08 — Estimate a difference-in-differences effect|Previous lesson: Estimate a difference-in-differences effect]][[Football CI 10 — Estimate a local effect at a threshold|Next lesson: Estimate a local effect at a threshold]]
