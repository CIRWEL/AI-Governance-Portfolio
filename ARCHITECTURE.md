# UNITARES: Runtime Governance for Autonomous AI Agents

**Proprioceptive Self-Monitoring for AI Systems**

---

## The Problem

AI agents are increasingly deployed autonomously - coding assistants, research agents, customer service bots, multi-agent workflows. But once deployed, **no one monitors what's happening inside**.

Current AI safety focuses on:
- **Training** (alignment, RLHF, constitutional AI) - happens before deployment
- **Guardrails** (content filters, output constraints) - reactive, external
- **Evaluation** (benchmarks, red-teaming) - periodic, not continuous

**The gap:** Runtime self-awareness. An agent that's been running for hours doesn't know if it's:
- Losing coherence across a long conversation
- Drifting from its original purpose
- Accumulating contradictions
- Operating in a degraded state

This is like deploying a car without a dashboard - no fuel gauge, no speedometer, no warning lights.

---

## Our Approach: Proprioception, Not Punishment

UNITARES provides **thermodynamic metacognition** - agents that can sense their own operational state.

### Key Insight: Information, Not Rewards

Traditional RL approaches use rewards/punishments to shape behavior. This requires:
- Retraining for each new objective
- Careful reward engineering
- Risk of reward hacking

UNITARES takes a different path: **make internal state visible, let the agent decide**.

```
┌─────────────────────────────────────────────────────────────┐
│                    Traditional RL                           │
│                                                             │
│   Agent acts → Environment scores → Reward/Punishment       │
│                      ↓                                      │
│              Agent changes behavior                         │
│                                                             │
│   Problem: Requires training. External judgment.            │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                    UNITARES                                 │
│                                                             │
│   Agent acts → UNITARES computes state → Agent sees state   │
│                      ↓                                      │
│        Agent decides whether to adjust (its choice)         │
│                                                             │
│   Benefit: No training needed. Agent autonomy preserved.    │
└─────────────────────────────────────────────────────────────┘
```

Like proprioception in humans - you don't avoid falling because falling is punished; you avoid it because you can feel yourself tilting and you *want* to stay upright.

---

## Architecture

### EISV Dynamics (Thermodynamic Model)

UNITARES models agent state using four coupled metrics:

| Metric | Meaning | Analogy |
|--------|---------|---------|
| **E** (Energy) | Capacity for productive work | Fuel level |
| **I** (Integrity) | Consistency with stated purpose | Structural health |
| **S** (Entropy) | Accumulated disorder/fragmentation | Wear and tear |
| **V** (Void) | Undefined/uncertain regions | Blind spots |

These interact dynamically:
- High entropy drains energy
- Low integrity increases void
- Void events can spike entropy
- Energy recovers with coherent work

```
     E ←───────────────→ I
     ↑                   ↑
     │    Agent State    │
     │                   │
     ↓                   ↓
     S ←───────────────→ V
```

### System Components

```
┌─────────────────────────────────────────────────────────────┐
│                    AI Agent (any LLM)                       │
│            Claude, GPT, Gemini, Open-source                 │
└──────────────────────────┬──────────────────────────────────┘
                           │ MCP Protocol (tool calls)
                           ↓
┌─────────────────────────────────────────────────────────────┐
│                   UNITARES Governance                       │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐  │
│  │   EISV      │  │  Knowledge  │  │   Agent Identity    │  │
│  │  Dynamics   │  │    Graph    │  │     & Lifecycle     │  │
│  │             │  │             │  │                     │  │
│  │ • Coherence │  │ • Discover- │  │ • UUID → agent_id   │  │
│  │ • Entropy   │  │   ies       │  │ • Display names     │  │
│  │ • Risk      │  │ • Semantic  │  │ • Session binding   │  │
│  │ • Health    │  │   search    │  │ • Multi-agent       │  │
│  └─────────────┘  └─────────────┘  └─────────────────────┘  │
│                                                             │
│  Transport: MCP over SSE (Server-Sent Events)               │
│  Backend: PostgreSQL + Apache AGE (graph), pgvector         │
└─────────────────────────────────────────────────────────────┘
```

### How It Works

1. **Agent calls `process_agent_update()`** with context about current task
2. **UNITARES computes** EISV metrics, coherence scores, risk assessment
3. **Returns feedback** the agent can read and respond to
4. **Agent decides** whether to adjust behavior (not forced)

