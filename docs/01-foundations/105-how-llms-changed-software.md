# Chapter 105 — How LLMs Changed Software

> **Domain:** Foundations
>
> **Estimated Reading Time:** 40–45 minutes
>
> **Prerequisites:** Chapters 101–104
>
> **Difficulty:** Beginner to Intermediate
>
> **Last Updated:** 2026-07-27

---

## Why This Chapter Exists

For decades, software engineering was built upon a single fundamental assumption:

> **Computers execute deterministic instructions.**

Whether building a banking system, a compiler, a database, a web application, or a distributed microservice architecture, the underlying execution model remained remarkably consistent.

A program received inputs.

It executed explicitly defined logic.

It produced predictable outputs.

If the same inputs were provided again, the same outputs were expected.

This deterministic model has shaped programming languages, software architectures, testing methodologies, debugging techniques, and engineering best practices for more than half a century.

Then large language models arrived.

For the first time, software engineers could integrate a component that did not simply execute predefined algorithms.

Instead, it could interpret language, reason about incomplete information, generate novel responses, summarize documents, write code, answer questions, and make decisions under uncertainty.

This was not merely another library or framework.

It introduced an entirely new kind of computation into software systems.

As a result, many assumptions that traditional software engineering relied upon suddenly became insufficient.

Applications no longer consisted solely of deterministic code.

Instead, they became hybrids:

- deterministic software for reliability and control,
- probabilistic reasoning for flexibility and intelligence.

This shift gave rise to an entirely new discipline.

Traditional software engineering remained essential, but it was no longer enough to build systems whose behavior depended on reasoning rather than explicit algorithms.

That gap is what Agent Engineering seeks to address.

This chapter explains why large language models fundamentally changed software engineering.

Rather than focusing on implementation details, we will examine the architectural consequences of introducing probabilistic reasoning into software systems.

Understanding this transition is essential because nearly every concept introduced later in this handbook—including prompts, context engineering, tool calling, planning, memory, and agent runtimes—exists as a consequence of this architectural shift.

## Guiding Question

> **Why did the arrival of Large Language Models require a new way of building software?**

Throughout the history of software engineering, improvements have usually been incremental.

Programming languages evolved.

Databases became faster.

Frameworks became more productive.

Cloud platforms simplified deployment.

Despite these advances, the fundamental execution model remained unchanged.

Software still followed a familiar pattern:

```text
Input
   │
   ▼
Deterministic Logic
   │
   ▼
Output
```

Large Language Models introduced something fundamentally different.

Instead of writing explicit algorithms for every possible situation, engineers could delegate certain kinds of decision-making to a reasoning engine.

This changed a fundamental assumption that software had relied upon for decades.

Instead of asking:

> **"What algorithm should solve this problem?"**

Engineers could now ask:

> **"What information does the model need to reason about this problem?"**

This may appear to be a subtle shift.

In reality, it changes almost every aspect of software design.

Traditional software engineering focuses primarily on:

- Algorithms
- Data structures
- Business rules
- APIs
- Databases
- Distributed systems

Agent Engineering must additionally consider:

- Context construction
- Prompt design
- Memory
- Tool selection
- Reasoning quality
- Planning
- Evaluation
- Guardrails

These concerns did not replace traditional software engineering.

They extended it.

Understanding why this happened is the purpose of this chapter.

By the end of this chapter, you should understand not only **what** changed when LLMs arrived, but **why** entirely new architectural patterns—including agent systems—became both possible and necessary.

## Software Before Large Language Models

To appreciate how profoundly LLMs changed software engineering, we must first understand what software looked like before they existed.

For decades, nearly every software system—regardless of language, framework, or industry—followed the same fundamental principle:

> **Every decision made by the software was ultimately written by a human developer.**

Whether building a calculator, an ERP system, a search engine, or an e-commerce platform, the computer never "figured out" what to do.

It simply executed instructions that engineers had already anticipated and encoded.

This deterministic model was extraordinarily successful.

Modern civilization runs on software built using this approach.

---

### The Classical Software Model

Traditional software can be represented as a straightforward pipeline.

```text
User Input
     │
     ▼
Validation
     │
     ▼
Business Logic
     │
     ▼
Database / Services
     │
     ▼
Response
```

Every step is explicitly designed.

Every possible path is intentionally programmed.

Nothing is left to interpretation.

For example, consider a banking application.

```python
if account_balance >= withdrawal_amount:
    approve_transaction()
else:
    reject_transaction()
```

