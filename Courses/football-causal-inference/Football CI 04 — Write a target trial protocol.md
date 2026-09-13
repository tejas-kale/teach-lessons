---
type: unit
course: "[[Causal Inference through Football]]"
status: not-started
order: 4
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

A target trial describes the randomised experiment you would run if it were practical. Writing the protocol before analysis prevents several common timing and eligibility errors.

About 25 minutesIncludes guided exercises[[Football CI 04 — Write a target trial protocol|Exercise sheet]]

## What you will be able to do

You will be able to specify a target trial and align eligibility, treatment assignment and follow-up at one time zero.

## Evidence gap

The paper search did not find a strong football target-trial emulation. This lesson constructs a coach-dismissal protocol from the mapped coach studies. It does not claim that those studies used target-trial emulation.

## The decision that the trial represents

After matchweek 10, a club board must decide whether to dismiss its coach now or retain the coach. We want to know how the decision affects league points over the next five matches. Historical club data contain many coach changes, but the records do not automatically form a trial. We first need to state the trial that the data should emulate.

## Specify each part of the protocol

<table>
<thead>
<tr>
<th>Protocol part</th>
<th>Specification</th>
</tr>
</thead>
<tbody>
<tr>
<td>Eligibility</td>
<td>Top-flight clubs immediately after matchweek 10, with the current coach in post for at least eight matches and no departure already announced.</td>
</tr>
<tr>
<td>Treatment strategies</td>
<td>Dismiss within 48 hours, or retain the coach for the next five league matches.</td>
</tr>
<tr>
<td>Assignment</td>
<td>The ideal trial would assign strategies at random. The emulation adjusts for measured causes of the observed decision.</td>
</tr>
<tr>
<td>Time zero</td>
<td>The final whistle of the club's matchweek-10 fixture.</td>
</tr>
<tr>
<td>Follow-up</td>
<td>From time zero through the next five league matches.</td>
</tr>
<tr>
<td>Outcome</td>
<td>Total league points across those five matches.</td>
</tr>
<tr>
<td>Estimand</td>
<td>The per-protocol effect of following dismissal rather than retention among eligible clubs.</td>
</tr>
<tr>
<td>Analysis</td>
<td>Compare strategies after adjustment for pre-time-zero confounders, with a stated method for later deviations.</td>
</tr>
</tbody>
</table>

## Align eligibility, assignment and follow-up

All three must occur at the same time zero. Suppose dismissed clubs enter the study on the day they dismiss, while retained clubs enter after matchweek 10. A club classified as dismissed must remain alive and dismissal-free between matchweek 10 and its later entry date. That guaranteed period creates immortal-time bias.

The protocol removes this problem by starting both strategies at the same decision point. A club's eligibility information and baseline confounders must also be measured before or at that point.

## Worked example of cloning and censoring

A club is eligible at time zero but does not dismiss its coach until the third follow-up match. At time zero, the observed data are compatible with both strategies. An analyst can create two copies of the club, assign one copy to each strategy, and follow both. The copy assigned to immediate dismissal is censored after 48 hours because the observed club did not follow that strategy. The copy assigned to retention is censored when the club dismisses before five matches.

If the reasons for deviation also predict points, censoring is informative. Inverse-probability-of-censoring weights can account for measured reasons, but the method needs exchangeability and positivity for continued adherence.

## Exercise 1. Find the timing error

An analyst defines treated clubs as those that dismiss at any point during the season. Follow-up begins on the first day of the season for every club. Explain the error and propose one correction.

Show a suggested answer

The treated group is defined using future information. Clubs must remain dismissal-free until their treatment date, so early person-time is misclassified. The analyst can define repeated weekly decision points, assess eligibility each week, assign current strategies at that week and begin follow-up there.

## Specify treatment versions clearly

"Dismiss the coach" can represent different actions. A club may install a permanent replacement, use an interim coach or remove several staff members. If these versions have different effects and their mix differs across clubs, consistency becomes unclear. The protocol should define which versions belong to the strategy or state that it estimates the effect of a policy that permits the observed mix.

The same care applies to retention. Retaining for five matches is a strategy, while "not dismissed today" allows dismissal tomorrow. Those comparisons answer different questions.

## Exercise 2. Draft a small protocol

Write eligibility, two strategies, time zero, outcome and follow-up for the effect of returning an injured player to full training. Keep each part to one sentence.

Show a suggested answer

Eligibility could require medical clearance for partial training on Monday morning. The strategies could be immediate full training or continued modified training for seven days. Time zero is Monday morning after assessment. The outcome is reinjury within 28 days, and follow-up starts at time zero for both groups.

## Practice

An [[Football CI 04 — Write a target trial protocol|exercise sheet of 7 questions]] accompanies this lesson. It takes about thirty minutes and carries worked answers at the end. Attempt it before moving on.

## Check yourself

What is the main problem with different entry dates? The outcome uses too few categories The groups have misaligned time zero The trial has random treatment assignmentCheck answer


> [!tip]- Reveal the answer
> Correct. Starting the groups at different decision times creates immortal-time bias.

## What to remember

- A target trial defines the experiment that an observational analysis aims to emulate.
- Eligibility, treatment assignment and follow-up must align at time zero.
- Treatment strategies must be specific enough to support consistency.
- Later deviations require a stated per-protocol analysis.

**Primary reading.** Hernan and Robins, *What If*, Section 3.6 and Chapter 22.

If any step is unclear, ask your teaching agent to work through another football example with you.

[[Football CI 03 — Use a causal diagram to choose controls|Previous lesson: Use a causal diagram to choose controls]][[Football CI 05 — Design a matched comparison|Next lesson: Design a matched comparison]]
