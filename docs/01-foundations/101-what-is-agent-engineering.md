# 001 — What Is Agent Engineering?

> **Domain:** Foundations
>
> **Estimated Reading Time:** 25 minutes
>
> **Prerequisites:** None
>
> **Difficulty:** Beginner
>
> **Last Updated:** 2026-07-27

---

## Why This Chapter Exists

Artificial Intelligence has fundamentally expanded what software can do. Modern language models can understand natural language, generate content, reason through complex tasks, write code, and interact with external systems in ways that traditional software could not. These capabilities have enabled a new generation of applications that are no longer limited to executing predefined instructions—they can also interpret goals, make decisions, and adapt their behavior to changing situations.

However, intelligence alone does not produce reliable software.

A language model cannot, by itself, authenticate with external services, enforce business rules, recover from failures, maintain long-term knowledge, protect sensitive data, or operate safely in production. Building dependable AI-powered systems requires much more than prompting a model. It requires engineering.

This chapter introduces **Agent Engineering** as the discipline responsible for designing, building, deploying, operating, and evolving software systems that combine language models with deterministic software, tools, memory, workflows, and operational controls to accomplish user-defined goals.

More importantly, this chapter establishes the mental models, terminology, and engineering perspective that the remainder of this handbook builds upon.

---

## Guiding Question

> **How do we engineer software systems that can reason, make decisions, interact with external systems, and reliably accomplish goals in production?**

Every chapter in this handbook contributes to answering this question.

---

# Introduction

Software engineering has continually evolved alongside advances in computing. Every major technological shift has expanded the kinds of systems engineers could build while introducing new challenges that demanded new architectural approaches.

The transition from standalone applications to client-server systems introduced distributed communication. The rise of the internet led to web engineering. Cloud computing changed how applications were deployed and operated. Microservices transformed how large systems were designed and maintained.

Today, language models represent another significant shift.

Unlike traditional software components, language models can interpret ambiguous requests, reason over large amounts of context, generate new information, and select actions that are not explicitly programmed. Rather than following a single deterministic execution path, they evaluate probabilities and produce responses based on learned patterns.

This capability fundamentally changes how software systems are built.

Instead of implementing every decision directly in code, engineers can delegate certain decisions to a language model while retaining deterministic control over the overall system. The result is a hybrid architecture in which conventional software provides structure, constraints, and reliability, while language models contribute reasoning and adaptability.

This shift enables software systems that can perform tasks that were previously impractical or prohibitively expensive to automate. Modern applications can research information, summarize documents, analyze contracts, coordinate workflows, generate software, assist human decision-making, and interact with external services using natural language.

Increasingly, users expect these systems not merely to answer questions, but to accomplish objectives.

Building such systems introduces a new set of engineering challenges.

How should a language model access external tools?

How should context be managed across long-running tasks?

How can memory be stored and retrieved safely?

How do we prevent unsafe actions?

How can we observe and debug probabilistic behavior?

How do we evaluate reliability when responses are not perfectly deterministic?

These questions extend beyond prompt design or model selection. They concern the architecture, operation, and lifecycle of an entire software system.

This need has given rise to **Agent Engineering**.

Agent Engineering extends the principles of software engineering into a new class of systems that combine language models, deterministic software, memory, tools, workflows, and operational controls to accomplish goals reliably.

Like Software Engineering, Agent Engineering is not defined by a programming language, framework, or vendor.

Frameworks evolve.

Models improve.

APIs change.

Engineering principles endure.

This handbook focuses on those enduring principles.

---

## Key Ideas

Before continuing, keep the following ideas in mind:

- Agent Engineering is an engineering discipline, not a framework.
- Language models are components within larger software systems, not complete applications.
- Reliable agent systems require architecture, not just prompts.
- Deterministic software and probabilistic reasoning complement each other.
- Production-ready AI systems require reliability, observability, security, and operational excellence.
- The principles presented in this handbook are intended to outlast today's models and frameworks.

---

## Looking Ahead

This chapter begins by explaining why Agent Engineering has emerged as a distinct engineering discipline. In the next section, we will examine the evolution of software systems and explore how each major shift in computing introduced new engineering practices. Understanding this historical progression provides the foundation for understanding why Agent Engineering has become necessary today.


---

# The Evolution of Software

Agent Engineering did not appear in isolation. Like every engineering discipline before it, it emerged in response to a new class of problems that existing approaches could no longer solve effectively.

Understanding why Agent Engineering exists requires understanding how software engineering has evolved over the past several decades.

Throughout the history of computing, every major technological advancement has expanded the capabilities of software while simultaneously increasing its complexity. Each transition introduced challenges that required new architectural patterns, new engineering practices, and often entirely new specializations.

The evolution of software can be viewed as a sequence of expanding capabilities.

```text
Standalone Programs
        ↓
Networked Applications
        ↓
Web Applications
        ↓
Distributed Systems
        ↓
Cloud-Native Systems
        ↓
AI-Powered Applications
        ↓
Agent Systems
```

Each stage represents more than a technological improvement. It represents a fundamental shift in how software is designed, built, deployed, and operated.

## Standalone Programs

Early software systems executed on a single machine and followed explicitly defined instructions. Inputs were processed, computations were performed, and outputs were produced in a predictable manner.

These systems were largely deterministic. Given the same inputs, they always produced the same outputs.

Engineering concerns focused on correctness, performance, maintainability, and efficient use of limited hardware resources.

Although these systems could become complex, the boundaries of execution were well understood. The application controlled its own state, interacted with relatively few external systems, and operated within a single execution environment.

## Networked and Web Applications

As computers became interconnected, software evolved from isolated programs into distributed applications.

Instead of serving a single user on a single machine, applications now communicated across networks, served thousands or millions of users, and depended on remote databases and services.

This transition introduced entirely new engineering challenges:

- Network latency
- Partial failures
- Concurrent users
- Authentication and authorization
- Distributed state
- Scalability
- Fault tolerance

Traditional programming techniques alone were no longer sufficient. Engineers developed new architectural styles, including client-server architectures, REST APIs, message queues, and eventually microservices.

Software engineering expanded beyond writing code. It now included designing reliable distributed systems.

## Cloud-Native Systems

Cloud computing transformed software once again.

Infrastructure became programmable.

Applications could scale dynamically, recover automatically from failures, and be deployed globally within minutes.

Engineering priorities shifted toward:

- Elastic scalability
- Infrastructure as code
- Observability
- Continuous delivery
- Distributed tracing
- Service reliability
- Operational automation

Building software now required understanding both application architecture and operational excellence.

The discipline of software engineering expanded once again.

## AI-Powered Applications

The introduction of machine learning enabled software to perform tasks that could not be implemented using explicit rules alone.

Applications could classify images, detect fraud, recommend products, translate languages, and recognize speech.

Despite these new capabilities, most AI systems remained relatively specialized.

