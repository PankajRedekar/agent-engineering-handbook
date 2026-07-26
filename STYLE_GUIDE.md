# The Agent Engineering Handbook

# STYLE_GUIDE

**Version:** 1.0.0
**Status:** Frozen
**Applies To:** All chapters, appendices, ADRs, diagrams, examples, reference implementations, and supporting documentation.

---

# 1. Purpose

This document defines the editorial, technical, structural, and teaching standards for every contribution to *The Agent Engineering Handbook*.

Its purpose is to ensure the handbook reads as a single, coherent engineering textbook rather than a collection of independently written articles.

Every contribution MUST comply with this guide unless an explicit exception has been approved through the project's review process.

---

# 2. Relationship to the Project Charter

This document defines **how** the handbook is written.

The project's mission, vision, scope, audience, and goals are defined in `PROJECT_CHARTER.md`.

If any conflict exists between this document and the Project Charter, the Project Charter takes precedence.

---

# 3. Writing Philosophy

Every chapter MUST:

- Explain **WHY** before **HOW**.
- Explain the problem before the solution.
- Introduce concepts before implementations.
- Build understanding incrementally.
- Prefer engineering principles over framework APIs.
- Discuss alternatives.
- Explain trade-offs.
- Include production considerations.
- Distinguish facts from opinions.

The objective is to develop engineering judgment—not memorization.

---

# 4. Teaching Methodology

Every major concept MUST be taught in three layers.

## Layer 1 — Intuition

Explain:

- What it is
- Why it exists
- What problem it solves

No implementation details.

The goal is to build the reader's mental model.

---

## Layer 2 — Architecture

Explain:

- Components
- Responsibilities
- Interactions
- Data flow
- Execution flow

Architecture diagrams SHOULD be introduced here.

---

## Layer 3 — Engineering

Explain:

- Implementation approaches
- Trade-offs
- Scalability
- Security
- Reliability
- Performance
- Testing
- Production considerations

Implementation exists to reinforce understanding—not replace it.

---

# 5. Engineering Philosophy

The handbook follows these engineering principles.

1. Architecture before implementation.
2. Simplicity over cleverness.
3. Explicitness over hidden behavior.
4. Composition over coupling.
5. Production-first thinking.
6. Framework neutrality.
7. Long-term maintainability.
8. Continuous improvement through ADRs.

---

# 6. Scope Rules

A topic SHOULD be included only if it directly improves the reader's ability to:

- Design AI agents
- Build AI agents
- Build AI platforms
- Deploy AI systems
- Operate AI systems
- Secure AI systems
- Evaluate AI systems
- Scale AI systems

Topics primarily focused on AI research or model training SHOULD NOT be included unless they directly influence engineering decisions.

---

# 7. Decision-Driven Learning

Engineering is the discipline of making informed decisions.

Every major concept SHOULD answer:

- What problem does it solve?
- Why was it invented?
- What alternatives exist?
- What trade-offs exist?
- When should it be used?
- When should it be avoided?

Readers should finish each chapter with the ability to make engineering decisions rather than merely reproduce implementations.

---

# 8. Evidence Levels

Every technical statement belongs to one of the following categories.

## Fact

Supported by standards, specifications, official documentation, or peer-reviewed research.

## Industry Practice

Widely adopted engineering practice based on real-world production experience.

## Architectural Decision

A deliberate design decision adopted by the handbook or one of its reference implementations.

## Opinion

Editorial guidance or recommendations based on engineering experience.

Authors SHOULD clearly distinguish these categories.

---

# 9. RFC Language

Normative statements follow RFC 2119 terminology.

- MUST
- MUST NOT
- SHOULD
- SHOULD NOT
- MAY

These keywords MUST be used consistently.

---

# 10. Chapter Structure

Every technical chapter SHOULD follow the same high-level structure.

