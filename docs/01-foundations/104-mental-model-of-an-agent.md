# Chapter 104 — Mental Model of an Agent

> **Domain:** Foundations
>
> **Estimated Reading Time:** 35–40 minutes
>
> **Prerequisites:** Chapters 101–103
>
> **Difficulty:** Beginner to Intermediate
>
> **Last Updated:** 2026-07-27

---

## Why This Chapter Exists

After understanding what an AI agent is, the next question naturally follows:

> **How does an AI agent actually work?**

This question is deceptively simple.

Many explanations present agents as intelligent black boxes that somehow "think," "plan," and "act." Others focus heavily on specific frameworks or implementation details, making it difficult to separate enduring engineering principles from framework-specific abstractions.

Neither approach provides a useful mental model.

Engineers need a way to reason about agent systems that remains valid regardless of the language model, framework, or runtime being used.

Just as software engineers think in terms of request lifecycles, state machines, event loops, or distributed systems, Agent Engineers need a mental model that explains how an agent continuously transforms a goal into a sequence of actions.

This chapter develops that model.

Rather than introducing new terminology or implementation details, we will examine the fundamental execution cycle that exists within every agent system.

Whether an agent is built using LangGraph, OpenAI Agents SDK, CrewAI, AutoGen, Semantic Kernel, PydanticAI, or a custom runtime, the underlying process remains remarkably similar.

By the end of this chapter, you should be able to visualize an agent not as a mysterious AI system, but as a software architecture that repeatedly observes its environment, reasons about the current situation, decides what to do next, executes actions, evaluates the results, and continues until its objective has been achieved.

This mental model will serve as the foundation for every subsequent topic in this handbook, including planning, memory, context engineering, tool execution, agent runtimes, and multi-agent collaboration.


## Guiding Question

> **How does an AI agent transform a goal into a completed outcome?**

In the previous chapter, we defined an AI agent as a software system that autonomously pursues objectives through reasoning, planning, memory, and tool execution.

That definition describes **what** an agent is.

This chapter focuses on **how** an agent works.

Understanding this distinction is important.

Knowing that an aircraft flies does not explain how lift is generated.

Knowing that a database stores information does not explain how query execution works.

Similarly, knowing that an AI agent can accomplish goals does not explain the internal processes that make this possible.

To become an effective Agent Engineer, it is not enough to know the components of an agent.

You must understand how those components interact during execution.

Throughout this chapter, we will examine the internal execution model shared by modern agent systems.

Although implementations differ across frameworks, nearly every agent follows the same fundamental pattern.

It repeatedly:

1. Understands the current objective.
2. Observes its environment.
3. Builds the context required for reasoning.
4. Determines the next action.
5. Executes that action.
6. Evaluates the outcome.
7. Repeats the process until the objective has been achieved or execution should stop.

This iterative process is the mental model that underlies modern Agent Engineering.

By understanding this execution cycle, seemingly complex concepts such as planning, memory, tool calling, context engineering, and multi-agent collaboration become much easier to reason about.

Rather than viewing these as independent features, you will see them as cooperating components within a single execution loop.

That perspective will guide the remainder of this handbook.

## Thinking About an Agent the Wrong Way

Before building a mental model, it is helpful to discard a few incorrect ones.

Many engineers approach agent systems with assumptions formed from traditional software engineering or from interacting with chatbots.

These assumptions are understandable, but they often lead to architectural mistakes.

Let's examine some common—but misleading—mental models.

---

### Mental Model 1: "An Agent Is Just an LLM"

This is perhaps the most widespread misconception.

The reasoning usually goes like this:

```text
User
  │
  ▼
LLM
  │
  ▼
Answer
```

If an application uses a language model, it must be an agent.

As we learned in the previous chapter, this is incorrect.

The language model performs reasoning.

It does not:

- Remember long-term objectives.
- Decide when to use tools.
- Track execution progress.
- Monitor external systems.
- Determine whether a goal has been achieved.

Those responsibilities belong to the surrounding software system.

Thinking of an agent as "just an LLM" is similar to thinking of a web application as "just a database."

The database is essential, but it is only one component of the overall system.

---

### Mental Model 2: "An Agent Is a Workflow"

Another common misconception is to view an agent as a more sophisticated workflow.

A workflow follows a predefined path.

```text
Start
  │
  ▼
Step A
  │
  ▼
Step B
  │
  ▼
Step C
  │
  ▼
Finish
```

The sequence of actions is determined before execution begins.

An agent behaves differently.