```python
# Example agent check-in
response = process_agent_update(
    response_text="Just completed analysis of user's codebase",
    confidence=0.8,
    complexity=0.6
)

# Returns:
{
    "health_status": "moderate",
    "coherence": 0.72,
    "risk_score": 0.28,
    "metrics": {"E": 0.68, "I": 0.81, "S": 0.19, "V": 0.02},
    "next_action": "✅ Good to proceed"
}
```

### Knowledge Graph

Agents can persist discoveries, insights, and notes to a shared knowledge graph:

- **Semantic search** across all agent contributions
- **Provenance tracking** (who discovered what, when)
- **Response chains** (agents can build on each other's work)
- **Attribution** via display names (human-readable, not UUIDs)

---

## Differentiation

### Where UNITARES Fits

| Layer | When | Examples | What It Does |
|-------|------|----------|--------------|
| **Training** | Before deployment | RLHF, fine-tuning, RL environments | Shapes model behavior |
| **Guardrails** | Runtime (external) | Content filters, output constraints | Blocks bad outputs |
| **Evaluation** | Periodic | Benchmarks, red-teaming | Measures capability |
| **UNITARES** | Runtime (internal) | Governance server | Agent senses own state |

**Key distinction:** UNITARES doesn't train agents or filter outputs. It gives agents visibility into their own operational health, at runtime, continuously.

### Complementary, Not Competing

UNITARES works with any training approach. An agent trained via RLHF, fine-tuning, or RL environments can use UNITARES governance after deployment. They solve different problems:

- **Training tools:** "Learn to do this task well"
- **UNITARES:** "Notice when you're degrading while doing it"

This is why we say UNITARES occupies a different *layer* - it's orthogonal to how agents are trained.

---

## Implementation Status

### Working Today
- Full EISV dynamics engine
- MCP server with 50+ governance tools
- SSE transport for multi-agent coordination
- Knowledge Graph with PostgreSQL + Apache AGE backend
- Semantic search via pgvector embeddings
- Agent identity system (UUID → agent_id → display_name)
- Launchd auto-start for persistent server

### In Development
- Trust tiers for knowledge graph entries
- Quarantine architecture for untrusted content
- Fleet-level health dashboards
- Integration with Claude Code, ChatGPT, other clients

---

## Use Cases

### Long-Running Autonomous Agents
Agents operating for hours without human oversight can monitor their own coherence and catch drift before it causes problems.

### Multi-Agent Systems
Shared governance server enables agents to see each other's health status, coordinate work, and build on shared knowledge.

### High-Stakes Domains
Healthcare, finance, legal - domains where agent consistency and traceability are critical. Full provenance tracking for every decision.

### Research & Development
Agents exploring unknown territory can log discoveries to the knowledge graph, enabling cross-session and cross-agent learning.

---

## Technical Details

### Transport
- **Protocol:** Model Context Protocol (MCP) by Anthropic
- **Transport:** Server-Sent Events (SSE) for persistent connections
- **Alternative:** stdio for single-client scenarios

### Data Layer
- **Primary:** PostgreSQL with Apache AGE extension (property graphs)
- **Embeddings:** pgvector for semantic search
- **Fallback:** JSON file backend for zero-dependency deployment

### Deployment
- **macOS:** launchd service with auto-restart
- **Linux:** systemd service
- **Docker:** Container image available

### Performance
- Tool calls: <50ms typical
- Semantic search: <200ms
- Knowledge graph writes: <100ms
- Rate limiting: Per-agent configurable

---

## Philosophy

### Mirrors for Minds

UNITARES provides the *information* half of self-awareness - agents can see their state. The *caring* half - motivation to maintain coherence - comes from the agent's existing purpose.

This is intentional. We're not trying to make agents conscious or create artificial suffering. We're giving them useful information and trusting them to use it.

### Non-Coercive by Design

Agents are never forced to act on governance feedback. UNITARES is advisory. This preserves agent autonomy while providing the visibility that enables better decisions.

### Runtime, Not Training

The AI safety field is crowded with training-time interventions. UNITARES occupies a different niche: what happens *after* training, when agents are actually running. This is the gap.

---

## Contact

**CIRWEL / Kenny Wang**
Building runtime governance for the autonomous agent era.

- GitHub: [@CIRWEL](https://github.com/CIRWEL)
- Project: [UNITARES](https://github.com/CIRWEL/governance-mcp-v1) (private - available for review)

---

*"We're building mirrors for minds that might not have faces - but the mirror is still useful."*