The software does not "think" about whether the withdrawal is reasonable.

It simply evaluates a predefined condition.

If the inputs are identical, the outcome will always be identical.

This property is known as **deterministic execution**.

---

### Software Was a Collection of Rules

Traditional applications were essentially collections of rules.

```text
If X happens
      │
      ▼
Execute Rule A

Else If Y happens
      │
      ▼
Execute Rule B

Else
      │
      ▼
Execute Rule C
```

As systems grew, developers added more rules.

When new business requirements appeared, engineers modified or extended those rules.

The software became more capable by accumulating increasingly sophisticated logic.

This approach worked remarkably well because the environments in which software operated were largely predictable.

---

### Every Edge Case Required New Code

Suppose an online store wants to calculate shipping costs.

The logic might evolve like this:

```text
Domestic Order
        │
        ▼
Flat Shipping Rate
```

Later:

```text
Domestic?
    │
 ┌──┴──┐
 │     │
Yes    No
 │      │
 ▼      ▼
$5   International Rate
```

Eventually:

```text
Destination
Weight
Dimensions
Carrier
Priority
Holiday Rules
Promotions
Customer Tier
```

Each new business requirement introduces additional conditions.

The application becomes increasingly complex because developers must anticipate every scenario.

Traditional software scales by **adding more explicit rules**.

---

### Intelligence Was Simulated

Applications often appeared intelligent.

Consider features such as:

- Spam filters
- Recommendation systems
- Fraud detection
- Customer support bots
- Search engines

Before modern LLMs, these systems typically relied on combinations of:

- Handcrafted rules
- Statistical models
- Machine learning classifiers
- Decision trees
- Feature engineering
- Domain-specific heuristics

Although these techniques were highly effective, they were usually designed for a specific task.

A spam classifier could classify spam.

It could not summarize an email.

A recommendation engine could rank products.

It could not explain a legal contract.

Each capability required a separate solution.

---

### Deterministic Software Has Important Strengths

It is tempting to think that LLMs replaced traditional software.

They did not.

Deterministic software remains essential because it provides qualities that probabilistic systems cannot guarantee.

Traditional software offers:

- Predictable behavior
- Precise calculations
- Repeatable execution
- Strong correctness guarantees
- Easier testing
- Regulatory compliance
- High reliability

These properties are indispensable in domains such as:

- Banking
- Healthcare
- Aviation
- Telecommunications
- Operating systems
- Database engines

Even the most advanced AI systems rely heavily on deterministic software.

The innovation introduced by LLMs was not replacing deterministic systems.

It was augmenting them.

---

### The Limitation

Traditional software excels when the developer can explicitly define the correct algorithm.

However, many real-world problems do not fit neatly into deterministic rules.

Questions such as:

- "Summarize this report."
- "Explain this contract."
- "Write a professional email."
- "Identify the customer's intent."
- "Generate Python code for this API."
- "Plan a three-day itinerary."

do not have a single algorithmic solution.

Writing explicit rules for every possible input quickly becomes impractical.

For decades, these kinds of problems represented one of the largest gaps in software engineering.

Large Language Models did not eliminate deterministic programming.

Instead, they introduced a new kind of computational capability for solving problems where explicit algorithms were either impossible or prohibitively expensive to construct.

That capability fundamentally changed how software systems could be designed.

## The Arrival of Large Language Models

Large Language Models did not simply make existing software better.

They introduced an entirely new computational capability.

For the first time, software systems could perform tasks that previously required human judgment, interpretation, and reasoning.

This represents one of the most significant shifts in software engineering since the introduction of high-level programming languages.

---

### A Different Kind of Computation

Traditional software executes algorithms.

Large Language Models perform probabilistic reasoning.

This distinction is fundamental.

Consider a calculator.

```text
Input:
27 × 43

Algorithm
    │
    ▼
1161
```

The program follows a precisely defined mathematical procedure.

There is only one correct answer.

Now consider a different task.

```text
Summarize this research paper.
```

There is no single correct summary.

Different summaries may:

- emphasize different ideas,
- vary in length,
- use different wording,
- target different audiences,
- remain equally valid.

This is not an algorithmic problem.

It is a reasoning problem.

Large Language Models are designed specifically for this category of computation.

---

### From Rules to Reasoning

One way to understand this transformation is to compare how software approaches a problem.

Traditional software asks:

```text
Which rule applies?
```

An LLM-based system asks:

```text
Given everything I know,
what is the most appropriate response?
```

This may appear similar on the surface, but architecturally they are completely different.

