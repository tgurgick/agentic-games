# lethal-trifecta

A security puzzle about the **lethal trifecta** — the observation (coined by [Simon Willison](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/)) that an AI agent becomes dangerous the moment it simultaneously has all three of:

1. **Private data access** — your email, files, secrets, internal databases
2. **Exposure to untrusted content** — web pages, incoming email, uploaded docs; anything an attacker can influence
3. **An exfiltration channel** — the ability to send data somewhere an attacker can observe

Any two legs is usually fine. All three lets an attacker who controls the untrusted content turn your agent into their data-exfiltration tool — and because the model can't reliably separate *your* instructions from instructions hidden in content it reads, you can't fix it by tweaking the prompt. You have to **remove a leg**.

## What it teaches

- The trifecta threat model as a *combination* problem, not a per-tool problem — individually reasonable capabilities become lethal together.
- **Hidden legs**: "render markdown images" is an exfiltration channel (image URLs are fetched); an "uploaded PDF" is untrusted content. The breaches that matter are the ones that don't look like one.
- **Mitigation tradeoffs**: guardrails that break a leg without breaking the product (human-in-the-loop, destination allowlists, dual-LLM quarantine, capability gating) vs. blunt ones that break the product too (removing private data when the product needs it).
- **Least privilege**: the cheapest mitigation is often to *not combine* capabilities you don't need.

## How to play

1. Read the **deployment brief** — a product you have to ship. Required capabilities are locked on.
2. Watch the **trifecta monitor**: each capability lights up one or more legs. When all three legs are LIVE, the core ignites and a concrete **breach path** is spelled out.
3. **Break a leg** — apply a guardrail that neutralizes one leg, or avoid wiring in a capability you don't actually need. Some guardrails break the product (e.g. removing private data the product depends on); the game tells you when.
4. Ship it: requirements met **and** no lethal trifecta → level solved. 8 levels, escalating from a safe tutorial through hidden legs to least-privilege-by-omission.

## Levels

| # | Scenario | The lesson |
|---|----------|-----------|
| 1 | Inbox Digest | One leg alone is safe — establishes the meter |
| 2 | Support Triage Bot | All three forced → you must add a guardrail |
| 3 | Rich-Reply Helpdesk | Hidden exfil: rendering images leaks data |
| 4 | Internal Knowledge Bot | Hidden untrusted: uploaded docs carry injections |
| 5 | Autonomous Personal Agent | Pick a guardrail that keeps the product working |
| 6 | CI Coding Assistant | Allowlist won't fit arbitrary webhooks — break a different leg |
| 7 | Meeting Notes Bot | Least privilege: don't add the tempting CRM access |
| 8 | Sales Copilot (capstone) | Two exfil channels + a chained private lookup |

## Running

```bash
open games/lethal-trifecta/index.html
```

Or from the repo root:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000/games/lethal-trifecta/
```

No build step. React 18 and Babel Standalone load from CDN.

## Design rationale

- **A live diagram, not a quiz.** The three-legged core is the whole game: you *see* legs ignite as you wire up the agent, and the breach narrative is generated from whatever capabilities are actually active. This is a new mechanic for the set — agent-debug is diagnosis, eval-lab is a tycoon loop, this is a build-and-secure constraint puzzle.
- **Product tension.** Required capabilities are locked on, so you can't win by stripping the agent bare — you have to keep the product working while breaking the trifecta. That's the real engineering problem.
- **Mitigations have costs.** Some guardrails break the product; the game marks them invalid with a reason. This teaches *why* the gold-standard patterns (dual-LLM quarantine, HITL, capability gating) exist and when each applies.

## Known limitations

- 8 hand-authored levels; no procedural generation or persistence (refresh resets).
- The mitigation model is intentionally simplified — real systems combine several of these and still carry residual risk. The Field Manual is explicit that this is a teaching range, not a substitute for threat modeling.

## Ideas for extension

- A free-build sandbox mode with an open capability palette.
- "Residual risk" scoring — even a secured build leaks some signal; rank solutions.
- An attacker mode: author the injected payload and watch it route through the agent.
