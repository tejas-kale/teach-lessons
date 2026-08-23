# Working notes

## Preferences and context (2026-08-21)
- **Practitioner, not researcher.** Tejas said explicitly: "I don't want to necessarily do research with AI agents." Every lecture idea must be translated into a dial he can turn on an agent he already runs, or dropped.
- **Strong technical baseline** (data scientist, heavy agent user). Assume transformers, sampling, metrics, statistics. Don't explain what a token is. Do explain anything that only exists in the last ~18 months of literature.
- **Three standing asks**, in his words: stay updated with practitioner-useful knowledge; get ideas for tuning agents better; learn to evaluate their effectiveness. Lessons 09–11 carry most of the evaluation weight, but every lesson should carry a measurement angle.
- Asked for **all lessons up front**, in a new directory — not drip-fed. So the arc must be readable end to end without a session in between.

## Teaching notes
- The course's own spine is a single argument: pre-training scaling flattened, so the returns moved to **inference-time compute + verification + learning from experience**. If a lesson can't be hung off that spine, it probably belongs in reference instead.
- The highest-leverage idea for this user is the **generation–verification gap**. It reframes "the model isn't good enough" as "my verifier isn't good enough", which is the version he can act on. Lessons 03–05 all orbit it.
- Second-highest: **pass@k vs pass^k**. Sampling metrics flatter agents; reliability metrics don't. This is the sharpest evaluation idea in the course for someone shipping workflows.
- Resist teaching RL internals (GRPO/PPO detail). Lesson 08 extracts only the buy-vs-tune decision residue.
- Quiz options must stay **length-matched** — see `assets/quiz.js` contract and the SKILL rule.

## Open threads / candidate next lessons
- A hands-on session: build the eval harness from Lesson 09 against one of Tejas's real workflows, with his data.
- Cost modelling: dollars per solved task as the primary metric, sweeping the test-time-compute dial.
- Multi-agent orchestration — the course is sceptical; worth a lesson if his workflows are heading that way.
- Reading group: pick three papers from `RESOURCES.md` and decode them together.

## Transcript provenance (2026-08-21)
- All nine transcripts now live in `transcripts/` inside this workspace, not in the Clippings library.
- **Part 04 was re-clipped.** The first pass returned a condensed, LLM-summarised version (1,242 words vs ~10k for the others); the retry produced a full transcript. Lesson 7 was written from the summary and then corrected against the full text — see [[0004-part-04-correction]]. If any other lesson ever feels thinly sourced, check the transcript's word count first.