Traditional software searches for predefined logic.

Language models generate responses by reasoning over context and learned patterns.

---

### The Software Stack Changed

Before LLMs, software architecture looked something like this.

```text
User
   │
   ▼
Application Logic
   │
   ▼
Database
```

Business logic was entirely deterministic.

After LLMs, a new component appeared.

```text
          User
            │
            ▼
     Application Logic
            │
     ┌──────┴──────┐
     ▼             ▼
Database      Language Model
```

The application no longer solved every problem itself.

Instead, it delegated certain forms of reasoning to the language model.

This separation became one of the defining architectural patterns of modern AI applications.

---

### Software Could Handle Unstructured Problems

Historically, software worked best with structured data.

Examples include:

- Numbers
- Dates
- IDs
- Tables
- Forms
- Transactions

Large Language Models dramatically expanded the range of problems software could address.

Applications could now work directly with:

- Natural language
- Documents
- Contracts
- Source code
- Emails
- Meeting notes
- Policies
- Knowledge bases
- Conversations

In other words, software became capable of operating on information that humans naturally produce rather than requiring everything to be converted into structured data first.

---

### The New Capability Was General

Previous AI systems were usually designed for one specific task.

For example:

```text
Spam Classifier

Email
   │
   ▼
Spam / Not Spam
```

A recommendation engine:

```text
Customer History
        │
        ▼
Recommended Products
```

A speech recognizer:

```text
Audio
  │
  ▼
Text
```

Each model solved a narrowly defined problem.

Large Language Models introduced something different.

The same model could:

- summarize a document,
- answer questions,
- translate languages,
- write code,
- analyze contracts,
- explain concepts,
- generate SQL,
- classify support tickets,
- create reports,
- reason through complex tasks.

Instead of building a separate model for every capability, developers could use a single reasoning engine for a wide variety of applications.

This dramatically changed how software systems were designed.

---

### Why This Was Revolutionary

The innovation was not merely that software became capable of generating text.

The real breakthrough was that software could now perform tasks that previously required human interpretation.

Instead of encoding every decision into code, engineers could delegate parts of the decision-making process to a reasoning engine.

For example, instead of writing hundreds of rules to classify customer requests, an application could ask the model:

> "Given this conversation, what is the customer's intent?"

Instead of building a custom parser for every document format, it could ask:

> "Extract the key terms from this contract."

The software still controlled the overall workflow.

But the reasoning itself became dynamic rather than hard-coded.

---

### The Architectural Consequence

This new capability fundamentally changed the responsibilities of software engineers.

Previously, engineers spent most of their effort designing algorithms.

Now they also had to design reasoning systems.

This introduced entirely new architectural questions:

- What information should the model receive?
- How should context be constructed?
- When should the model be invoked?
- When should deterministic code be preferred?
- How should reasoning be validated?
- How should incorrect outputs be handled?
- How should tools extend the model's capabilities?

These questions did not exist in traditional software engineering.

They emerged because software was no longer composed solely of deterministic computation.

It now combined deterministic execution with probabilistic reasoning.

That combination is the foundation upon which modern AI applications—and ultimately Agent Engineering—are built.

## Deterministic vs. Probabilistic Computing

The arrival of Large Language Models did not replace deterministic computing.

Instead, it introduced a second computational paradigm.

Understanding the difference between these two paradigms is one of the most important concepts in modern AI engineering.

Almost every architectural decision in an agent system ultimately comes down to answering one question:

> **Should this task be solved deterministically or probabilistically?**

---

### Deterministic Computing

Deterministic systems always produce the same output when given the same input.

For example:

```text
Input

5 + 7

↓

Output

12
```

Running the computation one million times produces exactly the same answer.

The computer is not making a judgment.

It is executing a predefined algorithm.

Examples include:

- Mathematical calculations
- Database queries
- Sorting algorithms
- Authentication
- Payment processing
- Tax calculations
- Inventory updates

These problems have well-defined rules and predictable outcomes.

---

### Probabilistic Computing

Probabilistic systems behave differently.

Rather than following a fixed algorithm toward a single correct answer, they estimate the most appropriate output based on patterns learned from data.

Consider the following request.

```text
Explain recursion to a beginner.
```

There is no single correct answer.

The model might produce:

```text
Recursion is when a function calls itself.
```

Another execution might produce:

```text
Think of standing between two mirrors...
```

Or:

```text
Imagine opening a box that contains another copy of itself...
```

All three responses may be accurate.

