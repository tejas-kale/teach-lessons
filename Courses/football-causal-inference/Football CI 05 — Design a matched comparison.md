---
type: unit
course: "[[Causal Inference through Football]]"
status: not-started
order: 5
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Matching tries to compare treated and untreated matches that had similar pre-treatment characteristics. A propensity score can help, but it does not replace balance checks or causal judgement.

About 25 minutesIncludes guided exercises[[Football CI 05 — Design a matched comparison|Exercise sheet]]

## What you will be able to do

You will be able to define a propensity score, assess overlap and balance, and explain what matching can and cannot remove.

## The unusual match-day study

Goller and Krumer studied 10,142 matches from the top leagues in England, France, Germany and Spain. They asked whether matches played on non-frequent days had different attendance and home advantage. Schedulers do not assign days at random. Television value, cup matches, team quality and expected demand can affect both scheduling and the outcomes.

The authors used radius matching on the propensity score. The method compared unusual-day matches with usual-day matches that had similar probabilities of receiving the unusual schedule.

## The propensity score describes treatment assignment

The propensity score is the probability of treatment given measured pre-treatment covariates. Here, treatment is a non-frequent match day and the covariates describe information known before scheduling and kick-off.

e(X) = P(non-frequent day | measured pre-treatment variables X)

If conditional exchangeability holds given the full covariate set, it also holds given a correctly estimated propensity score. This balancing result permits comparisons using one score rather than exact matching on every covariate. The result does not say that an accurate treatment-prediction model removes unmeasured confounding.

## Run the design before estimating outcomes

1.  Choose covariates because they are plausible common causes, and do not use variables caused by the match day.
2.  Estimate the propensity score without using the outcome to tune the causal result.
3.  Inspect the score distributions and define common support.
4.  Match treated and untreated matches within a chosen radius.
5.  Check covariate balance in the matched sample. Revise the design if balance remains poor.

Common support is the range where both day types occur for similar matches. A Champions League club playing a rearranged Monday fixture may have no credible Saturday comparison. Keeping that match can force extrapolation. Dropping it changes the target population, so the analysis must describe which matches remain.

## Worked matching decision

Suppose an unusual-day match has a propensity score of 0.72. Three usual-day matches have scores of 0.70, 0.73 and 0.48. With a radius of 0.05, the first two are candidate matches and the third is not. The analyst then checks the original covariates. If the unusual match is a derby and neither candidate is a derby, the numerical score has not produced adequate balance on an important feature.

The analyst should report standardised mean differences or another balance measure before and after matching. A low prediction error or high classification accuracy is not the main diagnostic.

## Exercise 1. Choose baseline covariates

Choose which variables can enter the propensity model for the effect of unusual scheduling on attendance: last season's average attendance, expected television audience, actual attendance at the treated match, and rain recorded during the match.

Show a suggested answer

Last season's attendance and expected television audience are pre-treatment variables that may affect scheduling and attendance. Actual attendance is the outcome and cannot be a baseline covariate. Rain during the match occurs after scheduling. Forecast weather known when scheduling could be considered, but realised rain should not be treated as a pre-treatment cause of scheduling.

## State what matching assumes

- **Conditional exchangeability.** The measured variables include the important common causes of day type and outcome.
- **Positivity.** Comparable usual and unusual days exist for the target matches.
- **Consistency.** The day categories represent clear scheduling interventions.
- **Stable outcomes.** One match's assigned day does not materially change attendance or performance at comparison matches.

## Exercise 2. Interpret the matched effect

A matched analysis drops every match with a propensity score above 0.80. Can the final estimate describe all league matches? Explain your answer.

Show a suggested answer

No. The estimate describes the matched population with common support, not the high-propensity matches that were removed. The analyst must state the resulting target population and should not generalise without further assumptions.

## Practice

An [[Football CI 05 — Design a matched comparison|exercise sheet of 7 questions]] accompanies this lesson. It takes about thirty minutes and carries worked answers at the end. Attempt it before moving on.

## Check yourself

Which diagnostic is most important after matching? Maximum treatment prediction accuracy achieved Maximum possible matched sample retained Baseline balance within common supportCheck answer


> [!tip]- Reveal the answer
> Correct. Balance and overlap show whether the matched groups are comparable on measured baseline variables.

## What to remember

- The propensity score is the treatment probability given measured baseline covariates.
- Matching needs common support and balance checks.
- A good treatment prediction score does not prove a good causal design.
- Matching cannot remove bias from unmeasured common causes.

**Primary reading.** [Goller and Krumer (2020)](https://doi.org/10.1016/j.ejor.2020.03.062); Hernan and Robins, *What If*, Section 4.5 and Chapter 15; and Ruiz de Villa, *Causal Inference for Data Science*, Chapter 5.

If any step is unclear, ask your teaching agent to work through another football example with you.

[[Football CI 04 — Write a target trial protocol|Previous lesson: Write a target trial protocol]][[Football CI 06 — Compare weighting, standardisation and AIPW|Next lesson: Compare adjustment estimators]]
