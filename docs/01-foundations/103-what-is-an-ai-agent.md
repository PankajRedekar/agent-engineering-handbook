# Chapter 103 — What Is an AI Agent

> **Domain:** Foundations  
>
> **Estimated Reading Time:** 30–35 minutes  
>
> **Prerequisites:** Chapter 102 — The Evolution of Software  
>
> **Difficulty:** Beginner to Intermediate  
>
> **Last Updated:** 2026-07-27

---

## Why Definitions Matter

Engineering begins with precise definitions.

Before designing databases, engineers define entities and relationships.

Before building distributed systems, they distinguish between processes, services, and nodes.

Before designing APIs, they define contracts.

The same principle applies to Agent Engineering.

If engineers use the term *AI agent* to describe every application that incorporates a language model, the term loses its value. Architectural discussions become ambiguous, system requirements become unclear, and different teams may build fundamentally different systems while believing they are building the same thing.

Consider the following statements:

- "We should build an AI agent for customer support."
- "Our product already has an AI agent."
- "Let's migrate this workflow to an agent."

Without a shared definition, each statement can be interpreted in multiple ways.

One engineer might envision a chatbot that answers questions.

Another might imagine a workflow that invokes an LLM.

A third might expect a system capable of independently planning, using tools, maintaining memory, and completing complex tasks.

Although all three systems involve artificial intelligence, they differ significantly in architecture, capabilities, operational complexity, and engineering requirements.

Precise terminology allows engineering teams to communicate effectively.

It helps answer important architectural questions such as:

- Does this system pursue a goal or simply respond to requests?
- Does it make decisions, or does it execute predefined logic?
- Can it adapt when new information becomes available?
- Does it decide which actions to take, or are those actions hard-coded?
- Does it operate autonomously, or is every step directed by a user?

These questions determine not only whether a system should be considered an agent but also how it should be designed, tested, monitored, and governed.

This distinction becomes increasingly important in production environments.

An AI-powered application that generates text has very different reliability, security, and operational requirements than a system capable of autonomously executing financial transactions, modifying infrastructure, or interacting with external services.

Using the same label for both systems obscures these critical differences.

Throughout this handbook, we will use terminology that emphasizes architectural capabilities rather than marketing language or framework-specific definitions.

Our goal is not to determine whether a vendor's product qualifies as an agent.

Our goal is to understand the engineering characteristics that distinguish different classes of intelligent software systems.

With that foundation established, we can now examine the spectrum of software systems and identify where agents fit within the broader evolution of software.


## The Spectrum of Software Systems

Before defining an AI agent, it is useful to place it within the broader landscape of software systems.

Software has evolved incrementally over decades. Each generation introduced new capabilities while retaining many characteristics of the previous one. As a result, there is no sharp boundary where traditional software suddenly becomes an agent. Instead, software exists along a spectrum of increasing capability.

Understanding this progression is essential because many systems that are marketed as *AI agents* are, from an engineering perspective, better described as workflows, AI-powered applications, or assistants.

The following spectrum provides a conceptual model that we will use throughout this handbook.

```text
Traditional Program
        │
        ▼
Workflow
        │
        ▼
AI-Powered Application
        │
        ▼
Assistant
        │
        ▼
AI Agent
        │
        ▼
Multi-Agent System
```

Each category builds upon the capabilities of the one before it.

For example:

- A workflow is still software, but it introduces orchestration.
- An AI-powered application combines deterministic software with probabilistic reasoning.
- An assistant adds conversational interaction and contextual awareness.
- An AI agent introduces goal-directed behavior, planning, and adaptive execution.
- A multi-agent system coordinates multiple agents working together toward shared or complementary objectives.

The progression is not defined by the technologies used.

A workflow may invoke a language model.

An assistant may use dozens of external tools.

An agent may perform only a small number of actions.

What distinguishes these systems is **how they make decisions, pursue objectives, and interact with their environment**, not whether they use a particular framework or model.

It is also important to recognize that these categories are not mutually exclusive.

A production system may contain several of them simultaneously.

For example, an enterprise application might include:

- Deterministic business logic for authentication and billing.
- Workflow automation for approval processes.
- An AI-powered document summarizer.
- A customer support assistant.
- An autonomous procurement agent.

Each component serves a different purpose and operates with different architectural assumptions.

Rather than asking, *"Does this application use AI?"*, Agent Engineers ask more precise questions:

- Is the system reactive or goal-directed?
- Who determines the sequence of actions—the developer or the system?
- Can the system adapt when new information becomes available?
- Does it execute predefined logic, or does it decide what to do next?
- Is intelligence confined to a single response, or does it drive an ongoing process?

These questions allow us to classify software based on its capabilities rather than its marketing description.