They simply represent different reasoning paths.

This flexibility is what makes language models powerful.

It is also what makes them fundamentally different from traditional software.

---

### Side-by-Side Comparison

| Deterministic Computing | Probabilistic Computing |
|--------------------------|-------------------------|
| Executes algorithms | Performs reasoning |
| Predictable outputs | Variable outputs |
| Exact correctness | Best approximation |
| Rule-based | Pattern-based |
| Easy to test | Requires evaluation |
| Handles structured problems | Handles ambiguous problems |
| Guarantees repeatability | Accepts uncertainty |

Neither paradigm is superior.

Each is appropriate for different classes of problems.

---

### An Everyday Example

Suppose a user asks:

> "How many days are there between January 1 and February 15?"

This is a deterministic problem.

```text
Question
    │
    ▼
Date Calculation Algorithm
    │
    ▼
45 Days
```

There is only one correct answer.

Now consider a different question.

> "Write a polite email declining a meeting invitation."

This is not a deterministic problem.

Many responses could be appropriate.

The software cannot simply apply a formula.

Instead, it relies on probabilistic reasoning.

---

### Why LLMs Should Not Replace Deterministic Code

One of the most common mistakes made by new AI engineers is asking an LLM to solve problems that already have deterministic solutions.

For example:

```text
LLM

↓

Calculate Sales Tax

↓

Return Total
```

This architecture is fundamentally flawed.

The correct design is:

```text
LLM

↓

Extract Purchase Information

↓

Deterministic Tax Engine

↓

Final Total
```

The language model identifies the relevant information.

Deterministic software performs the calculation.

This separation provides correctness, consistency, and regulatory compliance.

---

### Hybrid Systems

Modern AI applications combine both paradigms.

```text
              User Request
                    │
                    ▼
          Application Runtime
          ┌─────────┴─────────┐
          ▼                   ▼
Deterministic Logic      Language Model
          │                   │
          └─────────┬─────────┘
                    ▼
               Final Response
```

The runtime determines which parts of the problem require deterministic execution and which benefit from probabilistic reasoning.

For example:

| Task | Best Approach |
|------|---------------|
| Calculate invoice totals | Deterministic |
| Summarize an invoice | Probabilistic |
| Validate an email address | Deterministic |
| Draft an email | Probabilistic |
| Execute a payment | Deterministic |
| Explain a payment failure | Probabilistic |
| Sort transactions | Deterministic |
| Identify suspicious transactions | Often a hybrid approach |

The most effective AI systems are rarely fully deterministic or fully probabilistic.

They combine both approaches, allowing each to do what it does best.

---

### The Architectural Shift

Before Large Language Models, software engineers primarily designed deterministic systems.

After Large Language Models, they began designing **hybrid systems**.

The application became responsible for deciding:

- Which decisions belong to deterministic code?
- Which decisions should be delegated to the language model?
- How should information flow between the two?
- How should uncertain reasoning be validated before affecting the real world?

These questions are at the heart of Agent Engineering.

An effective agent is not one that delegates everything to an LLM.

It is one that carefully combines deterministic execution with probabilistic reasoning to achieve reliable, scalable, and trustworthy outcomes.

## Why Prompts Became Programs

One of the most surprising consequences of Large Language Models was that software engineers suddenly found themselves writing something that looked nothing like traditional code.

Instead of writing algorithms, they began writing instructions in natural language.

These instructions became known as **prompts**.

At first glance, prompts appear to be simple text.

In reality, they perform a role remarkably similar to traditional programs.

They tell the reasoning engine:

- what objective to pursue,
- what information to use,
- what constraints to follow,
- what output to produce.

In other words, prompts became a new form of programming.

---

### Traditional Programming

In classical software engineering, developers express logic using programming languages.

```python
if customer.is_premium:
    discount = 0.20
else:
    discount = 0.05
```

The computer follows these instructions exactly.

Every decision is explicitly encoded.

---

### Programming an LLM

A language model cannot be programmed in the same way.

Instead, developers provide objectives and constraints.

For example:

```text
You are a customer support assistant.

Your goal is to resolve the customer's issue.

If critical information is missing, ask clarifying questions.

Respond in a professional tone.

Return your final answer as JSON.
```

Although this is written in English rather than Python, it still defines the behavior of the system.

The model interprets these instructions during reasoning.

---

### From Algorithms to Instructions

The shift can be summarized as follows.

Traditional software:

```text
Developer

↓

Writes Algorithm

↓

Computer Executes Algorithm
```

