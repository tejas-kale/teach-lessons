---
type: unit
course: "[[Causal Inference through Football]]"
status: not-started
order: 11
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

An instrument changes treatment without changing the outcome through another path. Under additional assumptions, it identifies an effect for units whose treatment responds to the instrument.

About 25 minutesIncludes guided exercises[[Football CI 11 — Interpret an instrumental-variable estimate|Exercise sheet]]

## What you will be able to do

You will be able to assess relevance, independence, exclusion and monotonicity, and interpret a local average treatment effect.

## Foreign-player restrictions

Glennon and colleagues studied whether immigrant players improved European football clubs' competitive performance. Clubs that hire more skilled immigrants may differ from other clubs in money, recruitment and ambition. The authors used changes in national restrictions on foreign players as an instrument for the number of immigrant starters.

## Assess four conditions

**Relevance**

The policy change must alter the use of immigrant starters.

**Independence**

The policy variation must be unrelated to clubs' potential performance after suitable controls.

**Exclusion**

The policy must affect performance only through immigrant starters.

**Monotonicity**

Easing restrictions should not make some clubs use fewer immigrant starters because of the policy while making others use more.

Relevance can be studied in the data. Independence, exclusion and monotonicity require knowledge of the policy and football market. A strong statistical first stage does not establish the other conditions.

## Understand the Wald ratio and two-stage least squares

With a binary instrument and treatment, the Wald estimate divides the instrument's effect on the outcome by its effect on treatment.

LATE = \[E(Y|Z=1) - E(Y|Z=0)\] / \[E(A|Z=1) - E(A|Z=0)\]

Two-stage least squares extends the logic. The first stage predicts immigrant starters from the policy and controls. The second stage relates performance to the treatment variation predicted by the policy. Standard software must calculate the combined uncertainty; manually replacing treatment with fitted values and running ordinary standard errors is not enough.

## Worked calculation

Suppose easing a restriction increases immigrant starters by 0.8 players and increases average goal difference by 0.12 goals. The Wald estimate is 0.12 divided by 0.8, which equals 0.15 goals per additional immigrant starter.

Under the assumptions, this is a local average treatment effect for clubs whose use of immigrant starters changed because of the restriction. It is not necessarily the effect for clubs that would always hire many immigrant players or never hire them.

## Challenge the exclusion restriction

A restriction can affect transfer fees, wages, youth development and the pool of domestic players. If these changes affect performance through paths other than the number of immigrant starters, exclusion fails for the stated treatment. The analyst may need a broader treatment definition or a different design.

Policy timing can also coincide with other league reforms. Independence needs a case that the instrument was not introduced in response to expected club performance and that relevant concurrent changes are controlled or absent.

## Exercise 1. Calculate a local effect

A policy increases the probability of starting an academy player by 0.20 and increases win probability by 0.03. Calculate the Wald estimate.

Show a suggested answer

The estimate is 0.03 divided by 0.20, which equals 0.15. Under the assumptions, starting an academy player increases win probability by 15 percentage points for clubs whose choice responds to the policy.

## Exercise 2. Find an exclusion threat

A home-grown-player quota is proposed as an instrument for academy-player starts. Name one direct path from the quota to results that does not pass through academy starts.

Show a suggested answer

The quota could change transfer spending or squad composition among non-academy players. If that change affects results, the quota has a path to performance outside the treatment and exclusion is threatened.

## Practice

An [[Football CI 11 — Interpret an instrumental-variable estimate|exercise sheet of 7 questions]] accompanies this lesson. It takes about thirty minutes and carries worked answers at the end. Attempt it before moving on.

## Check yourself

Which condition is not proved by a strong first stage? Relevant treatment variation from policy Strong prediction of treatment received No direct policy outcome pathCheck answer


> [!tip]- Reveal the answer
> Correct. First-stage strength supports relevance, but it cannot establish the exclusion restriction.

## What to remember

- An instrument must be relevant, independent and excluded from other outcome paths.
- Monotonicity supports a local average treatment effect interpretation.
- A strong first stage addresses relevance only.
- The estimated effect applies to units whose treatment responds to the instrument.

**Primary reading.** [Glennon et al. (2025)](https://doi.org/10.1287/mnsc.2021.03356); Hernan and Robins, Chapter 16; Angrist and Pischke, Chapter 3; and Ruiz de Villa, Chapter 9.

If any step is unclear, ask your teaching agent to work through another football example with you.

[[Football CI 10 — Estimate a local effect at a threshold|Previous lesson: Estimate a local effect at a threshold]][[Football CI 12 — Build and assess a synthetic control|Next lesson: Build and assess a synthetic control]]
