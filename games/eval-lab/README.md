# eval-lab

A sim/tycoon game about building evaluation infrastructure for progressively complex agent systems.

## What it teaches

How evaluation strategy changes as agent complexity grows. The same metric that works for a simple chatbot is useless for a multi-agent swarm — and vice versa. The game makes that explicit by:

- Walking you up a **5-stage maturity ladder**: chatbot → RAG → tool-use → multi-step agent → multi-agent swarm.
- Introducing new **failure modes** at each stage (hallucination, retrieval miss, wrong tool, plan derailment, coordination collapse, etc.).
- Letting you **acquire 23 evaluation metrics** organized by tier, each with a clear "when to use" and a "pitfall" so you learn the tradeoffs.
- Generating **field incidents** that you diagnose by picking the right deployed metric.

## Concepts covered

| Tier | Metrics |
| --- | --- |
| T1 — Basic | Exact match, regex/heuristic, BLEU/ROUGE, human review |
| T2 — RAG | Retrieval precision@k, retrieval recall, faithfulness, answer relevance |
| T3 — Tool use | Tool selection accuracy, argument accuracy, task success rate, step efficiency |
| T4 — Multi-step | Trajectory match, subgoal completion, LLM-as-judge, reward hack detector |
| T5 — Swarm | Coordination score, role adherence, output diversity, cascade analysis |
| T0 — Cross-cutting | Safety probes, cost & latency, regression suite |

Plus key principles surfaced in the in-game Field Manual: Goodhart's Law, the eval pyramid, trajectory ≠ outcome, validating your judge, and why production is the real eval.

## How to play

1. Read the incident report (e.g. "User asks for the weather. Agent calls the calculator. Returns 47.").
2. Acquire a metric from the registry — each costs Insight Points (IP).
3. Pick which of your deployed metrics catches that failure mode.
4. Earn IP for correct diagnoses. Wrong? Try again — no penalty.
5. Spend IP to unlock the next stage of agent complexity.

Owning a metric just adds it to your toolkit — you choose which one fits each incident. The whole point is learning which metric catches which kind of bug.

## Running

```bash
open games/eval-lab/index.html
```

Or from the repo root:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000/games/eval-lab/
```

No build step. React 18 and Babel Standalone are loaded from CDN.

## Design rationale

- **Sim/tycoon over quiz** — the resource loop (earn IP, spend on metrics or stages) creates real opportunity cost. You can't acquire everything, so you have to think about which metrics matter for your current system.
- **Adaptive difficulty** — the same incident pool gets harder as you advance because the relevant metrics get more sophisticated. A T1 player picks "human review"; a T4 player picks "subgoal completion."
- **Pitfalls front and center** — every metric's info card includes its weakness. Real eval work is mostly about knowing what your metrics *don't* catch.
- **Retro terminal aesthetic** — green CRT phosphor, scanlines, grid. Signals "this is engineering, not a quiz app."

## Known limitations

- 10 scenarios total (2 per stage). Solvable in one sitting.
- No persistence — refresh and you start over. Intentional for the demo; could add localStorage.
- LLM-as-judge is described but not actually invoked. A future version could call an API to grade open-ended outputs against a rubric.

## Ideas for extension

- More scenarios per stage, with adversarial variants
- Boss fights where multiple metrics conflict (false positives vs. false negatives)
- Production mode after stage 5 — endless drift/regression management
- Live judge mode using the Anthropic API for real rubric-grading