A machine learning model typically solved one well-defined prediction problem within a larger deterministic application.

The software still controlled the workflow.

The model simply provided predictions.

Engineering practices evolved to include data pipelines, feature engineering, model training, deployment, monitoring, and evaluation.

This led to the emergence of AI Engineering and MLOps.

## Agent Systems

Language models introduced a fundamentally different capability.

Instead of producing a single prediction, they can interpret instructions, reason over context, generate plans, use external tools, write software, and adapt their behavior to changing situations.

More importantly, they can participate in decision-making.

This represents a significant architectural shift.

Rather than embedding intelligence within isolated machine learning models, software systems can now incorporate reasoning directly into their execution.

A language model can decide:

- which tool to use,
- what information to retrieve,
- how to decompose a task,
- when additional information is required,
- and how to synthesize results into a final response.

The application is no longer executing only predefined logic.

Instead, deterministic software and probabilistic reasoning work together to accomplish a goal.

This collaboration creates an entirely new category of software systems.

## Why This Evolution Matters

Every stage in the evolution of software expanded the capabilities of applications while introducing new engineering responsibilities.

Standalone applications required software engineering.

Distributed applications required distributed systems engineering.

Cloud-native applications required platform engineering and Site Reliability Engineering.

AI-powered applications required AI Engineering and MLOps.

Agent systems introduce another expansion.

Engineers must now design systems that combine deterministic software with probabilistic reasoning while remaining reliable, secure, observable, and maintainable.

This is the problem that Agent Engineering addresses.

Agent Engineering is not a replacement for Software Engineering.

It is its next evolution.

---

# Defining Agent Engineering

After examining the evolution of software systems, we can now define Agent Engineering.

Rather than viewing it as a collection of tools, frameworks, or programming techniques, we define it as an engineering discipline.

> **Agent Engineering is the discipline of designing, building, deploying, operating, and evolving software systems that use language models, tools, memory, workflows, and deterministic software to autonomously or semi-autonomously accomplish goals within defined constraints.**

Every phrase in this definition has been chosen deliberately.

Like Software Engineering, Cloud Engineering, or Site Reliability Engineering, Agent Engineering is defined by the problems it solves rather than the technologies it uses.

Frameworks, programming languages, orchestration libraries, and language models will continue to evolve. The engineering principles behind reliable agent systems are significantly more enduring.

The remainder of this section examines each part of the definition.

## Designing

Designing an agent system involves making architectural decisions before writing code.

Examples include:

- Defining system boundaries.
- Identifying which decisions should remain deterministic.
- Determining where language models should be used.
- Designing tool interfaces.
- Choosing memory strategies.
- Establishing security boundaries.
- Planning failure recovery.

Good architecture determines whether an agent system can remain maintainable as it grows.

## Building

Building is the implementation of the architecture.

This includes developing:

- Agent runtimes
- Tool integrations
- Context management
- Memory systems
- Evaluation pipelines
- Observability
- APIs
- User interfaces

Building an agent system requires considerably more than integrating an LLM API.

The language model is only one component within the larger software architecture.

## Deploying

An agent that works on a developer's laptop is not necessarily ready for production.

Deployment introduces additional concerns:

- Configuration management
- Secrets management
- Infrastructure
- Scalability
- High availability
- Model selection
- Cost optimization

Deployment transforms an implementation into an operational system.

## Operating

Unlike traditional software, agent systems exhibit probabilistic behavior.

Their outputs may vary even when given similar inputs.

As a result, operating an agent system requires continuous observation and improvement.

Operational responsibilities include:

- Monitoring latency
- Tracking token usage
- Measuring costs
- Detecting failures
- Evaluating response quality
- Monitoring tool execution
- Identifying hallucinations
- Observing user satisfaction

Operating an agent system is an ongoing engineering activity rather than a one-time deployment.

## Evolving

Agent systems continuously evolve.

Language models improve.

New tools become available.

Business requirements change.

Prompt strategies mature.

Memory architectures evolve.

Evaluation techniques improve.

An effective agent platform must accommodate change without requiring complete redesign.

Agent Engineering therefore emphasizes modular architectures, clear interfaces, and continuous evaluation.

## Software Systems

One of the most important ideas in this handbook is that agents are **software systems**, not prompts.

A production agent is composed of many cooperating components.

These commonly include:

- Application logic
- Agent runtime
- Language model
- Tool registry
- Memory
- Context management
- Workflow orchestration
- Databases
- APIs
- Monitoring
- Security controls

The language model is an essential component, but it is not the entire system.

Thinking of an agent as "just an LLM" leads to architectures that are difficult to maintain, secure, and operate.

## Language Models

Language models provide reasoning capabilities.

They interpret goals, generate plans, synthesize information, and decide how to use available capabilities.

However, they should not be treated as the source of truth.

Business rules, security policies, financial calculations, authorization decisions, and regulatory requirements should remain deterministic whenever possible.

One of the defining responsibilities of an Agent Engineer is deciding which decisions belong to deterministic software and which can safely be delegated to a language model.

## Tools

Tools allow an agent to interact with the external world.

Without tools, a language model is limited to generating text based on the information already available in its context.

Tools extend an agent's capabilities by enabling it to:

- Search for information
- Query databases
- Execute code
- Call APIs
- Send emails
- Read documents
- Modify files
- Trigger workflows

Tool integration transforms a conversational model into an interactive software system.

## Memory

Many tasks extend beyond a single interaction.

Memory enables an agent to retain relevant information across conversations, workflows, or long-running processes.

Different forms of memory serve different purposes.

For example:

- Conversation history
- User preferences
- Organizational knowledge
- Retrieved documents
- Task state

Designing effective memory systems is a core responsibility of Agent Engineering and will be explored in later chapters.

## Workflows

Many real-world objectives require multiple coordinated steps.

Rather than solving an entire problem with a single model invocation, agent systems often execute workflows that combine reasoning, deterministic software, external tools, and human interaction.

Examples include:

- Multi-step approval processes
- Research pipelines
- Document processing
- Customer support automation
- Software delivery workflows

Engineering reliable workflows is a defining characteristic of production agent systems.

## Goals

Traditional software executes predefined instructions.

Agent systems pursue objectives.

Rather than specifying every intermediate step, engineers define a goal together with constraints that determine acceptable behavior.

Examples include:

- Resolve a customer issue.
- Generate a compliance report.
- Review a pull request.
- Analyze a legal contract.
- Plan a travel itinerary.

The agent determines how to accomplish the objective while operating within the boundaries established by the surrounding software system.

## Constraints

Autonomy without constraints is unacceptable in production software.

Every agent system operates within explicit boundaries.

These constraints may include:

- Authorization rules
- Business policies
- Budget limits
- Time limits
- Tool permissions
- Regulatory requirements
- Human approval
- Organizational governance

Agent Engineering is therefore not about maximizing autonomy.

