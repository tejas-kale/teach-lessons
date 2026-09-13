---
type: unit
course: "[[ICE-ing the Economy]]"
status: not-started
order: 2
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

Three panels glued together: a news-built raid file, weekly visits at 5.4 million POIs, and card spend at a much thinner slice of those POIs. Treatment is an absorbing metro switch, not a raid dummy on the store’s doorstep.

≈8 minutes · Unlocks Data and Methods plus supplement §§1–5

> I rely on media reports, rather than official arrest records, for two reasons. First, I am interested in how individuals’ economic behaviour is affected by their perceptions of immigration enforcement activities. … Second, arrest records are incomplete and not well-suited to geocoding.
>
> Hernandez (2026), Data and Methods

## Layer 1 — 5,388 geocoded events

The raid database is an NLP pipeline, not a FOIA dump. Common Crawl News (~778k filtered articles) plus LexisNexis (~137k) are dual-keyword gated (agency term × action term, with “ice cream / ice hockey” exclusions). DeepSeek V3 extracts structured events against a 200-article human validation set (precision 77.8% vs Haiku 3.5’s 61.8%). Deduplication (within source, then BallTree across sources, then manual audit) takes 12,284 candidates down to **5,388** geocodable interior events.

Geocoding is mostly named establishments via Google Places (66%), then neighbourhood centroids (14%), rarely rooftop addresses (2%). That precision is good enough to support Lesson 3’s distance bins; it is not a GPS trace of every agent.

The same LLM pass yields 46,174 articles with chilling-effect language. That corpus is *not* the treatment. It is qualitative corroboration (Lesson 4).

## Layer 2 — Advan visits

Advan Weekly Patterns Plus (Dewey): ~45 million devices, Monday–Sunday weeks, January 2024 through the first week of February 2026 (110 weeks). After dropping non-CBSA POIs and mega-infrastructure (&gt;1 million visits/week), **5.36 million POIs in 935 metros**. Panel: 494 million POI-weeks, ~84% balanced, outcomes transformed with the *inverse hyperbolic sine* because of zeros and skew (mean weekly visitors 1,935; median 567).

## Layer 3 — SafeGraph spend

Weekly card spend is aggregated from `SPEND_BY_DAY` to the same Monday–Sunday weeks, ending December 2025. Matching PLACEKEY to Advan `footprint_id` is a four-tier NAICS/distance/name/address crosswalk: **1.17 million** unique pairs — about 24% of POIs, and only ~11% of POI-weeks in the analysis panel have spend.

Two vendor facts a data scientist should refuse to skip:

- Mid-October 2025, SafeGraph’s coding of non-branded POIs dropped spend ~33%. Hernandez applies Dewey’s recommended **1.5× multiplier** after 20 October 2025 — which *shrinks* a finding of decline — and also drops post-break weeks in Table S14.
- Eleven insane POIs are dropped; spend is winsorized at the 99.9th percentile.

## Check yourself

Why is the spend result estimated on a different sample than the visit result?

- [ ] SafeGraph only covers restaurants.
- [ ] Only a minority of Advan POIs match to SafeGraph, and spend is missing in many weeks even after a match.
- [ ] Advan visits are monthly, so they cannot be aligned to spend.
- [ ] Workplace POIs have spend but no visits.

> [!tip]- Reveal the answer
> The crosswalk is 24% of POIs and ~11% of POI-weeks. Workplace sites generally have visits and no retail spend. Table S12 re-runs visits on the spend-reporting subset as a check that the visit result is not an artifact of sample composition.

## Treatment is a metro burst, then it never turns off

ICE names city-level operations. Onset for CBSA \\m\\ is the first week, after 20 January 2025, in which **three or more events fall in a 14-day window**. 100 of 935 metros are treated; 835 never are. Once on, treatment is *absorbing* in this sample — consistent with a standing interior presence, not a one-off raid. Large metros use Census Metropolitan Divisions rather than the consolidated CBSA.

Table S15 reports 5-event and 10-event onset rules. The binary treatment is still endogenous: treated metros are larger, richer, more immigrant, more Hispanic, more Democratic, and had different 2024 visit and spend *trends*. That table is why Lesson 3 exists.

![[1e7111b4bfb16dc485c561ae46764701c2b8ab40.svg]]

*Onset is a cluster detector, not “any article mentioning ICE.” Isolated events do not turn the metro on. After onset, every subsequent POI-week in that metro is treated.*

## Covariates you will see interacted later

Visitor demographics are not individual race or nativity. Advan reports home census block groups of devices; those blocks are joined to 2023 ACS foreign-born and Hispanic shares, then compared to the *metro* median. The visitor mix is computed on January–October 2024 so it is not itself caused by the regime (or even by the November 2024 election). Chain vs independent uses Advan brand vs NAICS subcategory. Online spend and delivery-app overlap are monthly SafeGraph fields copied onto weeks, identified off POI and time fixed effects.

## Retrieval Practice

Name the three reasons a naïve POI-week regression of IHS(visits) on the treated dummy is not “the effect of ICE.”

> [!example]- Model answer
> (1) Treated metros differ in levels and in 2024 trends (size, nativity, partisanship, spend/visit slopes). (2) Treatment is defined from news, so coverage intensity could correlate with local media markets and with the same economic outcomes. (3) Onset is staggered and absorbing; OLS two-way FE with endogenous targeting is not a design. Lesson 3’s instrument and spatial gradient are the proposed answers — not extra controls in the same OLS.

## What you can now read

You can read the Data and Methods section as a *measurement paper* stuffed inside a causal paper. Next: why 2SLS with a 2010×national-week instrument, and why a second design throws the instrument away: [[ICE 03 — Two designs, not one robustness table|Lesson 3]].

**Ask your teacher** if the 3-in-14 rule feels arbitrary. The honest answer is: it is a burst detector chosen to mark operations, and S15 is the sensitivity table — not a proof that three is a natural constant.

Read one thing

1.  Supplement §§1–5 of Hernandez (2026): event pipeline, Advan filters, SafeGraph crosswalk and the October 2025 vendor break.

[[ICE 01 — The demand side was missing|← Lesson 1]] [[ICE 03 — Two designs, not one robustness table|Lesson 3: Two designs →]]
