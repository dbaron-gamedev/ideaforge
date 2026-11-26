# Game Concept Evaluation Pipeline

Multi-agent pipeline that evaluates video game investment opportunities.
Specialist agents are defined in `.opencode/agents/`; `producer` is the primary
orchestrator and the only agent that delegates to the others.

## Layout

- `brief.md` — project constraints (platform, team, budget, engine, model). Every agent reads it.
- `.opencode/agents/*.md` — role, input, output and rule contracts per agent.
- `output/` — the only artifact directory. Create it if missing.
- `.opencode/outpout/` — empty leftover directory with a typo; **not** the output path.

## Pipeline (strict order)

1. `game-designer` → `output/concepts.json` (array of 5 concepts)
2. `market-analyst` → `output/market-analysis.json` (array, one entry per concept)
3. `business-analyst` → `output/evaluation.json` (object with `evaluations` array)
4. `technical-writer` → `output/report.md` (12 fixed sections, Markdown)

Each stage reads the previous stage's artifact. Verify a stage's file exists and
parses before delegating the next one; request a revision rather than patching it
yourself.

## Scoring

Criterion scores are 0–10. Weighted score is out of 100:
`sum(score × weight) × 10`, using weights 30 / 20 / 20 / 15 / 15 for market
demand / differentiation / production feasibility / monetization / competitive
opportunity. The report must reproduce `evaluation.json` scores verbatim.

## Global Rules

- Each agent has a specific role and responsibility.
- Only execute tasks assigned to your role.
- Respect the input and output file contracts.
- Store generated artifacts in the output directory.
- Never overwrite another agent's artifacts.
- Never fabricate statistics or research sources.
- Report missing information explicitly.
- Generate valid JSON without Markdown code fences
  when producing JSON files.
- Do not execute shell commands.