In the following sections, we will examine each category in detail, beginning with the simplest form of software: the traditional program.

## Workflow

As software systems became more sophisticated, engineers encountered a new challenge.

Many business processes could not be expressed as a single algorithm.

Instead, they consisted of multiple independent steps that had to be executed in a specific sequence.

For example:

- Approving an expense claim
- Processing an insurance application
- Deploying software to production
- Onboarding a new employee
- Generating a monthly financial report

Each of these processes involves multiple actions, conditional decisions, and interactions with different systems.

Rather than embedding all of this logic into one large program, engineers introduced **workflows**.

A workflow defines **how a series of predefined tasks should be executed**.

Unlike a traditional program, which focuses on computation, a workflow focuses on orchestration.

---

### Characteristics

Workflows introduce coordination while remaining fundamentally deterministic.

Their defining characteristics include:

- Predefined sequence of tasks
- Explicit decision points
- Deterministic execution
- Process orchestration
- Integration with multiple systems
- Automation of repetitive business processes

Every possible execution path is designed by developers before the workflow is deployed.

The workflow engine determines **when** each step should execute, but it never decides **what** the steps should be.

---

### Typical Architecture

A workflow coordinates a sequence of predefined activities.

```text
              Start
                │
                ▼
      +-------------------+
      | Step 1            |
      +---------+---------+
                │
                ▼
      +-------------------+
      | Decision Rule     |
      +----+---------+----+
           │         │
         Yes         No
           │         │
           ▼         ▼
 +----------------+ +----------------+
 | Step 2         | | Step 3         |
 +--------+-------+ +--------+-------+
          │                  │
          +--------+---------+
                   │
                   ▼
                Complete
```

Although different paths may be taken, every path is explicitly defined before execution begins.

---

### Examples

Workflow systems are common across nearly every industry.

Examples include:

- CI/CD pipelines that build, test, and deploy software
- ETL pipelines that extract, transform, and load data
- Invoice approval workflows
- Customer onboarding processes
- Order fulfillment systems
- Document review and approval pipelines

Modern workflow engines include technologies such as Apache Airflow, Temporal, Camunda, GitHub Actions, and many enterprise Business Process Management (BPM) platforms.

Despite their sophistication, these systems execute predefined logic rather than making independent decisions.

---

### Engineering Perspective

When designing workflows, engineers focus on process reliability rather than computational correctness alone.

Important considerations include:

- Is every business scenario represented?
- Are dependencies between tasks correctly modeled?
- Can failed steps be retried safely?
- Is the workflow idempotent?
- Can long-running processes recover after interruptions?
- Is the execution observable and auditable?

Because workflows often span multiple systems, concerns such as retries, compensation, timeouts, and fault tolerance become central to their design.

---

### Limitations

Although workflows automate complex business processes, they remain fundamentally developer-driven.

The workflow cannot decide to introduce a new step.

It cannot determine that additional information is required.

It cannot choose an alternative strategy when an unfamiliar situation arises unless that strategy has already been encoded by the developer.

For example, consider a customer support workflow.

A traditional workflow might be defined as:

```text
Receive Ticket
      │
      ▼
Classify Issue
      │
      ▼
Assign to Team
      │
      ▼
Notify Customer
```

This process works well when every possible situation has been anticipated.

However, real-world requests are often ambiguous.

A customer may describe multiple unrelated issues in a single message.

A document may require interpretation before routing.

The appropriate next step may depend on context that cannot be expressed using simple decision rules.

At this point, deterministic workflows reach their practical limits.

Instead of executing predefined logic alone, software must begin interpreting information and generating responses.

This capability marks the transition from workflows to **AI-powered applications**.

## AI-Powered Application

The introduction of large language models fundamentally expanded what software could accomplish.

For decades, software excelled at processing structured data and executing deterministic rules. However, tasks involving natural language, images, documents, and human reasoning remained difficult to automate because they could not be expressed as explicit algorithms.

Large language models changed this landscape.

Rather than requiring developers to encode every rule manually, software could now delegate certain tasks—such as interpretation, summarization, classification, and content generation—to probabilistic models.

This gave rise to a new class of software: the **AI-powered application**.

An AI-powered application combines traditional software engineering with artificial intelligence.

The application continues to manage business logic, security, workflows, and data, while AI models provide capabilities that were previously unavailable through deterministic programming alone.

---

### Characteristics

AI-powered applications combine deterministic software with probabilistic reasoning.

Their defining characteristics include:

- Traditional application architecture
- Integration with one or more AI models
- Natural language understanding
- Content generation
- Semantic interpretation
- Request-driven interaction
- Deterministic orchestration around probabilistic components

