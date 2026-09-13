---
type: unit
course: "[[ICE-ing the Economy]]"
status: not-started
order: 5
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

The paper is a draft. The honest close is not a recap of −2.7%. It is the list of questions that would change your mind, plus the research agenda Hernandez names and does not estimate.

≈7 minutes · Unlocks caveats, S13–S15, and the last page of the Discussion

> Future work should expand beyond this study and explore longer-term outcomes of sustained enforcement such as entrepreneurship, investment, and innovation. Outcomes in these areas take longer to manifest, and may be even more damaging, than the short-term outcomes I study here.
>
> Hernandez (2026), Discussion

## Open questions the draft already points at

1.  **Longer-run firm outcomes.** Visits and weekly spend can mean deferred errands. Exit, delayed investment, and less founding are different parameters with different welfare weights. The panel ends early February 2026. That clock has not run.
2.  **Pure-play e-commerce.** Null online spend is spend *at the merchant’s own digital channel*. Amazon, Temu, a regional grocer’s warehouse that is not in the POI file — all off-screen. Delivery-app use falling is a hint against “everything moved to Uber Eats,” not a proof that consumption vanished rather than rerouted.
3.  **Income vs fear.** Workplace visit drops imply lost labour supply and possibly lost earnings, which can explain no online substitution. The paper does not decompose the spend decline into “scared to go out” versus “pay cheque missing.”
4.  **Who is the complier?** 2SLS with a shift-share is not the ATE for every US metro. It is a weighted response among metros whose onset is predicted by 2010 nativity × national volume. Small, low-immigrant places in the control pool are not the same object.

## Identification questions a data scientist should keep live

- **Share versus shock.** If 2010 immigrant metros are on a 2025 demand path that state×week FE miss (remote work, industrial mix, tourism), Design A is in trouble. Design B does not rescue the between-metro contrast. GSS vs BHJ is not pedantry here.
- **News as treatment.** Places with more local TV may enter the 5,388 with higher probability. If those same places have different foot-traffic measurement error (Advan device density), you have correlated selection into treatment and outcome. Manual audit helps; it does not break that knot.
- **Pre-trends and power.** Slope tests look fine; joint Wald often does not. With 494 million rows, “we fail to reject a zero slope” is more informative than “the joint test is happy.” Spending’s S4 panel with state×week FE is the one Hernandez himself flags.
- **1950 shares (S13).** Visits survive; spend’s first stage does not. That is a sensitivity, not a footnote to skip.
- **Vendor break.** The 1.5× fix is conservative for finding a decline, and S14 drops the post-20 Oct 2025 weeks. Still: independents are both the group with the vendor break and the group with the largest spend drop. Stay awake.
- **IHS and zeros.** Coefficients are approximate percent changes. They are not the same as a Poisson/PPML incidence-rate ratio, and they are a poor machine for summing to GDP without extra assumptions.
- **Staggered absorbing treatment.** The instrument is meant to handle endogenous timing. A reader trained on Goodman-Bacon / Sun-Abraham / Borusyak-Jaravel-Spiess should still ask how much of the pooled β is later-treated vs early-treated comparisons, even under IV.

## Check yourself

Which critique actually targets Design B rather than Design A?

- [ ] 2010 immigrant shares predict 2025 housing booms that state×week FE miss.
- [ ] The $14 billion figure double-counts SafeGraph coverage.
- [ ] Raids are reported more often in busy commercial corridors, so distance-to-raid partly picks “where journalists stand,” which is also where spend was always higher — unless POI FE and the running-minimum fully handle that.
- [ ] First-stage F below 10 in the 1950-share spend specification.

> [!tip]- Reveal the answer
> Design B lives on raid coordinates. If media geocoding concentrates on downtown retail, “near the epicenter” is not a random location in the metro. POI FE help with levels; they do not automatically fix time-varying reporting intensity along those corridors. The other options are Design A, scale-up, or weak-IV issues.

## What would count as a follow-up project

If you wanted to *use* this paper rather than summarise it:

- Rebuild onset with official ERO statistics at the finest geography they allow, and report the jaccard with the news file — even if you cannot geocode to POIs.
- PPML or a bounded IHS-to-percent translation on a 1% POI sample, to see whether the −6% spend is transformation-sensitive.
- A store-exit or employment (QCEW/ADP) outcome one year out — Hernandez’s stated next object.
- A BHJ-style shock-level design that treats weekly national (or regional) enforcement intensity as the as-if-random shifter and reports exposure-weighted reduced forms.

## Retrieval Practice

In four bullets: motivation, data, method, headline. Then one open question you personally find most threatening.

> [!example]- Model answer
> Motivation: 2025 interior enforcement is general, not focused; demand-side activity was unmeasured. Data: 5,388 news-geocoded raids; 5.4 million Advan POIs; ~1.1 million SafeGraph spend matches; absorbing metro onset after a 3-in-14 burst. Method: Shift-share IV (2010 foreign-born × national weekly raids) with POI and state×week FE; plus within-treated-metro distance bins with POI and CBSA×week FE. Headline: ≈ −2.7% visits, −6.2% spend per POI-week; broader than the targeted demographic; workplaces and independents hit harder; no clear online offset. Open (example): news coverage defining both treatment and qualitative “chilling,” plus the spend sample’s vendor break landing on independents.

## What you can now read

Re-read the abstract. Every clause should map to a lesson: regime shift (1), 5,388 raids and 5.4 million POIs (2), the implied contrast across metros (3), the percents and the generality claim (4), and the unsaid “draft, first year, visits and spend only” (5). The is the keep-file.

**This is a conversation.** If one threat above would make you refuse to cite the billions in a memo, that is the next session — not a reread of Figure 1.

Read one thing

1.  Borusyak, Hull and Jaravel, *REStud* 2022, through the section on when identification comes from shocks rather than shares — then decide which story you think Hernandez actually needs.

[[ICE 04 — What the estimates say|← Lesson 4]] ·
