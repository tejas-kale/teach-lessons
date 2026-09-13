---
type: unit
course: "[[Causal Inference through Football]]"
status: not-started
order: 16
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Mediation analysis asks how an intervention produces an outcome. Randomising the intervention identifies its total effect, but it does not automatically identify the effect that passes through a measured mediator.

About 25 minutesIncludes guided exercises[[Football CI 16 — Separate direct and indirect effects|Exercise sheet]]

## What you will be able to do

You will be able to distinguish total, direct and indirect effects and explain the extra assumptions needed for mediator-outcome comparisons.

## The EuroFIT supporter programme

Roynesdal and colleagues studied mediation in the EuroFIT programme, which was delivered through 15 professional football clubs to 1,113 male supporters. The football clubs provided a setting for a health programme. The units were supporters, not players or matches. The mediated effects changed under sensitivity checks, so the study is also a useful example of cautious interpretation.

## Define the three effects

**Total effect**

The outcome difference under the programme compared with control, allowing every causal path to operate.

**Indirect effect**

The part of the effect that operates through a specified mediator, such as autonomous motivation.

**Direct effect**

The part that does not operate through that mediator under the chosen definition.

The total effect is not automatically the sum of simple regression coefficients. Natural direct and indirect effects use nested counterfactuals, such as the outcome under treatment with the mediator set to the value it would have taken under control. These definitions require strong assumptions.

## Treatment randomisation does not randomise the mediator

Random assignment protects the programme comparison at baseline. Motivation is measured after assignment and is affected by the programme. Supporters with greater motivation may also differ in health, available time or social support. Those causes can affect the outcome. The mediator-outcome association can therefore remain confounded.

A treatment-induced mediator-outcome confounder creates another problem. Suppose the programme changes physical ability, and ability affects both motivation and activity. Adjusting for ability blocks part of treatment's effect, while ignoring it leaves mediator-outcome confounding.

## Worked causal paths

Consider three paths: programme to motivation to physical activity; programme to knowledge to physical activity; and baseline health to motivation and physical activity. The first path is the indirect path through the chosen mediator. The second contributes to the direct effect relative to motivation, even though knowledge is another mediator. Baseline health confounds the mediator-outcome relation and should be handled if measured before treatment.

## Choose the effect definition for the decision

Natural effects can be difficult to interpret when the mediator cannot be set independently. Interventional indirect effects instead compare mediator distributions generated under different treatments. They may require assumptions that fit practical interventions better. The analyst should state which definition is used and what hypothetical action it represents.

## Use sensitivity analysis

Mediation estimates depend on assumptions that cannot be fully tested. Report how results change under plausible unmeasured mediator-outcome confounding, alternative mediator measurements and timing choices. A weak or unstable mediated effect should be described as such.

## Exercise 1. Identify the mediator

A warm-up programme reduces injury partly by improving landing technique. Name treatment, mediator and outcome.

Show a suggested answer

The assigned warm-up programme is treatment, landing technique measured after training is the mediator, and subsequent injury is the outcome. Baseline athletic ability may confound technique and injury.

## Exercise 2. Explain the extra assumption

Why is random assignment of the warm-up programme not enough to identify the indirect effect through landing technique?

Show a suggested answer

Landing technique was not randomised. Variables such as baseline coordination, effort or prior injury can affect both technique and later injury. The mediation analysis needs assumptions or measurements that address that mediator-outcome confounding.

## Practice

An [[Football CI 16 — Separate direct and indirect effects|exercise sheet of 7 questions]] accompanies this lesson. It takes about thirty minutes and carries worked answers at the end. Attempt it before moving on.

## Check yourself

Why can mediator-outcome confounding remain? The programme uses professional club branding The mediator itself was not randomised The trial includes several football clubsCheck answer


> [!tip]- Reveal the answer
> Correct. The mediator is a post-treatment variable and was not randomised by the original treatment assignment.

## What to remember

- The total effect includes every causal path from treatment to outcome.
- Direct and indirect effects depend on a specific mediator and counterfactual definition.
- Random treatment assignment does not remove mediator-outcome confounding.
- Mediation conclusions need clear timing and sensitivity analysis.

**Primary reading.** [Roynesdal et al. (2026)](https://doi.org/10.1080/08870446.2026.2677081); Hernan and Robins, Chapters 4 to 8 for counterfactual identification; and Ruiz de Villa, Chapter 6.

If any step is unclear, ask your teaching agent to work through another football example with you.

[[Football CI 15 — Use double machine learning for a causal effect|Previous lesson: Use double machine learning for a causal effect]][[Football CI 17 — Choose a causal time-to-event estimand|Next lesson: Choose a causal time-to-event estimand]]