Unlike traditional programs and workflows, AI-powered applications can process information that does not follow predefined rules.

However, the application itself remains responsible for controlling the overall execution flow.

---

### Typical Architecture

An AI-powered application extends a traditional application by incorporating AI as one of its components.

```text
                 User
                   │
                   ▼
        +----------------------+
        | Application Logic    |
        +----------+-----------+
                   │
      +------------+------------+
      │                         │
      ▼                         ▼
+-------------+         +----------------+
| Business    |         | Language Model |
| Logic       |         +----------------+
+------+------+                  │
       │                         │
       +------------+------------+
                    │
                    ▼
             Application Response
```

The language model is not the application.

It is a service that the application invokes to perform tasks requiring probabilistic reasoning.

The application remains responsible for authentication, authorization, business rules, persistence, auditing, and overall system behavior.

---

### Examples

AI-powered applications are now common across many industries.

Examples include:

- A document summarization platform
- A customer support chatbot
- A coding assistant embedded in an IDE
- An enterprise search application using semantic retrieval
- An email drafting assistant
- A meeting transcription and summarization service
- An invoice data extraction system

Each of these systems uses AI to enhance a specific capability while remaining fundamentally application-driven.

---

### Engineering Perspective

Building AI-powered applications introduces engineering concerns that are fundamentally different from those of traditional software.

Engineers must now consider questions such as:

- Which model is appropriate for this task?
- How should prompts be designed?
- How much context should be provided?
- How should hallucinations be mitigated?
- How can responses be evaluated?
- What are the latency and cost implications of model inference?
- How should sensitive information be protected?

These concerns are added to—not substituted for—the traditional responsibilities of software engineering.

Developers must still design secure APIs, manage databases, enforce business rules, and build reliable distributed systems.

AI becomes another architectural component within the application rather than replacing the application itself.

---

### Limitations

Despite their impressive capabilities, AI-powered applications remain fundamentally **request-driven**.

Their execution model is typically straightforward:

```text
User Request
      │
      ▼
Application
      │
      ▼
Language Model
      │
      ▼
Response
```

The application receives a request, invokes a model, returns a response, and the interaction ends.

The system does not independently determine what should happen next.

It does not establish objectives.

It does not create plans.

It does not decide whether additional information is required.

It does not choose to execute external actions unless explicitly instructed by the surrounding application.

For example, consider an AI-powered travel application.

A user might ask:

> "Plan a three-day trip to Kyoto."

The application generates an itinerary and presents it to the user.

Once the response is delivered, its responsibility ends.

It does not proceed to compare hotel prices, check weather forecasts, reserve transportation, or adapt the itinerary as conditions change unless developers have explicitly programmed those actions.

The intelligence exists within a single interaction rather than across an ongoing process.

To move beyond responding to requests, software must gain the ability to pursue objectives, make decisions across multiple steps, and adapt its behavior based on new information.

This leads to the next stage in the evolution of intelligent software: the **assistant**.

## Assistant

As AI-powered applications became more capable, users expected more than single-question interactions.

Instead of asking isolated questions, they wanted software that could maintain a conversation, remember context, assist with ongoing tasks, and interact naturally over extended sessions.

This led to the emergence of **AI assistants**.

An assistant is an AI-powered application designed to help users accomplish tasks through conversational interaction.

Unlike a traditional AI-powered application, an assistant maintains conversational context and often integrates with external tools to perform user-requested actions.

However, despite these additional capabilities, an assistant remains fundamentally **user-driven**.

It exists to assist the user rather than independently pursue objectives.

---

### Characteristics

Assistants build upon AI-powered applications by adding conversational and contextual capabilities.

Their defining characteristics include:

- Conversational interaction
- Multi-turn context
- Natural language understanding
- Tool integration
- User-requested actions
- Context awareness
- Reactive behavior

An assistant may remember previous messages within a conversation, retrieve information from external systems, and invoke tools when requested.

Nevertheless, every significant action originates from the user's instructions.

The assistant does not establish its own objectives.

---

### Typical Architecture

An assistant extends an AI-powered application with conversational memory and optional tool usage.

```text
                 User
                   │
                   ▼
        +-----------------------+
        | Assistant Runtime     |
        +-----------+-----------+
                    │
      +-------------+--------------+
      │             │              │
      ▼             ▼              ▼
+------------+ +-----------+ +-------------+
| Conversation| | Language | | Tool Layer  |
| Memory      | | Model    | | (Optional)  |
+------------+ +-----------+ +-------------+
      │             │              │
      +-------------+--------------+
                    │
                    ▼
              Assistant Response
```

The assistant coordinates conversation history, AI reasoning, and tool execution to produce responses that are relevant to the current interaction.

---

### Examples