It is about engineering systems that use autonomy responsibly.

## Definition in Practice

The formal definition introduced at the beginning of this section can now be understood more completely.

Agent Engineering is not the practice of writing prompts.

It is not the practice of calling an LLM API.

It is not defined by a particular framework.

Instead, it is the engineering discipline responsible for creating reliable software systems that combine deterministic software with probabilistic reasoning to accomplish goals safely, efficiently, and at production scale.

---

# Anatomy of an Agent System

Having defined Agent Engineering as an engineering discipline, the next step is understanding what an agent system actually looks like.

Many newcomers imagine an AI agent as a language model responding to prompts. While this mental model is sufficient for simple demonstrations, it breaks down when building production systems.

A production agent is not a single model.

It is a software system composed of multiple cooperating components, each with a clearly defined responsibility.

The language model provides reasoning capabilities, but the surrounding software is responsible for providing context, enforcing constraints, executing actions, managing memory, and ensuring the system operates reliably.

Understanding these components is essential because every subsequent chapter in this handbook explores one of them in greater depth.

## Core Architecture

At a high level, an agent system can be represented as follows.

```text
                          User / System
                                │
                                ▼
                             Goal
                                │
                                ▼
                    ┌──────────────────────┐
                    │    Agent Runtime     │
                    │                      │
                    │  ┌────────────────┐  │
                    │  │ Language Model │  │
                    │  ├────────────────┤  │
                    │  │ Context Engine │  │
                    │  ├────────────────┤  │
                    │  │ Planner        │  │
                    │  ├────────────────┤  │
                    │  │ Memory         │  │
                    │  ├────────────────┤  │
                    │  │ Tool Manager   │  │
                    │  └────────────────┘  │
                    └──────────────────────┘
                                │
                                ▼
                   External Systems & Services
```

This diagram intentionally omits implementation details.

Its purpose is to establish the mental model that will guide the rest of this handbook.

Every component exists because the language model alone is insufficient for building reliable software.

## Goal

Every agent begins with a goal.

Unlike traditional software, where developers explicitly encode every step of execution, an agent receives an objective and determines how to achieve it within predefined constraints.

Examples include:

- Review a pull request.
- Analyze a legal contract.
- Resolve a customer support issue.
- Generate a financial report.
- Investigate a production incident.

The goal defines **what** should be accomplished, not necessarily **how** it should be accomplished.

One of the defining characteristics of agent systems is the separation between objectives and execution.

> **Engineering Note**
>
> Goals should always be explicit, measurable, and bounded by clear constraints. Ambiguous goals often produce unpredictable behavior.

## Agent Runtime

The Agent Runtime is the orchestrator of the entire system.

It coordinates every interaction between the language model, tools, memory, workflows, and external services.

Typical responsibilities include:

- Managing execution flow
- Maintaining execution state
- Selecting available tools
- Building model context
- Handling retries
- Managing failures
- Enforcing policies
- Recording execution traces

Without an Agent Runtime, a language model becomes little more than an advanced text generator.

The runtime transforms reasoning into an executable software system.

## Language Model

The language model is responsible for reasoning.

Depending on the task, it may:

- Interpret user intent
- Generate plans
- Analyze information
- Decide which tools to use
- Produce summaries
- Generate structured outputs
- Synthesize results

The language model should be viewed as a probabilistic decision-making component rather than the central application itself.

It contributes intelligence.

It does not replace software engineering.

> **Design Principle**
>
> Delegate reasoning to the language model. Retain control in deterministic software.

## Context Engine

Language models only know what exists inside their context window.

The Context Engine determines what information should be provided for each model invocation.

Typical responsibilities include:

- Building prompts
- Retrieving relevant documents
- Injecting conversation history
- Including system instructions
- Filtering unnecessary information
- Managing token budgets

Poor context engineering often produces worse results than poor model selection.

Providing the right information is usually more important than choosing the largest model.

## Planner

Many objectives cannot be completed in a single step.

The Planner decomposes complex goals into smaller executable tasks.

Depending on the system, planning may involve:

- Breaking objectives into subtasks
- Choosing execution order
- Determining dependencies
- Scheduling tool usage
- Coordinating multiple agents

Some systems rely entirely on the language model for planning.

Others combine deterministic planning algorithms with language-model reasoning.

The appropriate approach depends on the reliability requirements of the application.

## Memory

Many tasks extend beyond a single interaction.

Memory enables the system to retain information across requests, conversations, or long-running workflows.

Examples include:

- Conversation history
- User preferences
- Organizational knowledge
- Task progress
- Retrieved documents
- Intermediate results

Memory allows an agent to behave consistently over time rather than treating every request as an isolated event.

## Tool Manager

Language models cannot directly interact with the external world.

They require tools.

The Tool Manager exposes approved capabilities while ensuring they are executed safely and consistently.

Examples include:

- Database queries
- REST APIs
- Search engines
- File systems
- Email services
- Source control platforms
- Business applications

The Tool Manager also enforces permissions, validates inputs, records execution history, and handles failures.

> **Production Note**
>
> Every tool invocation should be observable, auditable, and attributable to both the originating request and the model decision that triggered it.

## External Systems

Ultimately, an agent exists to interact with the world outside the language model.

These external systems may include:

- Enterprise applications
- Internal APIs
- Cloud services
- Databases
- Knowledge bases
- Communication platforms
- Human operators

The surrounding software determines what the agent is allowed to access and under what conditions.

The language model never interacts with external systems directly.

All interactions should pass through deterministic software.

## Bringing the Components Together

Individually, each component solves a specific engineering problem.

Collectively, they enable a software system to interpret goals, reason about possible solutions, interact with external systems, and accomplish meaningful work.

This architecture also illustrates an important principle that appears throughout this handbook:

> **An agent is not a language model. An agent is a software system that uses a language model as one of its components.**

Understanding this distinction is one of the most important mental models in Agent Engineering.

The remaining chapters of this handbook progressively explore each component in detail, explaining why it exists, how it works, and how it should be engineered for production environments.

---

# Responsibilities of an Agent Engineer

Having explored the architecture of an agent system, we can now define the responsibilities of the engineer who builds and operates it.

Software engineers have always been responsible for designing reliable systems. Agent Engineering extends that responsibility into systems that combine deterministic software with probabilistic reasoning.

An Agent Engineer is not simply an application developer who integrates a language model.

Nor is an Agent Engineer a machine learning researcher who trains foundation models.

Instead, an Agent Engineer is responsible for engineering the entire lifecycle of an agent system—from initial architecture to long-term production operations.

While the specific responsibilities vary between organizations, they generally fall into several major areas.

## Designing System Architecture

Every successful agent system begins with a well-designed architecture.

Before selecting models or writing prompts, an Agent Engineer must answer questions such as:

- What problem is the system solving?
- Which decisions should remain deterministic?
- Which decisions can safely be delegated to a language model?
- What capabilities should the agent have?
- What are the system boundaries?
- What security constraints must be enforced?
- How will the system scale?

Poor architectural decisions are difficult to correct later.

As with traditional software engineering, architecture determines the long-term maintainability of the system.

> **Design Principle**
>
> Architecture should define where intelligence belongs, not simply where code executes.

---

## Selecting Models

Different language models have different strengths.

Some optimize for reasoning.

Some optimize for speed.

Some specialize in code generation.

Others excel at multilingual understanding or long-context processing.

Selecting a model requires balancing multiple factors, including:

- Accuracy
- Latency
- Cost
- Context window
- Reliability
- Availability
- Privacy requirements
- Licensing constraints

The largest model is rarely the best choice.

Production systems often use multiple models, selecting the most appropriate one for each task.

---

## Engineering Context

A language model can only reason about information that exists within its context window.

Providing the correct context is therefore one of the most important responsibilities of an Agent Engineer.

Context engineering includes:

- Designing system prompts
- Managing conversation history
- Retrieving relevant knowledge
- Filtering unnecessary information
- Injecting organizational policies
- Optimizing token usage

Poor context design frequently causes failures that are incorrectly attributed to the language model itself.

> **Engineering Note**
>
> Better context often produces larger improvements than switching to a more capable model.

---

## Designing Tool Integrations

Most production agents interact with external systems.

These interactions should never occur directly through the language model.

Instead, Agent Engineers design deterministic tool interfaces that expose well-defined capabilities.

Examples include:

- Database access
- Internal APIs
- Search services
- Email systems
- Source control platforms
- ERP systems
- CRM platforms
- Document repositories

Each tool should have:

- A clearly defined purpose
- Strong input validation
- Explicit authorization rules
- Error handling
- Logging
- Auditability

Tools are software interfaces—not prompts.

---

## Designing Memory Systems

Many tasks require information that extends beyond a single request.

Agent Engineers must determine:

- What information should be remembered?
- How long should it be retained?
- Where should it be stored?
- Who is allowed to access it?
- When should it be forgotten?

Memory design directly impacts:

- User experience
- Cost
- Performance
- Privacy
- Regulatory compliance

Choosing an appropriate memory strategy is an architectural decision rather than an implementation detail.

---

## Defining Workflows

Real-world business processes rarely consist of a single model invocation.

Instead, they involve multiple coordinated steps.

Examples include:

- Document review
- Invoice processing
- Software delivery
- Customer onboarding
- Incident response

Agent Engineers design workflows that combine:

- Reasoning
- Deterministic business logic
- Tool execution
- Human approvals
- Validation
- Error recovery

Reliable workflows are one of the defining characteristics of production agent systems.

---

## Ensuring Reliability

Unlike deterministic software, language models may produce different outputs for similar inputs.

Agent Engineers must therefore design systems that remain dependable despite probabilistic behavior.

Common strategies include:

- Structured outputs
- Validation layers
- Retry mechanisms
- Fallback models
- Human review
- Confidence thresholds
- Guardrails

Reliability is achieved through system design rather than expecting perfect model behavior.

---

## Operating Production Systems

Deployment is only the beginning.

Production agent systems require continuous operational oversight.

Typical operational responsibilities include:

- Monitoring latency
- Tracking token consumption
- Measuring operational cost
- Detecting failures
- Monitoring tool execution
- Evaluating response quality
- Observing user satisfaction
- Managing model upgrades

Operating an agent platform is an ongoing engineering discipline.

---

## Securing Agent Systems

Agent systems introduce security challenges beyond those found in traditional software.

Examples include:

- Prompt injection
- Data leakage
- Unauthorized tool execution
- Excessive permissions
- Sensitive information exposure
- Supply chain attacks through external tools

Agent Engineers design systems that minimize these risks through:

- Principle of least privilege
- Tool isolation
- Access control
- Output validation
- Audit logging
- Human approval for high-risk actions

Security must be considered part of the architecture, not an afterthought.

> **Production Note**
>
> Every capability granted to an agent should be explicitly justified. An agent should never receive permissions simply because they might be useful.

---

## Measuring Quality

Unlike conventional software, correctness is not always binary.

Agent Engineers define evaluation strategies that measure system performance over time.

Examples include:

- Task completion rate
- Tool success rate
- Hallucination rate
- User satisfaction
- Cost per task
- Average latency
- Failure recovery rate
- Human intervention rate

Without measurable quality, continuous improvement becomes impossible.

---

## Continuously Improving the System

Agent systems evolve continuously.

New models become available.

Business requirements change.

New tools are introduced.

Evaluation techniques improve.

An Agent Engineer is responsible for ensuring the system can evolve without sacrificing reliability or maintainability.

Continuous improvement requires:

- Modular architectures
- Automated testing
- Versioned prompts
- Reproducible evaluations
- Observability
- Incremental deployment strategies

Production agent systems are never truly finished.

They are continuously engineered.

---

## The Agent Engineer's Mindset

Although the technologies used in agent systems will continue to evolve, the mindset of an Agent Engineer remains consistent.

An Agent Engineer asks questions such as:

- What problem are we solving?
- What decisions should remain deterministic?
- What information does the model actually need?
- What tools should the system expose?
- How do we recover from failure?
- How do we evaluate success?
- How do we make the system safer tomorrow than it is today?

Ultimately, Agent Engineering is not about maximizing intelligence.

It is about engineering software systems that use intelligence responsibly, predictably, and reliably to accomplish meaningful goals.

---

# Relationship to Other Disciplines

Agent Engineering did not emerge to replace existing engineering disciplines.

Instead, it builds upon them.

A production agent system combines concepts from Software Engineering, Distributed Systems, Artificial Intelligence, Machine Learning, Security Engineering, Platform Engineering, and Site Reliability Engineering.

Understanding where Agent Engineering fits—and where it does not—is essential for building the correct mental model.

## Software Engineering

Software Engineering is the foundation upon which Agent Engineering is built.

It provides the principles required to design maintainable, testable, scalable, and reliable software systems.

These principles remain equally important when building agent systems.

Examples include:

- System architecture
- Modular design
- APIs
- Testing
- Version control
- Design patterns
- Error handling
- Performance optimization

Agent Engineering does **not** replace Software Engineering.

Instead, it extends Software Engineering to systems that incorporate probabilistic reasoning.

Without strong software engineering practices, even the most capable language model will produce an unreliable application.

> **Engineering Note**
>
> Agent Engineering assumes competence in Software Engineering. It is an extension of the discipline, not an alternative to it.

---

## Artificial Intelligence Engineering

AI Engineering focuses on integrating artificial intelligence capabilities into software products.

These capabilities may include:

- Computer Vision
- Speech Recognition
- Recommendation Systems
- Natural Language Processing
- Predictive Analytics
- Language Models