It determines the next step during execution.

```text
Current State
      │
      ▼
Reason
      │
      ▼
Choose Next Action
      │
      ▼
Observe Result
      │
      ▼
Reason Again
```

The execution path emerges from reasoning rather than being completely predefined.

The developer specifies the objective and constraints.

The agent determines the sequence of actions required to achieve that objective.

---

### Mental Model 3: "An Agent Knows Everything"

Large language models often produce fluent and confident responses.

This can create the illusion that an agent possesses complete knowledge about the world.

In reality, an agent knows only what it can access.

Its decisions are based on:

- The current goal.
- The context provided.
- Information stored in memory.
- Results returned by tools.
- Knowledge encoded within the language model.

If critical information is unavailable, the agent must acquire it before making a decision.

This is why production agents frequently invoke search systems, databases, APIs, and retrieval mechanisms before reasoning.

---

### Mental Model 4: "An Agent Thinks Like a Human"

People often describe AI agents using human language.

We say an agent "thinks," "understands," "remembers," or "decides."

These terms are useful analogies, but they should not be interpreted literally.

An agent does not possess consciousness, intentions, emotions, or self-awareness.

What appears to be thinking is the coordinated execution of software components:

- Context construction
- Probabilistic reasoning
- Planning
- Tool execution
- Memory management
- Evaluation

Using human terminology can aid intuition, but it should never replace architectural understanding.

---

### The Right Perspective

An AI agent is best understood as an **execution system** rather than an intelligent entity.

Its purpose is not simply to generate responses.

Its purpose is to repeatedly transform the current state of the world into a state that is closer to the desired goal.

Everything else—reasoning, planning, memory, tools, and context—exists to support that process.

With these misconceptions removed, we can now build the correct mental model from first principles.

## The Fundamental Mental Model

At its core, every AI agent can be understood using a remarkably simple idea.

> **An agent is an execution loop that continuously moves the world from its current state toward a desired goal.**

Everything else—language models, memory, planning, tools, workflows, and even multi-agent collaboration—exists to support this execution loop.

This is the most important mental model in Agent Engineering.

If you understand this loop, every advanced topic in the remainder of this handbook becomes easier to understand.

---

### The Agent Loop

Unlike traditional software, which often executes once and terminates, an agent repeatedly evaluates its situation and determines what should happen next.

The execution loop can be represented as follows.

```text
                Goal
                  │
                  ▼
        Observe Current State
                  │
                  ▼
          Build Context
                  │
                  ▼
               Reason
                  │
                  ▼
             Create Plan
                  │
                  ▼
          Select Next Action
                  │
                  ▼
           Execute Action
                  │
                  ▼
          Observe New State
                  │
                  ▼
       Goal Achieved?
          │         │
        Yes         No
          │         │
          ▼         │
      Terminate ◄───┘
```

Although implementations vary, this loop exists in nearly every modern agent architecture.

Some systems execute it only once.

Others repeat it hundreds or even thousands of times.

The principle remains the same.

---

### Step 1 — Observe

Every execution cycle begins with observation.

Before an agent can make a decision, it must understand the current state of the world.

Observation may include:

- Reading user input
- Inspecting memory
- Querying databases
- Calling external APIs
- Reading files
- Retrieving documents
- Receiving sensor data
- Examining previous execution results

An agent cannot make good decisions without accurate observations.

Just as humans gather information before acting, agents begin by understanding their environment.

---

### Step 2 — Build Context

Raw information is rarely sufficient.

The agent must determine what information is relevant to the current objective.

This process constructs the context that will be used during reasoning.

Context may include:

- The user's objective
- Previous conversation
- Relevant documents
- Memory
- Tool results
- Organizational policies
- System constraints

One of the most important responsibilities of an agent runtime is deciding **what information should be included** and **what information should be excluded**.

As we will see later in this handbook, this process is known as **context engineering**.

---

### Step 3 — Reason

Once the appropriate context has been assembled, the language model performs reasoning.

It evaluates questions such as:

- What is the current situation?
- What information is missing?
- Which actions are possible?
- Which action is most likely to move the system closer to its objective?

Reasoning does not change the external world.

It changes the agent's understanding of what should happen next.

This distinction is important.

Reasoning produces decisions.

Execution produces results.

---

### Step 4 — Plan

For simple objectives, planning may consist of selecting a single action.

For more complex objectives, the agent may decompose the goal into multiple tasks.

For example:

```text
Goal:
Deploy Application

Plan:
1. Build artifacts
2. Execute tests
3. Deploy staging
4. Validate deployment
5. Deploy production
6. Verify health
```

Planning transforms a high-level objective into an actionable strategy.

The sophistication of planning varies significantly between agent systems.

Some generate plans dynamically.

Others combine planning with predefined workflows.

---

### Step 5 — Act

Planning alone does not accomplish objectives.

The agent must interact with the external world.

Actions may include:

- Calling APIs
- Querying databases
- Executing code
- Sending emails
- Updating records
- Searching documentation
- Invoking other agents
- Requesting human approval

This is the point where software moves beyond reasoning and begins producing observable effects.

---

### Step 6 — Evaluate

After every action, the agent observes the outcome.

It asks questions such as:

- Did the action succeed?
- Has the objective been achieved?
- Has new information become available?
- Should the current plan change?
- Is additional reasoning required?

Evaluation closes the feedback loop.

Without evaluation, the agent cannot determine whether its actions are moving it closer to its goal.

---

### The Loop Never Assumes Success

One of the defining characteristics of an agent is that execution is adaptive.

A traditional workflow often assumes:

```text
Action A
    │
    ▼
Action B
    │
    ▼
Action C
```

An agent assumes uncertainty.

```text
Act
 │
 ▼
Observe Result
 │
 ▼
Succeeded?
 │        │
Yes       No
 │         │
 ▼         ▼
Continue  Reason Again
```

Every action produces new information.

That information influences the next decision.

This continuous feedback loop allows agents to operate effectively in environments where outcomes cannot be predicted in advance.

---

### The Core Insight

Everything we associate with modern agents ultimately serves one purpose:

> **Continuously reduce the gap between the current state and the desired goal.**

- Observation reduces uncertainty.
- Context improves reasoning.
- Reasoning guides decisions.
- Planning organizes execution.
- Actions change the world.
- Evaluation measures progress.

This execution loop is the foundation upon which every modern agent runtime is built.

The remaining chapters of this handbook will explore each part of this loop in depth, but every topic can ultimately be traced back to this single mental model.

## Thinking in State Transitions

One of the most valuable mental shifts an engineer can make is to stop thinking about an agent as something that "answers questions" and start thinking about it as something that **changes the state of the world**.

This perspective comes from software engineering rather than artificial intelligence.

A banking application changes the state of an account.

An inventory system changes the state of a warehouse.

A deployment pipeline changes the state of an application.

Similarly, an AI agent changes the state of a problem.

Every action performed by an agent should move the system from its current state toward its desired state.

---

### Current State vs. Desired State

Every objective can be described using two states.

- **Current State** — where the system is now.
- **Desired State** — where the system should be.

The gap between these two states defines the work the agent must perform.

```text
Current State
      │
      │  Gap
      ▼
Desired State
```

The purpose of the agent is not simply to generate text.

Its purpose is to reduce this gap.

---

### Example 1 — Travel Planning

Consider the objective:

> "Plan my business trip to Singapore."

Initially, the world might look like this.

```text
Current State

✓ Destination known
✗ Flights selected
✗ Hotel booked
✗ Visa requirements checked
✗ Itinerary created
```

The desired state is:

```text
Desired State

✓ Flights booked
✓ Hotel reserved
✓ Visa verified
✓ Daily itinerary prepared
✓ Confirmation delivered
```

The agent gradually transforms one state into the other.

Each action reduces uncertainty or completes part of the objective.

---

### Example 2 — Incident Investigation

Suppose an operations engineer provides the following objective.

> "Determine why yesterday's deployment failed."

Current state:

```text
Unknown root cause
```

Desired state:

```text
Root cause identified
Evidence collected
Recommendations prepared
```

The agent might perform the following state transitions.

```text
Unknown Failure
        │
        ▼
Collect Deployment Logs
        │
        ▼
Analyze Error Messages
        │
        ▼
Query Monitoring Systems
        │
        ▼
Compare Previous Deployments
        │
        ▼
Identify Root Cause
        │
        ▼
Generate Report
```

Notice that the objective is not achieved through a single reasoning step.

Instead, each action changes the state of the investigation.

---

### Every Action Produces a New State

An important consequence of this model is that **every action changes the information available to the agent**.

For example:

```text
Before API Call

Known:
✓ Customer ID

Unknown:
✗ Account Balance
```

After executing a tool:

```text
Known:
✓ Customer ID
✓ Account Balance
```

The world has changed.

More precisely, the agent's understanding of the world has changed.

This updated state becomes the starting point for the next reasoning cycle.