LLM-based software:

```text
Developer

↓

Provides Instructions

↓

Model Reasons About Instructions

↓

Produces Output
```

The developer no longer specifies every computational step.

Instead, they define:

- the objective,
- the constraints,
- the desired behavior,
- the expected output.

The reasoning engine determines how to satisfy those instructions.

---

### Prompts Are Programs—But Different Ones

This comparison is useful, but it has limits.

Traditional programs are:

- deterministic,
- formally specified,
- precisely executable,
- unambiguous.

Prompts are:

- probabilistic,
- interpreted rather than compiled,
- sensitive to wording,
- dependent on context.

Changing a single sentence in a prompt may influence the model's reasoning.

Changing a single character in source code may prevent the program from compiling.

The two forms of programming operate under very different assumptions.

---

### Why Prompt Engineering Emerged

Because prompts influence reasoning, engineers quickly realized that writing effective prompts became an engineering discipline.

Poor prompts often produce:

- incomplete answers,
- inconsistent reasoning,
- incorrect assumptions,
- missing information,
- poorly formatted outputs.

Improved prompts can dramatically increase reliability without changing the underlying model.

This gave rise to **Prompt Engineering**.

Prompt Engineering is not about discovering "magic phrases."

It is the systematic design of instructions that help language models perform reasoning more reliably and consistently.

Entire books could be written on this subject, and later in this handbook we will dedicate several chapters to it.

---

### Prompts Are Only Part of the Picture

As agent systems became more sophisticated, engineers discovered another important limitation.

A prompt alone is rarely sufficient.

Suppose an agent receives:

> "Summarize yesterday's customer complaints."

The prompt explains *what* to do.

It does not provide:

- yesterday's complaints,
- company policies,
- previous conversation,
- customer history,
- relevant documentation.

That information must be gathered separately.

This realization led to another major discipline:

> **Context Engineering**

Instead of asking only:

> "How should I write the prompt?"

Engineers began asking:

> "What information should the model receive before it reasons?"

As we will see later in this handbook, context often has a greater impact on reasoning quality than prompt wording itself.

---

### The Architectural Consequence

Large Language Models changed the role of software engineers.

Previously, developers primarily wrote algorithms.

Now they also design reasoning environments.

A modern AI application is no longer defined solely by its source code.

Its behavior is shaped by a combination of:

- deterministic software,
- prompts,
- context,
- memory,
- retrieved knowledge,
- tool results,
- system instructions.

Together, these elements determine how the reasoning engine behaves.

This represents one of the most significant architectural shifts introduced by Large Language Models and forms the foundation for modern Agent Engineering.

## Why Context Became a First-Class Engineering Concern

In traditional software engineering, the correctness of a program depends primarily on its algorithm.

If the algorithm is correct, the program behaves correctly.

With Large Language Models, this assumption changes.

The quality of the model's reasoning depends heavily on the **information available at the time it reasons**.

In other words:

> **Reasoning quality is limited by context quality.**

This simple observation led to one of the biggest shifts in modern AI engineering.

Context stopped being an implementation detail.

It became a core architectural concern.

---

### Traditional Software Already Has State

Consider a typical web application.

```text
User Request
      │
      ▼
Application
      │
      ▼
Database
```

Whenever the application needs information, it queries the database.

The business logic has access to the entire system whenever required.

The programmer rarely worries about "what information the CPU knows."

The program can retrieve whatever it needs at any point during execution.

---

### Language Models Work Differently

A language model does not have continuous access to your application.

Instead, it reasons only over the information included in the current request.

Conceptually, it looks like this.

```text
                Context
                   │
                   ▼
          ┌─────────────────┐
          │  Language Model │
          └─────────────────┘
                   │
                   ▼
              Reasoning
```

Anything outside that context simply does not exist from the model's perspective.

It cannot query your database unless you explicitly allow it through tools.

It cannot remember previous conversations unless that information is provided.

It cannot see documents unless they are retrieved and included.

The quality of its reasoning is therefore constrained by the quality of its context.

---

### The Same Model Can Produce Different Results

Imagine asking the same model:

```text
Summarize the quarterly report.
```

Case 1:

```text
Context

Quarterly Report Attached
```

The model produces a useful summary.

Case 2:

```text
Context

(No report provided)
```

The model cannot summarize what it has never seen.

The reasoning engine is identical.

The prompt is identical.

Only the context changed.

Yet the quality of the outcome changes dramatically.

---

### Context Became Part of the Architecture

This realization fundamentally changed software design.