An AI-powered application may use machine learning to solve one or more specific problems within a larger deterministic application.

Agent Engineering represents a specialized branch of AI Engineering that focuses on systems capable of reasoning, planning, using tools, and pursuing goals.

Every agent system is an AI-powered system.

Not every AI-powered system is an agent.

---

## Machine Learning Engineering

Machine Learning Engineering focuses on developing, deploying, and maintaining predictive models.

Typical responsibilities include:

- Data collection
- Feature engineering
- Model training
- Hyperparameter tuning
- Model deployment
- Model monitoring
- Data pipelines

Machine Learning Engineers create the models.

Agent Engineers build systems that use those models.

In many organizations, these responsibilities belong to different teams.

An Agent Engineer should understand machine learning concepts but is not necessarily responsible for training foundation models.

---

## Prompt Engineering

Prompt Engineering is the practice of designing prompts that guide language models toward producing useful outputs.

Prompt design is important.

However, prompts alone do not create production systems.

A production agent also requires:

- Architecture
- Tool integration
- Memory
- Context management
- Security
- Evaluation
- Monitoring
- Failure recovery

Prompt Engineering is therefore one capability within Agent Engineering rather than the discipline itself.

> **Design Principle**
>
> Prompts influence model behavior. Architecture determines system behavior.

---

## Platform Engineering

As organizations adopt multiple agent systems, shared infrastructure becomes increasingly important.

Platform Engineering focuses on building reusable capabilities that enable development teams to build, deploy, and operate software efficiently.

Examples include:

- Shared runtimes
- Authentication
- Deployment pipelines
- Logging
- Observability
- Configuration management
- Secret management
- Developer tooling

Large organizations often develop Agent Platforms that provide these capabilities as reusable services.

Agent Engineers frequently collaborate with Platform Engineers to ensure that agent systems can be deployed consistently and operated at scale.

---

## Site Reliability Engineering

Agent systems are production systems.

Like any production system, they must be reliable.

Site Reliability Engineering (SRE) focuses on ensuring that software systems remain available, observable, performant, and recoverable.

Agent systems introduce additional operational concerns, including:

- Model latency
- Token consumption
- Tool failures
- Hallucinations
- Context construction
- Evaluation quality

Agent Engineers apply many SRE principles while also accounting for probabilistic behavior.

Reliability remains a system property rather than a model property.

---

## Security Engineering

Traditional application security remains essential.

However, agent systems introduce entirely new attack surfaces.

Examples include:

- Prompt injection
- Indirect prompt injection
- Tool misuse
- Sensitive data leakage
- Excessive permissions
- Unsafe autonomous actions

Security Engineers establish organizational security standards.

Agent Engineers implement those standards within intelligent systems.

The two disciplines work together to ensure that intelligent capabilities do not compromise system security.

---

## Product Management

Every successful agent system begins with a business problem.

Product Managers define:

- Business objectives
- User needs
- Success metrics
- Product requirements
- Priorities

Agent Engineers translate these objectives into technical systems.

Successful agent systems require close collaboration between product and engineering teams.

A technically impressive agent that fails to solve meaningful business problems delivers little value.

---

## Comparison

The following table summarizes the primary focus of each discipline.

| Discipline | Primary Focus |
|------------|---------------|
| Software Engineering | Building reliable software systems |
| Machine Learning Engineering | Training and deploying predictive models |
| AI Engineering | Integrating AI capabilities into applications |
| Prompt Engineering | Designing effective model interactions |
| Platform Engineering | Building reusable infrastructure and developer platforms |
| Site Reliability Engineering | Operating reliable production systems |
| Security Engineering | Protecting systems and data |
| **Agent Engineering** | Engineering goal-directed software systems that combine language models, deterministic software, tools, memory, and workflows |

No single discipline replaces another.

Instead, they work together to create production-ready intelligent systems.

Agent Engineering sits at the intersection of these disciplines, combining established software engineering practices with the unique architectural and operational requirements introduced by language models.

---

# What Agent Engineering Is Not

Defining a discipline requires more than describing what it is.

It also requires clarifying what it is not.

The rapid growth of language models has led to a wide variety of terms such as *AI Agent*, *Prompt Engineering*, *Agent Framework*, *LLM Application*, and *Autonomous AI*. These terms are often used interchangeably, even though they describe different concepts.

As a result, many misconceptions have emerged.

This section clarifies the boundaries of Agent Engineering by addressing some of the most common misunderstandings.

## Agent Engineering Is Not Prompt Engineering

One of the most common misconceptions is that building agent systems is simply a matter of writing better prompts.

Prompt Engineering is undoubtedly an important skill.

A well-designed prompt can significantly improve the quality of a language model's responses.

However, prompts alone cannot create production systems.

A production agent must also manage:

- Context
- Memory
- Tool execution
- Security
- Reliability
- Evaluation
- Observability
- Failure recovery

Prompt Engineering focuses on improving a model's behavior.

Agent Engineering focuses on engineering an entire software system.

> **Engineering Note**
>
> A prompt is one artifact within an agent system. It is not the system itself.

---

## Agent Engineering Is Not Calling an LLM API

Many demonstrations of AI applications consist of a simple API call:

```text
User
  │
  ▼
Application
  │
  ▼
Language Model API
  │
  ▼
Response
```

This architecture is perfectly acceptable for prototypes, experiments, and simple assistants.

However, production agent systems require considerably more.

They must answer questions such as:

- What information should the model receive?
- Which tools may it access?
- How should failures be handled?
- How should responses be validated?
- How should execution be monitored?
- How should costs be controlled?

Calling a language model is only one operation within a much larger software system.

An API call does not constitute Agent Engineering.

---

## Agent Engineering Is Not Training Foundation Models

Foundation models require enormous investments in:

- Data
- Compute
- Distributed infrastructure
- Research
- Optimization
- Evaluation

Organizations that build foundation models operate at a very different layer of the AI ecosystem.

Agent Engineers typically consume existing models rather than creating new ones.

Their responsibility is to design software systems that use those models effectively.

Understanding how language models work is valuable.

Training them is usually outside the scope of Agent Engineering.

---

## Agent Engineering Is Not Building Chatbots

Many early applications of language models were conversational interfaces.

As a result, AI agents are often assumed to be sophisticated chatbots.

Conversation is merely one possible interface.

An agent may have no conversational interface at all.

Examples include:

- Reviewing pull requests
- Processing invoices
- Investigating security incidents
- Scheduling infrastructure maintenance
- Generating compliance reports
- Optimizing supply chains
- Coordinating business workflows

The defining characteristic of an agent is not conversation.

It is goal-directed behavior.

---

## Agent Engineering Is Not Autonomous Software Without Limits

The word *agent* sometimes suggests unlimited autonomy.

This is a dangerous misconception.

Production systems should never maximize autonomy without considering safety, governance, or business requirements.

Reliable agent systems operate within carefully designed constraints.

