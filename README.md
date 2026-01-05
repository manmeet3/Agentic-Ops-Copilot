# Agentic Ops Copilot

**Agentic Ops Copilot** is an end-to-end, evidence-grounded multi-agent system for incident triage, action planning, and human-in-the-loop remediation—built with evaluation, monitoring, and safety guardrails from day one.

This project is designed to demonstrate production-grade ML and agentic system design for Series B/C AI startups.

---

## Demo

**What the demo shows**
- Incident detection from raw telemetry
- Evidence retrieval from runbooks and past incidents
- Multi-agent reasoning (Analyst → Critic → Planner)
- Action recommendations with confidence and citations
- Escalation when uncertainty is high
- Human review and feedback loop

*(Add GIF or video link here)*

---

## Why This Exists (Problem Statement)

Modern operations teams face alert fatigue and fragmented evidence across metrics, logs, runbooks, and historical incidents.  
This system reduces time-to-triage while enforcing:

- Strict evidence grounding
- Explicit uncertainty handling
- Measurable quality and regressions
- Safe, auditable automation

---

## Core Capabilities

- **Incident detection & grouping**
  - Statistical baselines and heuristics
- **Evidence retrieval**
  - Runbooks, historical incidents, logs
  - Lexical + embedding-based retrieval
- **Multi-agent reasoning**
  - Analyst, Critic, Planner, Executor, Auditor
- **Strict grounding**
  - All claims must cite evidence IDs
- **Human-in-the-loop workflow**
  - Review, correction, and feedback capture
- **Evaluation harness**
  - Offline benchmarks + regression tests
- **Monitoring & observability**
  - Quality, cost, latency, disagreement metrics

---

## Architecture
Ingestion
↓
Event Store ──→ Detection & Grouping
↓ ↓
Knowledge Base Incident Objects
↓ ↓
Retrieval Layer ──→ Evidence Bundles
↓
Multi-Agent System
(Analyst → Critic → Planner → Executor → Auditor)
↓
Actions / Summaries / Escalations
↓
Human Review + Feedback
↓
Evaluation & Monitoring

---

## Quickstart

```bash
# Start all services
docker compose up --build

# Load sample data
make load-sample

# Run demo scenario
make demo
```

## Evaluation

### Offline Benchmarks
- Dataset: synthetic + curated incidents
- Metrics:
  - Incident triage accuracy
  - Evidence precision@k
  - Hallucination rate (unsupported claims)
  - Escalation precision
  - Agent disagreement rate

### Example Results

| System Variant | Accuracy | Hallucination Rate | Escalation Precision |
|---------------|----------|--------------------|----------------------|
| Baseline (rules only) | X | N/A | N/A |
| Single-agent LLM | Y | High | Low |
| Multi-agent + hybrid retrieval | Z | Low | High |

---

## Guardrails & Safety

- Read-only tools by default
- Explicit tool-call audit logs
- JSON schema validation for agent outputs
- Confidence thresholds and uncertainty escalation
- Rate limits and cost tracking

---

## Failure Modes (What Breaks and Why)

- Sparse or missing evidence → higher “unknown” responses
- Conflicting telemetry → agent disagreement spikes
- Ambiguous runbooks → retrieval confusion

**Mitigations**
- Hybrid retrieval (lexical + embedding)
- Critic agent challenges assumptions
- Escalation to human review when confidence is low

---

## Roadmap

- Streaming ingestion (near-real-time signals)
- Stronger reranking and learned retrievers
- Online evaluation via shadow deployments
- Automated policy checks for tool usage
- Cost-aware planning and action selection

---

## Repository Layout
backend/ # FastAPI services and agents
ui/ # Frontend UI
data/ # Sample datasets and generators
eval/ # Evaluation harness and reports
docs/ # Architecture diagrams and design notes