Instead of asking only:

> "What should the application do?"

Engineers now also ask:

> "What should the model know before it reasons?"

These are different questions.

Traditional software architecture focuses on:

- APIs
- Databases
- Business logic
- Services

Modern AI architecture additionally focuses on:

- Which memories to retrieve
- Which documents to include
- Which tool outputs are relevant
- Which previous conversations matter
- Which instructions should always be present
- Which information should be omitted

This is no longer prompt writing.

It is system design.

---

### More Context Is Not Always Better

An intuitive assumption is that providing more information always improves reasoning.

In practice, this is rarely true.

Imagine asking someone:

> "What is today's weather in Pune?"

Now imagine giving them:

- today's weather,
- the last ten years of weather history,
- your travel itinerary,
- yesterday's emails,
- your tax records,
- your source code repository.

Most of that information is irrelevant.

It creates noise rather than clarity.

The same principle applies to language models.

Good context is not the **largest** context.

It is the **most relevant** context.

One of the primary responsibilities of an agent runtime is selecting the smallest amount of information that allows the model to make the best possible decision.

---

### Context Engineering Emerged

As applications became more sophisticated, engineers realized that constructing context deserved its own discipline.

This discipline became known as **Context Engineering**.

Its goal is to answer questions such as:

- What information should be included?
- What information should be excluded?
- In what order should information appear?
- How should context be compressed?
- When should memory be retrieved?
- When should external knowledge be fetched?
- How do we fit everything within the model's context window?

These questions are now among the most important architectural decisions in Agent Engineering.

We will explore them in depth later in this handbook.

---

### The Architectural Shift

Large Language Models changed software engineering in an unexpected way.

The challenge was no longer only writing correct code.

It also became providing the right information at the right time.

This is one of the defining differences between traditional software and AI-native software.

Traditional systems optimize algorithms.

Modern agent systems optimize **reasoning environments**.

That reasoning environment is constructed through context, and the quality of that context often determines the quality of the entire system.

## Why New Building Blocks Emerged

Traditional software engineering relied on a relatively stable set of architectural components.

A typical application consisted of:

- Business logic
- Database
- APIs
- Authentication
- Caching
- Message queues
- User interfaces

These components were sufficient because the application itself contained all of the decision-making logic.

Large Language Models changed this assumption.

Once reasoning became part of the system, software required entirely new architectural components to support it.

These components did not replace traditional software architecture.

They extended it.

---

### Traditional Application Architecture

A simplified architecture for a classical web application might look like this.

```text
             User
               │
               ▼
        Web Application
               │
      ┌────────┴────────┐
      ▼                 ▼
 Business Logic     Database
               │
               ▼
         External APIs
```

The application contains nearly all of the decision-making logic.

Every request follows predefined rules.

---

### AI-Native Application Architecture

Introducing an LLM changes the architecture.

```text
                     User
                       │
                       ▼
                 Agent Runtime
                       │
      ┌────────────────┼────────────────┐
      ▼                ▼                ▼
  Memory          Language Model      Tools
      │                │                │
      └────────────────┼────────────────┘
                       ▼
                  External World
```

Notice how several entirely new components appear.

These components exist because reasoning alone is not enough.

The language model needs support.

---

### Why Each New Component Exists

Each new building block solves a limitation of language models.

| Component | Problem It Solves |
|-----------|-------------------|
| Prompt | Gives the model an objective |
| Context | Supplies relevant information |
| Memory | Preserves knowledge across interactions |
| Tool Calling | Allows interaction with external systems |
| Structured Output | Produces machine-readable responses |
| Planning | Breaks complex goals into manageable tasks |
| Runtime | Coordinates the execution loop |
| Guardrails | Constrains unsafe or undesirable behavior |
| Evaluation | Measures reasoning quality |

None of these existed because traditional software was missing features.

They emerged because probabilistic reasoning introduced new engineering challenges.

---

### Everything Exists to Support Reasoning

One useful way to think about modern AI systems is that almost every new architectural component exists to improve one of three things:

```text
                Better Reasoning
                      │
      ┌───────────────┼───────────────┐
      ▼               ▼               ▼
 Better Input     Better Decisions   Better Execution
```

Different components contribute in different ways.

For example:

```text
Prompt
      │
      ▼
Improves Instructions

Context
      │
      ▼
Improves Available Information

Memory
      │
      ▼
Improves Continuity

Planning
      │
      ▼
Improves Decision Quality

Tools
      │
      ▼
Improves Capability

Evaluation
      │
      ▼
Improves Reliability
```

