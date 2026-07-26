# Chapter 106 — The Anatomy of an Agent System

> **Domain:** Foundations
>
> **Estimated Reading Time:** 50–60 minutes
>
> **Prerequisites:** Chapters 101–105
>
> **Difficulty:** Beginner to Intermediate
>
> **Last Updated:** 2026-07-27

---

## Why This Chapter Exists

Throughout the previous chapters, we introduced many terms that are central to modern Agent Engineering.

We discussed:

- Large Language Models
- Prompts
- Context
- Memory
- Planning
- Tool Calling
- Agent Runtimes
- Guardrails
- Execution Loops
- Evaluation

Each concept was introduced only when it was needed to explain a larger idea.

At this point, however, we have not yet examined how these pieces fit together.

This chapter serves that purpose.

Rather than exploring each component in depth, we will construct a complete architectural map of a modern agent system.

Think of this chapter as the blueprint of an aircraft.

Before studying the engine, hydraulics, avionics, navigation systems, or flight controls individually, it is useful to understand where each subsystem fits within the whole.

The same principle applies to Agent Engineering.

Before diving deeply into topics such as Prompt Engineering, Context Engineering, Memory Systems, Planning, Tool Calling, or Model Context Protocol (MCP), we need a clear picture of the complete system they collectively create.

By the end of this chapter, you should be able to look at any modern agent architecture—whether built using LangGraph, OpenAI Agents SDK, CrewAI, AutoGen, Semantic Kernel, PydanticAI, or a custom runtime—and immediately recognize:

- every major architectural component,
- the responsibility of each component,
- how information flows between them,
- how they cooperate during execution,
- and where each component fits within the agent execution loop.

This chapter is intentionally framework-neutral.

Frameworks may organize these components differently or combine several responsibilities into a single abstraction.

The underlying architecture, however, remains remarkably consistent across production-grade agent systems.

The remaining parts of this handbook will revisit each of these components individually.

For now, our goal is simpler:

> **Build a complete mental map of a modern agent system before studying its individual parts.**

## Guiding Question

> **What are the essential building blocks of every modern agent system, and how do they work together to transform a goal into an outcome?**

By this point in the handbook, we understand:

- what an agent is,
- how an agent executes,
- and why Large Language Models changed software engineering.

The next logical question is:

> **If I were building an agent from scratch, what components would I actually need?**

This is one of the most important architectural questions in Agent Engineering.

A common misconception is that an agent consists primarily of a language model.

In reality, the language model is only one component within a much larger system.

A production-grade agent typically combines:

- deterministic software,
- probabilistic reasoning,
- persistent memory,
- external tools,
- planning,
- context construction,
- execution orchestration,
- safety mechanisms,
- and observability.

Each component exists because it solves a specific engineering problem.

Remove one of them, and the capabilities of the overall system change significantly.

Throughout this chapter, we will examine these components individually.

Rather than focusing on implementation details, we will answer four questions for each component:

1. **What problem does it solve?**
2. **Why does it exist?**
3. **Where does it fit within the execution loop?**
4. **Which later chapters explore it in depth?**

By the end of this chapter, you should no longer think of an agent as a single piece of software.

Instead, you should recognize it as a collection of specialized components working together through a coordinated execution loop.

That architectural perspective will serve as the foundation for the remainder of this handbook.

## The Big Picture

Before studying each component individually, let's first look at the complete architecture.

Every production-grade agent system can be viewed as a collection of cooperating subsystems.

Some systems combine these responsibilities.

Others separate them into independent services.

Regardless of the implementation, nearly every modern agent contains the same architectural building blocks.

---

### A High-Level Architecture

```text
                                   User / Goal
                                         │
                                         ▼
                              ┌─────────────────────┐
                              │    Agent Runtime    │
                              │  (Orchestrator)     │
                              └──────────┬──────────┘
                                         │
          ┌──────────────┬───────────────┼───────────────┬──────────────┐
          ▼              ▼               ▼               ▼              ▼
     Context         Memory          Planner        Guardrails     Observability
     Engine          System            │                                 │
          │              │             │                                 │
          └──────────────┴──────┬──────┴─────────────────────────────────┘
                                ▼
                      Prompt Construction
                                │
                                ▼
                     ┌────────────────────┐
                     │ Large Language     │
                     │ Model (Reasoner)   │
                     └─────────┬──────────┘
                               │
                ┌──────────────┴──────────────┐
                ▼                             ▼
         Structured Output              Tool Requests
                │                             │
                ▼                             ▼
         Runtime Parses             Tool Execution Layer
                                          │
            ┌──────────────┬──────────────┼───────────────┐
            ▼              ▼              ▼               ▼
        Database         APIs          File System      Other Agents
```

Although this diagram may appear complex, every component has a single, well-defined responsibility.

The complexity of modern agent systems comes from **coordination**, not from any individual component.

---

### Thinking in Layers

Another useful way to understand an agent is by viewing it as a layered architecture.

```text
┌─────────────────────────────────────────────┐
│ Layer 1  User Goals                         │
├─────────────────────────────────────────────┤
│ Layer 2  Runtime & Orchestration            │
├─────────────────────────────────────────────┤
│ Layer 3  Planning • Context • Memory        │
├─────────────────────────────────────────────┤
│ Layer 4  Language Model (Reasoning)         │
├─────────────────────────────────────────────┤
│ Layer 5  Tools & External Systems           │
├─────────────────────────────────────────────┤
│ Layer 6  The Real World                     │
└─────────────────────────────────────────────┘
```

Each layer depends on the one below it.

For example:

- The runtime depends on the language model for reasoning.
- The language model depends on context for informed decisions.
- Tools depend on external systems to perform work.
- The runtime coordinates them all to achieve the user's goal.

Thinking in layers helps separate responsibilities and prevents the common mistake of treating the language model as the entire system.

---

### Every Component Exists for a Reason

One of the defining characteristics of good software architecture is that every major component solves a specific problem.

The same principle applies here.

| Component | Primary Responsibility |
|-----------|------------------------|
| Runtime | Coordinates execution |
| Context Engine | Supplies relevant information |
| Memory | Preserves knowledge across time |
| Planner | Determines future actions |
| Prompt Builder | Instructs the reasoning engine |
| LLM | Performs probabilistic reasoning |
| Structured Output | Produces machine-readable results |
| Tool Layer | Interacts with external systems |
| Guardrails | Enforces constraints and safety |
| Observability | Records and analyzes execution |

Notice that none of these components replace one another.

Instead, they complement each other.

Each exists because the language model alone cannot reliably perform that responsibility.

---

### A Useful Analogy

An agent system is much like a modern organization.

```text
CEO (Goal)

        │

        ▼

Operations Manager
(Runtime)

        │

 ┌──────┼─────────────┐
 ▼      ▼             ▼
Planning  Knowledge  Operations
          (Memory)   (Tools)

        │

        ▼

Specialist
(LLM)

        │

        ▼

External World
```

The specialist (the LLM) performs expert reasoning.

However, it is not responsible for coordinating the organization, maintaining institutional knowledge, interacting with external systems, or ensuring compliance.

Those responsibilities belong to other parts of the organization.

Modern agent systems follow the same architectural principle.

---

### The Roadmap for This Chapter

The remainder of this chapter will explore each major component in turn.

For every component, we will answer four questions:

1. **What problem does it solve?**
2. **Why does it exist?**
3. **Where does it fit within the execution loop?**
4. **Where will we study it in depth later in this handbook?**

By the end of this chapter, the architecture shown above should feel familiar rather than intimidating.

You will have a complete mental map of a modern agent system, making it much easier to understand the detailed topics covered in the remaining parts of this handbook.

## Component 1 — Goal

Every agent system begins with a goal.

Without a goal, there is no reason to reason, no need to plan, and no basis for deciding whether execution has succeeded.

The goal is the driving force behind the entire system.

Everything else exists to help achieve it.

---

### What Problem Does It Solve?

A traditional application executes predefined functionality.

An agent executes toward an objective.

For example:

Traditional application:

```text
User clicks "Download PDF"

↓

Generate PDF

↓

Return File
```

The application performs a specific function.

Now consider an agent.

```text
Goal

↓

Prepare monthly sales report

↓

Determine required information

↓

Gather data

↓

Generate report

↓

Verify completion
```

The goal provides direction.

Instead of executing a fixed function, the agent continuously asks:

> **"What should I do next to achieve this objective?"**

---

### Why Does It Exist?

Without a goal, reasoning has no purpose.

Imagine asking an engineer:

> "Think."

The request is incomplete.

Think about **what**?

Reasoning always needs an objective.

Similarly, an LLM without a goal simply predicts text.