---

### Why State Matters

Thinking in terms of state transitions explains many architectural decisions in Agent Engineering.

For example:

- **Why is memory important?**
  - To preserve state across reasoning cycles.

- **Why do agents observe after every action?**
  - To determine how the state has changed.

- **Why is planning necessary?**
  - To identify the sequence of state transitions required to reach the goal.

- **Why are tools important?**
  - Because they allow the agent to change or discover the state of the external world.

Without state transitions, an agent cannot make measurable progress.

It can only produce responses.

---

### The Engineering Perspective

Traditional software engineers often think in terms of:

```text
Input
   │
   ▼
Processing
   │
   ▼
Output
```

Agent Engineers think differently.

```text
Current State
      │
      ▼
Observe
      │
      ▼
Reason
      │
      ▼
Act
      │
      ▼
New State
      │
      ▼
Goal Achieved?
```

This subtle change in perspective has profound implications.

Instead of optimizing for a single response, Agent Engineers optimize for **progress**.

Every reasoning cycle should move the system measurably closer to the desired outcome.

That is the essence of goal-directed execution and one of the defining characteristics of modern agent systems.

## The Agent as a Control Loop

The execution loop introduced earlier is not unique to AI systems.

It belongs to a much older engineering concept known as a **control loop**.

Control loops have been used for decades in fields such as:

- Robotics
- Aerospace
- Industrial automation
- Autonomous vehicles
- Process control
- Distributed systems

Although these systems differ significantly, they all share the same fundamental structure:

1. Observe the current state.
2. Compare it against the desired state.
3. Decide what action to take.
4. Execute the action.
5. Measure the result.
6. Repeat.

Modern AI agents apply the same principle.

The difference is that the "decision-making" component is performed using probabilistic reasoning rather than hard-coded logic.

---

### A Thermostat Is a Simple Agent

Consider a thermostat.

Its objective is simple.

> Keep the room at **22°C**.

Its execution loop looks like this.

```text
Desired Temperature
        │
        ▼
Measure Room Temperature
        │
        ▼
Compare Current vs Target
        │
        ▼
Heating Needed?
   │           │
 Yes          No
 │             │
 ▼             ▼
Turn Heater   Do Nothing
      │
      ▼
Measure Again
```

The thermostat does not execute its logic once.

It continuously repeats the loop until the desired state is maintained.

This is the essence of feedback-driven systems.

---

### An AI Agent Uses the Same Pattern

Now consider a customer support agent.

Objective:

> Resolve a customer's billing issue.

Its control loop follows a similar structure.

```text
Customer Goal
      │
      ▼
Observe Conversation
      │
      ▼
Gather Missing Information
      │
      ▼
Reason About Next Step
      │
      ▼
Execute Tool
      │
      ▼
Observe Result
      │
      ▼
Issue Resolved?
      │        │
     Yes       No
      │         │
      ▼         │
 Complete ◄─────┘
```

Although the reasoning is much more sophisticated than a thermostat, the underlying execution pattern is identical.

Both systems repeatedly compare the current state against the desired state and take actions to reduce the difference.

---

### Closed-Loop vs. Open-Loop Systems

This distinction is fundamental to Agent Engineering.

#### Open-Loop Execution

An open-loop system executes actions without checking whether they achieved the intended outcome.

```text
Execute A
    │
    ▼
Execute B
    │
    ▼
Execute C
```

Traditional scripts and many workflow engines operate this way.

They assume each step succeeds unless an explicit error occurs.

---

#### Closed-Loop Execution

A closed-loop system continuously measures progress and adapts its behavior.

```text
Act
 │
 ▼
Observe
 │
 ▼
Expected Result?
 │        │
Yes       No
 │         │
 ▼         ▼
Continue  Replan
```

Every action generates feedback.

That feedback influences future decisions.

Modern AI agents are fundamentally **closed-loop systems**.

This is one of the key reasons they can operate in uncertain and dynamic environments where deterministic workflows struggle.

---

### Why This Matters

Imagine an agent responsible for deploying an application.

A workflow might assume:

```text
Deploy
   │
   ▼
Run Health Check
   │
   ▼
Notify Success
```

If the deployment partially succeeds or the health check reveals degraded performance, the workflow has limited flexibility unless those cases were explicitly anticipated.

An agent behaves differently.

```text
Deploy
   │
   ▼
Observe Cluster State
   │
   ▼
Healthy?
 │          │
Yes         No
 │           │
 ▼           ▼
Complete   Investigate
              │
              ▼
       Gather Logs
              │
              ▼
       Identify Cause
              │
              ▼
      Retry or Escalate
```