Assistants are now common in both consumer and enterprise software.

Examples include:

- A coding assistant helping developers write code
- An email assistant drafting responses
- A customer support assistant answering product questions
- A virtual meeting assistant summarizing discussions
- A personal productivity assistant managing calendars and reminders

Although these assistants may appear highly capable, they generally operate within the boundaries established by the user's requests.

---

### Engineering Perspective

Designing assistants introduces challenges beyond those of AI-powered applications.

Engineers must consider:

- Conversation state management
- Context window optimization
- Memory retention strategies
- Tool integration
- User experience
- Permission management
- Response consistency
- Cost and latency optimization

A significant portion of assistant engineering focuses on ensuring that the system remains helpful, predictable, and responsive throughout an extended conversation.

Unlike traditional chatbots, modern assistants often combine retrieval systems, tool calling, and conversation memory to provide richer interactions.

---

### Limitations

Despite their conversational capabilities, assistants remain fundamentally **reactive**.

They wait for instructions.

They answer questions.

They execute requested actions.

Once the requested task has been completed, they stop.

Consider a coding assistant.

A developer might ask:

> "Review this pull request and suggest improvements."

The assistant analyzes the code and produces recommendations.

It does not subsequently decide to:

- Monitor future pull requests.
- Check whether the recommendations were implemented.
- Open issues for unresolved problems.
- Coordinate reviews with other engineers.

Similarly, an email assistant may draft a reply but does not decide to follow up next week if no response is received.

In every case, the assistant's behavior is initiated and bounded by user requests.

The assistant helps achieve the user's immediate objective, but it does not adopt that objective as its own.

This distinction is subtle but fundamental.

The transition from **assistant** to **agent** occurs when software no longer waits for continuous instructions but instead accepts a goal, determines the necessary actions, adapts to changing conditions, and continues working until the goal—or an explicit stopping condition—is reached.

That capability defines the next category in the spectrum: the **AI agent**.

## AI Agent

The term *AI agent* is often used to describe any application that incorporates a language model.

From an engineering perspective, however, this definition is too broad.

A chatbot is not necessarily an agent.

A workflow that invokes an LLM is not necessarily an agent.

Even an assistant capable of using tools may not qualify as an agent.

The defining characteristic of an AI agent is not the presence of artificial intelligence.

It is the ability to **autonomously pursue a goal**.

Unlike previous categories of software, an agent is not limited to producing a single response or executing a predefined sequence of actions.

Instead, it repeatedly evaluates the current situation, decides what should happen next, performs an action, observes the outcome, and continues until its objective has been achieved or an explicit stopping condition is reached.

This shift—from responding to requests to pursuing goals—is what fundamentally distinguishes an AI agent from other intelligent software systems.

---

### A Practical Engineering Definition

Throughout this handbook, we will use the following definition.

> **An AI agent is a software system that uses probabilistic reasoning together with memory, planning, and tool execution to autonomously pursue a goal by repeatedly observing, deciding, and acting until a stopping condition is reached.**

Every part of this definition is important.

An agent is:

- **A software system**, not merely a language model.
- **Goal-oriented**, rather than request-oriented.
- **Capable of reasoning** about its current situation.
- **Able to interact with external systems** through tools.
- **Adaptive**, modifying its behavior as new information becomes available.
- **Iterative**, operating through repeated cycles rather than a single model invocation.

This definition is intentionally framework-neutral.

Whether an agent is implemented using LangGraph, OpenAI Agents SDK, CrewAI, AutoGen, Semantic Kernel, or a custom runtime is an implementation detail.

The architectural characteristics remain the same.

---

### Characteristics

An AI agent introduces capabilities that are absent—or only partially present—in previous categories of software.

These include:

- Goal-directed execution
- Autonomous decision-making
- Multi-step reasoning
- Dynamic planning
- Memory
- Context management
- Tool usage
- Observation of outcomes
- Adaptive behavior
- Explicit completion criteria

These capabilities enable an agent to solve problems that cannot be completed through a single interaction or predefined workflow.

---

### Typical Architecture

A simplified agent architecture can be represented as follows.

```text
                  Goal
                    │
                    ▼
          +--------------------+
          | Agent Runtime      |
          +---------+----------+
                    │
     +--------------+---------------+
     │              │               │
     ▼              ▼               ▼
+----------+   +-----------+   +------------+
| Planner  |   | Memory    |   | Context    |
+----------+   +-----------+   +------------+
       │              │               │
       +--------------+---------------+
                      │
                      ▼
               +-------------+
               | Language    |
               | Model       |
               +------+------+ 
                      │
                      ▼
               +-------------+
               | Tool Layer  |
               +------+------+ 
                      │
                      ▼
            External Systems
```