An agent with a goal uses reasoning to move toward a desired outcome.

This distinction is fundamental.

The goal transforms reasoning into goal-directed execution.

---

### Different Kinds of Goals

Goals vary in complexity.

Some are simple.

```text
Translate this email.
```

Others require multiple reasoning cycles.

```text
Investigate why yesterday's deployment failed and prepare recommendations.
```

Some may take seconds.

Others may execute for hours or even days.

The runtime does not care about the duration.

Its responsibility is simply to continue execution until the goal has been achieved or execution should stop.

---

### Goals vs. Tasks

These terms are often confused.

They are not the same.

```text
Goal

Launch Product
```

Possible tasks:

```text
Research competitors

↓

Prepare marketing material

↓

Deploy website

↓

Publish announcement

↓

Monitor launch
```

The **goal** defines the desired outcome.

The **tasks** are the individual pieces of work required to reach that outcome.

Planning exists largely to transform goals into executable tasks.

---

### Goals Influence Every Component

The goal is not used only once.

It influences nearly every part of the architecture.

```text
Goal
 │
 ├────────► Context Selection
 │
 ├────────► Planning
 │
 ├────────► Tool Selection
 │
 ├────────► Memory Retrieval
 │
 ├────────► Reasoning
 │
 └────────► Stopping Condition
```

For example:

If the goal changes from:

> "Summarize this report"

to:

> "Identify financial risks in this report"

the system may retrieve different documents, construct different context, invoke different tools, and produce a completely different response.

The reasoning engine remains the same.

Only the goal changes.

---

### Where Does It Fit in the Execution Loop?

The goal exists before execution begins and remains active throughout the agent's lifetime.

```text
Receive Goal
      │
      ▼
Observe
      │
      ▼
Build Context
      │
      ▼
Reason
      │
      ▼
Plan
      │
      ▼
Act
      │
      ▼
Evaluate

Repeat Until Goal Achieved
```

Every execution cycle asks the same question:

> **"Given the current state, what action moves us closest to the goal?"**

Without the goal, this question cannot be answered.

---

### Where Will We Study This in Depth?

Goals appear throughout the remainder of the handbook.

In particular:

- **Chapter 107 — Agent Lifecycle**, where we examine how goals are created, refined, executed, and completed.
- **Planning**, where goals are decomposed into executable tasks.
- **Multi-Agent Systems**, where goals may be delegated across multiple collaborating agents.

---

### The Core Insight

The goal is not merely an input to an agent.

It is the organizing principle of the entire system.

Everything that follows—context, planning, memory, reasoning, tool execution, and evaluation—exists to answer a single question:

> **"What should happen next to move the system closer to the desired outcome?"**

That is what distinguishes an agent from software that merely responds to requests.

## Component 2 — Agent Runtime

If the goal defines **what** the agent should achieve, the runtime determines **how** the agent executes.

The runtime is the central coordinator of the entire system.

It does not perform reasoning.

It does not store all knowledge.

It does not interact directly with the external world.

Instead, it orchestrates the components that collectively produce intelligent behavior.

Without a runtime, an agent is simply a collection of disconnected capabilities.

---

### What Problem Does It Solve?

Consider all of the activities required during a single execution cycle.

The system must:

- receive a goal,
- retrieve memory,
- build context,
- call the language model,
- interpret the result,
- execute tools,
- handle failures,
- update state,
- determine whether execution should continue.

Someone must coordinate these activities.

That responsibility belongs to the runtime.

Conceptually:

```text
Goal
 │
 ▼
Runtime
 │
 ├── Retrieve Memory
 ├── Build Context
 ├── Call LLM
 ├── Execute Tools
 ├── Update State
 ├── Handle Errors
 └── Decide Next Step
```

Without a runtime, these components have no mechanism for working together.

---

### Why Does It Exist?

The language model is fundamentally a reasoning engine.

Given context, it produces a response.

It does **not**:

- execute loops,
- retry failed API calls,
- maintain execution state,
- schedule future work,
- manage memory,
- enforce safety rules,
- terminate execution.

Those responsibilities require deterministic software.

The runtime fills that gap.

A useful rule of thumb is:

> **The LLM thinks. The runtime runs.**

---

### The Runtime as an Orchestrator

Think of an orchestra.

```text
Audience
    │
    ▼
Conductor
    │
    ├──────────────┬───────────────┬──────────────┐
    ▼              ▼               ▼
Violin         Percussion       Piano
```

Each musician performs a specialized task.

The conductor coordinates them.

Similarly:

```text
Runtime
   │
   ├─────────────┬──────────────┬───────────────┐
   ▼             ▼              ▼
Memory         LLM           Tool Layer
```

The runtime determines:

- when each component is needed,
- what information flows between them,
- how the execution loop progresses.

The intelligence of the system emerges from this coordination.

---

### Typical Responsibilities

Although implementations differ, most runtimes perform responsibilities such as:

| Responsibility | Description |
|---------------|-------------|
| Goal Management | Track the current objective |
| State Management | Maintain execution state |
| Context Construction | Assemble information for reasoning |
| Model Invocation | Call the reasoning engine |
| Tool Execution | Invoke external capabilities |
| Planning Coordination | Execute or revise plans |
| Memory Management | Read and write memory |
| Retry Logic | Recover from transient failures |
| Guardrail Enforcement | Apply safety and policy constraints |
| Termination | Decide when execution should stop |
| Observability | Record traces, logs, and metrics |

Notice that almost none of these involve reasoning.

They involve orchestration.

---

### Where Does It Fit in the Execution Loop?

Unlike other components, the runtime participates in **every stage** of execution.

```text
Receive Goal
      │
      ▼
Runtime
      │
      ▼
Observe
      │
      ▼
Runtime
      │
      ▼
Build Context
      │
      ▼
Runtime
      │
      ▼
Reason
      │
      ▼
Runtime
      │
      ▼
Execute
      │
      ▼
Runtime
      │
      ▼
Evaluate
```

The runtime is the thread that connects every phase of the agent lifecycle.

---

### Frameworks Primarily Implement Runtimes

One of the most important insights in Agent Engineering is that popular frameworks are primarily runtime implementations.

For example:

| Framework | Runtime Focus |
|-----------|---------------|
| LangGraph | Graph-based execution |
| OpenAI Agents SDK | Agent execution orchestration |
| CrewAI | Multi-agent orchestration |
| AutoGen | Conversational coordination |
| Semantic Kernel | AI workflow orchestration |
| PydanticAI | Typed execution runtime |

Although their APIs differ, they all attempt to solve the same architectural problem:

> **How do we reliably coordinate an agent's execution loop?**

This perspective makes it much easier to evaluate and compare frameworks.

Rather than asking:

> "Which framework is best?"

You begin asking:

- How does it manage state?
- How does it execute tools?
- How does it represent workflows?
- How does it recover from failures?
- How observable is execution?
- How extensible is the runtime?

These are architectural questions rather than API questions.

---

### Where Will We Study This in Depth?

The runtime is one of the most important components in the entire handbook.

It will reappear in several later chapters, including:

- **Agent Lifecycle**, where we examine how the runtime drives execution from start to finish.
- **Agent Runtimes**, where we explore orchestration architectures, state management, execution models, and runtime design patterns.
- **Observability**, where we learn how runtimes expose traces, logs, metrics, and execution histories.

---

### The Core Insight

The runtime is the operating system of an agent.

It does not make the agent intelligent.

It makes the agent **operational**.

Without the runtime, a language model is simply a reasoning engine.

With the runtime coordinating goals, context, memory, tools, and execution, that reasoning engine becomes a complete, goal-directed software system.

## Component 3 — Context Engine

If the runtime decides **when** the language model should reason, the Context Engine decides **what information** the language model should reason with.

This component is responsible for constructing the model's reasoning environment.

In modern agent systems, context quality is often a stronger predictor of success than model size.

A powerful model with poor context usually performs worse than a smaller model with highly relevant context.

---

### What Problem Does It Solve?

A language model can only reason about information that exists within its current context.

It cannot:

- access your database automatically,
- remember previous conversations indefinitely,
- read company documentation by itself,
- inspect application state,
- understand business rules unless they are provided.

The Context Engine solves this limitation.

It determines exactly what information should be presented to the model before reasoning begins.

```text
Application State
        │
Memory ─┤
        │
User Input
        │
Knowledge Base
        │
Tool Results
        │
Policies
        │
────────┴──────────
        ▼
 Context Engine
        │
        ▼
 Final Context
        │
        ▼
 Language Model
```

Without this component, the model reasons with incomplete information.

---

### Why Does It Exist?

Imagine asking an experienced engineer:

> "Why did yesterday's deployment fail?"