These constraints may include:

- Authorization policies
- Tool permissions
- Budget limits
- Human approval
- Business rules
- Regulatory requirements
- Time limits

Autonomy is valuable only when exercised responsibly.

> **Design Principle**
>
> Production agents should maximize useful outcomes—not unrestricted autonomy.

---

## Agent Engineering Is Not Replacing Deterministic Software

Another misconception is that language models will replace conventional software.

In reality, deterministic software becomes even more important.

Business rules, financial calculations, security policies, compliance requirements, and infrastructure management should remain deterministic whenever possible.

Language models contribute reasoning.

Deterministic software provides correctness, consistency, and control.

Successful agent systems combine both approaches rather than replacing one with the other.

---

## Agent Engineering Is Not Defined by Frameworks

Every year, new frameworks emerge promising to simplify agent development.

Examples include orchestration libraries, multi-agent frameworks, workflow engines, and runtime platforms.

These frameworks are valuable.

They accelerate development and reduce implementation effort.

However, they are implementations—not definitions.

An engineer who understands only a framework is dependent on that framework.

An engineer who understands the underlying principles can evaluate, adopt, replace, or even build frameworks as technologies evolve.

This handbook therefore focuses on principles rather than framework-specific APIs.

Frameworks change.

Engineering principles endure.

---

## Agent Engineering Is Not About Maximizing Intelligence

Many discussions about AI focus on making models increasingly intelligent.

Agent Engineering has a different objective.

Its goal is not to create the smartest possible system.

Its goal is to create systems that are:

- Reliable
- Safe
- Maintainable
- Observable
- Secure
- Cost-effective
- Predictable

An extremely intelligent system that cannot be trusted in production is an engineering failure.

Reliability is more valuable than brilliance.

Consistency is more valuable than novelty.

Engineering is ultimately about building systems that people can depend on.

---

## A Better Mental Model

A useful way to think about Agent Engineering is the following.

Software Engineering asks:

> How do we build reliable software?

Agent Engineering asks:

> How do we build reliable software that can reason, make decisions, use tools, and accomplish goals within well-defined constraints?

The second question builds upon the first.

It does not replace it.

That distinction is one of the central ideas of this handbook and will appear repeatedly throughout the chapters that follow.

---

# Engineering Principles of Agent Engineering

Every engineering discipline is guided by a set of principles that remain valuable even as technologies evolve.

Programming languages change.

Frameworks rise and fall.

Cloud providers introduce new services.

Language models become more capable.

Yet the fundamental principles used to design reliable systems change much more slowly.

Agent Engineering is no different.

The principles introduced in this section serve as the foundation for the remainder of this handbook. Every architectural decision, design pattern, and production recommendation discussed later can be traced back to one or more of these principles.

## Principle 1: Systems Over Prompts

One of the earliest misconceptions surrounding language models was the belief that increasingly sophisticated prompts alone could solve increasingly sophisticated problems.

In practice, prompts are only one component of an agent system.

Reliable systems emerge from the interaction of many components, including:

- Application architecture
- Context management
- Tool integration
- Memory
- Workflows
- Validation
- Security
- Monitoring
- Evaluation

A well-designed architecture consistently outperforms an application that relies solely on prompt complexity.

> **Engineering Note**
>
> Prompts influence the behavior of the language model. Systems determine the behavior of the application.

---

## Principle 2: Deterministic Control, Probabilistic Reasoning

Language models excel at reasoning under uncertainty.

Traditional software excels at executing deterministic logic.

Production systems should leverage both.

Delegate tasks such as:

- Summarization
- Classification
- Planning
- Natural language understanding
- Content generation

to the language model.

Retain deterministic control over:

- Authorization
- Authentication
- Business rules
- Financial calculations
- Compliance
- State transitions
- Data integrity

Successful agent systems separate reasoning from control.

> **Design Principle**
>
> Let the language model think.
>
> Let the software system decide.

---

## Principle 3: Goals Over Instructions

Traditional software often specifies every execution step explicitly.

Agent systems begin with an objective.

Rather than describing every intermediate action, engineers define:

- The desired outcome
- Available capabilities
- Operational constraints
- Success criteria

The agent determines an appropriate execution strategy within those boundaries.

This shift from procedural instructions to goal-oriented execution is one of the defining characteristics of modern agent systems.

---

## Principle 4: Context Determines Capability

A language model can only reason about information that exists within its context.

If important information is missing, the model cannot compensate through intelligence alone.

Consequently, context engineering becomes one of the most important responsibilities of an Agent Engineer.

Effective context includes:

- Relevant information
- Clear instructions
- Organizational knowledge
- Previous interactions
- Retrieved documents
- Current task state

Providing more context is not always better.

Providing the right context is.

---

## Principle 5: Tools Extend Intelligence

Without tools, a language model can only generate responses.

With tools, it can interact with the external world.

Tool integration transforms passive reasoning into active problem solving.

Examples include:

- Querying databases
- Searching documentation
- Executing code
- Calling APIs
- Sending notifications
- Updating business systems

Every tool should expose a clearly defined capability while remaining protected by deterministic software.

The language model decides **when** a tool should be used.

The software system decides **whether** it is allowed.

---

## Principle 6: Reliability Is a System Property

Language models are inherently probabilistic.

Therefore, reliability cannot depend solely on model behavior.

Reliable agent systems achieve consistency through architecture.

Examples include:

- Validation layers
- Retry strategies
- Structured outputs
- Human approval
- Guardrails
- Error handling
- Observability
- Evaluation

The responsibility for reliability belongs to the system as a whole—not to any single component.

---

## Principle 7: Security Must Be Architectural

Security cannot be added after an agent system has been built.

Every capability granted to an agent increases its potential impact.

Security considerations include:

- Tool permissions
- Prompt injection
- Data access
- Memory isolation
- Authentication
- Authorization
- Audit logging

Security decisions should be incorporated into the architecture from the beginning.

> **Production Note**
>
> An agent should never receive more permissions than are necessary to accomplish its intended objective.

---

## Principle 8: Observability Enables Improvement

Unlike deterministic software, agent systems cannot always be understood by inspecting source code alone.

Production systems require comprehensive observability.

Engineers should be able to answer questions such as:

- What information did the model receive?
- Why was a tool selected?
- Which tools were executed?
- How many tokens were consumed?
- How much did the task cost?
- Where did failures occur?
- Why was a particular response generated?

Without observability, debugging becomes speculation.

Without measurement, improvement becomes impossible.

---

## Principle 9: Evaluation Is Continuous

Traditional software often relies on functional testing.

Agent systems require continuous evaluation.

Evaluation extends beyond correctness.

It also measures:

- Task completion
- Quality
- Safety
- Cost
- Latency
- Hallucination rate
- User satisfaction
- Tool effectiveness

Evaluation should accompany every significant architectural change, prompt revision, model upgrade, or workflow modification.

