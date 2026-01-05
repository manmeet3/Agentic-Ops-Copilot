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

The system is designed as a modular, end-to-end pipeline that mirrors how agentic systems operate in real production environments. Each layer is independently testable, observable, and replaceable.

```text
┌────────────────────┐
│   Data Ingestion   │
│  (Batch / Events)  │
└─────────┬──────────┘
          ↓
┌────────────────────┐
│     Event Store    │
│ (Metrics / Logs /  │
│  Traces / Labels)  │
└─────────┬──────────┘
          ↓
┌────────────────────┐
│ Incident Detection │
│ & Grouping Engine  │
│ (Stats + Rules)    │
└─────────┬──────────┘
          ↓
┌────────────────────┐
│  Knowledge Base    │
│ (Runbooks, Past    │
│  Incidents, Docs)  │
└─────────┬──────────┘
          ↓
┌────────────────────┐
│ Retrieval Layer    │
│ (Lexical + Embed)  │
└─────────┬──────────┘
          ↓
┌────────────────────────────────────────────┐
│          Evidence Bundle Generator         │
│ (Facts, Snippets, Sources, Confidence)     │
└─────────┬──────────────────────────────────┘
          ↓
┌────────────────────────────────────────────┐
│            Multi-Agent System              │
│                                            │
│  ┌─────────┐   ┌─────────┐   ┌─────────┐   │
│  │ Analyst │ → │ Critic  │ → │ Planner │   │
│  └─────────┘   └─────────┘   └─────────┘   │
│          ↓                    ↑            │
│      ┌─────────┐      ┌─────────┐          │
│      │Executor │  ←   │ Auditor │          │
│      └─────────┘      └─────────┘          │
└─────────┬──────────────────────────────────┘
          ↓
┌────────────────────┐
│ Actions / Summaries│
│ Escalations        │
└─────────┬──────────┘
          ↓
┌────────────────────┐
│ Human Review &     │
│ Feedback Loop      │
└─────────┬──────────┘
          ↓
┌────────────────────┐
│ Evaluation &       │
│ Monitoring         │
└────────────────────┘
```
### Component Responsibilities

#### Data Ingestion
- Batch loaders and synthetic data generators  
- Schema validation and data quality checks  
- Handles missing, delayed, and noisy events  

#### Event Store
- Normalized storage for metrics, logs, and traces  
- Time-indexed for efficient querying  
- Serves as the source of truth for downstream systems  

#### Incident Detection & Grouping
- Statistical baselines and heuristic-based detection  
- Change-point and anomaly detection methods  
- Groups related events into candidate incidents  

#### Knowledge Base
- Runbooks and operational documentation  
- Historical incident summaries  
- Catalog of known failure modes and remediation patterns  

#### Retrieval Layer
- Hybrid retrieval (lexical + embedding-based)  
- Chunking and reranking strategies  
- Optimized for high recall with precision controls  

#### Evidence Bundle Generator
- Aggregates retrieved facts and contextual snippets  
- Assigns confidence scores and provenance metadata  
- Enforces strict evidence referencing for all claims  

#### Multi-Agent System
- **Analyst:** proposes diagnoses and hypotheses  
- **Critic:** challenges assumptions and identifies gaps  
- **Planner:** ranks actions by expected information gain  
- **Executor:** safely invokes tools (read-only by default)  
- **Auditor:** validates outputs, logs decisions, tracks confidence  

#### Actions & Escalation
- Produces structured summaries and action plans  
- Signals uncertainty explicitly  
- Escalates to human review when confidence is low  

#### Human Review & Feedback
- Approval, correction, and annotation workflows  
- Captures feedback signals for evaluation and learning  
- Enables continuous system improvement  

#### Evaluation & Monitoring
- Offline benchmarks and regression testing  
- Live quality, cost, and latency metrics  
- Disagreement tracking and drift-like indicators  


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

