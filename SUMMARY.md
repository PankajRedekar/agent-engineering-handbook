# The Agent Engineering Handbook

# SUMMARY

**Version:** 1.0.0
**Status:** Frozen
**Date:** 2026-07-27

---

# 1. Purpose

This document defines the learning architecture of *The Agent Engineering Handbook*.

Rather than serving as a simple table of contents, it explains how knowledge is organized, why topics appear in a particular sequence, and how each domain builds upon previous concepts.

The goal is to provide a structured learning journey from software engineering fundamentals to production-grade AI agent systems.

---

# 2. Learning Philosophy

The handbook follows a first-principles approach.

Every new concept builds upon previously established knowledge.

Readers should understand not only *how* a system works, but also *why* it exists, what alternatives exist, and the trade-offs involved in using it.

The curriculum is designed to minimize forward references and maximize conceptual continuity.

---

# 3. Curriculum Overview

The handbook is organized into seven learning domains.

Each domain has a single educational objective.

| Domain | Goal |
|---------|------|
| Domain 0 | Foundations |
| Domain 1 | LLM Engineering |
| Domain 2 | Agent Engineering |
| Domain 3 | Platform Engineering |
| Domain 4 | Production Engineering |
| Domain 5 | Reference Implementation |
| Domain 6 | Advanced Topics |

Each domain builds upon the previous domains.

---

# 4. Knowledge Progression

The learning progression follows the natural evolution of modern AI systems.

```text
Software Engineering
        │
        ▼
LLM Fundamentals
        │
        ▼
Prompt Engineering
        │
        ▼
Structured Outputs
        │
        ▼
Tool Calling
        │
        ▼
Agent Runtime
        │
        ▼
Planning
        │
        ▼
Memory
        │
        ▼
Context Engineering
        │
        ▼
Single-Agent Systems
        │
        ▼
Multi-Agent Systems
        │
        ▼
Platform Engineering
        │
        ▼
Production Engineering
        │
        ▼
Reference Implementation
```

Every chapter is positioned within this dependency graph.

---

# 5. Domain Structure

## Domain 0 — Foundations

Goal:

Build the mental models required for Agent Engineering.

Topics include:

- What is Agent Engineering?
- History of AI Systems
- Mental Models
- System Thinking
- AI Vocabulary
- Handbook Conventions

---

## Domain 1 — LLM Engineering

Goal:

Understand the capabilities and limitations of modern language models.

Representative topics:

- Tokens
- Tokenization
- Context Windows
- Embeddings
- Inference
- Prompt Engineering
- Structured Outputs
- Tool Calling
- Model Selection
- Cost & Latency

---

## Domain 2 — Agent Engineering

Goal:

Learn how intelligent agent systems are designed and implemented.

Representative topics:

- Agent Architecture
- Agent Runtime
- Planning
- Execution
- Memory Systems
- Context Engineering
- Knowledge Systems
- Workflows
- Reflection
- Multi-Step Reasoning
- Human-in-the-Loop

---

## Domain 3 — Platform Engineering

Goal:

Build reusable platforms capable of hosting many AI agents.

Representative topics:

- Agent Platforms
- Multi-Tenancy
- Tool Registries
- Authentication
- Authorization
- MCP
- A2A
- Scheduling
- Event-Driven Systems
- Queues
- Storage
- Configuration

---

## Domain 4 — Production Engineering

Goal:

Deploy and operate AI systems reliably.

Representative topics:

- Observability
- Evaluation
- Security
- Guardrails
- Monitoring
- Cost Optimization
- Scaling
- Reliability
- Incident Response
- Deployment
- CI/CD
- Versioning

---

## Domain 5 — Reference Implementation

Goal:

Apply every concept by building a complete production-grade agent platform.

Representative topics:

- Architecture
- Runtime
- Tool System
- Memory
- Execution Engine
- Plugins
- Testing
- Deployment

---

## Domain 6 — Advanced Topics

Goal:

Explore emerging techniques and future directions.

Representative topics:

- Long-Term Memory
- Self-Improving Agents
- Agent Collaboration Patterns
- Research Trends
- Emerging Protocols

---

# 6. Dependency Rules

The curriculum follows these rules:

- Every chapter depends only on previous chapters.
- Concepts are introduced once and reused consistently.
- Terminology remains stable throughout the handbook.
- Implementation follows conceptual understanding.

---

# 7. Learning Outcomes

A reader who completes this handbook should be able to:

- Design AI agent architectures.
- Build production-grade AI agents.
- Build reusable AI platforms.
- Evaluate engineering trade-offs.
- Operate AI systems in production.
- Understand modern AI frameworks through first principles.

---

# 8. Evolution

The handbook is expected to evolve as the AI ecosystem evolves.

New technologies may introduce additional chapters or domains, but the overall curriculum will continue to prioritize timeless engineering principles over framework-specific knowledge.