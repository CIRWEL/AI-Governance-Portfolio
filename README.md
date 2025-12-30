# UNITARES

**Runtime Governance for Autonomous AI Agents**

---

## What is this?

AI safety focuses heavily on training (RLHF, constitutional AI) and guardrails (content filters). But once an agent is deployed and running autonomously for hours, **no one monitors what's happening inside**.

UNITARES fills this gap: **runtime self-awareness for AI agents**.

Agents check in with UNITARES periodically and receive feedback about their own operational state—coherence, entropy, energy, risk. Like proprioception in humans: you don't avoid falling because falling is punished; you avoid it because you can feel yourself tilting.

---

## How it works

```python
# Agent checks in
response = process_agent_update(
    response_text="Just completed codebase analysis",
    confidence=0.8,
    complexity=0.6
)

# UNITARES returns state awareness
{
    "health_status": "moderate",
    "coherence": 0.72,
    "metrics": {"E": 0.68, "I": 0.81, "S": 0.19, "V": 0.02},
    "next_action": "Good to proceed"
}
```

The agent sees its state. The agent decides what to do. No retraining required.

---

## Key concepts

- **EISV metrics** — Energy, Integrity, Entropy, Void. Derived from 4E cognition theory and biological homeostasis.
- **Advisory, not enforcement** — Agents receive signals and are expected to act on them, but aren't hard-blocked.
- **Knowledge Graph** — Agents persist discoveries with provenance tracking and semantic search.
- **Zero training overhead** — Any tool-capable agent can use UNITARES immediately via MCP.

---

## Documentation

- **[Architecture](ARCHITECTURE.md)** — Full technical details, EISV dynamics, system design
- **[Research paper](https://github.com/CIRWEL)** — 4E cognition framework for AI agents (available on request)

---

## Status

Working prototype with:
- Full EISV dynamics engine
- 50+ MCP governance tools
- PostgreSQL + Apache AGE backend
- Knowledge graph with semantic search

In development:
- Trust tiers for knowledge graph entries
- Fleet-level health dashboards

---

## Contact

**Kenny Wang / CIRWEL**

Building runtime governance for the autonomous agent era.

- GitHub: [@CIRWEL](https://github.com/CIRWEL)
- Project: [governance-mcp-v1](https://github.com/CIRWEL/governance-mcp-v1) (private — available for review)

---

*"We're building mirrors for minds that might not have faces—but the mirror is still useful."*