Unlike an assistant, the language model is only one component of the overall system.

The agent runtime coordinates planning, memory, reasoning, context management, and tool execution to achieve the desired objective.

---

### Examples

Examples of AI agents include:

- A procurement agent that compares suppliers, negotiates within defined limits, and submits purchase requests.
- A security agent that investigates suspicious activity, gathers evidence, and recommends remediation.
- A travel planning agent that books transportation and accommodation while adapting to changing availability.
- A DevOps agent that diagnoses deployment failures, gathers logs, identifies likely causes, and proposes corrective actions.
- A financial operations agent that reconciles invoices across multiple systems and escalates exceptions requiring human review.

Each of these systems performs more than information retrieval or response generation.

They actively work toward completing an objective.

---

### Engineering Perspective

Designing an AI agent requires engineers to think beyond individual model invocations.

Instead of asking:

> "How do I generate the best response?"

Engineers ask questions such as:

- How should the agent determine its next action?
- When should it use a tool instead of reasoning internally?
- What information should it remember?
- How should it recover from failure?
- When should it ask for human approval?
- How should progress toward the goal be measured?
- When should execution stop?

These are architectural questions rather than prompt engineering questions.

They require designing an execution system rather than simply integrating an AI model.

---

### A Fundamental Shift

The progression from assistant to agent represents a change in responsibility.

An assistant is responsible for producing helpful responses.

An agent is responsible for achieving outcomes.

Consider the difference.

An assistant receives a request such as:

> "Summarize these incident logs."

It produces a summary and waits for the next instruction.

An agent may receive a broader objective:

> "Investigate why yesterday's deployment failed."

To achieve this goal, it might:

1. Retrieve deployment logs.
2. Analyze error messages.
3. Compare the deployment with previous successful releases.
4. Query monitoring systems.
5. Identify the most likely root cause.
6. Generate a report.
7. Recommend corrective actions.
8. Request human approval before making changes.

The user specifies **what** needs to be achieved.

The agent determines **how** to achieve it.

This distinction marks the transition from intelligent assistance to autonomous execution.

In the next section, we will extend this concept further by examining what happens when multiple agents collaborate to solve problems that exceed the capabilities of a single agent.

## Multi-Agent System

As objectives become more complex, a single agent may no longer possess all the knowledge, capabilities, or resources required to complete them efficiently.

Just as modern software systems are decomposed into multiple services with specialized responsibilities, agent systems can also be decomposed into multiple specialized agents.

This gives rise to the **multi-agent system**.

A multi-agent system consists of multiple autonomous agents that collaborate, coordinate, or delegate work to achieve objectives that would be difficult or inefficient for a single agent to accomplish alone.

Rather than creating one agent capable of doing everything, engineers design a collection of agents, each responsible for a well-defined role.

---

### Characteristics

Multi-agent systems extend the capabilities of individual agents through collaboration.

Their defining characteristics include:

- Multiple autonomous agents
- Specialized responsibilities
- Goal decomposition
- Inter-agent communication
- Task delegation
- Coordinated execution
- Shared or distributed memory
- Collective decision-making

Each agent remains independently capable of reasoning, planning, and acting.

The system's overall intelligence emerges from the interaction between these specialized agents rather than from a single centralized decision-maker.

---

### Typical Architecture

A simplified multi-agent architecture may resemble the following.

```text
                    User Goal
                        │
                        ▼
               +------------------+
               | Coordinator      |
               | Agent            |
               +----+------+------+ 
                    │
     +--------------+---------------+
     │              │               │
     ▼              ▼               ▼
+-----------+ +------------+ +--------------+
| Research  | | Planning   | | Execution    |
| Agent     | | Agent      | | Agent        |
+-----------+ +------------+ +--------------+
      │              │               │
      +--------------+---------------+
                     │
                     ▼
             External Systems
```

In some architectures, a coordinator agent assigns work to specialized agents.

In others, agents communicate directly, negotiate responsibilities, or collaborate without centralized control.

The coordination strategy depends on the requirements of the system rather than on a universally correct architectural pattern.

---

### Examples

Examples of multi-agent systems include:

- A software engineering platform where one agent analyzes requirements, another generates code, another performs testing, and another reviews security implications.
- A financial analysis system with separate agents responsible for market research, risk assessment, portfolio optimization, and compliance verification.
- A customer support platform where specialized agents handle billing, technical issues, account management, and escalation.
- A research system in which multiple agents independently investigate different sources before combining their findings into a consolidated report.

In each case, specialization allows the system to divide complex work into manageable components.

---

### Engineering Perspective

Designing multi-agent systems introduces architectural challenges beyond those encountered when building a single agent.

Engineers must address questions such as:

