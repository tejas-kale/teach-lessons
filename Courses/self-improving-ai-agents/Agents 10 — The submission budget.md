---
type: unit
course: "[[Self-Improving AI Agents]]"
status: not-started
order: 10
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

You can generate a million candidates and check only ten. What happens in between is the entire system, and one year of improvements to it doubled performance without adding a single sample.

About 10 minutes · Lecture 7 · The most complete worked example in the course

Every lesson so far assumed you could evaluate the candidates you generate. Competitive programming breaks that assumption in a way your work also does, because you may submit only a handful of solutions to the contest platform. Generating is cheap and **checking is rationed**.

That constraint has its own metric. Alongside pass@k, AlphaCode reports **10@k**, which asks how many problems are solved if you generate k candidates and submit ten.[¹](#s1) pass@k measures your search, and 10@k measures search and selection together. The gap between them is the generation-verification gap from Lesson 3 with a hard budget attached, and in the AlphaCode results it is large, at roughly 40 percent against 30 percent.

You have this constraint more often than you notice. Anything with a rate limit, a per-call cost, a human reviewer, or an irreversible action is rationed checking. If a human approves your agent's output, your submission budget is one, and 10@k with ten set to one is the metric describing your working life.

## AlphaCode, a million samples and a funnel

The pipeline is worth walking through as a template.

![[2449333542237b5ee58f77b0ed64f377db4e1825.svg]]

*Roughly 95 percent of samples are removed by running the example tests, which is free, deterministic, and by far the highest-yield step. Clustering then groups programs that behave identically, by running each candidate on generated test inputs and grouping by output behaviour. Submitting one from each of the ten largest clusters spends the budget on ten genuinely different approaches rather than on ten spellings of one.*

The clustering step is the elegant part, and it generalises. You cannot tell which candidate is right, and you can cheaply tell which candidates are the same. Deduplicating by behaviour converts a scarce checking budget into the widest possible coverage of distinct hypotheses. The equivalence is behavioural rather than textual, so two programs written differently that agree on every input count as one candidate.

The paper is honest about two limitations. Training loss is a poor proxy for solve rate, and the whole thing needed sampling at a scale that is impractical for most uses.

## Check yourself

Your agent produces eight candidate analyses and a human can review two. What is the highest-value step before choosing?

- [ ] Rank all eight with a judge model and pass the two highest-scoring ones to the reviewer.
- [ ] Group them by the conclusion they reach, then send one from each of the two largest groups.
- [ ] Fuse all eight into a single analysis and use the freed review slot for a second opinion.
- [ ] Discard any that fail your deterministic checks and rank whatever survives by length.

> [!tip]- Reveal the answer
> If six of the eight reach the same conclusion, ranking them probably sends the reviewer two versions of one answer, and the point of a second slot is to see a genuinely different one. Cluster first, then rank within clusters. The third answer is strong and often right, as Lesson 6 argues, but it destroys exactly the disagreement a human reviewer is best placed to settle. When the scarce resource is human judgement, preserve the diversity rather than averaging it away.

## AlphaCode 2, and what a year of system-building bought

The follow-up is the more useful half of this lecture, because it isolates which improvements mattered.[²](#s2) There were three changes.

- **Stop pre-training your own model** and fine-tune a strong general model instead, in this case Gemini Pro.
- **Engineer diversity deliberately.** Rather than relying on temperature alone, fine-tune a family of model variants on different data segments and hyperparameters, then split sampling across them.
- **Replace heuristic selection with a learned scoring model**, meaning a fine-tuned reward model that ranks within clusters.

> [!abstract] The number that matters
> AlphaCode 2 reached AlphaCode's solve rate with about 100 samples instead of a million, which is four orders of magnitude less compute. At an equal million-sample budget it roughly doubled the solve rate, from about 25 percent to about 43 percent, landing near the 85th percentile of human contestants. None of that came from sampling harder. It came from a better base model, deliberately diverse candidates, and a learned selector.

That comparison is the strategic lesson of the whole course. Sampling is the dial you reach for first because it is trivially available, as Lesson 2 showed. It also has the worst returns per unit of engineering. The compounding gains live in the parts around it, and each of the three improvements is one you can make.

## What this means for you

- **Filter with free checks before spending expensive ones.** Running the example tests removed 95 percent of candidates at nearly zero cost. Your equivalents are whether it parses, whether the query runs, whether the schema matches, and whether the total reconciles, and they should all run before any model or human is consulted.
- **Deduplicate by behaviour rather than by text.** Two analyses reaching the same conclusion by different routes are one candidate, and two reaching different conclusions are the interesting case that a scarce reviewer should see.
- **Manufacture diversity rather than hoping for it.** Temperature gives you shallow variation, and different models, prompt framings, and assumed constraints give you different approaches. The AlphaCode 2 team fine-tuned variants for this, and you get most of it free by using several model families.
- **Know your real submission budget.** Write down how many candidates you can actually verify per task and design the funnel to fit. Most pipelines are implicitly built for a budget of one and never ask whether that was right.

One metric follows from all of this. Track n@k, where n is your true checking capacity, rather than pass@k alone. It is the number that describes the system you actually run.

The lecture raises one caution. Sampling more only helps if the extra samples are genuinely diverse, and ten times more samples that collapse into the same few behaviours buy nothing. That makes clustering more than a budget-saving device, because it also tells you whether your generation is exploring at all. If your candidate count rises and your cluster count does not, stop paying for samples.

Project hook

How fast does your cluster count saturate? Generate 50 candidates for each of ten tasks, cluster them by behaviour or conclusion, and plot distinct clusters against candidates generated. The curve tells you the k beyond which sampling is buying nothing.

Measure Distinct clusters against k, per task type, and compare against the same curve when candidates come from two model families instead of one. The second curve should saturate later, and if it does not, temperature was doing more than you thought.

## Retrieval Practice

Write out the funnel for one workflow you own. How many candidates could you generate, what free checks remove most of them, how would you cluster the survivors, and how many can you actually verify?

> [!example]- Model answer
> Here is a worked example for an agent proposing data-quality fixes. Generate 20 proposals across two model families. The free checks ask whether the SQL parses, whether it runs against a sandbox, and whether it changes a plausible number of rows, which typically kills half. Cluster by the set of rows each proposal actually changes when run on a sample, so proposals with identical effects count as one candidate no matter how differently they are written. Then a human reviews three. The funnel turns 20 proposals into perhaps four distinct behaviours, and the reviewer sees the three most distinct rather than three phrasings of one fix. Notice that the clustering step needed a sandbox, which is Lesson 8's prerequisite again.

## What you can now do

You can design a funnel from a large candidate pool down to a rationed checking budget, using free checks first and behavioural clustering to preserve diversity. You also have the AlphaCode 2 comparison as an argument for where to spend engineering effort, because the base model, diversity, and selection compound while raw sample count does not.

Next is [[Agents 11 — Deep research agents|Lesson 11, deep research agents]], the architecture from this same lecture that maps most directly onto analysis work.

**The clustering step is where designs differ most.** What "the same answer" means is domain-specific and genuinely hard. For code it is output behaviour, and for analysis it might be the conclusion or the decision implied. Worth working out on a real case.

Read one thing

1.  [Li et al., *Competition-Level Code Generation with AlphaCode*](https://arxiv.org/abs/2203.07814) (DeepMind, 2022). Read section 4 on filtering and clustering, which is the transferable part.
2.  **Primary source:** [CS329A Lecture 7, Self-Improvement and Deep Research Agents](https://www.youtube.com/watch?v=Uni9dqyuuDM) (Tandon), which presents both AlphaCode papers side by side. The year-over-year comparison is the reason to watch it.
3.  [Brown et al., *Large Language Monkeys*](https://arxiv.org/abs/2407.21787). Revisit section 4 now, because the log-linear solve-rate trend survives the selection stage, which is not obvious and is what makes the funnel worth building.

[[Agents 09 — Buy or tune|← Lesson 9]] · [[Agents 11 — Deep research agents|Lesson 11: Deep research agents →]]
