---
type: unit
course: "[[Self-Improving AI Agents]]"
status: not-started
order: 15
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

The failure mode of a first project is not running the experiment badly. It is picking a question too big to answer, and finding out in week three.

About 10 minutes · Grounded in how CS329A assesses its own course projects · Produces one written question

Fourteen lessons have handed you fourteen project hooks. This lesson turns one of them into something you can finish in two to four weeks of part-time work, alone, with no GPU budget and no co-author.

It is worth being clear about the target, because it is a specific one. CS329A asks its students for a hypothesis, an experiment, and a result, and explicitly rejects two things: a survey of what other people found, and an app that does something without asking a question. As the lecturers put it, they want "here is the hypothesis, here is the question we wanted to answer, here is the type of improvement we wanted to see", and not a piece of web coding. You are aiming at that shape, scaled down from a team of four across a quarter to one person across a month.

## What makes a question finishable

A question you can finish has four properties. Test any candidate against all four before you commit, because a question that fails even one will consume the whole month.

<table>
<thead>
<tr>
<th>Property</th>
<th>The test</th>
<th>What failing it looks like</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Falsifiable</strong></td>
<td>You can state now what result would prove you wrong</td>
<td>"I want to explore whether agents can help with X"</td>
</tr>
<tr>
<td><strong>Measurable today</strong></td>
<td>The metric exists, or you can build it in one day</td>
<td>The metric needs a labelled set you do not have</td>
</tr>
<tr>
<td><strong>Controlled</strong></td>
<td>There is an obvious A and B, differing in one thing</td>
<td>The comparison needs three things to change at once</td>
</tr>
<tr>
<td><strong>Cheap to run</strong></td>
<td>One full experimental pass costs under an hour and under your API budget</td>
<td>A single pass takes overnight, so you get five attempts total</td>
</tr>
</tbody>
</table>

The fourth property matters more than it looks. You will run the experiment wrong the first two times, discover a bug in your grader on the third, and only then start collecting real data. A question whose experimental loop takes ten minutes gives you room for that, and a question whose loop takes eight hours does not. Optimise the loop before you optimise the design.

## Cutting a hook down to size

Most of the hooks in this course are still too big as written. Here is the same question at three scales, so you can see what shrinking actually does.

<table>
<thead>
<tr>
<th>Scale</th>
<th>The question</th>
<th>Time</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Too big</strong></td>
<td>Does verifier ensembling improve agent reliability?</td>
<td>Never finishes. No population, no metric, no control.</td>
</tr>
<tr>
<td><strong>Still too big</strong></td>
<td>Does a three-verifier ensemble beat a single judge across my workflows?</td>
<td>Two months. "My workflows" hides five different task types.</td>
</tr>
<tr>
<td><strong>Right size</strong></td>
<td>On 40 labelled outputs from my weekly analysis workflow, does a three-verifier ensemble with learned weights select correct outputs more often than the single strongest verifier alone?</td>
<td>Three weeks, part-time</td>
</tr>
</tbody>
</table>

Three things happened in that last cut. The population became specific and countable, at 40 outputs from one workflow. The comparison became a pair, at ensemble against best-single. And the metric became one number, which is selection accuracy. You should be able to describe your own question in one sentence with those three parts in it.

> [!abstract] The honest scope rule
> Estimate how long your project will take, then ask what you would cut if you had one week rather than four. Do the one-week version first. If it produces a result, you have three weeks to extend it, and if it does not, you have learned that in week one rather than week four. Most project failures are scope failures discovered too late to recover.

## Where good questions come from

Lesson 14 gave you the field's own list of open problems, and each has a practitioner-scale version that nobody has published because nobody publishes results about one company's workflows. That is not a weakness of your project. It is the reason your result is worth having, since it is about the system you actually run.

Four sources are worth mining, in rough order of how likely they are to produce something finishable.

