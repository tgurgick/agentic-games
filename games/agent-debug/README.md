# agent-debug

An interactive game for building diagnostic intuition about agentic LLM systems. You're shown a failing agent — incident text, console trace, and a set of corrective "dials" — and you pick the right fix.

**38 scenarios** across 5 difficulty tiers.

## Play

Open `index.html` in a browser. Progress is saved in browser storage (`agent_debug_progress_v3`).

Keyboard: `1`–`9` select a dial · `Enter` advance · `D` open the deep-dive · `Esc` to menu.

## Levels

| # | Name | Scenarios | What it tests |
|---|------|-----------|---------------|
| 1 | Apprentice | 6 | Clean signals, single dial. Learn the symptom→fix mapping. |
| 2 | Engineer | 8 | Intermittent failures, noisier traces, plausible decoys. |
| 3 | Senior | 8 | Multi-cause failures. The obvious dial is often wrong. |
| 4 | Staff | 8 | Subtle regressions. Trust the trace, not your priors. |
| 5 | Distinguished | 6 | Pick 2–3 dials in the right order. Sequence matters. |

Each level unlocks at grade **B+** or better on the previous one. **S-rank** requires ≥90% correct *and* zero wrong answers.

## Framework

Every scenario maps to one of seven symptom categories:

- **A** — Factually wrong / hallucinated
- **B** — Incomplete / truncated
- **C** — Wrong tool / wrong action
- **D** — Right tool, bad arguments
- **E** — Loops / never terminates
- **F** — Tone / format / refusal
- **G** — Latency / cost regression

For each, you branch on reproducibility (deterministic vs. intermittent) and the question *"what changed in the last 7 days?"* — the two heuristics that do most of the work in real triage.

## Dials

There are ~36 corrective levers grouped by area:

- **Retrieval**: `retrieval_k`, `decrease_k`, `chunking`, `reranker`, `reindex`, `hybrid_search`, `metadata_filter`, `embedding_model`, `context_order`
- **Prompt / context**: `prompt_guard`, `system_prompt`, `few_shots`, `context_compress`
- **Sampling / model**: `lower_temp`, `pin_version`, `swap_model`, `seed_lock`
- **Tools**: `tool_schema`, `tool_descriptions`, `structured_output`, `input_normalize`, `tool_consolidate`, `tool_split`
- **Agent loop**: `max_steps`, `stop_signal`, `retry_policy`, `max_tokens`, `planner_split`
- **Triage / infra**: `bisect_release`, `capacity_check`, `trace_diff`, `eval_set`, `rate_limit_check`, `cache_check`, `rollback`, `feature_flag`

## Scoring

| Outcome | Points |
|---------|--------|
| Correct (single-dial) | +10, plus streak bonus |
| Partial (defensible alternative) | +4 |
| Wrong | 0 |
| Chain step correct | +6 |
| Chain step out-of-order | +1 |
| Chain perfect (Level 5) | +12 plus streak bonus |
| Chain right-dials-wrong-order | +5 |

## Design notes

- **Deep-dive panel** on every scenario: Root Cause → Why This Works → Why Not The Others → Next Steps → Principle. Hidden by default, expanded on click or `D`.
- **Level 5 chains** require ordered fixes. Defensible alternate orderings still complete the chain but score lower than the canonical sequence. The canonical patterns being trained: *stop-the-bleed → fix-cause → prevent-recurrence* (incident response), *filter → retrieve → rerank* (retrieval pipelines), *diagnose → restructure → protect* (multi-symptom regressions).
- All scenarios are technically substantive — drawn from real failure modes (lost-in-the-middle, silent provider updates, tool-selection ceiling at ~20 tools, gateway timeouts vs. model truncation, tokenization-driven numeric drift, hybrid search at scale, etc.).

## Roadmap

- [ ] **Evals** — scenario coverage audit, answer-key review, difficulty calibration. Coming in a follow-up.
- [ ] Possible additions: a "blind mode" where the symptom label is hidden and the player identifies A–G first; harder decoys; a Level 6 with longer chains.

## File

This game is a single self-contained HTML file with no build step and no external dependencies beyond Google Fonts. View source to see all scenario content, scoring logic, and game state inline.