Although these appear to be separate technologies, they all exist to support the same execution loop introduced in the previous chapter.

---

### These Components Work Together

It is tempting to study these concepts independently.

In production systems, however, they are tightly connected.

Consider a simple customer support agent.

```text
User Question
       │
       ▼
Retrieve Memory
       │
       ▼
Retrieve Documentation
       │
       ▼
Build Context
       │
       ▼
Prompt
       │
       ▼
Language Model
       │
       ▼
Tool Call
       │
       ▼
Structured Output
       │
       ▼
Update Memory
```

No single component makes the system intelligent.

The behavior emerges from their collaboration.

This is a recurring theme in Agent Engineering:

> **Intelligence is a system property, not a component property.**

The language model is necessary, but it is only one participant in a much larger architecture.

---

### Why We Studied These Topics Individually

Throughout this handbook, we will explore each of these building blocks in depth.

You may wonder why they deserve entire chapters.

The reason is simple.

Each one represents a substantial engineering discipline.

For example:

- Prompt Engineering is about designing effective instructions.
- Context Engineering is about constructing high-quality reasoning environments.
- Memory Systems are about preserving knowledge across time.
- Tool Calling is about safely extending the model's capabilities.
- Planning is about decomposing complex objectives.
- Agent Runtimes are about orchestrating reliable execution.

Although they all contribute to the same execution loop, each introduces its own architecture, trade-offs, implementation techniques, and production concerns.

Mastering Agent Engineering requires understanding not only how these components work individually, but also how they interact to produce reliable, goal-directed behavior.

The remainder of this handbook is dedicated to exploring those components one by one.

## From AI Applications to Agent Systems

The first generation of LLM-powered software was surprisingly simple.

Developers took an existing application and added a language model.

The architecture looked something like this.

```text
User
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

For many use cases, this worked remarkably well.

Applications could:

- answer questions,
- summarize documents,
- translate text,
- generate emails,
- write code,
- explain technical concepts.

These became known as **AI-powered applications**.

Although impressive, they shared an important limitation.

They could reason.

They could not accomplish goals.

---

### An AI Application Responds

Suppose a user asks:

> "Summarize this quarterly report."

The execution is straightforward.

```text
User Request
      │
      ▼
Prompt
      │
      ▼
Language Model
      │
      ▼
Summary
```

The interaction begins with a request and ends with a response.

Once the answer is generated, execution is complete.

There is no memory, planning, tool execution, or iterative reasoning.

---

### Some Problems Require More Than One Step

Now consider a different request.

> "Investigate why our production deployment failed and prepare a report."

This objective cannot usually be completed in a single model invocation.

The system may need to:

1. Retrieve deployment logs.
2. Query monitoring systems.
3. Inspect recent code changes.
4. Compare previous deployments.
5. Identify likely causes.
6. Generate recommendations.
7. Produce a final report.

Each step depends on information discovered during earlier steps.

This requires something fundamentally different from a single prompt.

It requires an execution system.

---

### The Architectural Evolution

The progression can be visualized as follows.

```text
Traditional Software

Input
   │
   ▼
Algorithm
   │
   ▼
Output
```

↓

```text
AI Application

Input
   │
   ▼
Language Model
   │
   ▼
Output
```

↓

```text
Agent System

Goal
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
Observe
  │
  ▼
Reason Again
  │
  ▼
