---
type: unit
course: "[[ICE-ing the Economy]]"
status: not-started
order: 4
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Keep two bins: the IV percent effects per POI-week, and the national scale-ups. Then the paper’s real claim — generality — which is a pattern of interactions, not a single star.

≈8 minutes · Unlocks Figures 1–5 and the Discussion

> Foot traffic falls 2.7% and spending declines 6.2% per POI per week—or 8.1 billion fewer visits and $3–14 billion in foregone spending in a single year.
>
> Hernandez (2026), abstract

## Headline IV effects (Design A)

Instrumented metro onset: **−2.73%** visits (p &lt; 0.001) and **−6.18%** spend (p = 0.01) per POI-week, or about 53 fewer visitors and $181 less spend. Spend splits into −5.4% transactions and −0.68% per transaction — mostly extensive margin. Visits remain significant on the SafeGraph-matched subset (Table S12).

The 8.1 billion visits and $3.1 billion (SafeGraph coverage) / $14 billion (if SafeGraph averages are applied to all Advan POIs) are *extrapolations* against 2024, described as a 2.9% drop in visits and 6.9% drop in spend nationally among covered activity. Treat those as communication, not as a second independent experiment.

## Closer is worse, and it does not wash out

Design B: effects intensify toward raid epicenters and decay with distance. Design A dynamics in four-week bands: visits −0.59% in weeks 1–4, deepening to −1.90% at week 13+ (Wald trend p &lt; 0.001). Spend is messier; the linear trend is insignificant (p = 0.488). The paper’s interpretation: a roving, standing presence, not a one-week scare.

The time-band visit coefficients are smaller than the pooled −2.73%. Read them as a split of the dynamic path, not as a contradiction of the headline until you re-open the tables with the same sample and FE.

## Generality is the result

<table>
<thead>
<tr>
<th>Slice</th>
<th>What happens</th>
</tr>
</thead>
<tbody>
<tr>
<td>Demography (Fig. 2)</td>
<td>Visits and spend fall in <em>both</em> above- and below-median foreign-born/Hispanic visitor and store neighbourhoods. Visit declines are larger in high-immigrant/Hispanic cells; spend gaps are statistically indistinguishable. This is the break from prior “chilling is concentrated in the targeted group” papers.</td>
</tr>
<tr>
<td>Industry (Fig. 3)</td>
<td>17 of 21 consumer sectors: hotels −9.11%, personal care −4.45%, fitness −3.62%; groceries −2.09%, gas −1.89%, pharmacies −1.72%. All five workplace groups decline; transportation −5.64%, construction −5.33%. Spend (fewer industries, noisier): home improvement −10.95%, full-service restaurants −7.51%, fast food −7.45%.</td>
</tr>
<tr>
<td>Workplace vs consumer</td>
<td>Workplace visits −4.87% vs consumer −2.50% (difference p &lt; 0.001). Workers not showing up is not a side note; it is the larger percent hit.</td>
</tr>
<tr>
<td>Independent vs chain</td>
<td>Independents: visits −4.09%, spend −10.85%. Chains: visits −0.69% (p = 0.092), spend −5.02%. Interaction p &lt; 0.001. Brand insulation, not immunity.</td>
</tr>
<tr>
<td>Online (Fig. 4D / text Fig. 5)</td>
<td>Online spend at physical merchants +1.22%, p = 0.592 (a nothing). Share of in-store card customers who also use delivery apps −0.51 pp (p &lt; 0.001). Caveat: Amazon-style pure-play is invisible.</td>
</tr>
</tbody>
</table>

## Check yourself

Which finding is the cleanest evidence that this is not only “Hispanic neighbourhoods stop shopping”?

- [ ] Spend and visits decline in below-median immigrant and Hispanic cells as well, and workplace POIs fall harder than consumer POIs.
- [ ] The first-stage F-statistic exceeds 30.
- [ ] Hotels fall 9% and groceries fall 2%.
- [ ] The qualitative news corpus contains 44,271 fear-and-hiding articles.

> [!tip]- Reveal the answer
> The demographic split is the direct test of “only the profiled group.” Workplace vs consumer says labour supply is in the outcome, not only frightened shoppers. Industry ranking and news quotes are consistent but not the identification of generality.

## Qualitative congruence, not a second causal estimate

46,174 articles with chilling language: fear/hiding 44,271; reduced commerce 12,612; work absenteeism 5,828; school absenteeism 5,312; lower foot traffic 4,603; closures 3,525. Correlation of raid counts with chilling language jumps from 0.372 in 2024 to 0.755 after inauguration. A December 2024 spike is an NPR 2019 poultry-raid story being recirculated — Hernandez flags it so you do not treat that week as 2024 operations. This is mechanism colour. It is not 2SLS.

> [!abstract] Policy sentence, stripped
> The welfare calculus of interior enforcement is not only fiscal cost plus humanitarian cost. Targeted metros absorb lost activity that lands on workers, customers, and owners regardless of status — with independent stores taking the worse percent hit, and local tax bases sitting on that same activity.

## Retrieval Practice

State the main result in three numbers and one qualifier, without the billions.

> [!example]- Model answer
> About −2.7% visits and −6.2% spend per POI-week in targeted metros (IV); effects stronger near raid sites and not fading over the first year in visits; declines show up across demographic cells, across most industries, on both workplace and consumer POIs, and without a detectable shift into the merchant’s own online channel.

## What you can now read

The Discussion’s “broaden the canonical agenda” line is earned only if you buy generality. Lesson 5 is the list of reasons you might not, yet: [[ICE 05 — What a reviewer still owns|Lesson 5]].

**Ask your teacher** to walk the $3bn vs $14bn fork with a whiteboard. One is “SafeGraph’s actual covered spend.” The other is “pretend every Advan POI spent like a SafeGraph POI.” They are not two estimates of the same parameter.

Read one thing

1.  Hernandez (2026), Figures 1–3 and the Discussion (policy vs scholarship paragraphs). Then Table S11 for the transaction vs ticket-size split.

[[ICE 03 — Two designs, not one robustness table|← Lesson 3]] [[ICE 05 — What a reviewer still owns|Lesson 5: Open questions →]]
