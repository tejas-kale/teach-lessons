---
type: unit
course: "[[Causal Inference through Football]]"
status: not-started
order: 1
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

A causal analysis starts with a precise comparison. This lesson shows how to turn a general question about coach dismissal into a quantity that a study could estimate.

About 25 minutesIncludes two exercises[[Football CI 01 — Define a causal question and estimand|Exercise sheet]]No prior knowledge required

## What you will be able to do

By the end of the lesson, you will be able to define the population, treatment strategies, outcome, time horizon and estimand for a football question. You will also be able to explain why an observed result is not the same as a causal effect.

## The football question

A club has earned two points from its last five league matches. The board dismisses the head coach. The club then earns eight points from the next five matches. A newspaper reports that the dismissal produced six extra points.

The newspaper has compared two periods, but it has not measured a causal effect. Results may have improved because the opponents were weaker, injured players returned, or the earlier run was unusually poor. We need to compare what happened after dismissal with what would have happened during the same five matches if the club had retained the coach.

## Potential outcomes define the comparison

Let *Y(1)* be the points that a club would earn over the next five matches if it dismissed the coach now. Let *Y(0)* be the points that the same club would earn over those matches if it retained the coach. The club-level causal effect is *Y(1) minus Y(0)*.

Club-level causal effect = Y(1) - Y(0)

Only one potential outcome can be observed for a club at one decision point. If the board dismisses the coach, we observe *Y(1)* and do not observe *Y(0)*. If the board retains the coach, we observe *Y(0)* and do not observe *Y(1)*. This missing potential outcome is the main problem that every causal design tries to solve.

The previous five matches are not the missing *Y(0)*. They took place at a different time and against different opponents. The results of clubs that retained their coaches are also not automatically the missing *Y(0)*, because boards usually dismiss coaches after poor results. A useful comparison group must represent what the dismissed clubs would have experienced without dismissal.

## The estimand states the average effect you want

An estimand is the causal quantity that the analysis aims to estimate. Two common estimands are the average treatment effect and the average treatment effect among the treated.

**Average treatment effect**

The ATE averages *Y(1) minus Y(0)* across every eligible club. It asks what would happen if all eligible clubs followed one strategy rather than the other.

**Average treatment effect among the treated**

The ATT averages *Y(1) minus Y(0)* among clubs that actually dismissed their coaches. It asks whether dismissal helped the clubs that used it.

The ATE and ATT can differ. Boards may dismiss coaches at clubs with unusually poor form, weak dressing-room support or demanding owners. Those clubs may respond differently from the full population of eligible clubs. A study must state which population its estimate describes.

## Worked example

Suppose four clubs are eligible at matchweek 10. The table shows both potential outcomes for teaching purposes. A real dataset would reveal only one outcome for each club.

<table>
<thead>
<tr>
<th>Club</th>
<th>Points if dismissed, Y(1)</th>
<th>Points if retained, Y(0)</th>
<th>Causal effect</th>
<th>Actual decision</th>
</tr>
</thead>
<tbody>
<tr>
<td>North FC</td>
<td>8</td>
<td>5</td>
<td>+3</td>
<td>Dismiss</td>
</tr>
<tr>
<td>City Athletic</td>
<td>6</td>
<td>7</td>
<td>-1</td>
<td>Retain</td>
</tr>
<tr>
<td>United Town</td>
<td>7</td>
<td>3</td>
<td>+4</td>
<td>Dismiss</td>
</tr>
<tr>
<td>Rovers</td>
<td>5</td>
<td>5</td>
<td>0</td>
<td>Retain</td>
</tr>
</tbody>
</table>

The ATE is (3 - 1 + 4 + 0) divided by 4, which equals 1.5 points. The ATT is (3 + 4) divided by 2, which equals 3.5 points. The difference shows why the target population belongs in the question.

## Exercise 1. Write the causal question

A performance analyst asks, "Do unusual match days reduce attendance?" Rewrite the question so it defines a population, two treatment strategies, an outcome and a time horizon. Write your answer before opening the suggested answer.

Use this form: Among \[population\], what is the average effect of \[strategy 1\] rather than \[strategy 0\] on \[outcome\] measured over \[time\]?

Show a suggested answer

Among top-flight league matches that could be scheduled on either type of day, what is the average effect of scheduling a match on a non-frequent day rather than a frequent day on the number of spectators who attend that match?

The match itself supplies the time horizon, because attendance is measured once for the scheduled fixture. A fuller protocol would define which days count as frequent and non-frequent.

## Identification requires assumptions

The estimand states what you want to know. Identification states the conditions under which observed data can reveal that estimand. The following assumptions will appear throughout the course.

- **Consistency.** The treatment strategies must be clear enough that the observed outcome under a strategy equals the corresponding potential outcome. "Dismiss the coach" may need details about timing and replacement arrangements.
- **Exchangeability.** The comparison group must represent the missing potential outcomes. Random assignment creates exchangeability on average. Observational studies need a design and assumptions that make conditional exchangeability plausible.
- **Positivity.** Each relevant type of club must have some chance of following either strategy. If every bottom-placed club always dismisses its coach, the data do not show what retention would do for that type of club.
- **No relevant interference.** One club's treatment should not change another club's potential outcome, unless the analysis models that connection. This assumption can be difficult in a league where clubs play one another.

## Exercise 2. Diagnose an incomplete question

Consider the question, "Does a 4-3-3 formation improve performance?" List the parts that are missing. Then write one precise version of the question.

Show a suggested answer

The question does not define the teams or matches, the comparison formation, the outcome, the decision time or the follow-up period. One precise version is: Among Premier League teams that could plausibly start in either formation, what is the average effect of starting a match in a 4-3-3 rather than a 4-4-2 on goal difference at the end of that match?

The revised question is still not identified. Teams choose formations in response to opponents, available players and match plans. Later lessons explain how to represent and address those causes.

## What comes next

This lesson stops before the answer. We still have no way to see the missing *Y(0)*, and no amount of care in writing the question will produce one. The next lessons build the comparison group that stands in for it: Lesson 3 chooses the variables to adjust for, Lesson 4 fixes the timing, and Lessons 5 to 7 construct the comparison itself. Lesson 7 returns to De Paola and Scoppa and shows how they did it, and what they found.

## Practice

An [[Football CI 01 — Define a causal question and estimand|exercise sheet of eight questions]] accompanies this lesson. It takes about thirty minutes and carries worked answers at the end. Attempt it before moving to Lesson 2.

## Check yourself

Which question defines an ATT most clearly? Observed points after coach dismissal among eligible clubs Five-match dismissal effect among clubs that dismissed coaches Dismissal frequency among clubs with successful recent resultsCheck answer


> [!tip]- Reveal the answer
> Correct. The question defines the eligible clubs, both strategies, the outcome, the horizon and the ATT target population.

## What to remember

- A causal effect compares potential outcomes under two treatment strategies for the same unit and decision point.
- An observed outcome is not a causal effect, because the other potential outcome is missing.
- An estimand names the target population and the average effect that the study will try to identify.
- Identification depends on assumptions such as consistency, exchangeability and positivity.

**Primary reading.** Hernan and Robins, *Causal Inference: What If*, Chapters 1 and 3. The football setting comes from [De Paola and Scoppa (2012)](https://doi.org/10.1177/1527002511402155). Use the to review the new terms.

If any part of the potential-outcome comparison is unclear, ask your teaching agent to work through another football example with you.

Start of course[[Football CI 02 — Analyse a cluster-randomised football trial|Next lesson: Cluster randomisation and intention to treat]]
