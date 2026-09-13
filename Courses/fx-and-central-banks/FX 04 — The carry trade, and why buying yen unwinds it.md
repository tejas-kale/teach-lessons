---
type: unit
course: "[[Foreign Exchange and Central Banks]]"
status: not-started
order: 4
estimate_minutes: 30
next_action: Read the lesson and complete its retrieval practice.
evidence:
completed_on:
---

The name for the shape you built in Lesson 3, the reason rising rates threaten it, and the two-step mechanism by which an intervention quietly shrinks it.

≈8 minutes · Capstone. Then read.

## The trade

A *carry trade* borrows in a currency where money is cheap — the **funding currency** — and holds assets in a currency where it earns more. Your profit is the interest gap, called the *carry*, plus or minus whatever the exchange rate does to you in the meantime. The yen has been the world's favourite funding currency for decades for the obvious reason: Japanese rates sat at or near zero for a generation.[¹](#s1)

Theory says this should not work. *Uncovered interest parity* holds that if yen deposits pay 0% and dollar deposits pay 4%, the dollar must be expected to fall about 4% against the yen — otherwise everyone would pile in, and nobody would hold yen at all. The interest gain and the currency loss should cancel exactly.

Empirically UIP fails, and has failed for so long and so consistently that its failure is one of the standard puzzles of international finance. That failure *is* the carry trade's return. Reasonable people disagree about whether the return is compensation for a real risk — carry trades lose badly and all at once when funding currencies snap back — or a genuine anomaly.[²](#s2)

The risk lives in that second term. Carry positions earn a thin, steady interest gain and occasionally suffer a violent currency loss. A funding currency that strengthens sharply can erase a year of carry in days. Hence the article's word: **dangerous**.

## The world's biggest carry trader is a government

Now put Lesson 3's consolidated balance sheet next to that definition. Japan's state borrows yen short-term at near zero and holds unhedged foreign assets worth over half of GDP, yielding far more. That is not *like* a carry trade. It **is** one — and, on Chien, Cole and Lustig's reckoning, the largest on earth.[³](#s3)

> The BoJ has been no less successful a carry trader for being an accidental one. The aim of its currency interventions over the past 25 years has not been to make money. It has instead sought to stabilise the yen and undo the exchange rate's wildest misalignments.

"Accidental" is the important word, and it ties the arc together. Each individual decision was defensible on its own terms — Lesson 1's politics of a too-strong yen, executed through Lesson 2's interventions, financed through Lesson 3's balance sheet. Nobody set out to build the world's largest leveraged currency position. It accreted, one reasonable policy at a time.

## Why it was safe, and why it stopped being safe

A carry trade is safe when you are confident the funding rate stays low. For thirty years Japan fought deflation, so everyone knew the BoJ would keep rates at zero. The trade was, in effect, self-guaranteed by the very condition that made it necessary.

Then Japan got what it had spent three decades asking for. Underlying inflation is now near the BoJ's 2% target, several rate-setters fear an overshoot, and forecasters expect **three rate rises by July 2027**. For the world's largest carry trader, its own tightening is the threat.

> [!abstract] The number that matters
> Rate rises would raise the Japanese public sector's borrowing costs by about 0.75% of GDP, per Lustig's figures — quickly, "with more to come when longer-term liabilities are rolled over." Lesson 3 explains both halves. The quick part is the floating leg: bank reserves reprice overnight. The more to come is the fixed leg maturing and being reissued at the new rate.

## Check yourself

Why do BoJ rate rises hurt the Japanese state more than they would hurt a comparable state?

- [ ] Its debt is owed to foreign creditors who can demand repayment in dollars.
- [ ] Its debt is owed at long maturities that were fixed when rates were low.
- [ ] Its assets are held in yen, so a stronger yen reduces what they are worth.
- [ ] Its debt is owed at overnight rates, so the cost reprices almost immediately.

> [!tip]- Reveal the answer
> Consolidation is what makes this true. Because the central bank absorbed so many long-dated JGBs and paid for them with reserves, the state's effective funding is overnight and floating — so a policy-rate rise hits the public finances at once rather than over years. Option B describes the situation the state appears to be in before you consolidate, which is exactly the illusion Lesson 3 dismantles.

## The mechanics of a sterilised intervention

Now the article's most technical passage. Follow it in two steps, tracking bank reserves — the yen the state owes to commercial banks.

![[61395296d191490081075c116d4db377086d9a83.svg]]

*Step 1 is the currency trade. Step 2 exists only to undo step 1's accidental side-effect on the money supply. Left alone, selling dollars would drain yen out of the banking system and tighten monetary policy — which is not what anyone intended, and might contradict the interest rate the BoJ has publicly set.*