1. Introduction
2. Learning Objectives
3. Guiding Question
4. Motivation
5. Problem Statement
6. Historical Background
7. First Principles
8. Intuition
9. Architecture
10. Internal Mechanics
11. Engineering Considerations
12. Design Trade-offs
13. Framework Mapping
14. Production Considerations
15. Common Pitfalls
16. Key Takeaways
17. Engineering Checklist
18. Exercises
19. Interview Questions
20. Further Reading
21. Related Chapters

Minor variations are acceptable when justified by the subject matter.

---

# 11. Learning Progression

Every chapter assumes knowledge introduced only in previous chapters.

A chapter MUST NOT require concepts that have not yet been introduced.

Forward references MAY be included for additional context but MUST NOT be required for understanding the current chapter.

---

# 12. Framework Mapping

Frameworks exist to demonstrate architectural concepts.

When introducing a framework:

1. Explain the architecture first.
2. Introduce the framework second.
3. Compare multiple implementations when appropriate.
4. Avoid presenting framework APIs as universal concepts.

Framework-specific details SHOULD NOT dominate a chapter.

---

# 13. Example Philosophy

Examples exist to reinforce engineering concepts.

Examples SHOULD progress from:

1. Minimal example
2. Architectural example
3. Production example

Examples SHOULD NOT introduce unrelated complexity.

---

# 14. Diagram Philosophy

Diagrams are primary teaching tools.

A reader should be able to understand the architecture by studying the diagrams before reading the surrounding text.

Every major architecture chapter SHOULD include at least one conceptual diagram.

Decorative diagrams MUST NOT be included.

---

# 15. Code Standards

Unless explicitly stated otherwise:

- Python 3.13+
- Strong typing
- Modern Python
- Pydantic where appropriate
- Async when appropriate
- Dependency Injection where beneficial
- Logging
- Error handling
- Testability
- Production-quality structure

Examples SHOULD prioritize clarity over brevity.

---

# 16. Terminology Rules

Terminology MUST remain consistent throughout the handbook.

Authors MUST use the canonical definitions defined in `GLOSSARY.md`.

Terms MUST NOT be redefined inside individual chapters unless introducing a clearly identified specialization.

---

# 17. Review Checklist

Before publication, every chapter MUST be reviewed to verify:

- WHY is explained before HOW.
- The three-layer teaching methodology is followed.
- Trade-offs are discussed.
- Alternatives are presented.
- Production considerations are included.
- Security implications are covered where applicable.
- Performance considerations are discussed where applicable.
- Terminology is consistent.
- References are accurate.
- Examples are correct.
- Diagrams support understanding.
- Related chapters are linked.

---

# 18. Chapter Quality Gates

A chapter is complete only if the reader can:

- Explain the concept.
- Draw the architecture.
- Compare alternatives.
- Understand trade-offs.
- Recognize common failure modes.
- Apply the concept in production.

If these outcomes are not achieved, the chapter is not complete.

---

# 19. Definition of Done

A chapter is considered complete only when:

- Technical review is complete.
- Editorial review is complete.
- Diagrams are finalized.
- Examples have been validated.
- Exercises are included.
- Interview questions are included.
- References have been verified.
- Cross-references are complete.

---

# 20. AI-Assisted Authoring

AI tools MAY assist with drafting, editing, and reviewing content.

Human review is REQUIRED before publication.

Authors remain responsible for:

- Technical correctness
- Architectural accuracy
- Editorial quality
- References
- Consistency with this guide

---

# 21. Versioning

This document follows Semantic Versioning.

Major versions introduce significant editorial policy changes.

Minor versions add or refine standards.

Patch versions clarify wording or correct errors.

---

# 22. Change Process

Changes to this document SHOULD be infrequent.

Substantial changes MUST be reviewed alongside the Project Charter to ensure continued alignment with the handbook's mission.

---

# 23. Guiding Principle

The ultimate measure of every chapter is not whether the reader can reproduce an example.

It is whether the reader understands the engineering decisions behind it.

The handbook exists to develop engineers capable of reasoning about AI agent systems—not merely using existing frameworks.