Agent systems are continuously engineered rather than permanently completed.

---

## Principle 10: Humans Remain Accountable

Agent systems may automate reasoning and execution.

Responsibility, however, remains with humans.

Engineers define:

- Objectives
- Constraints
- Permissions
- Governance
- Evaluation criteria
- Operational policies

Organizations—not language models—are accountable for the behavior of production systems.

Agent Engineering therefore emphasizes responsible autonomy rather than unrestricted automation.

---

## Principles in Practice

These principles are intentionally framework-independent.

Whether an organization uses:

- OpenAI Agents SDK
- LangGraph
- CrewAI
- AutoGen
- Semantic Kernel
- PydanticAI
- A custom runtime

the underlying engineering principles remain the same.

Frameworks provide implementation choices.

Engineering principles guide architectural decisions.

Throughout the remainder of this handbook, these principles will appear repeatedly.

Every design pattern, architecture, workflow, and production recommendation can be understood as the practical application of one or more of these foundational principles.

As you continue reading, refer back to this section whenever you encounter a new concept.

The technologies may evolve.

These principles should remain remarkably stable.

---

# Real-World Applications of Agent Engineering

The concepts introduced in this chapter may appear abstract. However, Agent Engineering is not a theoretical discipline. Organizations across industries are already building systems that reason, use tools, retrieve information, collaborate with humans, and automate complex workflows.

These systems vary widely in their capabilities, but they all share a common characteristic:

They are engineered software systems that use language models as one component within a larger architecture.

The following examples illustrate how Agent Engineering is applied in practice.

## Customer Support Automation

Traditional chatbots relied on predefined decision trees and scripted responses.

Modern support agents operate very differently.

Given a customer request, an agent may:

- Understand the customer's intent
- Retrieve account information
- Search internal knowledge bases
- Review previous support conversations
- Execute approved account actions
- Draft personalized responses
- Escalate complex issues to a human representative

The language model provides reasoning.

The surrounding software provides:

- Authentication
- Authorization
- Business rules
- Tool access
- Audit logging
- Customer data protection

The result is not simply a conversational interface.

It is an intelligent customer support system engineered for production.

---

## Software Engineering Assistants

One of the fastest-growing applications of Agent Engineering is software development.

Modern engineering assistants can:

- Explain unfamiliar code
- Generate new implementations
- Review pull requests
- Suggest improvements
- Execute tests
- Search documentation
- Analyze build failures
- Investigate production incidents

Despite these capabilities, they do not replace the software engineer.

Instead, they function as collaborative engineering assistants operating within well-defined constraints.

The surrounding software determines:

- Which repositories are accessible
- Which files may be modified
- Which commands may be executed
- Whether human approval is required
- How changes are audited

The language model contributes reasoning.

The platform provides governance.

---

## Enterprise Knowledge Assistants

Large organizations accumulate enormous amounts of information.

Examples include:

- Technical documentation
- Policies
- Contracts
- Emails
- Meeting notes
- Product specifications
- Standard operating procedures

Finding the correct information often consumes significant time.

Enterprise knowledge agents combine:

- Search
- Retrieval
- Context engineering
- Language models
- Access control

to provide accurate, context-aware answers while respecting organizational permissions.

These systems must ensure that users only receive information they are authorized to access.

Consequently, security and authorization remain deterministic responsibilities of the surrounding software.

---

## Legal Document Analysis

Legal professionals regularly analyze contracts, regulations, and case documents.

Agent systems can assist by:

- Identifying important clauses
- Summarizing lengthy agreements
- Comparing contract versions
- Detecting missing provisions
- Extracting structured information
- Highlighting potential risks

However, the final legal judgment remains the responsibility of qualified professionals.

The agent accelerates analysis.

It does not replace legal expertise.

---

## Financial Operations

Financial organizations increasingly use agent systems to automate operational workflows.

Examples include:

- Invoice processing
- Expense validation
- Compliance reporting
- Risk assessment
- Financial reconciliation
- Audit preparation

These systems often combine:

- Document retrieval
- Data extraction
- Validation
- Business rules
- Human approval
- Reporting

Although language models assist with reasoning and document understanding, deterministic software remains responsible for financial correctness and regulatory compliance.

---

## Healthcare Support

Healthcare organizations use intelligent systems to assist clinicians and administrative staff.

Typical applications include:

- Medical documentation
- Clinical summarization
- Appointment coordination
- Coding assistance
- Research support
- Administrative automation

Because healthcare involves sensitive information and high-risk decisions, these systems require strict governance.

Human professionals remain responsible for diagnosis and treatment decisions.

Agent systems provide assistance rather than autonomous medical decision-making.

---

## Cybersecurity Operations

Security Operations Centers receive enormous volumes of alerts every day.

Agent systems can assist analysts by:

- Correlating alerts
- Gathering threat intelligence
- Searching security documentation
- Summarizing incidents
- Recommending investigation steps
- Generating incident reports

The agent reduces investigation time while allowing security analysts to make the final decisions regarding containment and remediation.

---

## Internal Business Operations

Many organizations deploy internal agents to automate repetitive business activities.

Examples include:

- Employee onboarding
- Procurement workflows
- Travel approvals
- Meeting preparation
- Knowledge management
- Report generation
- Policy assistance

These systems often integrate dozens of internal applications through standardized tool interfaces.

Rather than replacing employees, they reduce administrative overhead and allow people to focus on higher-value work.

---

## Common Characteristics

Although these systems operate in different industries, they share several architectural characteristics.

They all include:

- A language model
- Deterministic application logic
- Tool integrations
- Context management
- Memory
- Security controls
- Observability
- Evaluation
- Human oversight where appropriate

The specific implementation differs from one organization to another.

The engineering principles remain remarkably consistent.

---

## The Broader Perspective

These examples demonstrate an important idea.

Agent Engineering is not limited to conversational assistants.

It applies wherever software systems must:

- Understand complex information
- Reason about possible actions
- Interact with external systems
- Operate within defined constraints
- Assist humans in accomplishing meaningful goals

As language models continue to improve, the range of applications will expand.

The engineering challenges, however, will remain fundamentally the same.

Designing reliable architectures, enforcing deterministic control, protecting sensitive information, evaluating quality, and operating systems safely will continue to define the work of an Agent Engineer.

Ultimately, organizations do not adopt agent systems because they contain language models.

They adopt them because they solve real business problems.

The purpose of Agent Engineering is to ensure those solutions are reliable, maintainable, secure, and worthy of production use.

---

# Key Takeaways

This chapter introduced Agent Engineering as a new engineering discipline and established the mental models that will guide the remainder of this handbook.

Before learning how to build agent systems, it is essential to understand why this discipline exists, what problems it solves, and how it relates to existing areas of software engineering.

The most important ideas from this chapter are summarized below.

## 1. Agent Engineering Is an Engineering Discipline

