# Mission: Self-improving AI agents, for a practitioner who runs them daily

## Why
Tejas is a data scientist who uses AI agents heavily as *tools* — not as research subjects. The field's centre of gravity has moved in the last two years from "make the base model bigger" to "spend compute at inference, verify the output, and let the system improve itself". Stanford's **CS329A — Self-Improving AI Agents** (Fall 2026, Azalia Mirhoseini and Ankur Tandon) is the most complete public map of that shift. This workspace turns the nine lectures into a practitioner's curriculum.

The goal is not to train models or publish. It is to:
1. **Stay current** — know what the frontier techniques actually are, in enough depth to recognise them when a vendor, a paper, or a colleague invokes them.
2. **Tune agents better** — turn the course's mechanisms (test-time compute, verification, tool feedback, planning, memory, context management) into concrete dials on the agents already in daily use.
3. **Evaluate effectiveness** — build the habit and the harness for measuring whether an agent is actually good, rather than trusting the demo.

## Success looks like
- Can explain, in one sentence each, the difference between **train-time scaling**, **test-time scaling**, and **self-improvement**, and say which of the three a given product claim is really about.
- Can name the **generation–verification gap** and explain why it, not model quality, is usually the binding constraint on an agentic workflow.
- Can look at a failing agent run and diagnose it against a short list: bad plan, missing tool feedback, weak verifier, context rot, or genuinely missing capability.
- Can design a **verifier** for a task at work — rubric, unit test, or judge — and state its known failure modes before trusting it.
- Can stand up a small **eval set** for one real workflow, with a metric that would actually change a decision, and read pass@k / pass^k correctly.
- Can decide, with reasons, when a problem needs **more inference compute**, **better context**, or **a fine-tune** — and why the third is almost never the first answer.
- Can read an agents paper from arXiv and place it on the course's map without needing the abstract to explain itself.

## Constraints
- **Strong ML/DS baseline.** Comfortable with transformers, training loops, sampling, evaluation metrics, statistics. Do not re-teach gradient descent or what a token is.
- **No research ambitions.** Skip proofs and ablations except where they change a practitioner's decision. When a lecture goes deep into RL algorithm internals, extract only the decision-relevant residue.
- **Lessons stay short** — 8–12 minutes each, one idea per lesson, completable in a sitting.
- Every lesson must end with something **applicable to an agent Tejas already runs**.

## Out of scope
- Doing original research with agents; writing papers; the course's project and homework track.
- Training or post-training a model from scratch; GPU-level infrastructure.
- Prompt-engineering folklore unsupported by the course or a primary source.
- Vendor-specific tutorials, except as concrete illustrations of a general mechanism.

## Primary source
Stanford CS329A "Self-Improving AI Agents" (Fall 2026) — [course site](https://cs329a.stanford.edu/), [lecture playlist](https://www.youtube.com/playlist?list=PLangBM27OtEA). Transcripts of all nine lectures are clipped locally; see `RESOURCES.md`.