Without any additional information, they can only speculate.

Now imagine giving them:

- deployment logs,
- monitoring dashboards,
- recent code changes,
- infrastructure alerts,
- previous deployment history.

The quality of their reasoning improves dramatically.

The same principle applies to language models.

Reasoning quality depends on information quality.

The Context Engine exists to maximize that quality.

---

### Context Is More Than a Prompt

A common misconception is that context is simply the prompt.

It is not.

The prompt is only one part of the complete reasoning environment.

A typical context package may include:

```text
System Instructions

↓

User Goal

↓

Conversation History

↓

Retrieved Memory

↓

Relevant Documents

↓

Recent Tool Results

↓

Business Rules

↓

Current Task

↓

Formatting Instructions
```

The prompt is simply one component within this larger structure.

Modern agent systems spend far more effort constructing context than writing prompts.

---

### The Context Engine Is a Filter

The Context Engine does not attempt to include everything.

Instead, it performs intelligent selection.

Conceptually:

```text
Available Information

100 Documents
20 Memories
15 Tool Results
50 Policies

        │

        ▼

Context Engine

        │

        ▼

Most Relevant
Information Only
```

Its job is to answer questions such as:

- Which documents are relevant?
- Which memories matter?
- Which previous messages should be included?
- Which tool results are still useful?
- What information is unnecessary?

This filtering process is essential because language models have limited context windows.

---

### More Context Is Usually Worse

One of the most common mistakes in Agent Engineering is assuming that larger context always produces better reasoning.

Suppose an agent receives:

```text
Relevant Information

10 KB
```

This is often ideal.

Now imagine including:

- irrelevant emails,
- unrelated documents,
- outdated memories,
- obsolete tool outputs,
- unnecessary policies.

The context grows to:

```text
Relevant

+

Irrelevant

↓

200 KB
```

The model must now distinguish signal from noise.

Reasoning often becomes less accurate rather than more accurate.

The Context Engine therefore optimizes for **relevance**, not quantity.

---

### Where Does It Fit in the Execution Loop?

The Context Engine operates immediately before reasoning.

```text
Observe
     │
     ▼
Retrieve Information
     │
     ▼
Context Engine
     │
     ▼
Language Model
     │
     ▼
Reason
```

Every reasoning cycle constructs a fresh context based on the current objective and the latest observations.

This means the context may change on every iteration.

---

### Relationship to Other Components

The Context Engine depends heavily on several other components.

```text
          Memory
             │
             ▼
 Knowledge Base
             │
             ▼
 Tool Results
             │
             ▼
 User Goal
             │
             ▼
      Context Engine
             │
             ▼
           Prompt
             │
             ▼
             LLM
```

Notice that the Context Engine sits at the intersection of nearly every information source in the system.

It transforms raw information into a coherent reasoning environment.

---

### Where Will We Study This in Depth?

Context Engineering is one of the largest disciplines in modern Agent Engineering.

Later chapters will explore topics such as:

- relevance ranking,
- retrieval strategies,
- context compression,
- long-context management,
- prompt assembly,
- context caching,
- token budgeting,
- dynamic context construction.

Together, these techniques form the discipline known as **Context Engineering**.

---

### The Core Insight

The language model does not reason over your entire application.

It reasons over the context that the runtime provides.

The Context Engine therefore acts as the agent's information architect.

Its responsibility is not to gather the most information.

Its responsibility is to provide the **right information**, at the **right time**, in the **right form**.

In modern agent systems, constructing high-quality context is one of the most valuable engineering capabilities you can develop.

## Component 4 — Memory System

If the Context Engine determines **what information is available right now**, the Memory System determines **what information can survive beyond the current reasoning cycle**.

Memory gives an agent continuity.

Without memory, every execution begins as if the agent has never existed before.

---

### What Problem Does It Solve?

Large Language Models are fundamentally stateless.

When a model finishes generating a response, it does not permanently remember:

- previous conversations,
- user preferences,
- completed tasks,
- business knowledge,
- observations,
- or lessons learned.

Each model invocation is independent.

For example:

```text
Request 1

↓

Model Responds

↓

Request Ends

↓

Everything Disappears
```

If we want an agent to remember anything, deterministic software must store that information somewhere.

That responsibility belongs to the Memory System.

---

### Why Does It Exist?

Imagine working with a colleague who forgets everything after every conversation.

Every morning you would need to explain:

- your project,
- your team,
- your coding standards,
- yesterday's decisions,
- and today's priorities.

The collaboration would be frustrating and inefficient.

Now imagine another colleague who remembers:

- previous discussions,
- project history,
- common mistakes,
- team conventions,
- and your working style.

The quality of collaboration improves dramatically.

The same principle applies to agents.

Memory allows reasoning to build upon previous experience rather than starting from scratch every time.

---

### Memory Is Not Context

One of the most common misconceptions is treating memory and context as the same thing.

They are closely related but fundamentally different.

Think of a library.

```text
Entire Library

↓

Memory
```

When you begin writing a research paper, you do not place every book on your desk.

Instead, you select only the relevant ones.

```text
Entire Library
        │
        ▼
Relevant Books
        │
        ▼
Desk
```

The desk represents the current context.

The library represents long-term memory.

In other words:

```text
Memory stores information.

Context selects information.
```

The Context Engine retrieves useful information from memory and prepares it for the language model.

---

### Different Types of Memory

Not all memories serve the same purpose.

A modern agent may maintain several kinds of memory simultaneously.

| Memory Type | Purpose |
|-------------|---------|
| Conversation Memory | Previous interactions with the user |
| User Memory | Preferences, profile, long-term facts |
| Working Memory | Information needed during the current task |
| Episodic Memory | Records of previous executions and outcomes |
| Semantic Memory | Stable knowledge and learned facts |
| Procedural Memory | Instructions, workflows, and operating procedures |

Different systems organize these categories differently, but the underlying idea remains the same:

Different information has different lifetimes and retrieval strategies.

---

### The Memory Lifecycle

Memory is not static.

Information continually flows into and out of the system.

```text
Experience
      │
      ▼
Should this be remembered?
      │
      ▼
Store Memory
      │
      ▼
Future Goal
      │
      ▼
Retrieve Relevant Memory
      │
      ▼
Context Engine
      │
      ▼
Language Model
```

Notice that memory itself does not perform reasoning.

It simply preserves information until it becomes useful again.

---

### Memory Should Be Selective

Another common mistake is attempting to remember everything.

Suppose an agent stores:

- every conversation,
- every tool result,
- every API response,
- every intermediate reasoning step.

Over time, memory becomes enormous.

Retrieving useful information becomes slower and less accurate.

A good Memory System therefore answers questions such as:

- Is this worth remembering?
- How long should it be retained?
- Can it be summarized?
- Can outdated memories be removed?
- How should it be indexed for retrieval?

In production systems, forgetting is often just as important as remembering.

---

### Where Does It Fit in the Execution Loop?

Memory participates in multiple stages of execution.

```text
Receive Goal
      │
      ▼
Retrieve Relevant Memory
      │
      ▼
Context Engine
      │
      ▼
Reason
      │
      ▼
Act
      │
      ▼
Learn Something New?
      │
      ▼
Store Memory
```

This creates a feedback loop where every execution has the potential to improve future executions.

---

### Relationship to Other Components

Memory does not interact directly with the language model.

Instead, it collaborates closely with the Context Engine.

```text
Memory Store
      │
      ▼
Memory Retrieval
      │
      ▼
Context Engine
      │
      ▼
Prompt Construction
      │
      ▼
Language Model
```

This separation of responsibilities is intentional.

The Memory System focuses on persistence.

The Context Engine focuses on relevance.

Together, they enable informed reasoning across multiple interactions.

---

### Where Will We Study This in Depth?

Memory systems are one of the defining capabilities of advanced agents.

Later chapters will explore topics such as:

- short-term and long-term memory,
- episodic and semantic memory,
- memory retrieval strategies,
- memory summarization,
- vector-based memory,
- memory consolidation,
- forgetting strategies,
- memory architectures for multi-agent systems.

These topics collectively form the discipline of **Agent Memory Systems**.

---

### The Core Insight

Memory is what allows an agent to improve over time.

Without memory, every execution is isolated.

With memory, every execution becomes part of a larger history.

The Memory System does not make an agent intelligent.

It makes the agent **persistent**.

That persistence enables continuity, personalization, adaptation, and long-term problem solving—capabilities that are impossible when every reasoning cycle starts from a blank slate.

## Component 5 — Planner

If the goal defines **where** the agent should go, the Planner determines **how to get there**.

Planning is the process of transforming a high-level objective into a sequence of executable actions.

Not every agent requires a planner.