- **A published result you can replicate on your data.** The paper did the design work. You are testing whether the finding survives contact with your domain, which is a real contribution and a bounded one. The difficulty rule from Lesson 2 and the fusion result from Lesson 6 are both good candidates.
- **A number nobody has measured about your own system.** The generation-verification gap on your tasks, your verifier's turnover point, your reliability gap in human minutes. These are guaranteed to produce a result because measurement always does.
- **A claim in this course you doubt.** Disagreement is a research question wearing casual clothes. If you think fusion will lose on your task type, that is a hypothesis with a control built in.
- **A cheap version of a frontier bottleneck**, meaning diversity across model families, meta-verification of your judge, or eval curation toward the mixed band.

## Check yourself

Which of these is the best-scoped first project?

- [ ] Build an agent that writes our weekly reports and evaluate whether it saves the team time.
- [ ] Investigate whether multi-agent debate improves the quality of analytical reasoning overall.
- [ ] Test whether error messages with expected and actual values cut turns-to-fix on 30 tasks.
- [ ] Compare every technique in this course on one workflow and report which helped the most.

> [!tip]- Reveal the answer
> The third one names the population, the intervention, and the metric, and one experimental pass costs minutes. The first is an engineering project with an evaluation bolted on, and time saved is confounded by everything. The second has no population and no control. The fourth is six projects, and it will produce six underpowered comparisons rather than one solid result. Notice the third is also the least ambitious-sounding, which is normally the sign of a project that finishes.

## Write the pre-registration

Before running anything, write half a page. This is the single highest-value habit in Arc V, because it stops you from finding a result after the fact and believing you predicted it.

1.  **The question,** in one sentence, with the population, the comparison, and the metric in it.
2.  **The prediction,** with a direction and a rough size. "The ensemble wins by 5 to 15 points" is a real prediction, and "the ensemble will be better" is not.
3.  **What would falsify it.** Write the result that would make you abandon the hypothesis, so you cannot quietly move the goalposts later.
4.  **The stopping rule.** How many tasks, how many runs each, and when you stop collecting. Decide before you see any data, because deciding afterwards is how noise becomes a finding.

Keep it in the workspace as a file. When you write up in Lesson 17, the difference between what you predicted and what you found is the most interesting paragraph you will write, and you can only have it if you wrote the prediction down.

Do this now

Pick the hook you have most wanted to run, then cut it twice using the three-scale table above. Write the one-sentence version, then write the four-part pre-registration underneath it.

Measure If you cannot state what result would prove you wrong, the question is not ready and no amount of experimental care will rescue it. Go back and cut again.

## Retrieval Practice

Write your question in one sentence, containing the population, the comparison, and the metric. Then write the sentence describing the result that would falsify it.

> [!example]- Model answer
> Here is a worked pair. "On 30 tasks from my SQL-generation workflow, does replacing the bare error string with a message containing the failing query, the database error, and the table schema reduce the mean number of turns to a passing result?" And the falsifier: "If mean turns-to-fix differs by less than half a turn, or if the rich-error condition is worse, the hypothesis is wrong and error richness does not matter at this task difficulty." Notice the falsifier names a threshold. Without one you will find a 0.2-turn improvement and be tempted to call it a result.

## What you can now do

You have one written question, scoped to something you can finish, with a prediction and a falsifier attached. That is the deliverable of this lesson, and it is worth more than the experiment itself, because a well-chosen question makes the experiment straightforward and a badly chosen one makes it impossible.

Next is [[Agents 16 — Running the experiment|Lesson 16, running the experiment]], which covers the control condition and the ways a small experiment lies to you.

**Bring the question here before you build anything.** Scoping is the step where a second opinion is worth the most, and it is much cheaper to cut a question in conversation than to discover in week three that it was two questions. Paste your pre-registration in and we will attack it.

Read one thing

1.  **Primary source:** [CS329A Lecture 1, Course Overview](https://www.youtube.com/watch?v=6YnLB0XbTnI), the final third on course logistics. The lecturers describe what they will and will not accept as a project, which is where this lesson's target comes from.
2.  [The CS329A course site](https://cs329a.stanford.edu/) publishes previous years' successful student projects, which are the closest thing available to a worked example at this scale.
3. in this workspace compresses Arc V into one page, including the pre-registration template.

[[Agents 14 — The frontier|← Lesson 14]] · [[Agents 16 — Running the experiment|Lesson 16: Running the experiment →]]