Instead of following a fixed path, the agent adapts its execution based on observed outcomes.

This ability to incorporate feedback after every action is one of the defining characteristics of agentic systems.

---

### The Engineering Insight

A useful way to summarize the distinction is:

- **Programs** execute instructions.
- **Workflows** execute predefined processes.
- **Agents** execute feedback loops.

This may appear to be a small difference, but it fundamentally changes how software is designed.

Rather than asking:

> "What sequence of steps should the program execute?"

Agent Engineers ask:

> "How should the system decide what to do next based on the current state?"

That shift—from predefined execution to continuous feedback-driven decision-making—is what transforms software from being merely automated to being truly agentic.

## Anatomy of an Agent Execution Cycle

Now that we understand the execution loop conceptually, let's zoom in and examine what actually happens during a **single iteration** of the loop.

Although different frameworks use different terminology and internal implementations, nearly every agent execution cycle consists of the same stages.

Understanding these stages will help you reason about any agent runtime, regardless of the framework being used.

---

### High-Level Execution Flow

A single execution cycle can be visualized as follows.

```text
                 Goal
                  │
                  ▼
        ┌──────────────────┐
        │ Observe          │
        └──────────────────┘
                  │
                  ▼
        ┌──────────────────┐
        │ Build Context    │
        └──────────────────┘
                  │
                  ▼
        ┌──────────────────┐
        │ Reason           │
        └──────────────────┘
                  │
                  ▼
        ┌──────────────────┐
        │ Decide           │
        └──────────────────┘
                  │
                  ▼
        ┌──────────────────┐
        │ Execute Action   │
        └──────────────────┘
                  │
                  ▼
        ┌──────────────────┐
        │ Evaluate Result  │
        └──────────────────┘
                  │
                  ▼
        Continue or Stop
```

Each stage has a distinct responsibility.

Let's examine them one by one.

---

### Stage 1 — Observe

The execution cycle always begins by collecting the latest information.

The agent asks:

- What has changed?
- What is the user requesting?
- What information do I already have?
- What information is missing?

Observation may involve:

- Reading user messages
- Inspecting memory
- Fetching recent documents
- Reading database records
- Checking API responses
- Examining previous tool outputs

Observation is fundamentally a **data collection phase**.

No decisions are made yet.

The objective is simply to understand the current situation as accurately as possible.

---

### Stage 2 — Build Context

Raw observations are transformed into a context package suitable for reasoning.

This stage determines:

- Which observations are relevant?
- Which memories should be retrieved?
- Which documents should be included?
- Which previous steps matter?
- Which information should be discarded?

This is one of the most important responsibilities of an agent runtime.

Language models have finite context windows.

Poor context selection often leads to poor reasoning.

Later chapters will explore this process in depth under the topic of **Context Engineering**.

---

### Stage 3 — Reason

The prepared context is now sent to the reasoning engine, typically a language model.

At this stage, the agent evaluates questions such as:

- What is the current state?
- What constraints exist?
- What options are available?
- What risks should be considered?
- What should happen next?

Importantly, reasoning does **not** change the external world.

It produces an internal decision.

Think of this as the agent answering:

> "Given everything I know right now, what should I do next?"

---

### Stage 4 — Decide

Reasoning produces possibilities.

Decision-making selects one of them.

Examples include:

- Call a tool
- Ask the user for clarification
- Retrieve additional information
- Execute a predefined workflow
- Delegate work to another agent
- Conclude that the objective has been achieved

Many frameworks merge reasoning and decision-making into a single model invocation.

Conceptually, however, they represent different responsibilities.

Reasoning evaluates possibilities.

Decision-making commits to one.

---

### Stage 5 — Execute

The chosen action is performed.

Examples include:

- Querying a database
- Executing Python code
- Calling an external API
- Searching documentation
- Updating a CRM
- Sending an email
- Creating a calendar event
- Invoking another agent

Execution is where the agent interacts with the external world.

Unlike reasoning, this stage produces observable side effects.

Because external systems are involved, execution is also where most failures occur.

Production agent systems therefore treat tool execution as a reliability problem rather than simply a programming task.

---

### Stage 6 — Evaluate

Once execution completes, the agent examines the outcome.

Questions include:

- Did the action succeed?
- Was the expected information returned?
- Did the external system report an error?
- Has the objective been achieved?
- Has the environment changed?