However, as goals become larger, longer, or more complex, planning becomes one of the most valuable capabilities an agent can possess.

---

### What Problem Does It Solve?

Some goals can be completed with a single reasoning step.

For example:

```text
Translate this paragraph.
```

The agent can immediately produce the answer.

Now consider a different goal:

```text
Analyze our competitors and recommend a pricing strategy.
```

This cannot be solved in one step.

The agent may need to:

1. Identify competitors.
2. Collect pricing data.
3. Compare products.
4. Analyze market positioning.
5. Prepare recommendations.

Planning bridges the gap between a high-level objective and the concrete actions required to achieve it.

---

### Why Does It Exist?

Large Language Models excel at reasoning.

They are less reliable at managing long sequences of work over time.

Without planning, an agent often behaves reactively.

```text
Goal

↓

Think

↓

Respond
```

With planning, execution becomes deliberate.

```text
Goal

↓

Create Plan

↓

Execute Step 1

↓

Evaluate

↓

Execute Step 2

↓

Evaluate

↓

...

↓

Goal Achieved
```

The planner provides structure.

Instead of repeatedly asking, *"What should I do?"*, the agent follows an evolving roadmap.

---

### Planning Is Not Scheduling

These terms are often confused.

Planning determines **what** should be done.

Scheduling determines **when** it should be done.

For example:

Goal:

```text
Launch a new product.
```

Planner:

```text
Research market

↓

Build landing page

↓

Prepare campaign

↓

Deploy website
```

Scheduler:

```text
Monday
↓

Wednesday
↓

Friday
↓

Launch Day
```

Most agent systems focus primarily on planning rather than scheduling.

---

### Plans Are Not Static

A common misconception is that a plan is created once and then executed exactly as written.

Real-world environments change.

Suppose the initial plan is:

```text
1. Retrieve customer profile

2. Generate invoice

3. Email customer
```

During execution, step one fails because the customer no longer exists.

A rigid workflow would fail immediately.

A planner may instead revise the plan.

```text
1. Search alternative account

2. Verify ownership

3. Generate invoice

4. Notify support if verification fails
```

Planning is therefore an iterative process.

Modern planners continuously adapt based on new observations.

---

### Planning Happens at Multiple Levels

Planning can occur at different levels of granularity.

High-level planning:

```text
Organize a company conference.
```

Mid-level planning:

```text
Book venue.

Invite speakers.

Arrange catering.
```

Low-level planning:

```text
Call Venue API

↓

Check Availability

↓

Reserve Date
```

A sophisticated agent may maintain plans at several levels simultaneously.

---

### Relationship to the Runtime

The planner does not execute work directly.

Instead, it collaborates with the runtime.

```text
Goal
 │
 ▼
Planner
 │
 ▼
Execution Plan
 │
 ▼
Runtime
 │
 ├── Execute Step
 ├── Observe Result
 ├── Update State
 └── Ask Planner if Revision Is Needed
```

This separation is important.

The planner decides **what** should happen next.

The runtime ensures that it actually happens.

---

### Where Does It Fit in the Execution Loop?

Planning usually occurs after the agent has gathered enough information to understand the problem.

```text
Receive Goal
      │
      ▼
Observe
      │
      ▼
Build Context
      │
      ▼
Reason
      │
      ▼
Planner
      │
      ▼
Execute Step
      │
      ▼
Evaluate
      │
      ▼
Need New Plan?
      │
      ├── Yes → Planner
      └── No → Continue
```

Notice that planning is not necessarily a one-time activity.

It can occur repeatedly throughout execution.

---

### When Is a Planner Needed?

Not every agent requires explicit planning.

Simple agents often execute successfully without it.

| Task | Planner Needed? |
|------|------------------|
| Translate text | No |
| Summarize a document | No |
| Answer a question | Usually No |
| Research a topic | Often |
| Investigate an incident | Yes |
| Build a travel itinerary | Yes |
| Coordinate multiple agents | Almost Always |

As tasks become longer, more uncertain, or involve multiple dependencies, planning becomes increasingly valuable.

---

### Where Will We Study This in Depth?

Planning is a major discipline in Agent Engineering.

Later chapters will explore:

- task decomposition,
- hierarchical planning,
- dynamic replanning,
- plan validation,
- execution monitoring,
- planning algorithms,
- planner-executor architectures,
- ReAct and reasoning-action loops.

These techniques allow agents to solve problems that extend far beyond a single model invocation.

---

### The Core Insight

Reasoning determines **what seems like a good next step**.

Planning determines **how many steps are required and in what order they should be executed**.

The Planner transforms objectives into actionable roadmaps.

It gives an agent the ability to tackle problems that cannot be solved in a single thought, enabling deliberate, adaptive, and multi-step execution.

## Component 6 — Large Language Model (LLM)

At the center of every modern agent system is the Large Language Model.

This is the component responsible for reasoning.

It interprets information, analyzes situations, generates plans, produces structured outputs, and decides which actions should be taken.

However, despite its central role, the LLM is **only one component** of the overall architecture.

One of the biggest mistakes new Agent Engineers make is assuming that the LLM *is* the agent.

It is not.

It is the agent's reasoning engine.

---

### What Problem Does It Solve?

Traditional software executes algorithms written by developers.

For example:

```text
Input

↓

Algorithm

↓

Output
```

Every possible decision must be explicitly programmed.

Now consider a task like:

- summarize a legal contract,
- explain an error log,
- write an email,
- compare two proposals,
- determine whether a support ticket should be escalated.

Writing deterministic algorithms for these tasks is either extremely difficult or practically impossible.

Instead of hardcoding every decision, we provide the problem to an LLM.

```text
Context

↓

LLM

↓

Reasoned Response
```

The LLM replaces thousands of handcrafted rules with learned reasoning.

---

### Why Does It Exist?

An agent continuously encounters situations that cannot be solved using deterministic logic alone.

Examples include:

- interpreting natural language,
- identifying user intent,
- generating code,
- summarizing information,
- choosing among multiple reasonable actions,
- explaining complex concepts,
- making educated judgments under uncertainty.

These are reasoning problems.

The LLM exists because traditional software is poorly suited to solving them.

It provides a general-purpose reasoning capability that can be applied across many domains.

---

### What the LLM Actually Does

Although language models appear to "know everything," their responsibility inside an agent is surprisingly focused.

Given a reasoning environment, they transform information into decisions.

Conceptually:

```text
Goal

+

Context

+

Instructions

↓

LLM

↓

Reasoning

↓

Decision
```

That decision might be:

- a natural language response,
- a structured object,
- a tool request,
- a plan,
- a summary,
- a classification,
- or the next action to execute.

The LLM does not execute the decision.

It proposes it.

---

### The LLM Does Not Work Alone

A useful mental model is to think of the LLM as a specialist working within a larger organization.

```text
Runtime
   │
   ├──────────────┐
   ▼              ▼
Context       Memory
   │              │
   └──────┬───────┘
          ▼
         LLM
          │
          ▼
Structured Decision
          │
          ▼
Runtime Executes
```

Notice how many components surround the LLM.

Without them, the model has:

- no persistent memory,
- no access to external systems,
- no execution capability,
- no awareness of application state,
- no mechanism for retrying failures.

The surrounding architecture gives the LLM useful information and turns its decisions into real-world actions.

---

### The LLM Is Probabilistic

Unlike deterministic software, an LLM does not compute a single correct answer.

Instead, it predicts the most likely continuation based on:

- training,
- current context,
- sampling strategy,
- model parameters.

This has important implications.

The same prompt may produce:

```text
Response A

or

Response B

or

Response C
```

All may be acceptable.

This variability is not a bug.

It is a property of probabilistic reasoning.

Agent systems therefore rely on:

- structured outputs,
- validation,
- guardrails,
- deterministic software,

to ensure reliable execution.

---

### The LLM Is Stateless

Another important characteristic is that the LLM itself remembers nothing.

Every invocation begins with:

```text
Context

↓

Reason

↓

Generate

↓

Forget
```

Everything the model knows during execution comes from the context constructed by the runtime.

This is why Context Engineering and Memory Systems are so important.

---

### Where Does It Fit in the Execution Loop?

The LLM participates during the reasoning phase.

```text
Goal
     │
     ▼
Context Engine
     │
     ▼
LLM
     │
     ▼
Decision
     │
     ▼
Runtime
     │
     ▼
Execute
```

Everything before the LLM prepares information.

Everything after the LLM acts upon its decision.

---

### Relationship to Other Components

The LLM sits at the center of the architecture, but it depends on nearly every surrounding component.