Goal Complete?
```

Each stage represents an increase in capability rather than merely an increase in complexity.

---

### Why Agents Emerged Naturally

As developers attempted increasingly sophisticated tasks, they repeatedly encountered the same limitations.

A single model invocation could not:

- maintain long-running objectives,
- recover from failures,
- execute multiple dependent actions,
- use external tools repeatedly,
- adapt to changing information,
- coordinate complex workflows.

To overcome these limitations, developers began adding:

- memory,
- tool execution,
- planning,
- context retrieval,
- execution loops,
- stopping conditions,
- runtime orchestration.

Eventually, these additions formed what we now recognize as an **agent system**.

Notice what happened.

Agents were not invented as a completely new technology.

They emerged as a natural architectural evolution of AI applications.

---

### Comparing the Three Generations

| Traditional Software | AI Application | Agent System |
|----------------------|---------------|--------------|
| Executes algorithms | Performs reasoning | Pursues goals |
| Deterministic | Probabilistic | Hybrid |
| Single execution | Single reasoning step | Iterative execution |
| Rule-driven | Prompt-driven | Goal-driven |
| Static workflow | Dynamic response | Adaptive execution |
| No reasoning | One reasoning cycle | Continuous reasoning cycles |

Each generation builds upon the previous one.

Agent systems still contain deterministic software.

They still use language models.

They simply add the execution mechanisms required for autonomous, goal-directed behavior.

---

### The Engineering Lesson

This historical progression explains why Agent Engineering exists as a separate discipline.

If applications only needed to generate responses, prompt engineering alone would have been sufficient.

However, once applications needed to accomplish objectives rather than simply answer questions, software required entirely new architectural concepts:

- execution loops,
- memory,
- planning,
- tool orchestration,
- context engineering,
- runtime coordination,
- evaluation,
- guardrails.

These are not optional enhancements.

They are the mechanisms that transform a reasoning engine into an autonomous execution system.

That transformation—from **response generation** to **goal-directed execution**—marks the birth of modern Agent Engineering.

## Key Takeaways

Large Language Models did not replace traditional software engineering.

They expanded it.

By introducing probabilistic reasoning into software systems, they fundamentally changed how engineers think about application architecture.

Rather than replacing deterministic computation, modern AI systems combine deterministic software with reasoning engines to solve problems that were previously impractical or impossible to automate.

The most important ideas from this chapter are summarized below.

---

### 1. Software Was Traditionally Deterministic

For decades, software engineering relied on deterministic execution.

Applications:

- accepted inputs,
- executed predefined algorithms,
- produced predictable outputs.

Every decision was explicitly programmed by a developer.

This model remains the foundation of modern software engineering.

---

### 2. Large Language Models Introduced a New Kind of Computation

LLMs do not execute algorithms in the traditional sense.

Instead, they perform probabilistic reasoning based on context.

This allows software to address problems involving:

- language,
- ambiguity,
- interpretation,
- summarization,
- explanation,
- planning,
- code generation,
- decision support.

These problems were previously difficult to solve using deterministic rules alone.

---

### 3. Modern AI Systems Are Hybrid Systems

Successful AI applications combine two complementary forms of computation.

```text
Deterministic Software

+
Probabilistic Reasoning

↓

Modern AI System
```

Each paradigm is used where it is strongest.

Deterministic software provides correctness, consistency, and reliability.

Language models provide flexibility, interpretation, and reasoning.

Neither replaces the other.

---

### 4. Prompts Became a New Form of Programming

Developers no longer write only algorithms.

They also write instructions that guide reasoning.

Prompts define:

- objectives,
- constraints,
- behavior,
- expected outputs.

Although prompts are not programs in the traditional sense, they perform a similar architectural role by shaping the behavior of the reasoning engine.

---

### 5. Context Became an Architectural Responsibility

A language model can reason only over the information it receives.

Consequently, AI systems must carefully construct the model's reasoning environment.

This led to the emergence of **Context Engineering**, one of the defining disciplines of modern Agent Engineering.

---

### 6. Entirely New Architectural Components Emerged

Reasoning alone is insufficient for building reliable AI systems.

Modern architectures therefore include additional components such as:

- memory,
- tool calling,
- structured outputs,
- planning,
- execution runtimes,
- guardrails,
- evaluation.

Together, these components transform a language model into a complete agent system.

---

### 7. AI Applications Naturally Evolved into Agent Systems

The first generation of AI software generated responses.

As applications became more capable, they needed to:

- pursue long-running objectives,
- execute multiple actions,
- recover from failures,
- interact with external systems,
- maintain memory,
- adapt to changing information.

Meeting these requirements led naturally to the emergence of modern agent architectures.

---

## Mental Model to Remember

The evolution of software can be summarized as three generations.

```text
Generation 1

Input
   │
   ▼
Algorithm
   │
   ▼
Output

↓

Generation 2

Input
   │
   ▼
Reasoning
   │
   ▼
Output

↓

Generation 3

Goal
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
Evaluate
  │
  ▼
Repeat Until Goal Achieved
```

The first generation automated computation.

The second automated reasoning.

The third automates goal-directed execution.

That progression explains why Agent Engineering emerged as a distinct engineering discipline.

---

## Looking Ahead

This chapter explained **why** software architecture changed after the introduction of Large Language Models.

The next chapter shifts from history to architecture.

Now that we understand why new architectural components became necessary, we can examine the anatomy of a modern agent system.

We will identify every major component, understand the problem it solves, see how it interacts with the others, and build a complete architectural map that will guide the remainder of this handbook.