Agent Engineering is not defined by a programming language, framework, or language model.

It is the discipline of designing, building, deploying, operating, and evolving software systems that combine language models with deterministic software to accomplish goals safely and reliably.

The discipline exists because language models introduced an entirely new class of software systems that traditional engineering practices alone cannot fully address.

---

## 2. Language Models Are Components, Not Applications

A language model provides reasoning capabilities.

It is not a complete software system.

Production agents require many additional components, including:

- An agent runtime
- Context management
- Tool integrations
- Memory
- Security
- Evaluation
- Observability
- Workflow orchestration

The surrounding software transforms a language model into a production-ready system.

---

## 3. Agent Systems Combine Deterministic and Probabilistic Computing

Traditional software executes deterministic logic.

Language models perform probabilistic reasoning.

Successful agent systems deliberately combine these two approaches.

Deterministic software provides:

- Correctness
- Consistency
- Security
- Governance

Language models provide:

- Reasoning
- Planning
- Interpretation
- Adaptability

Understanding where each approach should be used is one of the primary responsibilities of an Agent Engineer.

---

## 4. Architecture Matters More Than Frameworks

Frameworks accelerate implementation.

Architecture determines long-term success.

A well-designed architecture remains valuable even when:

- Models change
- Frameworks evolve
- Vendors are replaced
- Infrastructure is modernized

Throughout this handbook, architectural principles take precedence over framework-specific APIs.

---

## 5. Agent Engineering Builds Upon Existing Disciplines

Agent Engineering does not replace:

- Software Engineering
- AI Engineering
- Machine Learning Engineering
- Platform Engineering
- Site Reliability Engineering
- Security Engineering

Instead, it integrates concepts from each discipline to create reliable goal-directed software systems.

Strong software engineering remains the foundation of every successful agent system.

---

## 6. Production Systems Require More Than Intelligence

Intelligence alone does not create reliable software.

Production agent systems must also be:

- Secure
- Observable
- Maintainable
- Scalable
- Cost-effective
- Governed
- Continuously evaluated

Engineering transforms intelligent capabilities into dependable systems.

---

## 7. Responsible Autonomy Is the Goal

The objective of Agent Engineering is not unrestricted autonomy.

Instead, it is to build systems that can safely exercise autonomy within clearly defined constraints.

Every production agent should operate within explicit boundaries established by deterministic software.

Human accountability remains essential.

---

## Summary

By the end of this chapter, you should understand that Agent Engineering is not simply another AI trend or framework.

It is the natural evolution of Software Engineering in response to language models and goal-directed intelligent systems.

Just as cloud computing introduced Cloud Engineering and large-scale operations introduced Site Reliability Engineering, language models have introduced a new set of engineering challenges that require new architectural thinking.

The remainder of this handbook explores those challenges in depth.

Each subsequent chapter examines one aspect of Agent Engineering, gradually building the knowledge required to design, build, and operate production-grade agent systems.

---

# Exercises

The following exercises are intended to reinforce the concepts introduced in this chapter.

Rather than memorizing definitions, focus on developing engineering intuition.

## Exercise 1 — Identify the Discipline

For each of the following activities, determine whether it primarily belongs to:

- Software Engineering
- Machine Learning Engineering
- AI Engineering
- Agent Engineering

Activities:

- Designing a tool interface for an agent
- Training a transformer model
- Building a REST API
- Engineering a context retrieval pipeline
- Creating a deployment pipeline
- Designing a memory architecture

Explain your reasoning for each answer.

---

## Exercise 2 — Analyze an Existing AI Product

Choose an AI-powered product that you use regularly.

Examples include coding assistants, customer support assistants, document analysis systems, or enterprise knowledge assistants.

Identify:

- The goal of the system
- The language model
- The deterministic components
- The available tools
- Possible memory mechanisms
- Security considerations
- Human oversight

Draw a simple architecture diagram showing how the components interact.

---

## Exercise 3 — Separate Reasoning from Control

Consider an application that automatically reviews expense reports.

Classify each responsibility as either:

- Deterministic Software
- Language Model

Responsibilities:

- Detect duplicate receipts
- Summarize expense descriptions
- Verify policy compliance
- Calculate reimbursement totals
- Explain policy violations
- Approve payments

Discuss why each responsibility belongs where it does.

---

## Exercise 4 — Define an Agent System

Select a business problem from your organization or industry.

Examples might include:

- Customer support
- Contract analysis
- Incident response
- Software delivery
- Compliance reporting

Describe:

- The goal
- Available tools
- Required memory
- Security constraints
- Human approval points
- Success metrics

Do not discuss frameworks.

Focus on architecture.

---

# Interview Questions

The following questions test conceptual understanding rather than framework knowledge.

### Fundamental

1. What is Agent Engineering?

2. Why has Agent Engineering emerged as a separate discipline?

3. How does Agent Engineering differ from Software Engineering?

4. How does Agent Engineering differ from AI Engineering?

5. Why is Prompt Engineering only one part of Agent Engineering?

---

### Architecture

6. Why is a language model not considered a complete application?

7. What components typically make up a production agent system?

8. Why should deterministic software retain control over business rules?

9. Why is context engineering critical to agent performance?

10. Why are tools essential for production agents?

---

### Production

11. What responsibilities does an Agent Engineer have after deployment?

12. Why is evaluation a continuous process?

13. What new security challenges do agent systems introduce?

14. Why is observability important in probabilistic systems?

15. What does responsible autonomy mean?

---

# Further Reading

The concepts introduced in this chapter are expanded throughout this handbook.

For additional background, the following resources are recommended.

## Books

- *Designing Data-Intensive Applications* — Martin Kleppmann
- *Site Reliability Engineering* — Google
- *Software Architecture: The Hard Parts* — Neal Ford, Mark Richards, Pramod Sadalage, and Zhamak Dehghani
- *Fundamentals of Software Architecture* — Mark Richards and Neal Ford

## Research

- *Attention Is All You Need* (Transformer Architecture)
- *ReAct: Synergizing Reasoning and Acting in Language Models*
- *Toolformer: Language Models Can Teach Themselves to Use Tools*

## Standards and Documentation

- OpenAI API Documentation
- Anthropic Documentation
- Model Context Protocol (MCP) Specification

---

# Looking Ahead

In this chapter, we established Agent Engineering as a distinct engineering discipline and introduced the architectural mindset required to build reliable intelligent systems.

We deliberately avoided discussing implementation details, frameworks, or specific technologies.

Before designing agent runtimes, memory systems, planning algorithms, or tool architectures, we must first understand how software itself evolved to reach this point.

In the next chapter, **002 — The Evolution of Software**, we step back and examine the historical progression of computing—from standalone programs to distributed systems, cloud-native architectures, AI-powered applications, and finally agent systems.

Understanding this evolution provides the context for every architectural decision discussed throughout the rest of this handbook.