```text
          Goal
            │
            ▼
       Agent Runtime
            │
   ┌────────┼────────┐
   ▼        ▼        ▼
Context   Memory   Planner
   │        │        │
   └────────┴────────┘
            │
            ▼
           LLM
            │
     Structured Output
            │
            ▼
      Tool Execution
```

The LLM is therefore best understood as the system's reasoning engine rather than its controller.

---

### Where Will We Study This in Depth?

The next part of this handbook is devoted almost entirely to understanding Large Language Models.

Topics include:

- transformers,
- tokenization,
- context windows,
- embeddings,
- prompting,
- structured outputs,
- function calling,
- model selection,
- model limitations.

Understanding these topics will make it much easier to build effective agent systems.

---

### The Core Insight

The Large Language Model is the component that gives an agent the ability to reason.

It transforms information into decisions.

It does **not** coordinate execution, store knowledge, interact with external systems, or enforce reliability.

Those responsibilities belong to the surrounding architecture.

The most successful agent systems are not built by using a larger model.

They are built by combining a capable reasoning engine with well-designed runtime, context, memory, planning, and execution components.

## Component 7 — Prompt Builder

If the Context Engine determines **what information** should be available to the language model, the Prompt Builder determines **how that information is presented**.

It transforms raw information into a structured instruction that the language model can understand and reason over effectively.

Although prompts are often associated with simple chat interactions, production agent systems rarely construct prompts manually.

Instead, prompts are assembled dynamically for every execution.

---

### What Problem Does It Solve?

A language model cannot consume arbitrary application objects.

For example, suppose an application has:

- the user's goal,
- customer information,
- retrieved memories,
- search results,
- company policies,
- recent tool outputs.

Internally, these may exist as Python objects, JSON documents, SQL rows, or API responses.

The LLM cannot reason directly over these structures.

Someone must transform them into a coherent reasoning request.

That responsibility belongs to the Prompt Builder.

```text
Application State
        │
        ▼
Prompt Builder
        │
        ▼
Final Prompt
        │
        ▼
Language Model
```

Without this transformation, the model receives fragmented or ambiguous information.

---

### Why Does It Exist?

Imagine asking a colleague for help.

Instead of saying:

> "Can you review yesterday's deployment failure?"

you dump:

- raw log files,
- API responses,
- SQL rows,
- Kubernetes events,
- monitoring metrics,
- configuration files,

onto their desk without explanation.

Even an expert would struggle.

Now imagine presenting the same information like this:

```text
Goal

Investigate yesterday's deployment failure.

Context

Deployment logs:
...

Recent configuration changes:
...

Monitoring alerts:
...

Instructions

Identify the root cause and recommend corrective actions.
```

The information is the same.

The presentation is completely different.

The Prompt Builder performs this organization automatically.

---

### A Prompt Is More Than Instructions

Many developers think a prompt is simply:

```text
You are a helpful assistant.
```

Production prompts are far richer.

A typical prompt may contain:

```text
System Instructions

↓

Current Goal

↓

Conversation History

↓

Retrieved Memory

↓

Relevant Documents

↓

Tool Results

↓

Policies

↓

Formatting Requirements

↓

Output Schema
```

Notice that much of this information originates from other components.

The Prompt Builder assembles these pieces into a coherent reasoning request.

---

### Dynamic Prompt Construction

Modern agent systems almost never use static prompts.

Instead, prompts are generated for every execution.

For example:

```text
Goal:
Investigate support ticket #4821
```

might produce:

```text
System Instructions

+

Customer History

+

Ticket Details

+

Previous Incidents

+

Internal Troubleshooting Guide

+

JSON Output Schema
```

Whereas:

```text
Goal:
Generate weekly sales report
```

might produce:

```text
System Instructions

+

Sales Database Results

+

Financial Policies

+

Formatting Rules

+

Spreadsheet Schema
```

The prompt changes because the task changes.

---

### Prompt Builder vs. Context Engine

These two components are closely related but have different responsibilities.

Think of preparing a presentation.

The Context Engine decides:

> **Which documents should be included?**

The Prompt Builder decides:

> **How should those documents be organized?**

Conceptually:

```text
Memory

Knowledge Base

Tool Results

Conversation

        │

        ▼

Context Engine

(selects information)

        │

        ▼

Prompt Builder

(organizes information)

        │

        ▼

Language Model
```

Selection and presentation are different engineering problems.

Separating them leads to cleaner architectures.

---

### Prompt Builders Often Include Templates

Many production systems use reusable templates.

For example:

```text
Role

Task

Context

Constraints

Expected Output
```

or

```text
System Prompt

User Prompt

Tool Results

Structured Output Schema
```

Templates improve:

- consistency,
- maintainability,
- testing,
- versioning,
- observability.

Changing a template can improve every future reasoning cycle without modifying application logic.

---

### Where Does It Fit in the Execution Loop?

The Prompt Builder operates immediately before model invocation.

```text
Goal
     │
     ▼
Context Engine
     │
     ▼
Prompt Builder
     │
     ▼
Language Model
```

It represents the final transformation before reasoning begins.

---

### Relationship to Other Components

The Prompt Builder acts as the bridge between information and reasoning.

```text
Goal
   │
Memory
   │
Knowledge
   │
Tool Results
   │
Conversation
   │
Policies
   │
───────────────
       │
       ▼
 Context Engine
       │
       ▼
 Prompt Builder
       │
       ▼
       LLM
```

It depends on nearly every upstream component, but its responsibility is singular:

Convert structured application state into a high-quality reasoning request.

---

### Where Will We Study This in Depth?

Prompt construction is explored extensively in later chapters covering:

- prompt engineering,
- prompt templates,
- dynamic prompt generation,
- instruction hierarchy,
- role prompting,
- few-shot prompting,
- prompt versioning,
- prompt evaluation,
- prompt security.

These topics collectively form the discipline of **Prompt Engineering**.

---

### The Core Insight

The language model never interacts directly with your application.

It interacts only with the prompt it receives.

The Prompt Builder is therefore the translator between software and reasoning.

Its job is not to invent new information.

Its job is to present the right information in a structure that enables the language model to reason effectively, consistently, and predictably.

## Component 8 — Structured Output

If the Large Language Model is responsible for reasoning, the Structured Output component is responsible for making that reasoning usable by software.

Humans can understand paragraphs of text.

Applications cannot.

Software requires information in a predictable, machine-readable format.

Structured Outputs bridge the gap between probabilistic reasoning and deterministic execution.

---

### What Problem Does It Solve?

Consider asking an LLM:

> "Extract the customer's name, email, and phone number."

The model might respond with:

```text
The customer's name is John Smith.
You can contact him at john@example.com.
His phone number is +1-555-123-4567.
```

A human immediately understands this.

A computer now has to:

- locate the name,
- identify the email,
- parse the phone number,
- handle unexpected wording,
- recover from formatting changes.

This approach is fragile.

Instead, we ask the model to produce:

```json
{
  "name": "John Smith",
  "email": "john@example.com",
  "phone": "+1-555-123-4567"
}
```

Now deterministic software can immediately consume the result.

---

### Why Does It Exist?

Language models naturally generate language.

Applications naturally process structured data.

These are fundamentally different representations.

The Structured Output component exists to translate probabilistic reasoning into deterministic software interfaces.

Conceptually:

```text
Context

↓

LLM

↓

Reasoning

↓

Structured Output

↓

Application Logic
```

Without structured outputs, every downstream system would need custom parsing logic.

---

### Structured Output Is an Interface Contract

Think of a REST API.

When an API promises:

```json
{
  "user_id": "...",
  "status": "...",
  "created_at": "..."
}
```

every client knows what to expect.

Structured outputs provide the same guarantee for language model responses.

Instead of saying:

> "Return something similar to JSON."

we define an explicit schema.

For example:

```text
Customer

- name: string
- email: string
- phone: string
```

or

```text
Invoice

- invoice_number
- total_amount
- currency
- due_date
```

The runtime can now validate that the response satisfies the expected contract.

---

### Why Parsing Natural Language Is Dangerous

Many early AI applications relied on parsing free-form text.

For example:

```text
Status:
Approved

Priority:
High
```

This works until the model produces:

```text
Everything looks good.

I'd classify this request as high priority.
```

The meaning is correct.

The format has changed.

Software breaks.

Structured outputs eliminate this class of failures by separating **meaning** from **presentation**.

---

### Structured Output Enables Automation

Reliable automation depends on predictable outputs.

Consider an invoice-processing agent.

```text
Invoice PDF

↓

LLM

↓

{
  vendor,
  invoice_number,
  total,
  due_date
}

↓

ERP System
```

No manual interpretation is required.

The runtime validates the schema and immediately continues execution.

This is one of the key reasons modern agents can automate business workflows.

---

