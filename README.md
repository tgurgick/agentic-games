# agentic-games

A collection of interactive games and exercises for building intuition about agentic LLM systems — how they fail, how to diagnose them, and how to make them more reliable.

Each game lives in its own subfolder under `games/` and is a self-contained HTML file with no build step required.

## Games

| Game | Path | What it teaches |
| --- | --- | --- |
| **agent-debug** | [`games/agent-debug/`](games/agent-debug) | Diagnosing bad outputs in production agents. 38 scenarios across 5 difficulty tiers, including chained-fix incident response. |
| **eval-lab** | [`games/eval-lab/`](games/eval-lab) | Building evaluation infrastructure across an agent maturity ladder. Sim/tycoon loop covering 23 metrics from exact-match to multi-agent cascade analysis. |

## Running locally

Each game is a single `index.html` file. Open it directly in a browser:

```
open games/agent-debug/index.html
open games/eval-lab/index.html
```

Or serve the whole repo for clean URLs:

```
python3 -m http.server 8000
# then visit http://localhost:8000/games/agent-debug/
#       or  http://localhost:8000/games/eval-lab/
```

## Repo layout

```
agentic-games/
├── README.md
└── games/
    ├── agent-debug/
    │   ├── index.html      # the game
    │   └── README.md       # game-specific notes, design rationale
    └── eval-lab/
        ├── index.html
        └── README.md
```

## Roadmap

- [ ] Evals for `agent-debug` (scenario coverage, difficulty calibration, answer-key audits)
- [ ] Expand `eval-lab` scenario pool and add boss-fight incidents with conflicting metrics
- [ ] Additional games covering other agent failure modes

## License

TBD.
