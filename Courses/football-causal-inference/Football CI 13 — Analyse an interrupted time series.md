---
type: unit
course: "[[Causal Inference through Football]]"
status: not-started
order: 13
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Interrupted time series compares the observed post-interruption path with a continuation of the pre-interruption path. The design must separate an immediate level change from a change in trend.

About 20 minutesIncludes guided exercises[[Football CI 13 — Analyse an interrupted time series|Exercise sheet]]

## What you will be able to do

You will be able to specify a segmented model, interpret level and slope changes, and assess autocorrelation and concurrent events.

## Injuries before and after competition resumed

Duque-Arias and colleagues followed muscular injuries among 94 Colombian professional players before and during the COVID-19 return to competition. The interruption creates a clear date, but many conditions changed at the same time. Training, fixture congestion, player fitness and injury surveillance may all differ.

## Model the pre-period and the change

A segmented regression includes elapsed time, an indicator for the post-interruption period and time since the interruption. The post indicator estimates an immediate level change. Time since interruption estimates the change in slope.

Y\_t = beta0 + beta1 time\_t + beta2 post\_t + beta3 time\_after\_t + error\_t

The counterfactual continues the pre-interruption trend into the post-period. That continuation is an assumption, not an observed comparison.

## Worked interpretation

Suppose the injury rate was increasing by 0.02 per week before competition stopped. At return, the fitted rate jumps by 0.30, and the post-return slope is 0.01 lower than before. The immediate level effect is +0.30. The new slope is +0.01 per week because +0.02 plus -0.01 equals +0.01.

After ten weeks, the modelled difference from the old trend is +0.30 plus ten times -0.01, which equals +0.20. The immediate jump has partly declined but has not disappeared.

## Account for repeated time points

Measurements close in time often share shocks. This serial correlation can make ordinary regression standard errors too small. Inspect residual plots and autocorrelation, and use a suitable time-series error model or inference that accounts for serial correlation.

Seasonality also matters. Injury risk may change with preseason, winter congestion and cumulative load. Exposure hours should enter the outcome definition or model, because a count of injuries can rise when players simply spend more time training and playing.

## Concurrent changes limit causal attribution

The design is weakest when several events occur at the same date. A control series that did not receive the interruption can help, as can multiple treated and untreated leagues or a known effect pattern. The researcher should list co-interventions and explain why they cannot account for the result.

## Exercise 1. Interpret coefficients

A model estimates beta2 = -0.4 and beta3 = +0.05. Explain both coefficients.

Show a suggested answer

The outcome falls immediately by 0.4 units at the interruption relative to the continued old trend. After the interruption, the slope increases by 0.05 units per period compared with the old slope.

## Exercise 2. List concurrent causes

Name two changes at football's pandemic restart that could affect muscular injuries apart from the restart itself.

Show a suggested answer

A compressed fixture schedule could increase load, while longer substitutions or changed training access could alter player exposure. Either change can create a level or slope difference at the same time as competition restarts.

## Practice

An [[Football CI 13 — Analyse an interrupted time series|exercise sheet of 7 questions]] accompanies this lesson. It takes about thirty minutes and carries worked answers at the end. Attempt it before moving on.

## Check yourself

Why should the analyst inspect residual autocorrelation? To redefine the causal treatment itself To avoid overstated time-series estimate precision To prove no concurrent events existedCheck answer


> [!tip]- Reveal the answer
> Correct. Serial correlation can make ordinary standard errors overstate the precision of time-series estimates.

## What to remember

- A segmented model separates immediate level change from slope change.
- The counterfactual extends the pre-interruption trend.
- Serial correlation and seasonality affect estimation and uncertainty.
- Concurrent events at the interruption date weaken causal attribution.

**Primary reading.** [Duque-Arias et al. (2024)](https://doi.org/10.17533/udea.iatreia.238); Ruiz de Villa, Chapter 11; and Wooldridge, *Introductory Econometrics*, Chapters 10 to 12.

If any step is unclear, ask your teaching agent to work through another football example with you.

[[Football CI 12 — Build and assess a synthetic control|Previous lesson: Build and assess a synthetic control]][[Football CI 14 — Choose a method for time-varying treatment|Next lesson: Choose a method for time-varying treatment]]