- How should responsibilities be divided?
- Which agent owns the overall objective?
- How do agents communicate?
- How is shared context maintained?
- How are conflicting decisions resolved?
- What happens if one agent fails?
- How can execution be observed across multiple agents?

These challenges closely resemble those found in distributed systems.

Instead of coordinating services, engineers coordinate autonomous reasoning systems.

Concepts such as communication protocols, fault tolerance, consistency, and observability become just as important in multi-agent architectures as they are in distributed software systems.

---

### Trade-offs

Although multi-agent systems are powerful, they are not inherently superior to single-agent systems.

Introducing additional agents increases architectural complexity.

Benefits include:

- Separation of responsibilities
- Improved modularity
- Greater specialization
- Better scalability for complex objectives

However, these benefits come with costs:

- Increased communication overhead
- More complex debugging
- Higher operational costs
- Greater coordination complexity
- Additional latency
- More difficult evaluation

Many production systems are better served by a well-designed single agent than by a poorly designed collection of multiple agents.

Engineers should adopt multi-agent architectures only when specialization or parallel collaboration provides a clear architectural advantage.

---

### From Capability Spectrum to Engineering Discipline

We have now examined the spectrum of intelligent software systems.

```text
Traditional Program
        │
        ▼
Workflow
        │
        ▼
AI-Powered Application
        │
        ▼
Assistant
        │
        ▼
AI Agent
        │
        ▼
Multi-Agent System
```

Each category builds upon the capabilities of the previous one while introducing new architectural challenges.

The transition from one category to the next is not determined by the presence of artificial intelligence alone.

Instead, it is defined by increasing levels of autonomy, reasoning, planning, adaptation, and coordination.

Understanding this spectrum provides the conceptual foundation for Agent Engineering.

Before designing agent architectures, engineers must first recognize which class of system they are building and the engineering principles that govern it.

In the next section, we will distill this progression into a precise set of characteristics that define what makes an AI agent an agent.

## Characteristics of an AI Agent

Throughout this chapter, we have examined a spectrum of software systems, each introducing progressively more sophisticated capabilities.

At this point, we can identify the characteristics that distinguish an AI agent from every other category.

No single capability defines an agent in isolation.

For example:

- Using a language model does not make a system an agent.
- Calling external tools does not make a system an agent.
- Maintaining conversation history does not make a system an agent.

Instead, an agent emerges from the combination of several architectural capabilities working together toward a common objective.

The following characteristics form the foundation of modern agent systems.

| Characteristic | Description | Required? |
|---------------|-------------|:---------:|
| **Goal-Oriented** | Works toward achieving an objective rather than simply responding to requests. | ✓ |
| **Reasoning** | Evaluates available information to determine the next action. | ✓ |
| **Observation** | Collects information from its environment before making decisions. | ✓ |
| **Action** | Performs operations that change or interact with external systems. | ✓ |
| **Iteration** | Repeats the observe → decide → act cycle until completion. | ✓ |
| **Planning** | Breaks complex objectives into manageable tasks. | Usually |
| **Memory** | Retains information across multiple reasoning steps or sessions. | Usually |
| **Tool Usage** | Interacts with external systems through well-defined interfaces. | Usually |
| **Adaptation** | Modifies behavior when new information becomes available. | ✓ |
| **Stopping Condition** | Determines when the objective has been completed or should terminate. | ✓ |

Several observations are worth emphasizing.

First, not every agent requires every capability.

A simple troubleshooting agent may perform limited planning.

An autonomous research agent may maintain extensive long-term memory.

An embedded device agent may use no external tools at all.

The implementation depends on the problem being solved.

Second, these capabilities are architectural rather than model-specific.

Replacing one language model with another does not fundamentally change whether a system is an agent.

Similarly, changing frameworks—from LangGraph to OpenAI Agents SDK or a custom runtime—does not alter these characteristics.

The architecture determines the nature of the system.

Finally, these capabilities reinforce an important principle introduced earlier in this chapter.

An agent is not defined by the technologies it uses.

It is defined by the behavior it exhibits.

If a system can accept an objective, determine appropriate actions, adapt as new information becomes available, and continue working until that objective has been achieved, it exhibits the defining characteristics of an AI agent.

These characteristics provide a practical checklist for evaluating both existing systems and new designs.

Rather than asking,

> "Does this application use an LLM?"

Agent Engineers ask,

> "Does this system exhibit the architectural characteristics of an agent?"

That shift in perspective is essential for designing systems based on engineering principles rather than marketing terminology.

## Common Misconceptions

As the popularity of AI has grown, the term *AI agent* has been applied to an increasingly broad range of systems.

Many of these uses are driven by marketing rather than engineering precision.

As a result, several misconceptions have become widespread.

