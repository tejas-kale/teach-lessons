---
type: unit
course: "[[Causal Inference through Football]]"
status: not-started
order: 15
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Double machine learning uses flexible models for treatment and outcome relationships while protecting the final effect estimate from some overfitting bias. It does not choose the causal controls for you.

About 25 minutesIncludes guided exercises[[Football CI 15 — Use double machine learning for a causal effect|Exercise sheet]]

## What you will be able to do

You will be able to describe residualisation and cross-fitting, design leakage-safe folds and distinguish nuisance prediction from causal identification.

## Formation effects

Ruiz-Menarguez and Badiella used more than 22,000 fixtures to estimate how categorical formations affected match outcomes. They used cross-fitted XGBoost models for nuisance relationships. The study is a new, non-peer-reviewed preprint, so the football application should be treated as an advanced example rather than the sole authority on the method.

## Separate the causal parameter from nuisance functions

The causal parameter is the formation contrast of interest, such as 4-3-3 versus 4-4-2. Nuisance functions describe the expected outcome given baseline variables and the probability of each formation given those variables. Flexible machine-learning models can estimate these complicated relationships.

Good nuisance prediction is useful, but prediction alone does not identify a causal effect. The covariates must be pre-treatment common causes, overlap must hold and the formation versions must be clear.

## Residualise treatment and outcome

In a partially linear setting, one model predicts the outcome from covariates and another predicts treatment from covariates. The analyst subtracts those predictions from observed outcome and treatment. The final regression relates the outcome residual to the treatment residual.

Y - predicted Y from X = effect x (A - predicted A from X) + residual

The residual treatment variation is the part not explained by measured baseline covariates. Orthogonal scores reduce the sensitivity of the causal estimate to small nuisance-model errors.

## Cross-fitting keeps observations out of their own predictions

1.  Split the data into folds.
2.  Train nuisance models on all folds except one.
3.  Predict treatment and outcome for the held-out fold.
4.  Repeat until every observation has out-of-fold predictions, then estimate the causal effect.

Randomly splitting individual matches can leak information when the same teams and seasons appear in training and validation. Folds may need to keep seasons, competitions or teams together. The split should match the dependence structure and the intended generalisation.

## Worked control decision

Pre-match injuries, opponent strength and venue can affect formation and result, so they are possible controls. Possession, shots and expected goals occur after kick-off and can carry the formation effect. Adding them because they improve prediction changes the causal estimand and can introduce bias. A powerful learner does not make a bad control valid.

## Define categorical treatment contrasts

Formation has more than two categories. The analyst should define each contrast and target population, check whether both formations occur for similar matches, and avoid one vague "formation effect". Effects may vary by opponent or team quality, but subgroup analyses need adequate overlap and pre-specified interpretation.

## Exercise 1. Design the folds

A dataset has ten seasons for the same 20 clubs. Why might match-level random folds be optimistic, and what is one alternative?

Show a suggested answer

Random folds let the model learn the same clubs and nearby matches in both training and validation data. This can overstate out-of-sample performance. One alternative is to hold out complete seasons, or to group by club-season when that matches the target use.

## Exercise 2. Remove bad controls

A model uses half-time score to estimate the effect of starting formation on full-time goal difference. Explain the problem.

Show a suggested answer

Starting formation can affect the half-time score, which then affects the full-time result. Half-time score is post-treatment and can mediate the total effect. Adjusting for it changes the question and can create bias.

## Practice

An [[Football CI 15 — Use double machine learning for a causal effect|exercise sheet of 7 questions]] accompanies this lesson. It takes about thirty minutes and carries worked answers at the end. Attempt it before moving on.

## Check yourself

What does cross-fitting provide? Out-of-fold nuisance predictions for observations Automatic adjustment for every mediator Guaranteed overlap for every formationCheck answer


> [!tip]- Reveal the answer
> Correct. Cross-fitting produces nuisance predictions from models that did not train on the predicted observation.

## What to remember

- Double machine learning separates the causal parameter from nuisance functions.
- Cross-fitting creates predictions for observations excluded from model training.
- Folds must respect repeated teams, seasons and other dependence.
- Machine learning does not fix a bad adjustment set or poor overlap.

**Primary reading.** [Ruiz-Menarguez and Badiella (2026)](https://arxiv.org/abs/2602.16830); Hernan and Robins, Sections 18.3 and 18.4; and Ruiz de Villa, Chapters 4 and 8.

If any step is unclear, ask your teaching agent to work through another football example with you.

[[Football CI 14 — Choose a method for time-varying treatment|Previous lesson: Choose a method for time-varying treatment]][[Football CI 16 — Separate direct and indirect effects|Next lesson: Separate direct and indirect effects]]