Evaluation provides the feedback required for adaptive behavior.

Without this stage, the agent would simply execute actions blindly.

---

### Continue or Stop

At the end of every execution cycle, the agent makes one final decision.

```text
Goal Complete?
      │
 ┌────┴────┐
 │         │
Yes        No
 │          │
 ▼          ▼
Stop    Begin Next Cycle
```

Stopping conditions vary depending on the application.

Common examples include:

- The user's request has been satisfied.
- The plan has been completed.
- A maximum iteration limit has been reached.
- Human approval is required.
- An unrecoverable error has occurred.
- The agent determines that additional progress is impossible.

A well-designed stopping condition is just as important as effective reasoning.

Without one, an agent may continue executing indefinitely, wasting time, compute, and resources.

---

### The Key Insight

Notice that none of these stages are inherently tied to a specific framework or language model.

Whether you build an agent using LangGraph, OpenAI Agents SDK, CrewAI, AutoGen, Semantic Kernel, PydanticAI, or a custom runtime, the execution cycle still consists of the same essential responsibilities:

1. Observe.
2. Build context.
3. Reason.
4. Decide.
5. Execute.
6. Evaluate.
7. Repeat or stop.

Frameworks differ in **how** they implement these stages.

Agent Engineering is about understanding **why** these stages exist and how they work together to transform goals into completed outcomes.

## The Agent Runtime

So far, we have described the execution cycle as if it simply "happens."

In reality, something must coordinate every stage of that cycle.

That responsibility belongs to the **agent runtime**.

The runtime is the orchestration layer that keeps the agent alive.

It manages the execution loop, coordinates components, invokes the language model, executes tools, handles failures, tracks progress, and determines when execution should stop.

Without a runtime, an LLM is simply a function that converts input text into output text.

The runtime transforms that capability into an autonomous system.

---

### What Does the Runtime Actually Do?

Think of the runtime as the operating system for an agent.

Just as an operating system manages processes, memory, files, and hardware, an agent runtime manages the resources required for goal-directed execution.

Its responsibilities typically include:

- Managing the execution loop
- Building prompts and context
- Invoking language models
- Executing tools
- Managing memory
- Tracking intermediate state
- Handling retries and failures
- Enforcing safety policies
- Determining stopping conditions
- Recording logs and traces

The runtime is responsible for **coordination**, not reasoning.

Reasoning is delegated to the language model.

---

### A Layered View of an Agent

One useful way to visualize an agent is as a stack of cooperating layers.

```text
┌─────────────────────────────────────┐
│            User Goal                │
└─────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────┐
│         Agent Runtime               │
│                                     │
│ • Execution Loop                    │
│ • State Management                  │
│ • Planning Coordination             │
│ • Retry Logic                       │
│ • Safety Rules                      │
└─────────────────────────────────────┘
                 │
        ┌────────┼────────┐
        ▼        ▼        ▼
┌──────────┐ ┌──────────┐ ┌──────────┐
│  Memory  │ │   LLM    │ │  Tools   │
└──────────┘ └──────────┘ └──────────┘
```

Notice what is **not** happening.

The runtime is not "thinking."

Instead, it orchestrates the components that collectively produce intelligent behavior.

---

### The Runtime Is the Conductor

An orchestra provides a useful analogy.

```text
Musicians
   │
   ▼
Conductor
   │
   ▼
Performance
```

Each musician has a specialized role.

- The violin does not coordinate the orchestra.
- The percussion section does not decide the tempo.
- The conductor does not play every instrument.

Similarly:

- The **LLM** performs reasoning.
- **Tools** interact with external systems.
- **Memory** stores information.
- The **runtime** coordinates them.

The runtime ensures that the right component performs the right task at the right time.

---

### A Typical Runtime Iteration

During a single execution cycle, the runtime might perform the following sequence.

```text
Receive Goal
      │
      ▼
Load Current State
      │
      ▼
Retrieve Memory
      │
      ▼
Build Context
      │
      ▼
Call LLM
      │
      ▼
LLM Requests Tool?
   │            │
  Yes           No
   │             │
   ▼             ▼
Execute Tool   Produce Answer
   │
   ▼
Update State
   │
   ▼
Continue?
```

The runtime repeats this process until the stopping condition is reached.

The language model participates only during the reasoning phase.

Everything else is managed by the runtime.

---

### Why This Separation Matters

Separating the runtime from the language model has significant engineering benefits.

The language model can be replaced without redesigning the system.

For example:

```text
Today

Runtime
   │
   ▼
GPT-5.5
```