Understanding these misconceptions is important because they influence architectural decisions, project planning, and stakeholder expectations.

---

### Misconception 1: Every LLM Is an Agent

This is perhaps the most common misconception.

A large language model is **not** an agent.

A language model is a probabilistic reasoning component that predicts the next token based on the context it receives.

It has no inherent concept of:

- Goals
- Planning
- Memory
- Tool execution
- Long-running tasks
- Autonomous decision-making

An LLM becomes part of an agent only when it is integrated into a software system that provides these capabilities.

The relationship is similar to that between a database and an application.

A database stores data.

An application uses that database to achieve business objectives.

Likewise, an agent uses an LLM as one of its components.

---

### Misconception 2: Every Chatbot Is an Agent

Many conversational systems are marketed as AI agents simply because they use a language model.

Most chatbots are better described as assistants.

They answer questions.

They generate responses.

They may even remember recent conversation history.

However, they generally do not:

- Pursue goals independently
- Create plans
- Execute multi-step workflows
- Continue working after responding

A conversational interface alone does not make a system an agent.

---

### Misconception 3: Tool Calling Creates an Agent

Modern language models can invoke external tools.

While tool usage is an important capability, it is not sufficient to qualify a system as an agent.

Consider two systems.

The first receives a user's request, calls a weather API, and returns the result.

The second receives the objective:

> "Plan the safest hiking trip this weekend."

It gathers weather forecasts, compares trail conditions, checks transportation options, evaluates alternatives, and recommends the best itinerary.

Both systems use tools.

Only the second demonstrates goal-directed behavior.

Tool usage is therefore a capability of an agent—not its defining characteristic.

---

### Misconception 4: Every Workflow Is an Agent

Modern workflow platforms often integrate language models.

For example, a document processing pipeline may classify documents using an LLM before routing them to the appropriate department.

Although artificial intelligence is involved, the workflow remains deterministic.

Every execution path was designed by a developer before deployment.

The workflow does not determine what should happen next.

It simply executes predefined logic.

An agent, by contrast, determines its own sequence of actions while pursuing an objective.

---

### Misconception 5: More Autonomy Is Always Better

It is tempting to assume that greater autonomy automatically produces better systems.

In practice, this is rarely true.

Increasing autonomy introduces additional challenges:

- More complex evaluation
- Higher operational risk
- Greater security concerns
- Increased infrastructure costs
- More difficult debugging
- Reduced predictability

Many business problems are better solved using deterministic workflows or AI-powered applications than fully autonomous agents.

Agent Engineering is not about maximizing autonomy.

It is about selecting the appropriate level of autonomy for the problem being solved.

---

### Misconception 6: Multi-Agent Systems Are Superior to Single Agents

Multi-agent architectures have attracted significant attention because they resemble human organizations.

However, adding more agents does not automatically improve a system.

Each additional agent introduces:

- Communication overhead
- Coordination complexity
- Additional latency
- Higher inference costs
- More difficult observability

A well-designed single agent is often simpler, faster, less expensive, and easier to maintain than an unnecessarily complex multi-agent system.

Engineers should adopt multi-agent architectures only when specialization or collaboration provides measurable architectural benefits.

---

### The Engineering Perspective

These misconceptions share a common theme.

They focus on individual technologies rather than system behavior.

Agent Engineering approaches the problem differently.

Instead of asking:

- Does this system use an LLM?
- Does it call tools?
- Does it have a chat interface?

Agent Engineers ask:

- Does the system pursue objectives?
- Does it determine its own next actions?
- Does it adapt based on observations?
- Does it coordinate reasoning and execution over multiple steps?

These questions focus on architecture rather than implementation.

They distinguish an AI agent from other intelligent software systems and provide a more reliable foundation for designing production-grade agent systems.

## The Relationship Between LLMs and Agents

One of the most common sources of confusion in modern AI engineering is the relationship between a **Large Language Model (LLM)** and an **AI agent**.

These terms are often used interchangeably.

From an engineering perspective, however, they represent fundamentally different concepts.

An LLM is a component.

An agent is a software system.

Understanding this distinction is essential because many architectural decisions depend on recognizing where the responsibility of the language model ends and where the responsibility of the agent begins.

---

### The Language Model Is Not the System

A Large Language Model is responsible for probabilistic reasoning.

Given a prompt and contextual information, it predicts the most likely sequence of tokens.

Depending on how it is prompted, this allows it to:

- Understand natural language
- Generate text
- Summarize information
- Answer questions
- Extract structured data
- Produce code
- Explain concepts

These capabilities are powerful.

However, an LLM has no inherent understanding of:

- Business objectives
- Application state
- External systems
- Long-running workflows
- Organizational policies
- Operational constraints