### Validation Becomes Possible

Because outputs follow a schema, deterministic software can validate them.

For example:

```text
LLM Output

↓

Schema Validation

↓

Valid?

├── Yes → Continue

└── No → Retry or Repair
```

Validation introduces reliability into an otherwise probabilistic system.

If the model forgets a required field or returns an invalid type, the runtime can detect the problem before any downstream action occurs.

---

### Where Does It Fit in the Execution Loop?

Structured Output sits immediately after reasoning.

```text
Goal
     │
     ▼
Context
     │
     ▼
LLM
     │
     ▼
Structured Output
     │
     ▼
Runtime
     │
     ▼
Execute
```

The runtime rarely works directly with raw language.

Instead, it consumes structured decisions produced by the model.

---

### Relationship to Other Components

Structured Output connects the probabilistic world of the LLM with the deterministic world of software.

```text
Context
     │
     ▼
LLM
     │
     ▼
Structured Output
     │
     ▼
Runtime
     │
     ├────────► Tool Layer
     ├────────► Planner
     ├────────► Memory
     └────────► Application
```

Without this bridge, every downstream component would need to interpret natural language, making the system far less reliable.

---

### Where Will We Study This in Depth?

Structured Outputs are a foundational capability in modern Agent Engineering.

Later chapters will explore:

- JSON schemas,
- type-safe outputs,
- schema validation,
- function calling,
- constrained decoding,
- retry strategies,
- output repair,
- typed application architectures.

Together, these techniques enable deterministic software to work safely with probabilistic models.

---

### The Core Insight

Reasoning alone is not enough.

For software to act on the model's decisions, those decisions must be expressed in a predictable form.

Structured Outputs transform language into data.

They provide the contract that allows deterministic software to trust, validate, and execute the decisions produced by a probabilistic reasoning engine.

Without Structured Outputs, modern production-grade agent systems would be significantly less reliable and far more difficult to automate.

## Component 9 — Tool Layer

Reasoning alone does not change the world.

An agent can reason about sending an email, updating a database, booking a meeting, or restarting a server—but reasoning by itself performs none of those actions.

The Tool Layer gives an agent the ability to interact with the outside world.

It is the bridge between thought and action.

---

### What Problem Does It Solve?

Imagine asking an LLM:

> "What is the current weather in Tokyo?"

The model may provide an estimate based on its training data.

However, it cannot know the actual weather at this moment.

To answer accurately, it must retrieve live information.

Similarly, consider these requests:

- Send an email
- Create a Jira ticket
- Query a PostgreSQL database
- Read a PDF
- Deploy an application
- Search the web
- Generate an invoice
- Update a CRM record

These tasks require interaction with external systems.

The Tool Layer enables those interactions.

```text
Goal

↓

Reason

↓

Need External Action?

↓

Yes

↓

Tool Layer

↓

External System
```

Without tools, an agent remains confined to reasoning over the information already present in its context.

---

### Why Does It Exist?

Large Language Models are not databases.

They are not web browsers.

They are not operating systems.

They cannot directly:

- call REST APIs,
- execute SQL,
- read files,
- control Kubernetes,
- send Slack messages,
- invoke payment gateways,
- access company systems.

These capabilities must be provided explicitly.

The Tool Layer exposes deterministic software capabilities that the LLM can request during execution.

---

### Tools Extend the Agent's Capabilities

Think of the LLM as an expert consultant.

A consultant may recommend:

> "We should check the latest sales numbers."

But the consultant cannot directly access your company's ERP system.

Instead, someone retrieves the information.

The Tool Layer plays that role.

```text
LLM

↓

"Retrieve sales data."

↓

Runtime

↓

Sales API Tool

↓

ERP System

↓

Runtime

↓

LLM
```

The model decides **what information or action is needed**.

The Tool Layer performs the actual work.

---

### A Tool Is Just Software

One of the most important ideas in Agent Engineering is that tools are not magical AI capabilities.

A tool is simply deterministic software with a well-defined interface.

For example:

```text
SearchCustomer(customer_id)

↓

Customer Record
```

or

```text
CreateInvoice(...)

↓

Invoice ID
```

or

```text
SendEmail(...)

↓

Success
```

The implementation might be:

- a Python function,
- a REST API,
- a gRPC service,
- a database query,
- a shell command,
- a cloud service,
- or another agent.

From the runtime's perspective, they are all tools.

---

### The Tool Lifecycle

A typical tool invocation follows a predictable sequence.

```text
Context

↓

LLM

↓

Tool Request

↓

Runtime

↓

Validate Request

↓

Execute Tool

↓

Receive Result

↓

Update Context

↓

LLM Continues Reasoning
```

Notice that the LLM never executes the tool directly.

The runtime always mediates the interaction.

This separation improves security, reliability, and observability.

---

### Tool Results Become New Context

Tool execution rarely ends the reasoning process.

Instead, it provides new information.

For example:

```text
Question

↓

Search Database

↓

Customer Balance

↓

Add Result to Context

↓

LLM Continues Reasoning
```

This creates a reasoning-action-reasoning loop.

```text
Reason

↓

Use Tool

↓

Observe Result

↓

Reason Again
```

Many sophisticated agent behaviors emerge from repeatedly cycling through this loop.

---

### Tools Must Be Deterministic

The language model is probabilistic.

Tools should be deterministic.

For example:

```text
CreateInvoice(123)

↓

Invoice #8491
```

Every execution should behave predictably.

This allows the runtime to:

- retry failures,
- validate results,
- audit execution,
- reproduce workflows,
- recover from errors.

Keeping reasoning and execution separate is one of the key architectural principles of production agent systems.

---

### Where Does It Fit in the Execution Loop?

The Tool Layer participates whenever reasoning requires interaction with the outside world.

```text
Goal
     │
     ▼
Context
     │
     ▼
LLM
     │
Need Tool?
     │
     ├── No → Continue
     │
     └── Yes
           │
           ▼
     Runtime
           │
           ▼
     Tool Layer
           │
           ▼
 External System
           │
           ▼
 Tool Result
           │
           ▼
 Updated Context
           │
           ▼
        LLM
```

This loop may repeat multiple times before the goal is achieved.

---

### Relationship to Other Components

The Tool Layer connects the agent to the external world.

```text
               Runtime
                  │
                  ▼
                 LLM
                  │
          Tool Request
                  │
                  ▼
             Tool Layer
      ┌─────────┼─────────┐
      ▼         ▼         ▼
   Database    APIs    File System
      │         │         │
      └─────────┴─────────┘
                  │
                  ▼
            Tool Results
                  │
                  ▼
            Context Engine
                  │
                  ▼
                 LLM
```

Notice that tool results flow back into the agent's reasoning process.

Execution and reasoning continually reinforce one another.

---

### Where Will We Study This in Depth?

Tool Calling is one of the core disciplines of Agent Engineering.

Later chapters will explore:

- tool schemas,
- function calling,
- tool selection,
- tool chaining,
- error handling,
- retries,
- authentication,
- permission models,
- Model Context Protocol (MCP),
- Agent-to-Agent (A2A) communication.

These topics explain how agents safely and reliably interact with external systems.

---

### The Core Insight

The Large Language Model gives an agent the ability to think.

The Tool Layer gives it the ability to act.

Together, they transform an agent from a conversational system into an operational system capable of interacting with databases, APIs, cloud services, files, enterprise software, and the real world.

Reasoning without tools is intelligence without influence.

Tools turn decisions into outcomes.

## Component 10 — Guardrails

An intelligent agent that can reason, plan, and use tools is powerful.

A powerful system without constraints is dangerous.

Guardrails ensure that an agent behaves within defined boundaries, even when the language model produces unexpected or undesirable outputs.

They are the mechanisms that make agent systems safe, reliable, compliant, and predictable.

---

### What Problem Does It Solve?

Large Language Models are probabilistic.

They can:

- misunderstand instructions,
- hallucinate information,
- produce unsafe outputs,
- misuse tools,
- violate business policies,
- expose sensitive information,
- or take actions that should never be allowed.

For example, suppose a user asks:

> "Delete all customer records."

The LLM may decide that deleting the database satisfies the request.

From a reasoning perspective, that might seem logical.

From a business perspective, it is catastrophic.

Someone must decide:

> **"Is this action actually allowed?"**

That responsibility belongs to Guardrails.

---

### Why Does It Exist?

Reasoning and policy are different concerns.

The language model answers:

> **"What should happen?"**

Guardrails answer:

> **"What is allowed to happen?"**

This distinction is fundamental.

Consider an employee.

An employee may know *how* to approve a $1,000,000 payment.

That does not mean they are authorized to do so.

Authorization is enforced by company policy.

