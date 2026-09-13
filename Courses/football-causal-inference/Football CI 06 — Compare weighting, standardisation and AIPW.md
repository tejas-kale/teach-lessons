---
type: unit
course: "[[Causal Inference through Football]]"
status: not-started
order: 6
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Several estimators can use the same identification assumptions. They differ in how they reconstruct the missing potential outcomes and in which models they require.

About 25 minutesIncludes guided exercises[[Football CI 06 — Compare weighting, standardisation and AIPW|Exercise sheet]]

## What you will be able to do

You will be able to explain weighting, standardisation and AIPW, and you will know what doubly robust estimation does not guarantee.

## The head-coach study

Bryson and colleagues studied head-coach departures in the top two divisions of France, Germany, Italy and Spain. Coach departures are not random. Clubs with poor recent results, low league positions or unmet expectations are more likely to change coaches. The authors used entropy balancing to reweight control observations so their measured pre-departure characteristics resembled those of departure observations.

## Start with one identification argument

All three methods in this lesson need conditional exchangeability, positivity and consistency. The analyst must first choose a defensible set of pre-treatment common causes. An estimator does not decide which controls are valid.

## Weighting changes the contribution of each observation

Inverse-probability weighting gives more weight to observations that received an unusual treatment for their covariate profile. For an ATE, a treated observation receives a weight related to one divided by its propensity score, and an untreated observation receives a weight related to one divided by one minus its propensity score. The weighted sample should balance measured baseline covariates.

Entropy balancing works in the same broad direction but chooses weights that directly balance selected moments, such as means and variances. Large weights indicate weak overlap and can make an estimate unstable.

## Standardisation predicts both potential outcomes

Standardisation fits an outcome model using treatment and baseline covariates. The analyst then predicts every club's outcome twice, once under departure and once under retention, and averages each set of predictions over the target population. The difference between the two averages estimates the causal contrast if the outcome model and identification assumptions are adequate.

## Worked comparison

Suppose a club with poor recent form has a 0.80 probability of dismissal. In an ATE weighting analysis, its observed dismissal outcome receives a treated weight of 1 divided by 0.80, or 1.25. If a similar club retains its coach despite the same 0.80 dismissal probability, its untreated weight is 1 divided by 0.20, or 5. The second club receives a large weight because it provides scarce information about retention among high-dismissal-risk clubs.

Standardisation instead uses the fitted outcome model to predict both dismissal and retention outcomes for each club profile. It does not create the same explicit pseudo-population, but it targets the same adjusted means when both methods are specified for the same estimand.

## AIPW combines both models

Augmented inverse-probability weighting starts with outcome predictions and adds weighted residual corrections. Its estimating expression contains an outcome model for each treatment and a propensity model for treatment assignment.

mean\[m1(X) - m0(X) + A(Y-m1(X))/e(X) - (1-A)(Y-m0(X))/(1-e(X))\]

The estimator is called doubly robust because it can remain consistent when either the propensity model or the outcome model is correctly specified, under regularity and identification conditions. The phrase does not mean that the estimate survives unmeasured confounding, poor overlap or two badly specified models.

## Evidence gap

The paper search did not find a strong standalone football AIPW study. The AIPW discussion is a constructed extension of the head-coach setting. Bryson and colleagues used entropy balancing and weighted outcome regression, not the AIPW calculation shown here.

## Exercise 1. Diagnose a large weight

An untreated club receives a weight of 18. Explain what the weight says about overlap and name one responsible response.

Show a suggested answer

The club had a very high estimated probability of dismissal, so its retention outcome represents many similar clubs for which retention is rare. The analyst should inspect overlap and model specification, report the weight distribution, and consider redefining the target population or using transparent weight truncation with a sensitivity analysis.

## Exercise 2. Choose a statement

An analyst says, "AIPW means I do not need to worry about which variables I control." Rewrite the statement correctly.

Show a suggested answer

AIPW still needs a causal adjustment set, conditional exchangeability and positivity. Double robustness concerns two nuisance-model specifications after identification has been justified.

## Practice

An [[Football CI 06 — Compare weighting, standardisation and AIPW|exercise sheet of 7 questions]] accompanies this lesson. It takes about thirty minutes and carries worked answers at the end. Attempt it before moving on.

## Check yourself

Which AIPW claim is defensible? One nuisance model can be misspecified Unmeasured confounding is always automatically removed Positivity is no longer required anywhereCheck answer


> [!tip]- Reveal the answer
> Correct. AIPW can tolerate one misspecified nuisance model under the stated identification and regularity conditions.

## What to remember

- Weighting builds a balanced pseudo-population through observation weights.
- Standardisation averages outcome predictions under each treatment.
- AIPW combines outcome prediction with weighted residual correction.
- Double robustness does not remove the need for identification or overlap.

**Primary reading.** [Bryson et al. (2024)](https://doi.org/10.1111/sjpe.12369); Hernan and Robins, *What If*, Chapters 12, 13, 15 and 18; and Ruiz de Villa, Chapter 8.

If any step is unclear, ask your teaching agent to work through another football example with you.

[[Football CI 05 — Design a matched comparison|Previous lesson: Design a matched comparison]][[Football CI 07 — Use fixed effects without mistaking recovery for impact|Next lesson: Use fixed effects without mistaking recovery for impact]]