It only reasons over the information that has been provided in its current context.

Once inference is complete, its work is finished.

---

### The Agent Is the System

An AI agent surrounds the language model with the software components required to accomplish real-world objectives.

Rather than relying on a single model invocation, the agent coordinates multiple capabilities.

These typically include:

- Goal management
- Context construction
- Memory
- Planning
- Tool execution
- Observation
- Evaluation
- Execution control

The language model provides reasoning.

The surrounding software determines how that reasoning is used.

---

### A Helpful Analogy

A useful analogy is to compare an LLM with a CPU.

A CPU performs computation.

It does not determine:

- Which program should execute
- When execution begins
- What data should be loaded
- Which files should be accessed
- How errors should be handled

Those responsibilities belong to the operating system and the applications running on it.

Similarly:

- An **LLM performs reasoning.**
- An **agent runtime orchestrates reasoning.**

The intelligence of an agent emerges from the interaction between these components rather than from the language model alone.

---

### Architectural Relationship

The relationship can be visualized as follows.

```text
                    AI Agent
                       │
       +---------------+----------------+
       │               │                │
       ▼               ▼                ▼
  Goal Management   Memory        Tool Layer
       │               │                │
       +---------------+----------------+
                       │
                       ▼
              Context Construction
                       │
                       ▼
              Large Language Model
                       │
                       ▼
                 Reasoning Output
                       │
                       ▼
             Execution & Observation
```

Notice that the language model occupies only one position within the overall architecture.

The surrounding components determine:

- What information the model receives.
- Which tools it may use.
- Whether additional reasoning is required.
- Whether execution should continue.
- When the objective has been achieved.

---

### Why This Distinction Matters

Confusing the LLM with the agent leads to poor architectural decisions.

For example, engineers may attempt to solve problems by:

- Selecting a larger model.
- Writing increasingly complex prompts.
- Repeatedly changing providers.

In many cases, the real issue lies elsewhere.

The system may require:

- Better context engineering
- Improved memory management
- More effective planning
- Safer tool interfaces
- Clearer execution policies
- Stronger evaluation mechanisms

Replacing the language model cannot compensate for weaknesses in the surrounding architecture.

Likewise, an exceptionally capable language model cannot transform a poorly designed system into a reliable production agent.

The quality of the agent depends on the quality of the entire system.

---

### A Fundamental Principle

Throughout this handbook, we will treat the language model as one component within a larger software architecture.

This distinction is deliberate.

Agent Engineering is not the practice of integrating language models into applications.

It is the discipline of designing software systems that use language models, together with planning, memory, tools, and execution control, to reliably achieve objectives.

Understanding this relationship is one of the most important conceptual shifts for engineers transitioning from AI application development to Agent Engineering.

## Key Takeaways

This chapter introduced the fundamental concepts required to distinguish AI agents from other classes of software systems.

Rather than beginning with a definition, we examined the evolution of software capabilities and identified the characteristics that differentiate traditional programs, workflows, AI-powered applications, assistants, agents, and multi-agent systems.

The following principles form the foundation for the remainder of this handbook.

- **An AI agent is a software system, not a language model.** The language model provides reasoning, while the surrounding software provides planning, memory, tool execution, context management, and execution control.

- **The defining characteristic of an agent is goal-directed behavior.** Unlike AI-powered applications and assistants, an agent accepts an objective and determines the sequence of actions required to achieve it.

- **Agents operate through an iterative execution loop.** They repeatedly observe, reason, decide, act, and evaluate until the objective has been completed or a stopping condition has been reached.

- **Tool usage alone does not create an agent.** Likewise, conversation history, chat interfaces, or language model integration are important capabilities but are not sufficient to classify a system as an agent.

- **Autonomy exists on a spectrum.** Traditional programs, workflows, AI-powered applications, assistants, agents, and multi-agent systems represent progressively more capable forms of software rather than entirely separate categories.

- **More autonomy is not always better.** The appropriate architecture depends on the problem being solved. Many production systems are better served by deterministic workflows or AI-powered applications than by fully autonomous agents.

- **Multi-agent systems solve different problems than single-agent systems.** They should be adopted when specialization, collaboration, or parallel execution provides clear architectural benefits rather than because they are fashionable.

- **Frameworks do not define what an agent is.** Whether an agent is implemented using LangGraph, OpenAI Agents SDK, CrewAI, AutoGen, Semantic Kernel, or a custom runtime, its defining characteristics remain the same.

The most important lesson from this chapter is that **Agent Engineering is concerned with designing systems, not integrating models**.

An AI agent is not defined by the technologies it uses but by the architectural behaviors it exhibits.

This perspective will guide every subsequent chapter in this handbook.