Agent systems require the same separation.

---

### Guardrails Operate at Multiple Stages

Guardrails are not applied only after the model responds.

They can exist throughout the execution lifecycle.

```text
User Input
      │
Input Guardrails
      │
      ▼
Context Construction
      │
Prompt Guardrails
      │
      ▼
LLM
      │
Output Guardrails
      │
      ▼
Tool Request
      │
Execution Guardrails
      │
      ▼
External System
```

Each stage protects against different classes of failures.

---

### Examples of Guardrails

Guardrails can enforce many different types of constraints.

| Type | Example |
|------|---------|
| Security | Prevent access to confidential data |
| Authorization | Ensure only permitted users execute actions |
| Privacy | Remove personally identifiable information |
| Compliance | Enforce regulatory requirements |
| Business Rules | Prevent invalid business operations |
| Output Validation | Ensure responses satisfy expected constraints |
| Tool Restrictions | Block dangerous tool invocations |
| Rate Limits | Prevent excessive usage |
| Cost Controls | Limit expensive operations |

Notice that none of these require reasoning.

They require deterministic enforcement.

---

### Guardrails Are Not Prompts

A common beginner mistake is attempting to enforce policies using instructions alone.

For example:

```text
Please never delete customer records.
```

This is merely guidance.

A stronger approach is:

```text
LLM Requests DeleteCustomer()

↓

Runtime

↓

Permission Check

↓

Denied
```

Even if the model requests the operation, deterministic software prevents it.

Policies should not depend solely on model compliance.

---

### The Runtime Enforces Guardrails

The language model does not enforce its own restrictions.

The runtime does.

```text
LLM

↓

Tool Request

↓

Runtime

↓

Guardrail Check

↓

Allowed?

├── Yes → Execute

└── No → Reject
```

This architecture ensures that critical decisions remain under deterministic control.

---

### Guardrails and Trust

As agents become more autonomous, trust becomes increasingly important.

Imagine an agent that can:

- deploy production code,
- transfer money,
- access customer records,
- modify cloud infrastructure.

Without strong guardrails, organizations would never permit such capabilities.

Guardrails enable autonomy by reducing risk.

They transform:

```text
Power

↓

Controlled Power
```

---

### Where Does It Fit in the Execution Loop?

Unlike planning or reasoning, Guardrails can participate throughout the entire lifecycle.

```text
Receive Goal
      │
Input Validation
      │
Context Validation
      │
Reason
      │
Output Validation
      │
Tool Authorization
      │
Execute
      │
Post-Execution Checks
```

Rather than being a single component, Guardrails form a protective layer around the execution loop.

---

### Relationship to Other Components

Guardrails collaborate closely with nearly every other subsystem.

```text
            Runtime
               │
      ┌────────┼────────┐
      ▼        ▼        ▼
 Context     LLM      Tool Layer
      │        │           │
      └────────┼───────────┘
               ▼
          Guardrails
               │
               ▼
      Safe Execution
```

Their purpose is not to improve reasoning.

Their purpose is to ensure that reasoning leads only to acceptable outcomes.

---

### Where Will We Study This in Depth?

Guardrails are a major production concern.

Later chapters will cover:

- prompt injection defense,
- data leakage prevention,
- permission models,
- policy engines,
- human approval workflows,
- content moderation,
- tool authorization,
- sandboxing,
- compliance,
- security architectures.

Together, these techniques enable agents to operate safely in production environments.

---

### The Core Insight

Intelligence does not guarantee good judgment.

Even excellent reasoning can produce unacceptable actions if appropriate constraints are absent.

Guardrails provide those constraints.

They ensure that an agent remains aligned with business policies, security requirements, legal obligations, and operational boundaries.

Reasoning determines what is possible.

Guardrails determine what is permissible.

## Component 11 — Observability

Imagine deploying an agent into production.

A user reports:

> "The agent made the wrong decision."

Your first questions will likely be:

- What context did it receive?
- Which model was used?
- Which prompt was sent?
- What tools were called?
- Which reasoning path did it follow?
- Why did it fail?
- Can we reproduce the issue?

Without answers to these questions, debugging becomes almost impossible.

Observability exists to make agent behavior understandable.

---

### What Problem Does It Solve?

Traditional software is relatively easy to debug.

If an API returns the wrong value, we can examine:

- application logs,
- stack traces,
- database queries,
- HTTP requests.

Agent systems introduce an additional layer of complexity.

Every execution may involve:

- multiple reasoning cycles,
- dynamic context construction,
- memory retrieval,
- several tool invocations,
- planning,
- retries,
- multiple language model calls.

Understanding what happened requires visibility into the entire execution.

That is the purpose of Observability.

---

### Why Does It Exist?

Imagine an airline.

If a flight disappears from radar, the airline immediately loses visibility into:

- its location,
- its altitude,
- its speed,
- its direction.

The aircraft may still be flying.

The operators simply cannot see it.

Running agents without observability is similar.

The agent continues executing, but engineers cannot determine:

- what it is doing,
- why it made certain decisions,
- whether it is succeeding,
- where failures occurred.

Production systems cannot operate reliably without this visibility.

---

### Observability Is More Than Logging

A common misconception is that observability simply means writing log messages.

Logs are important, but they are only one piece of the picture.

Modern agent systems typically capture:

| Signal | Purpose |
|---------|---------|
| Logs | Human-readable events |
| Traces | Complete execution flow |
| Metrics | Quantitative measurements |
| Events | Significant state changes |
| Token Usage | Cost and model consumption |
| Tool Calls | External interactions |
| Prompt History | Inputs sent to the model |
| Model Responses | Outputs returned by the model |
| Errors | Failures and exceptions |

Together, these signals provide a complete picture of agent behavior.

---

### Execution Traces

One of the most valuable concepts in Agent Engineering is the execution trace.

Instead of recording isolated events, a trace captures the entire journey.

For example:

```text
Goal Received

↓

Retrieve Memory

↓

Build Context

↓

Call LLM

↓

Tool Request

↓

Execute SQL

↓

Update Context

↓

Second LLM Call

↓

Generate Report

↓

Complete
```

This allows engineers to replay the execution and understand exactly how the final outcome was produced.

---

### Observability Answers Important Questions

Consider a production incident.

An engineer may ask:

```text
Why did the agent call this tool?

↓

What information was in context?

↓

Did memory retrieval fail?

↓

Did the model return invalid output?

↓

Did validation reject it?

↓

Did the runtime retry?

↓

Was the final answer generated after the retry?
```

Without observability, these questions become guesswork.

With observability, they become straightforward investigations.

---

### Observability Enables Continuous Improvement

Observability is not only about debugging.

It also helps improve the system over time.

For example, engineers can identify:

- prompts that consistently fail,
- tools with high error rates,
- expensive workflows,
- unnecessary reasoning cycles,
- slow model responses,
- ineffective retrieval strategies.

These insights lead directly to better architectures.

---

### Where Does It Fit in the Execution Loop?

Unlike the LLM or Planner, Observability is not a single execution step.

Instead, it surrounds the entire lifecycle.

```text
Receive Goal
      │
 Record Trace
      │
Build Context
      │
 Record Trace
      │
Reason
      │
 Record Trace
      │
Tool Execution
      │
 Record Trace
      │
Evaluate
      │
 Record Metrics
      │
Complete
```

Every important action generates telemetry.

---

### Relationship to Other Components

Observability interacts with every subsystem.

```text
                Runtime
                   │
      ┌────────────┼────────────┐
      ▼            ▼            ▼
   Context       Memory       Planner
      │            │            │
      └────────────┼────────────┘
                   ▼
                  LLM
                   │
              Tool Layer
                   │
                   ▼
             Observability
                   │
                   ▼
      Logs • Traces • Metrics • Events
```

Rather than influencing execution, Observability records it.

This distinction is important.

Observability explains behavior.

It does not determine behavior.

---

### Where Will We Study This in Depth?

Observability is one of the defining characteristics of production-grade agent systems.

Later chapters will cover:

- distributed tracing,
- execution visualization,
- OpenTelemetry,
- LangSmith,
- metrics,
- logging,
- prompt version tracking,
- token accounting,
- cost monitoring,
- performance analysis,
- debugging production agents.

These techniques make complex agent systems understandable and maintainable.

---

### The Core Insight

As agents become more autonomous, they also become more difficult to debug.

Observability provides the visibility needed to understand, troubleshoot, evaluate, and improve their behavior.

If the Runtime is the operating system of an agent, Observability is its telemetry system.

You cannot reliably operate what you cannot see.

Production Agent Engineering therefore depends as much on observability as it does on reasoning itself.

## Putting It All Together

We have now explored the major architectural components of a modern agent system.

