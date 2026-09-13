---
type: unit
course: "[[Causal Inference through Football]]"
status: not-started
order: 3
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

A causal diagram records your assumptions about how variables cause one another. You can use the diagram to decide which variables to adjust for and which variables to leave alone.

About 25 minutesIncludes guided exercises[[Football CI 03 — Use a causal diagram to choose controls|Exercise sheet]]

## What you will be able to do

You will be able to read paths in a directed acyclic graph, identify a back-door path, and avoid adjusting for a mediator or collider.

## Evidence gap

The paper search did not find a strong football study that used a directed acyclic graph as its main identification method. This lesson uses a constructed coach-dismissal diagram based on the variables discussed in the mapped coach studies. The diagram is a teaching example and is not a diagram published by those authors.

## The coach-dismissal problem

Clubs tend to dismiss coaches after poor results. Recent form also predicts future points, because weak performance can continue. Squad strength affects recent form, the board's decision and future points. A comparison that ignores these common causes will mix the effect of dismissal with the reasons for dismissal.

A causal diagram in which squad strength and recent form affect coach dismissal and future points. Coach dismissal affects tactical change, which affects future points.

## Read the arrows as causal assumptions

An arrow from recent form to dismissal means that recent form helps cause the board's decision. An arrow from recent form to future points means that recent form also helps cause the outcome. The path from dismissal back to recent form and then forward to future points is a back-door path. It creates an association between dismissal and points that is not part of the effect of dismissal.

Adjusting for recent form and squad strength blocks the back-door paths in this simplified diagram. The adjustment is justified by the assumed causal structure, not by how strongly the variables predict points.

## Do not adjust for every available variable

Tactical change occurs after dismissal and carries part of its effect to future points. It is a mediator. If the target is the total effect of dismissal, adjusting for tactical change removes part of the effect you want to estimate.

A collider is a common effect of two variables. Suppose media crisis is caused by dismissal rumours and by player unrest. Conditioning on media crisis can create an association between rumours and unrest even if none existed before. That opened path can bias the estimate. More controls can therefore make an analysis worse.

## Worked control decision

An analyst has five variables: pre-season wage bill, points from the last five matches, post-dismissal formation, referee strictness in the next match and a media-crisis indicator. The diagram says that wage bill and recent points are pre-treatment common causes, so they belong in the adjustment set. Formation occurs after treatment and should not be controlled when estimating the total effect. Future referee strictness is not a cause of dismissal at the decision time. The media indicator needs its own causal analysis because it may be a collider.

## Exercise 1. Classify the variables

For the effect of starting formation on final goal difference, classify each variable as a possible confounder, mediator or neither: opponent strength measured before kick-off, possession measured after kick-off, and the referee's shirt colour.

Show a suggested answer

Opponent strength can affect both formation choice and goal difference, so it is a possible confounder. Possession can be changed by formation and can affect goal difference, so it is a possible mediator. Shirt colour has no plausible causal role in this example, so it is neither. The classification depends on the causal assumptions, not only on correlations.

## A diagram does not prove identification

A diagram makes assumptions visible, but the data do not verify that the diagram is complete. The analysis still assumes that important common causes are included and measured well enough. Experts who understand coaching decisions should review the diagram before the analyst chooses controls.

The back-door criterion gives a sufficient adjustment rule. The chosen set must block every path that enters treatment through an incoming arrow, and it must not include descendants of treatment. Different valid sets may exist. The smallest adequate set can preserve more overlap and avoid unnecessary estimation noise.

## Exercise 2. Explain a bad control

A formation study adjusts for shots taken during the match. Write two sentences explaining why this choice can change the estimand.

Show a suggested answer

Starting formation can affect the number and quality of shots, which then affects goal difference. Adjusting for shots blocks part of the total effect of formation, so the estimate no longer represents the total effect of choosing that formation.

## Practice

An [[Football CI 03 — Use a causal diagram to choose controls|exercise sheet of 7 questions]] accompanies this lesson. It takes about thirty minutes and carries worked answers at the end. Attempt it before moving on.

## Check yourself

Which variable is a mediator in the diagram? Post-dismissal tactical change during matches Pre-dismissal recent form across matches Pre-season squad strength before matchesCheck answer


> [!tip]- Reveal the answer
> Correct. Tactical change follows dismissal and carries part of its effect, so it is a mediator.

## What to remember

- A causal diagram records assumptions about causal direction.
- Adjust for variables that block back-door paths between treatment and outcome.
- Do not adjust for a mediator when estimating a total effect.
- Conditioning on a collider can create bias.

**Primary reading.** Hernan and Robins, *What If*, Chapters 6 and 7; and Ruiz de Villa, *Causal Inference for Data Science*, Chapters 3 and 7.

If any step is unclear, ask your teaching agent to work through another football example with you.

[[Football CI 02 — Analyse a cluster-randomised football trial|Previous lesson: Analyse a cluster-randomised football trial]][[Football CI 04 — Write a target trial protocol|Next lesson: Write a target trial protocol]]
