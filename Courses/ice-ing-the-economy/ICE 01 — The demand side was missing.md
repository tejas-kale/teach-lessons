---
type: unit
course: "[[ICE-ing the Economy]]"
status: in-progress
order: 1
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---
The paper’s claim is not “raids are bad.” It is that interior enforcement in 2025 is a *general* local shock, and that the literature had almost no estimates on the demand side of that shock.

≈8 minutes · Unlocks the first two pages of Hernandez (2026)

> Research has not considered whether immigration enforcement affects the demand side of the economy (i.e. spending-related activities), even though theory has long held that immigration’s supply- and demand-side effects are inseparable.
>
> Hernandez, *ICE-ing the Economy*, draft 13 May 2026

## What changed in January 2025

Hernandez is not restudying “Secure Communities” or a single worksite raid. The object is the first year of the second Trump administration: resources move from the border to the interior; street arrests rise sharply; operations are highly visible in public space; and, citing East, Cox and Patler (NBER 34794), people *without* criminal convictions overtake those with convictions among arrestees.

Call that a shift from *focused* to *general* enforcement. Focused enforcement is easier to think about as a treatment aimed at a demographic slice. General, roving presence is easier to think about as a local public-safety and local-demand shock that non-targeted people can also see.

East et al. are the measurement paper for *who* is arrested. Hernandez’s contribution is *what local businesses see* once that regime is in the streets.

## What the prior literature already knew

Chilling effects are not new. Focused enforcement has been shown to deter immigrants and Hispanics from Medicaid and other safety-net programs, from school, from healthcare, and from labour-market engagement (Watson 2014; Alsan and Yang 2024; Dee and Murphy 2020; East et al. 2023; Amuedo-Dorantes and Antman 2022). When papers ask about people who are *not* the likely target, they almost always ask a labour-supply question: do native employment and wages move when immigrant workers are removed?

That is a supply-side, factor-market frame. Peri (2016) and Cortes (2008) are cited for the theoretical reminder that immigrants are also consumers, and that prices, amenities, and product demand move with them. Hernandez’s move is empirical: take high-frequency visits and card spend at millions of establishments and ask whether the new interior regime shows up there.

## Check yourself

Which sentence is closest to the paper’s stated gap?

- [ ] Nobody had studied whether ICE arrests reduce immigrant employment.
- [ ] Nobody had geocoded a raid, so all prior work was qualitative.
- [ ] Demand-side activity (visits and spending) after enforcement had not been estimated at national scale, even though theory treats supply and demand as a package.
- [ ] Labour economists had ignored native workers entirely.

> [!tip]- Reveal the answer
> Labour-market and safety-net chilling are a thick literature. The missing object is local economic activity on the demand side — and, in this paper, workplace visits as the supply counterpart — under a regime that is no longer tightly focused.

## The estimand, said without the instrument

Hold this sentence until Lesson 3 gives it a design: **what is the effect of a metropolitan area being pulled into the 2025 interior-enforcement regime on weekly visits and weekly card spending at commercial points of interest?**

Two implications follow immediately, before any regression.

1.  **The unit of treatment is the metro, not the arrested person.** ICE runs named city operations. A bakery can lose traffic because the city feels occupied, not only because a customer was detained yesterday.
2.  **Mechanical removal is the wrong mental model for the whole effect.** 5,388 geocoded events cannot, by headcount, explain an 8.1 billion visit shortfall if the only channel is “those people are in detention.” The paper’s theory of change is *chilling*: fear, hiding, skipped shifts, skipped errands, quieter sidewalks.

> [!abstract] The one-line version
> Prior work asked whether targeted people stop using public programs and whether native wages move. This paper asks whether the city itself gets quieter and poorer at the cash register.

## Retrieval Practice

Without scrolling: why would a data scientist refuse to treat “number of ICE arrests in the county” as the same treatment as Hernandez’s object?

> [!example]- Model answer
> Arrests are the yield of an operation and are poorly geocoded below the city. The paper wants the community’s perception of presence — agents in public, including events with no arrest — because that is what can chill people who are not detained. Official totals also miss the qualitative shift to a standing, roving interior presence after January 2025.

## What you can now read

The abstract’s “breadth of the local economic damage mirrors the breadth of the enforcement approach” is a research-design claim, not a slogan. If enforcement is general, the prediction is declines that are not confined to Hispanic-neighbourhood grocery stores. Lesson 4 will test that. First you need to know what was measured: [[ICE 02 — What was actually measured|Lesson 2]].

**Bring the objection.** If your prior is “this is just labour supply with extra steps,” say so. Lesson 4’s workplace vs consumer split is the quantitative reply; you should be able to state the *logical* reply already.

Read one thing

1.  Hernandez (2026), Introduction (pages 2–3 of the draft): the focused-to-general contrast and the demand-side gap. Pair with East, Cox and Patler, NBER 34794, for who is being arrested.