Later:

```text
Same Runtime
      │
      ▼
Claude
```

Or:

```text
Same Runtime
      │
      ▼
Local Llama Model
```

The execution loop remains unchanged.

Only the reasoning engine changes.

This separation of concerns is one of the reasons modern agent frameworks support multiple language model providers with relatively little architectural change.

---

### Frameworks Primarily Provide Runtimes

This observation also helps explain the role of popular agent frameworks.

Although their APIs differ, most frameworks are fundamentally runtime implementations.

| Framework | Primary Responsibility |
|-----------|------------------------|
| LangGraph | Graph-based execution runtime |
| OpenAI Agents SDK | Agent execution runtime |
| CrewAI | Multi-agent coordination runtime |
| AutoGen | Conversational agent runtime |
| Semantic Kernel | Orchestration runtime |
| PydanticAI | Typed agent runtime |

Each framework makes different architectural decisions about:

- state management
- planning
- execution flow
- tool invocation
- memory integration
- observability
- extensibility

But all of them exist to solve the same underlying problem:

> **How do we reliably execute an agent's decision-making loop?**

---

### The Mental Model to Remember

When engineers first encounter Agent Engineering, they often focus almost exclusively on the language model.

In production systems, however, the runtime frequently contains more engineering complexity than the model itself.

A useful rule of thumb is:

> **The LLM decides. The runtime orchestrates.**

That distinction will become increasingly important as we explore planning, memory, tool execution, observability, and multi-agent systems in the chapters ahead.


## Why This Mental Model Matters

At this point, you might wonder:

> **Why spend so much time understanding the execution loop instead of simply learning an agent framework?**

Because frameworks evolve.

Mental models endure.

Over the past decade, software engineering has repeatedly demonstrated that engineers who understand the underlying architecture adapt to new technologies far more quickly than those who memorize framework APIs.

The same principle applies to Agent Engineering.

---

### Framework APIs Change

Consider web development.

Over the years, popular frameworks have come and gone.

```text
CGI
   │
   ▼
PHP
   │
   ▼
Ruby on Rails
   │
   ▼
Django
   │
   ▼
Node.js
   │
   ▼
Next.js
```

Despite these changes, the underlying concepts remained remarkably stable.

Every web framework still manages:

- HTTP requests
- Routing
- Middleware
- Business logic
- Persistence
- Responses

Engineers who understood the request lifecycle could learn new frameworks quickly.

Those who memorized framework-specific APIs often had to start over.

Agent Engineering is following the same trajectory.

---

### Agent Frameworks Will Continue to Evolve

Today's ecosystem includes frameworks such as:

- LangGraph
- OpenAI Agents SDK
- CrewAI
- AutoGen
- Semantic Kernel
- PydanticAI

Five years from now, many of these frameworks may look very different.

Some may disappear entirely.

New frameworks will almost certainly emerge.

However, the underlying execution model is unlikely to change significantly.

Every future runtime will still need to answer questions such as:

- What is the current goal?
- What information should be included in context?
- Which action should be taken next?
- How should tools be executed safely?
- How should progress be tracked?
- When should execution stop?

The implementation may change.

The architecture will remain.

---

### The Mental Model Transfers Across Frameworks

Suppose you understand the execution loop deeply.

Learning a new framework becomes an exercise in mapping concepts.

```text
Execution Loop
        │
        ├── Observe
        ├── Build Context
        ├── Reason
        ├── Decide
        ├── Execute
        └── Evaluate
```

When exploring a new framework, you naturally ask:

- Where is context constructed?
- How is state stored?
- How are tools invoked?
- How is memory managed?
- How is planning represented?
- Where are stopping conditions enforced?

Instead of memorizing APIs, you identify where the framework implements each architectural responsibility.

This is a far more durable way to learn.

---

### This Mental Model Also Improves Design

The execution loop is not only useful for understanding existing systems.

It is equally valuable when designing new ones.

Imagine someone proposes the following architecture.

```text
User
  │
  ▼
LLM
  │
  ▼
Tool
  │
  ▼
Done
```

Using the mental model from this chapter, you immediately recognize several missing concerns.

- Where is the execution state stored?
- How are failures handled?
- What happens if the tool returns unexpected results?
- How does the agent determine whether the goal has been achieved?
- How many iterations are allowed?
- What information is retained between reasoning cycles?

The execution loop provides a checklist for evaluating the completeness of an architecture.

---

### A Foundation for the Rest of This Handbook