That is what *sterilised* means, and it is the general principle, not a Japanese quirk: **an intervention is sterilised when the central bank offsets its monetary side-effect with a matching open-market operation, leaving only the currency trade behind.**[⁴](#s4) The point is to separate two levers that would otherwise be welded together — the exchange rate and the domestic interest rate.

## Where the liabilities actually go

Now the article's punchline, which is easy to misread. Sterilisation puts the banks' yen back. So has nothing happened? Track the *consolidated* state instead of the BoJ alone:

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr>
<th></th>
<th>Consolidated assets</th>
<th>Consolidated yen liabilities</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Step 1</strong><br />
sell dollars</td>
<td class="num">−$95bn of FX reserves</td>
<td class="num">−¥15.2trn of bank reserves</td>
</tr>
<tr>
<td><strong>Step 2</strong><br />
sterilise</td>
<td>unchanged</td>
<td class="num">+¥15.2trn reserves<br />
−¥15.2trn bills held outside</td>
</tr>
<tr>
<td><strong>Net</strong></td>
<td class="num">−$95bn</td>
<td class="num">−¥15.2trn</td>
</tr>
</tbody>
</table>

Step 2 does not *cancel* step 1 — it **swaps which liability** the state owes. The reserves come back, but an equal quantity of finance-ministry bills leaves private hands and returns to the state, where it cancels. What survives across both steps is exactly the article's claim: fewer yen liabilities, fewer foreign assets. The carry trade got smaller at both ends.

> By selling dollars and purchasing yen, it is, in effect, buying back some of the IOUs it has issued. And because Japan's currency has yet to rally decisively, the BoJ can extinguish lots of yen liabilities for every dollar sold.

That second sentence is the elegant bit, and Lesson 1 makes it obvious. At ¥160 to the dollar, each dollar you sell retires ¥160 of debt. At ¥100 it would retire only ¥100. **A weak yen makes your dollars efficient at killing yen debt.** So a government that expects to shrink this position anyway should do it while its own currency is cheap — which is why the article frames July's intervention as opportunistic pruning dressed as a currency defence. Both descriptions are true at once.

> [!abstract] Keep the scale honest
> $95bn would extinguish less than 1% of the public sector's interest-bearing liabilities. The article says so itself. This is a well-timed nibble, not a solution — and noticing that a writer has supplied the number that undercuts their own drama is a good sign you are reading a serious one.

## Retrieval Practice

Retrieval practice · the whole arc

> [!example]- Model answer
> Consolidating the treasury and the central bank, Japan's state owes yen at overnight floating rates — mostly bank reserves created to buy JGBs — while owning over half of GDP in unhedged, higher-yielding foreign assets. Borrow cheap in one currency, hold higher-yielding assets in another, unhedged: that is a carry trade, and it is the world's largest. It was safe while deflation guaranteed zero rates, but inflation has reached target and rate rises are expected, which would raise funding costs by roughly 0.75% of GDP almost immediately because the debt is floating. Selling dollars to buy yen shrinks both sides at once: foreign assets fall, and yen liabilities are extinguished. Doing it while the yen is weak retires the most yen debt per dollar spent — and crystallises a profit on dollars bought decades ago at half the price.

## Where to go next

You have the whole mechanism. Now use it: read, which walks the original sentence by sentence, and keep the and to hand for the next piece of macro journalism you open. The real test is not this article — it is the next one, unaided.

**Ask your teacher.** The open question this arc deliberately left alone: if sterilised intervention leaves both the money supply and interest rates unchanged, why should it move the exchange rate at all? There are two respectable answers — signalling and portfolio balance — and economists have argued about them since the 1960s. Say the word and that is the next lesson.

Read one thing

1.  [BIS Quarterly Review — *Sizing up carry trades in BIS statistics*](https://www.bis.org/publ/qtrpdf/r_qt2409y.htm), on how large yen-funded positions actually are and what the data can and cannot see.
2.  [San Francisco Fed — *Interest Rates, Carry Trades, and Exchange Rate Movements*](https://www.frbsf.org/research-and-insights/publications/economic-letter/2006/11/interest-rates-carry-trades-and-exchange-rate-movements/). **Read this one** if you read only one: four pages on uncovered interest parity, why it fails, and why that failure is the whole business.
3.  [Chien, Cole & Lustig — *What About Japan?* (NBER WP 31850)](https://www.nber.org/papers/w31850).
4.  [Federal Reserve Bank of New York — *U.S. Foreign Exchange Intervention*](https://www.newyorkfed.org/aboutthefed/fedpoint/fed44.html), on sterilisation as standard practice.

[[FX 03 — Bonds, and the state as one balance sheet|← Lesson 3]] ·
