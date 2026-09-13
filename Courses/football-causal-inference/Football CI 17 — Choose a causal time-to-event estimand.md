---
type: unit
course: "[[Causal Inference through Football]]"
status: not-started
order: 17
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Time-to-event outcomes need a treatment contrast, a time horizon and a clear approach to censoring. Risks and restricted mean survival times are often easier to interpret than hazard ratios.

About 25 minutesIncludes guided exercises[[Football CI 17 — Choose a causal time-to-event estimand|Exercise sheet]]

## What you will be able to do

You will be able to compare risk, restricted mean survival time and hazard-ratio estimands, and explain when censoring threatens causal interpretation.

## The 11+ Kids trial

Rossler and colleagues randomised clubs to the 11+ Kids injury-prevention programme and observed injuries over football exposure time. They used mixed-effects Cox models for time to injury. Randomisation supports a causal assignment effect, while clustering, censoring and the choice of time scale still affect the analysis.

## Choose an estimand that answers the football question

**Risk by a fixed time**

The probability of injury by week 20 under the programme compared with control. A risk difference gives an absolute change in injured players.

**Restricted mean injury-free time**

The expected injury-free time up to a fixed horizon. The contrast is expressed in days or weeks without injury.

**Hazard ratio**

The ratio of instantaneous event rates among players who remain injury-free at each time. The compared risk sets change over follow-up.

A hazard ratio is not a risk ratio. Even in a randomised trial, conditioning on remaining event-free can make later risk sets differ because earlier treatment affects who remains. Proportional hazards also requires the hazard ratio to stay constant over time if one number is reported.

## Worked calculation

By week 20, 12 per cent of control players and 8 per cent of programme players have been injured. The risk difference is 8 minus 12, which equals -4 percentage points. The risk ratio is 8 divided by 12, which is about 0.67. The result means 4 fewer injured players per 100 by week 20 under assignment to the programme, subject to the trial assumptions.

If the restricted mean injury-free time is 18.2 weeks under the programme and 17.4 weeks under control, the difference is 0.8 injury-free weeks through week 20.

## Censoring hides later event times

A player who transfers at week 12 has an unknown later injury time in the study. Treating this censoring as harmless assumes that, conditional on the included history, transferred and retained players would have the same later injury distribution under each treatment.

Inverse-probability-of-censoring weighting models the chance of remaining observed at each time and gives more weight to similar players who remain. The method requires measured causes of censoring and injury, positivity for remaining observed and a suitable censoring model.

## Define time and competing events

Calendar weeks, training hours and match exposures answer different questions. Exposure time can be useful for injury incidence, but treatment may itself change exposure. The analyst should avoid conditioning away a pathway without defining the estimand.

A competing event prevents the injury of interest from occurring as defined. A player who leaves football permanently cannot later record the study injury. The analysis must define whether the target is a real-world cumulative incidence with competing events or a hypothetical effect that removes them.

## Exercise 1. Interpret a risk difference

The 12-week injury risks are 0.18 under control and 0.13 under treatment. State the risk difference in plain language.

Show a suggested answer

The treatment reduces 12-week injury risk by 5 percentage points. This corresponds to 5 fewer injured players per 100 over 12 weeks if the estimate is causal and applies to the target population.

## Exercise 2. Diagnose informative censoring

Players with early soreness are more likely to leave the study and more likely to be injured later. What should the analyst record and model?

Show a suggested answer

The analyst should record soreness before dropout and include it, with relevant history, in a censoring model. An inverse-probability-of-censoring analysis can then adjust for measured informative censoring, subject to exchangeability and positivity.

## Practice

An [[Football CI 17 — Choose a causal time-to-event estimand|exercise sheet of 7 questions]] accompanies this lesson. It takes about thirty minutes and carries worked answers at the end. Attempt it before moving on.

## Check yourself

Which estimand is easiest to express in weeks? An estimated baseline hazard intercept An estimated proportional-hazards model coefficient Restricted mean injury-free survival timeCheck answer


> [!tip]- Reveal the answer
> Correct. Restricted mean injury-free time gives an absolute difference in expected injury-free time through a fixed horizon.

## Final course application

Choose one football decision and write six lines: the eligible population, two treatment strategies, outcome and horizon, estimand, identification assumptions, and estimator with diagnostics. If you cannot write the first five lines, return to the earlier design lessons before choosing the estimator.

## What to remember

- Time-to-event questions still need a target population, treatment contrast and horizon.
- Risk and restricted mean survival time are absolute, interpretable estimands.
- A hazard ratio compares changing risk sets and needs careful interpretation.
- Informative censoring requires assumptions and measured history.

**Primary reading.** [Rossler et al. (2018)](https://doi.org/10.1007/s40279-017-0834-8); Hernan and Robins, Chapter 17 and Section 21.5.

If any step is unclear, ask your teaching agent to work through another football example with you.

[[Football CI 16 — Separate direct and indirect effects|Previous lesson: Separate direct and indirect effects]]