Nearly every major topic in the remaining chapters builds upon the execution loop introduced here.

For example:

| Topic | Where It Fits in the Loop |
|-------|----------------------------|
| Context Engineering | Build Context |
| Prompt Engineering | Build Context + Reason |
| Memory Systems | Observe + Build Context |
| Tool Calling | Execute |
| Planning | Plan + Decide |
| Reflection | Evaluate |
| Human-in-the-Loop | Decide + Execute |
| Multi-Agent Systems | Execute (delegation) |
| Observability | Entire Loop |
| Guardrails | Entire Loop |
| Agent Runtimes | Coordinate Entire Loop |

Notice that these are not isolated concepts.

They are specialized solutions to specific responsibilities within the same execution cycle.

This is why understanding the loop first makes every later topic easier to place into its proper architectural context.

---

### The Core Insight

As an Agent Engineer, your primary responsibility is not to write prompts.

It is not to memorize framework APIs.

It is not even to choose the "best" language model.

Your responsibility is to design reliable systems that repeatedly transform goals into outcomes.

Everything else—planning, memory, reasoning, tools, workflows, and orchestration—exists to support that objective.

If you remember only one idea from this chapter, let it be this:

> **An AI agent is not a magical thinking machine. It is a feedback-driven execution system that continuously observes, reasons, acts, and learns from the results until its objective is achieved.**

That mental model will remain valid regardless of how agent frameworks evolve in the years ahead.

## Key Takeaways

This chapter introduced the most important mental model in Agent Engineering.

Rather than thinking of an AI agent as a chatbot or an intelligent black box, we examined it as a software system built around a continuous execution loop.

The key ideas from this chapter are summarized below.

---

### 1. An Agent Is an Execution System

An AI agent is fundamentally an execution system that repeatedly works toward a goal.

Its purpose is not merely to generate responses.

Its purpose is to transform the current state of the world into the desired state.

---

### 2. The Agent Operates as a Feedback Loop

Unlike traditional programs that execute once, agents continuously:

- Observe
- Build context
- Reason
- Decide
- Execute
- Evaluate

This loop continues until a stopping condition is reached.

Feedback after every action enables adaptation to changing environments and unexpected outcomes.

---

### 3. State Is Central to Agent Behavior

Every execution cycle begins with a current state and attempts to move closer to a desired state.

Thinking in terms of state transitions provides a more accurate engineering model than thinking in terms of prompts and responses.

---

### 4. Reasoning Is Only One Component

The language model performs reasoning.

Everything else—including context construction, memory management, tool execution, retries, and orchestration—is handled by the surrounding software system.

An agent is therefore much more than a language model.

---

### 5. The Runtime Orchestrates the System

The runtime coordinates the execution loop.

Its responsibilities include:

- Managing state
- Building context
- Invoking the language model
- Executing tools
- Tracking progress
- Handling failures
- Determining when execution should stop

A useful rule to remember is:

> **The LLM decides. The runtime orchestrates.**

---

### 6. Frameworks Implement the Same Fundamental Model

Although frameworks expose different APIs and abstractions, they all solve the same architectural problem:

> How can a software system reliably execute an iterative observe → reason → act loop?

Understanding this execution model makes it significantly easier to learn any current or future agent framework.

---

### 7. Agent Engineering Is About Systems

Modern agent systems are built from multiple cooperating components:

- Language models
- Memory systems
- Tool layers
- Planning mechanisms
- Context engineering
- Execution runtimes
- Observability
- Guardrails

These components work together to produce goal-directed behavior.

No single component is "the agent."

The agent emerges from their coordinated execution.

---

## Mental Model to Remember

Whenever you encounter a new agent architecture, ask yourself these questions:

```text
What is the goal?

↓

What does the agent observe?

↓

How is context constructed?

↓

How does it reason?

↓

What decisions can it make?

↓

What actions can it execute?

↓

How does it evaluate results?

↓

When does it stop?
```

If you can answer these questions, you understand the architecture—regardless of the framework or language model involved.

---

## Looking Ahead

This chapter explained **how an agent executes**.

The next chapter shifts focus to **why modern software changed after the introduction of large language models**.

Until recently, software engineering relied almost entirely on deterministic computation.

Large language models introduced a fundamentally different kind of computation—one based on probabilistic reasoning.

Understanding this shift is essential because it explains why traditional software architectures are no longer sufficient for building modern AI systems.

In the next chapter, we will explore how LLMs transformed software engineering and laid the foundation for the emergence of Agent Engineering.