Individually, each component has a clear responsibility.

Collectively, they transform a user's goal into real-world outcomes.

The most important realization at this point is that **intelligence does not reside in any single component**.

It emerges from the interaction between them.

---

### The Complete Architecture

We can now revisit the architecture introduced at the beginning of the chapter.

This time, every component should have a clear meaning.

```text
                                User Goal
                                    │
                                    ▼
                          ┌───────────────────┐
                          │   Agent Runtime   │
                          │  (Orchestrator)   │
                          └─────────┬─────────┘
                                    │
        ┌──────────────┬────────────┼──────────────┬──────────────┐
        ▼              ▼            ▼              ▼              ▼
  Context Engine   Memory System  Planner     Guardrails   Observability
        │              │            │              │              │
        └──────────────┴──────┬─────┴──────────────┴──────────────┘
                               ▼
                        Prompt Builder
                               │
                               ▼
                   Large Language Model
                     (Reasoning Engine)
                               │
                               ▼
                     Structured Output
                               │
                               ▼
                          Agent Runtime
                               │
                               ▼
                           Tool Layer
                               │
         ┌──────────────┬───────────────┬───────────────┐
         ▼              ▼               ▼               ▼
      Database        APIs         File Systems    Other Agents
```

Notice something important.

The Runtime appears **twice**.

That is intentional.

The runtime coordinates the system **before** reasoning and **after** reasoning.

The LLM never operates independently.

It is always surrounded by deterministic software.

---

## A Single Execution Cycle

Let's follow a single request through the entire system.

Suppose a user asks:

> **"Find my three highest-value customers, summarize recent support issues, and draft a follow-up email."**

The execution might proceed like this:

```text
1. Goal Received
        │
        ▼
2. Runtime Starts Execution
        │
        ▼
3. Planner
   Determine required work
        │
        ▼
4. Context Engine
   Retrieve customer history
   Retrieve support tickets
   Retrieve user preferences
        │
        ▼
5. Prompt Builder
   Assemble reasoning request
        │
        ▼
6. LLM
   Decide next actions
        │
        ▼
7. Structured Output
   Request CRM query
        │
        ▼
8. Runtime
   Validate request
        │
        ▼
9. Tool Layer
   Query CRM
        │
        ▼
10. Tool Result
        │
        ▼
11. Context Updated
        │
        ▼
12. LLM Reasons Again
        │
        ▼
13. Draft Email
        │
        ▼
14. Runtime Completes
```

During this entire process:

- Memory may be read and updated.
- Guardrails validate every important action.
- Observability records every step.
- The runtime coordinates the entire lifecycle.

This illustrates an important principle:

> **An agent is not one model invocation. It is an execution loop composed of many coordinated interactions.**

---

## The Responsibilities at a Glance

One useful way to remember the architecture is to associate each component with its primary responsibility.

| Component | Responsibility |
|-----------|----------------|
| Goal | Defines the desired outcome |
| Agent Runtime | Coordinates execution |
| Context Engine | Selects relevant information |
| Memory System | Preserves knowledge across time |
| Planner | Breaks goals into executable work |
| Prompt Builder | Organizes information for reasoning |
| Large Language Model | Performs probabilistic reasoning |
| Structured Output | Produces machine-readable decisions |
| Tool Layer | Interacts with external systems |
| Guardrails | Enforces safety and policy |
| Observability | Makes execution visible |

Notice that no two components have the same responsibility.

This separation is what makes agent systems modular, testable, and maintainable.

---

## A Useful Mental Model

Throughout this handbook, it will help to think of an agent system as an organization rather than a single intelligent entity.

```text
Goal
   │
   ▼
Project Manager
(Runtime)

   │

───────────────

Planner
Creates the work plan

Memory
Provides historical knowledge

Context Engine
Collects relevant information

Prompt Builder
Prepares the briefing

───────────────

Specialist
(LLM)

───────────────

Structured Output
Documents the decision

───────────────

Operations Team
(Tool Layer)

───────────────

Compliance
(Guardrails)

───────────────

Audit Team
(Observability)
```

No single department runs the organization.

Success comes from collaboration.

Modern agent systems follow the same architectural principle.

---

## Frameworks Hide These Components

One final observation is worth emphasizing.

When using frameworks such as LangGraph, OpenAI Agents SDK, CrewAI, AutoGen, Semantic Kernel, or PydanticAI, many of these components are hidden behind convenient abstractions.

For example:

- the runtime may be represented as a graph or workflow,
- prompt construction may happen automatically,
- tool execution may require only decorators,
- memory may be configured with a few lines of code,
- observability may be integrated by default.

These abstractions improve developer productivity.

However, they do not eliminate the underlying architecture.

Frameworks simplify implementation.

They do not change the responsibilities of the components.

Understanding the architecture first makes it much easier to evaluate frameworks, debug complex systems, and build custom runtimes when needed.

---

## The Core Insight

Modern agent systems are **hybrid software systems**.

Some components are deterministic.

Others are probabilistic.

Each solves a different engineering problem.

When combined, they create systems capable of:

- reasoning,
- planning,
- remembering,
- interacting with external systems,
- adapting to new information,
- and reliably pursuing goals.

The architecture is therefore far more important than the language model alone.

The model provides intelligence.

The surrounding system turns that intelligence into dependable software.

## Key Takeaways

This chapter introduced the architectural anatomy of a modern agent system.

Rather than studying implementation details, our objective was to understand the purpose of each major component and how they collaborate to transform a goal into real-world outcomes.

At this point, you should no longer think of an agent as "an LLM with some tools."

Instead, you should recognize it as a coordinated software system composed of specialized components, each solving a distinct engineering problem.

---

### The Eleven Core Components

Every modern agent system can be understood through these eleven architectural building blocks.

| Component | Primary Responsibility |
|-----------|------------------------|
| Goal | Defines the desired outcome |
| Agent Runtime | Coordinates execution |
| Context Engine | Selects relevant information |
| Memory System | Preserves information across time |
| Planner | Converts goals into executable work |
| Prompt Builder | Organizes information for reasoning |
| Large Language Model | Performs probabilistic reasoning |
| Structured Output | Produces machine-readable decisions |
| Tool Layer | Interacts with external systems |
| Guardrails | Enforces safety and policy |
| Observability | Makes execution understandable |

Each component has a single responsibility.

Together, they create intelligent behavior.

---

### The Agent Execution Loop

Although implementations differ, nearly every production agent repeatedly executes the same high-level loop.

```text
Goal
  │
  ▼
Runtime
  │
  ▼
Retrieve Context
  │
  ▼
Retrieve Memory
  │
  ▼
Build Prompt
  │
  ▼
Reason (LLM)
  │
  ▼
Structured Decision
  │
  ▼
Need Tool?
  │
 ├── No → Complete
 │
 └── Yes
       │
       ▼
 Execute Tool
       │
       ▼
 Observe Result
       │
       ▼
 Update Context
       │
       ▼
 Repeat
```

Everything we build throughout the remainder of this handbook will fit somewhere within this execution loop.

---

### The Most Important Lessons

There are several ideas from this chapter that are worth remembering.

**An LLM is not an agent.**

It is the reasoning engine inside an agent.

---

**Context is the model's working world.**

The quality of reasoning depends heavily on the quality of the context provided.

---

**Memory and context are different.**

Memory stores information.

Context selects information.

---

**The runtime is the operating system of the agent.**

It coordinates every stage of execution.

---

**Tools allow the agent to affect the real world.**

Reasoning without execution cannot accomplish work.

---

**Guardrails and observability are production requirements.**

They are not optional additions.

They make autonomous systems trustworthy, secure, and maintainable.

---

### A Mental Checklist

Whenever you encounter a new agent framework, architecture, or research paper, ask yourself these questions:

- Where does the goal enter the system?
- What coordinates execution?
- How is context constructed?
- Where is memory stored?
- How does planning work?
- How is the prompt assembled?
- Which model performs reasoning?
- How are outputs validated?
- How are tools executed?
- What enforces safety?
- How is execution observed?

If you can answer these eleven questions, you understand the architecture, regardless of which framework or programming language is being used.

---

### Looking Ahead

This concludes the **Foundations** portion of our architectural overview.

In the next chapter, we will shift from static architecture to dynamic behavior.

Instead of asking:

> **"What components make up an agent?"**

we will ask:

> **"What actually happens from the moment a goal is received until the agent finishes its work?"**

That journey introduces the **Agent Lifecycle**, where we will examine the complete execution flow of an agent—from initialization, through reasoning and action, to evaluation and completion.

Understanding the lifecycle will provide the missing link between the architectural components introduced in this chapter and the production systems you will build later in this handbook.