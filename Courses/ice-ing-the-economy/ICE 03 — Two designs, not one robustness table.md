---
type: unit
course: "[[ICE-ing the Economy]]"
status: not-started
order: 3
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

OLS on treated metros is confounded. The paper’s answer is not “add more controls.” It is a shift-share instrument for metro onset, and a completely different within-metro distance design that does not use the instrument at all.

≈9 minutes · Unlocks supplement “Shift-Share Estimation” and “Spatial Intensity Estimation”

> The identification logic is that places with historically higher shares of immigrants are more likely to be targeted when a national surge in enforcement is enacted, but the timing of targeting any given metro is exogenous to economic conditions affecting POIs in that metro.
>
> Hernandez (2026), Data and Methods

## Why the treated dummy is endogenous

In 2024, eventually-treated metros had ~1.8 million people vs 153 thousand; foreign-born share 0.147 vs 0.056; Harris vote share 0.524 vs 0.349; higher weekly spend; *rising* spend trends and *weaker* visit trends. Hernandez notes that even OLS with those covariates still leaves endogenous pre-trends. You cannot difference that away by staring at the coefficient.

## Design A — shift-share 2SLS

Instrument for metro *m* in week *t*:

`Z`<sub>`m,t`</sub>` = s`<sub>`m,2010`</sub>` × R`<sub>`t`</sub>

`s`<sub>`m,2010`</sub> is the foreign-born share from the 2006–2010 ACS — frozen more than a decade before treatment. `R`<sub>`t`</sub> is the *national* weekly count of the 5,388 events (about 12/week pre-inauguration, 86/week after, peak 355). First stage: onset on `Z`, with the same fixed effects as the second stage.

Second stage (IHS visits or IHS spend): IHS(y<sub>it</sub>) on instrumented Treated<sub>m(i),t</sub>, plus POI FE and state×week FE.

- **POI FE**: brand, location, industry, baseline mix — anything time-invariant about the store.
- **State × week FE**: weather, holidays, state policy, statewide news, macro week effects that hit every metro in the state equally.
- **Cluster:** CBSA. The treatment is assigned at metro level; clustering anywhere finer is theatre.

Reported Kleibergen-Paap first-stage \\F\\ is ~30 to &gt;100. Table S13 swaps in **1950** foreign-born shares: visits still work; spend’s first stage is too weak for a precise second stage. That is a useful tell — the instrument is not infinitely robust to “any historical share.”

Goldsmith-Pinkham, Sorkin and Swift (2020) emphasise share exogeneity. Borusyak, Hull and Jaravel (2022) emphasise shock exogeneity. Hernandez cites both and narrates a *mix*: old shares × a national policy surge. A journal-club fight over which story is load-bearing is legitimate; the draft does not run a full BHJ exposure-weighted shock design as a separate paper.

## Pre-trends as a share test

Figure S4 is a reduced-form event study: outcomes on event-time bins × the share component, from 20 two-week bins before onset down to −2. The linear-slope tests fail to reject zero. Joint Wald tests often reject — Hernandez attributes that to N ≈ 500 million, where tiny wiggles are “significant.” Spending with state×week FE is the panel where even he flags a joint-test rejection. Remember that sentence when someone waves “we passed pre-trends.”

## Check yourself

What variation identifies \(\beta\) in the main 2SLS?

- [ ] Comparing two stores on the same street, the week a raid hits the nearer one.
- [ ] National week dummies, because \(R_t\) is national.
- [ ] Cross-sectional differences in 2024 income between treated and untreated metros.
- [ ] Within-POI changes in onset, with onset instrumented by 2010 immigrant share × national raid volume, after state-week shocks are absorbed.

> [!tip]- Reveal the answer
> POI FE kill store levels. State×week FE kill anything common to a state that week. What remains is differential onset across metros in the same state, with the differential predicted by historical nativity interacting with the national surge — not the raw treated dummy.

## Design B — spatial intensity, instrument off

Restrict to treated metros. For each POI-week, take great-circle distance to the *nearest raid so far* (running minimum). Bin: 0–0.5, 0.5–1, 1–2, 2–5, 5–10, 10+ miles, plus a “none yet” omitted group. IHS outcomes on those bin dummies, plus POI FE and CBSA×week FE.

The new fixed effect is **CBSA × week**, not state × week. Anything that hits the whole metro that week — including the metro-average treatment effect, the local protest, the sports calendar, the entire national enforcement intensity as it lands on Chicago that week — is absorbed. Identification is a *dose*: stores closer to the accumulating epicenter versus farther stores in the same city-week.

Predicted pattern under local chilling: coefficients most negative near 0–0.5 miles, fading toward the periphery (linear trend p = 0.005 visits, p &lt; 0.001 spend). A flat gradient would be a problem for the mechanism even if Design A looked pretty.

Hernandez is explicit that this design is *too saturated* for demographic and industry interactions. Those interactions go back to Design A.

> [!abstract] Do not collapse the two designs
> Design A answers “what happens to POIs in targeted metros versus not, using predicted targeting.” Design B answers “conditional on the metro already being targeted, is the damage local to raid sites?” Agreement is a joint test of a chilling story. It is not two prints of the same IV.

## Retrieval Practice

A colleague says: “CBSA×week FE in Design B already absorb confounding, so we can throw away the shift-share.” What do you lose if you do?

> [!example]- Model answer
> You lose the contrast between targeted and never-targeted metros — the extensive-margin, citywide chill. Design B can only see a gradient inside treated cities. If enforcement makes the whole metro quieter by the same percent, Design B’s distance coefficients can be near zero while Design A’s β is large. You need both sentences.

## What you can now read

You can read “I use a shift-share, and also distance” as two estimands. Next, the numbers: [[ICE 04 — What the estimates say|Lesson 4]].

**The fork worth arguing.** Are you happier with share exogeneity (2010 nativity uncorrelated with 2025 metro-week residual demand after FE) or with shock exogeneity (national weekly raid volume as-if random relative to any one metro)? Say which, then read GSS and BHJ with that question marked.

Read one thing

1.  Goldsmith-Pinkham, Sorkin and Swift, *AER* 2020, introduction plus the “when are shares exogenous?” discussion — then Hernandez’s S4 pre-trend write-up.

[[ICE 02 — What was actually measured|← Lesson 2]] [[ICE 04 — What the estimates say|Lesson 4: Estimates →]]
