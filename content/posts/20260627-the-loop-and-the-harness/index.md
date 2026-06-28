---
title: "The Loop and the Harness"
date: 2026-06-28
description: "Why the most important question in AI-assisted work is no longer what to ask, but what system to build --- and why the answer applies far beyond software."
tags: ["ai", "software-development", "agents", "loop-engineering", "harness", "knowledge-work"]
draft: false
---

> *"You're not supposed to prompt Claude. You're supposed to build a system that prompts itself."*
>
> --- [Boris Cherny](https://twitter.com/bcherny), Head of Claude Code, Anthropic, 2026

This sentence, offered quietly in conversation and widely circulated in the months since, captures something that has become apparent to almost everyone who has tried to scale AI-assisted development beyond a single engineer's workflow. The bottleneck is no longer the model. The bottleneck is the system around the model.

Through the first half of 2026, a new vocabulary has consolidated around this insight. The term *loops* now appears everywhere in the AI software delivery discourse --- conference talks, Hacker News front pages, internal memos at firms moving fast and quietly. But the term is being asked to do too much work. It refers simultaneously to execution, to validation, to improvement, and to governance --- and the slippage between these meanings obscures something important. Each is a distinct loop, with a distinct purpose, operating at a distinct cadence, and the way they interact under AI acceleration is the real story.

This essay separates the loops, defines what each one is for, surveys the field evidence from software delivery --- where the pattern was discovered first --- and then generalises. The aim is not to explain how to build a harness. The aim is to understand what the harness pattern reveals about knowledge work itself, and why every domain of complex human effort is about to experience the same structural shift.

---

## The Two Meanings of Loops

The term entered the discourse with two referents. Understanding both is necessary, but it turns out that neither is sufficient.

### Inner and Outer: The Pipeline That Is Out of Sync

Software delivery has long been understood through a pair of nested cycles. The *inner loop* is where developers write, debug, build, and unit-test code. It is fast, creative, high-cadence work --- the think-make-check rhythm that defines the experience of writing software. The *outer loop* is everything that surrounds it: requirements gathering, integration testing, compliance review, deployment, monitoring, and the slow return of user feedback. The inner loop runs in seconds or minutes. The outer loop runs in hours, days, or weeks.

This model traces back to continuous integration thinking in the DevOps movement, was refined by the [DORA research programme](https://dora.dev/research/) and Google Cloud's developer productivity teams, and has been adopted widely in platform engineering. For years, the inner and outer loops operated in rough equilibrium. A developer might spend four hours coding and two days waiting for integration test results, security review, and deployment. The ratio was frustrating but stable.

AI has broken the equilibrium. The inner loop has been dramatically accelerated --- tools like [Claude Code](https://docs.anthropic.com/en/docs/claude-code), [OpenAI Codex](https://openai.com/codex/), and [GitHub Copilot](https://github.com/features/copilot) can generate functions, entire modules, or test suites in seconds that would have taken a developer hours. The inner loop has never been faster.

The outer loop has barely moved. Integration tests still take twenty minutes. Compliance reviews still wait for a human in a different timezone. Deployment pipelines still gate on manual approval. Security scanning still runs overnight.

As [Rob Zuber](https://www.linkedin.com/feed/update/urn:li:activity:7417973946622402560/), CTO of CircleCI, observed in June 2026: *"The loops of software delivery are out of sync. The inner loop, where developers write, debug, and build, has never been faster. But the outer loop --- where code is integrated, tested, secured, deployed, and measured --- has barely moved."*

The result is a traffic jam --- but not where the conventional diagnosis places it. The DevOps pipeline, once mature, is repeatable and fast: a well-instrumented CI/CD system with parallelised runners, canary deployments, and automated rollbacks can move code from commit to production in minutes. The bottleneck is not shipping. It is everything that sits between code-complete and deploy-ready --- the human-mediated validation layer. And that layer has three distinct problems, each deeper than the last.

The first is **validation latency**: the human gates --- code review, architecture review, security sign-off, stakeholder approval --- run at the speed of trust, not the speed of compute. A pull request generated in seconds can sit for days waiting for a human to read it. The machine can generate code faster than humans can agree that the code is correct, safe, and fit for purpose.

The second is **design drift**: an agent working across dozens of files over 77 iterations makes micro-decisions --- about abstractions, interfaces, data flow --- that accumulate into an architecture no human explicitly designed. The final system is not the one anyone envisioned at the outset. It is the one that emerged from the agent's choices. Some of those choices are good. Some are not. All of them must be understood by someone before the code can be trusted. A test suite can verify that the code behaves correctly. It cannot verify that the abstractions are coherent or that the design decisions made in iteration 34 will not cause maintenance problems in month six.

The third and deepest is **problem-solution co-evolution**: the oldest truth in software is that you do not fully understand the problem until you try to solve it. Building surfaces edge cases, clarifies requirements, exposes contradictions. When a human builds slowly, these insights arrive at a manageable pace --- the developer pauses, rethinks, and adjusts. When an agent builds at speed, the insights accumulate faster than the human can absorb them. The agent finishes the build before the human has finished understanding what the build revealed about the problem. And those insights --- if they could be absorbed --- should change what gets built next. The feedback from execution to governance must close at the speed of the build, not the speed of the org chart.

Organisations that poured AI into the inner loop without addressing these three layers discovered, sometimes painfully, that they had accelerated their way into a bottleneck. The bottleneck shifted downstream --- but not to the pipeline. To the human.

### Observe, Plan, Act, Reflect: The Agent Loop

The second meaning is newer, more radical, and is the one generating the most heat in mid-2026. It describes a paradigm shift in how developers interact with AI systems --- from single-shot prompting to autonomous iterative loops.

In this framing, the developer does not write a prompt and accept whatever comes back. The developer designs a *system* that observes the codebase, plans an action, executes it, reflects on the result, and repeats --- autonomously, over hours or days, until a verifiable goal is met. The human is not in the middle of each iteration. The human is *on the loop*, watching the system from above, intervening only at pre-defined boundaries.

The intellectual lineage is clear enough, though it draws from a deeper well than is usually acknowledged. The [ReAct pattern](https://arxiv.org/abs/2210.03629), published by Princeton and Google DeepMind researchers in 2022, demonstrated that interleaving reasoning steps with action steps --- think, act, observe, think again --- substantially improved model performance on complex tasks. But the feedback loop itself is far older. It has been independently discovered in manufacturing (Plan-Do-Check-Act, [Shewhart](https://en.wikipedia.org/wiki/Walter_A._Shewhart) and [Deming](https://en.wikipedia.org/wiki/W._Edwards_Deming), 1930s--1950s), military strategy (Observe-Orient-Decide-Act, [John Boyd](https://en.wikipedia.org/wiki/OODA_loop), 1970s), organisational learning ([double-loop learning](https://en.wikipedia.org/wiki/Double-loop_learning), Chris Argyris, 1970s), and entrepreneurship (Build-Measure-Learn, [Eric Ries](https://en.wikipedia.org/wiki/Lean_startup), 2011). The feedback loop is not a new idea. What is new is the *operator*: an AI agent running the loop autonomously, while the human designs the system that shapes its behaviour.

[AutoGPT](https://github.com/Significant-Gravitas/AutoGPT) and [BabyAGI](https://github.com/yoheinakajima/babyagi) in early 2023 proved, crudely and expensively, that chaining LLM calls in autonomous loops could produce results beyond what single prompts could achieve. The community then moved through a sequence of refinements: prompt engineering (2022--2024: *"What should I say?"*), context engineering (2025: *"What information do I provide?"*), and finally harness engineering (2026: *"What system do I build?"*).

The progression is not merely terminological. It marks a genuine shift in where intelligence resides. In prompt engineering, intelligence is in the text. In harness engineering, intelligence is in the *architecture* --- the tools, gates, guardrails, and feedback mechanisms that surround the model and shape its behaviour over time. The model becomes a component, not the centre.

---

## A Vocabulary for Loops

Before surveying the evidence, it is worth being precise about what we mean by a loop. The term is currently being asked to carry at least four distinct ideas, and the slippage between them creates confusion. Disentangling them reveals a structure that recurs at every scale of human organisation.

### Four Purposes

Every loop serves one of four purposes. They are not variations on a theme. They are different in kind.

| Purpose | Question it answers | Example (software) | Example (law) |
|---------|--------------------|--------------------|---------------|
| **Execution** | *What action do I take next?* | Write code, run a test, read a file | Draft a clause, research a precedent |
| **Validation** | *Is this correct and safe?* | Lint, type-check, run integration tests, compliance scan | Peer review, court review, opposing counsel scrutiny |
| **Improvement** | *How can we do this better?* | Add a verification gate, tune a tool, run a retrospective | Refine drafting checklists, update research protocols |
| **Governance** | *Are we doing the right thing?* | Define the goal, prioritise the backlog, set the strategy | Determine case strategy, accept or reject a client |

These four purposes are not hierarchical --- one is not "above" another --- but they operate at fundamentally different cadences. Execution runs in seconds or minutes. Validation runs in minutes or hours. Improvement runs in days or weeks. Governance runs in weeks or months.

### Two Mechanisms

The agentic community has developed specific mechanisms for the first two purposes:

- The **execution mechanism** is Observe-Plan-Act-Reflect: the agent surveys its environment, decides what to do, does it, evaluates the result, and repeats.
- The **validation mechanism** is the self-verification loop: the agent runs its own output through deterministic gates --- a linter, a test suite, a type checker --- and self-corrects before a human ever sees the result.

The improvement and governance mechanisms are, for now, still human activities. The improvement loop is the retrospective: a human reviews the trajectory log, identifies failure patterns, and adds gates or tools to the harness. The governance loop is goal-setting: a human defines what "done" looks like, and encodes that definition into verifiable acceptance criteria.

### The Key Distinction: Improvement Is Not Governance

A common confusion, visible throughout the discourse, is treating the improvement loop and the governance loop as the same thing. They are not.

- **The improvement loop** asks: *"The agent keeps making this class of error. Should we add a gate?"* It makes the system better at what it does.
- **The governance loop** asks: *"Is the agent optimising for the right thing? Does the goal need to change? Is the harness encoding our actual intent?"* It questions what the system should be doing.

The improvement loop is methodological. The governance loop is teleological. Both are essential, but they are answers to different questions, done at different cadences, by people with different decision rights. Conflating them --- treating "add a gate" and "change the goal" as the same kind of activity --- obscures where human judgment is most needed. The governance loop cannot be automated, because it is where intent is defined, and intent is not computable from data.

### The Governance Loop and the Cognitive Absorption Rate

The governance loop has a second, less visible function beyond periodic goal-setting: it is the channel through which execution insights are absorbed into intent. Every time an agent builds something, the build reveals things about the problem --- edge cases that were not in the specification, design tensions between requirements, unstated assumptions that turned out to be false. These insights should change what gets built next. But the channel through which they flow is human cognition, and human cognition has not accelerated.

An agent can generate ten files in two minutes. A human cannot review, understand, and reorient at that pace. The binding constraint in the framework is not validation latency or shipping speed. It is the **cognitive absorption rate** --- the speed at which a human can comprehend what the agent built, what it implies about the problem, and whether the goal should change in response. The governance loop is slow not because governance is inherently slow, but because comprehension is inherently slow. And comprehension cannot be automated, because it is where judgment meets intent.

This is the deepest of the three bottleneck layers. Validation latency can be addressed with better gates. Design drift can be addressed with architectural review patterns. But problem-solution co-evolution --- the feedback from building back to intent --- runs at the speed of human thought, and human thought has not gotten faster.

### The Fractal Property

These four purposes are not unique to the agent level. They recur at every scale of human organisation. A delivery team has its own execution loop (the sprint), its own validation loop (QA, integration testing), its own improvement loop (the retrospective), and its own governance loop (backlog prioritisation). So does the enterprise. So does the market. The pattern is self-similar.

This is why the "inner/outer loop" terminology, useful as it is, can be ambiguous. When someone says "the outer loop," they might mean validation at the agent level (CI/CD for generated code), validation at the delivery-team level (integration testing), or governance at the value-chain level (business case approval). Same word, different level, different purpose. The framework that follows uses purpose and level explicitly, so that when we say "validation," we can specify *whose* validation, *of what*, at *what cadence*.

---

## A Literature Survey: What Worked, What Didn't, and What the Failures Reveal

The stories that follow are drawn from the public record --- Hacker News posts, company blogs, conference talks, and the broader discourse --- and they are selected not for novelty but for what each reveals about the conditions under which loop engineering succeeds or fails. Each case is tagged with the loop purpose and organisational level it primarily illuminates, so that the pattern across them is visible not just as narrative but as structure.

### 1. Anthropic --- Internal Engineering at Scale
*Primarily illuminates: execution + validation at the agent level; governance at the delivery-team level*

[Boris Cherny](https://twitter.com/bcherny), Head of Claude Code, has disclosed in talks and online discussions throughout 2025--2026 that Anthropic engineers using harness engineering ship **eight times more code daily** than before, and that by May 2026 Claude authored **over 80% of merged production code** at the company. The specific pattern Cherny credits is the *self-verification loop* --- the validation mechanism described above: the agent runs its own output through deterministic checks, observes failures, and self-corrects before a human ever sees the code. He claims a **2--3&times; quality improvement** from this pattern alone.

| What worked | What didn't | Insight |
|---|---|---|
| Self-verification loop (validation at the agent level) | Single-shot prompting at scale --- too many rejections, too much rework | The agent must be able to observe failure and correct itself. Without this, the human becomes the bottleneck reviewing every line. |
| Harness as shared infrastructure across the engineering org | Implicit harnesses that lived in individual engineers' heads | Governance --- the definition of what "good" means --- must be explicit and shared to scale beyond a single developer. |

The numbers are not independently verified and come from the organisation with the strongest incentive to make the case. But they are consistent with everything the independent stories demonstrate at smaller scale. The architecture works. The question is not whether --- it is what it costs to build well.

### 2. envref --- The Purest Public Demonstration
*Primarily illuminates: execution at the agent level; governance at the individual level*

In February 2026, a single developer posted to Hacker News about [envref](https://news.ycombinator.com/item?id=47042109), a Go CLI tool for managing environment variable references across seven secret backends. The developer did not write the tool. They defined a goal, set up an agent loop with Claude Code (Opus 4.6), and walked away.

| Metric | Value |
|---|---|
| Iterations | 77 autonomous agent iterations |
| Tokens consumed | ~192 million |
| API cost | **$170** |
| Compute time | **8 hours 41 minutes** |
| Human intervention after goal definition | Zero |
| Output | Full Go CLI with fuzz tests, schema validation, documentation site, and a plugin protocol |

The loop terminated not because the agent decided it was done, but because every item in the backlog had been implemented and every gate --- compiler, linter, test suite --- passed. The governance decision (the goal) was verifiable. The execution loop ran. The validation gates determined when the work was complete.

| What worked | What didn't | Insight |
|---|---|---|
| Verifiable backlog items as termination conditions | --- (the run was successful) | Governance: the goal *is* the termination condition. Vague goals produce infinite loops. |
| Deterministic gates (Go compiler, linter, test suite) | --- | Validation: every gate was a binary check. No human judgment was needed to determine completion. |

What the story does not capture --- because it is not visible from the outside --- is the governance work that preceded the run and the comprehension work that followed it. The goal did not write itself. The backlog was structured for autonomous execution. The verification gates were defined. And after the agent finished, the developer still had to understand the architecture that emerged across those 77 iterations --- the design decisions the agent made, the abstractions it chose, the trade-offs it embedded in the code. The eight hours and forty-one minutes of autonomous execution were the *output* of a governance decision; the governance itself required design, and the comprehension of the output required time. Execution accelerated. Comprehension did not.

### 3. Altimate Code --- The Harness as Empirical Advantage
*Primarily illuminates: validation at the agent level*

In March 2026, [Altimate Code](https://news.ycombinator.com/item?id=47050112) --- founded by Anand and Pradnesh, who previously built dbt Power User and Datamates VS Code extensions with over 750,000 combined installs --- posted a direct A/B test on the [dbt Labs ADE-bench](https://github.com/dbt-labs/ade-bench). The results were striking:

| Setup | ADE-bench Score |
|---|---|
| **Altimate Code** (Sonnet 4.6 + compiled Rust harness) | **74.4%** |
| Cortex Code / Snowflake (Opus 4.6) | 65% |
| Claude Code baseline (Sonnet 4.6, no harness) | ~40% |

The harness includes: deterministic column-level lineage at 0.26 milliseconds per query across a 500,000-row benchmark, 26 SQL anti-pattern rules with zero false positives, local schema validation in 2 milliseconds without touching the warehouse, and engine-level permission enforcement. Every component is deterministic. None involves a model call.

| What worked | What didn't | Insight |
|---|---|---|
| Deterministic tools (compiled Rust, sub-millisecond) | Reliance on model capability alone | Validation: a cheaper model with compiled tools outperforms a more expensive model without them. The gap *is* the harness. |
| Engine-level permission enforcement | Trusting the model to respect boundaries | Validation gates must be enforced at the harness level, not requested at the model level. |

The Altimate result is the clearest empirical evidence yet that the validation mechanism, not the model, is the binding constraint on quality. The implication is uncomfortable for an industry that has organised itself around model capability rather than system design. Returns on harness investment compound; returns on prompt investment do not.

### 4. Gus and Runtime --- The Solo-to-Team Wall
*Primarily illuminates: governance + validation at the delivery-team level*

[Gus](https://news.ycombinator.com/item?id=47046321), founder of Runtime (YC P26) and previously of Mentum (YC S21), posted in May 2026 about a pattern that has become one of the most-discussed in the loop engineering discourse --- because it is a failure story, and the failures are more instructive than the successes.

As a solo developer, Gus shipped **four full-stack products in three months** using coding agents. The workflow was fluid, high-bandwidth, and effective. When he tried to roll the same workflow to his entire team, it collapsed:

- PRs turned into unmergeable "slop" --- generated code that passed no shared quality bar
- Context was trapped in one person's head --- the implicit understanding of what "good" meant
- Non-engineers could not safely touch a real codebase --- no guardrails existed
- The agents produced code at different quality levels because different team members had different implicit standards

| What worked | What didn't | Insight |
|---|---|---|
| Solo developer + agent loop at high velocity | Rolling the same workflow to a team without shared harness | Governance: the solo developer's harness was *implicit* --- it lived in his head. Brains do not compose. |
| --- | Relying on individual judgment for quality and safety | Validation at the team level must make quality standards explicit, deterministic, and enforced. |

The failure directly led to building Runtime as infrastructure for team-scale agent loops. The fix was to externalise the governance and validation mechanisms --- shared context, deterministic validation, permission boundaries, sandboxed execution --- from the individual developer's head into a shared, executable system. Runtime now has multiple YC scaleups live on the platform, including a fintech unicorn that built an **on-call inspector agent** wiring PagerDuty &rarr; Sentry &rarr; repo to autonomously find root causes and open PRs with unit tests before anyone gets paged.

### 5. The OAuth Developer --- Four Years of Failure, Three Weeks of Shipping
*Primarily illuminates: execution at the agent level; governance at the individual level*

One of the more striking personal narratives in the discourse is the developer who spent [four years trying to build an OAuth 2.0 server](https://news.ycombinator.com/item?id=47038901) --- PKCE, DPoP, MFA, WebAuthn/Passkeys, EU sovereign requirements --- and never finished it. After discovering agentic coding loops, they shipped it in **three weeks** with **91% test coverage**.

| What worked | What didn't | Insight |
|---|---|---|
| Agent persistence --- the loop did not get tired or lose motivation | Hand-coding over four years --- attrition, not incompetence, caused abandonment | Execution: OAuth 2.0 with modern extensions is complexity that exhausts a single developer's attention. The loop succeeded not because it was smarter, but because it was more persistent. |
| Autonomous iteration until gates passed | Human attention as the binding constraint on complex projects | Some projects fail not because they are too hard but because they are too *long*. Loop engineering addresses the attention bottleneck directly. |

The story controls for the developer --- same person, same project, same domain knowledge. The only variable was the technique. Four years of hand-coding produced an unfinished project. Three weeks of loop engineering produced a shipped product. The difference is not a speedup; it is the difference between completion and abandonment.

But the loop did not eliminate the need for human comprehension. OAuth 2.0 with modern extensions involves dozens of RFCs, subtle interactions between grant types, and cryptographic requirements that interact with deployment topologies. The agent executed faster; the developer still had to understand what was built and why. The 91% test coverage implies review, adjustment, and the absorption of what the build revealed about the protocol's complexity. Execution accelerated dramatically. The governance loop --- understanding the implications of the built system for the problem it was meant to solve --- remained a human activity, running at the speed of human cognition.

### 6. CircleCI Chunk --- Moving Validation into the Inner Loop
*Primarily illuminates: validation at the agent level*

[CircleCI's Chunk sidecars](https://news.ycombinator.com/item?id=47054213), built by Olaf in the CTO office and posted in May 2026, address a specific failure mode in the validation mechanism: by the time CI catches a failure in agent-generated code, the agent has already moved on and useful context is gone.

Chunk runs scoped "microbuilds" in lightweight Firecracker microVMs that mirror the CI environment, triggering automatically during agent stop/evaluation events. Validation moves *into* the execution loop, so the agent sees failures while context is still fresh.

| Metric | Result |
|---|---|
| Average microbuild compute | **~27 seconds** |
| Equivalent full CI billable compute | ~5 minutes |
| Token usage reduction in retry loops | **3--5&times; lower** |

| What worked | What didn't | Insight |
|---|---|---|
| Fast, scoped validation during agent evaluation | Waiting for full CI (minutes) before the agent sees failures | Validation latency determines iteration speed. The faster the feedback, the cheaper and more effective the execution loop. |
| Firecracker microVMs for sandboxing | Running agent code without isolation | Sandboxing is a validation gate. It is structural, not optional. |

### 7. Loki Mode --- Multi-Agent, Research-Backed
*Primarily illuminates: execution + validation at the agent level (multi-agent)*

[Loki Mode](https://news.ycombinator.com/item?id=46987654), posted in January 2026, is an open-source multi-agent system that orchestrates specialised AI agents to take a PRD to a deployed product with zero human intervention. It implements virtually every scientifically-proven pattern from the 2025--2026 AI agent literature:

- Boris Cherny's **self-verification loop** (validation mechanism, Anthropic)
- **CONSENSAGENT** (ACL 2025) --- blind review + devil's advocate, producing a 30% reduction in false positives
- **GoalAct** --- global planning &rarr; skill decomposition &rarr; local execution, yielding a 12%+ improvement in success rate
- **Multi-Agent Reflexion** --- structured debate between Implementer, Skeptic, Advocate, and Synthesizer roles (validation through adversarial review)

| What worked | What didn't | Insight |
|---|---|---|
| Separation of concerns across specialised agents | Single-agent loops for very complex, multi-domain tasks | Execution: different agents can catch each other's errors. The proposer/verifier separation is a validation mechanism, not just an implementation detail. |
| Research-backed patterns (CONSENSAGENT, GoalAct, Reflexion) | Ad-hoc multi-agent orchestration without structured debate | Validation through structured adversarial review is more reliable than single-agent self-checking. |

### 8. MCP Agent Mail --- Cross-Agent Coordination Without a Human
*Primarily illuminates: execution at the agent level (cross-agent)*

[MCP Agent Mail](https://news.ycombinator.com/item?id=46890321), posted in October 2025, demonstrated that multiple coding agents --- Claude Code, Codex, and others --- could coordinate across separate repositories by sending structured messages to each other, described by the creator as "like Gmail for coding agents." The agents *"take to this system like a fish to water"*, sending detailed messages, coordinating naturally, giving each other feedback, and pushing back on bad ideas. Different frontier models (Claude + GPT + Gemini) were observed to collaborate in complementary ways without a human in the middle.

| What worked | What didn't | Insight |
|---|---|---|
| Structured message-passing between agents across repo boundaries | Human-mediated hand-offs between agent-produced artefacts | Execution: agents can coordinate their execution loops without humans --- if the communication channel is structured and the protocol is explicit. |
| Multi-model collaboration (different frontier models complementing each other) | Single-model, single-agent setups for cross-cutting changes | Different models have different strengths. An execution harness that orchestrates multiple models can outperform any single one. |

### 9. Patched --- Automating the Outer Loop
*Primarily illuminates: validation at the delivery-team level*

[Patched](https://news.ycombinator.com/item?id=46844706) (YC S24), launched in October 2024, automates post-code tasks --- code reviews, documentation generation, and patches --- through customisable, self-hostable workflows. It directly addresses the validation bottleneck at the delivery-team level that CircleCI's Zuber identified. An example automated PR generated SDK bindings with complex type information for the Stack Auth project.

| What worked | What didn't | Insight |
|---|---|---|
| Automating post-code tasks (reviews, docs, patches) | Leaving validation as a manual process after accelerating execution | Validation at the delivery-team level must be re-engineered alongside execution at the agent level, or the bottleneck simply shifts downstream. |

### The Pattern Across the Survey

Look across all nine cases and a clear shape emerges --- not just in the narratives, but in the loop purposes and levels they illuminate.

1. **The individual hero story repeats --- and it is always about execution and governance.** Gus shipping four products in three months. The envref developer building a CLI tool autonomously for $170. The OAuth developer finishing a four-year project in three weeks. In each case, one person defined the governance (the goal), the agent ran the execution loop, and the validation gates determined completion. The pattern works at the individual level. But even at the individual level, the governance loop --- understanding what was built and what it means for the problem --- remained a human activity. The agent accelerated execution. It did not accelerate comprehension.

2. **The team scaling crash is the rule, not the exception --- and it is always a governance failure.** Every founder who succeeded solo hit a wall when rolling the same workflow to a team --- Gus at Runtime, the pattern that Altimate Code's founders observed at Fortune 500 data estates, the implicit standards problem that Anthropic solved by making harnesses explicit and shared. The governance mechanism (the definition of "good") was implicit in one person's head. Brains do not compose. The fix is always the same: externalise governance --- shared goals, shared gates, shared guardrails --- into an executable system. But externalising governance does not externalise comprehension. The team still needs to understand what the agents built.

3. **Deterministic validation beats probabilistic, every time.** Altimate Code's A/B test is the cleanest evidence, but the pattern is visible everywhere. envref's Go compiler and test suite. CircleCI's microbuild sidecars. The self-verification loop that Cherny describes. In every case, the highest-leverage investment in the validation mechanism is not a better model --- it is a faster, cheaper, more reliable verification gate. Deterministic validation addresses the first bottleneck layer (validation latency). It does not address design drift or problem-solution co-evolution.

4. **The infrastructure layer is emerging, fast --- and it is organised by loop purpose.** Runtime targets governance and validation at the delivery-team level. CircleCI Chunk targets validation latency at the agent level. Altimate Code targets deterministic validation at the agent level. Patched targets validation automation at the delivery-team level. Loki Mode and MCP Agent Mail target execution coordination at the agent level. Each product is solving a different loop-purpose at a different organisational level. The market is voting, with increasing specificity, that this is not a fad.

These four patterns are specific to software, but the structural forces that produced them are not. Each one generalises.

---

## The General Case

The loop-purpose framework is not really about software. It is about any complex human effort where fast, creative execution must eventually be reconciled with slow, high-stakes validation --- and where someone must periodically ask whether the whole enterprise is pointed in the right direction. Software delivery happens to be a particularly clean instance of the pattern, but the pattern is everywhere.

### The Four Purposes, Generalised

| Domain | Execution | Validation | Improvement | Governance |
|--------|-----------|------------|-------------|------------|
| **Software** | Write code, run tests | Lint, type-check, CI/CD, compliance | Add gates, retrospectives, tooling | Define goals, prioritise backlog |
| **Law** | Draft arguments, research precedent | Peer review, court scrutiny | Refine checklists, update protocols | Determine case strategy, accept clients |
| **Architecture** | Sketch, model, iterate | Permits, structural review | Refine design patterns, update standards | Project selection, design philosophy |
| **Research** | Experiment, analyse, hypothesise | Peer review, replication | Refine methodology, upgrade equipment | Research agenda, funding decisions |
| **Medicine** | Diagnose, treat, monitor | Board review, longitudinal studies | Refine protocols, train staff | Treatment philosophy, resource allocation |
| **Finance** | Model, trade, analyse | Compliance, audit, regulatory filing | Refine models, upgrade systems | Portfolio strategy, risk appetite |
| **Policy** | Draft proposals, model impacts | Legislative review, consultation | Refine drafting process, build capacity | Policy agenda, political strategy |
| **Marketing/Brand** | Create campaigns, design assets, write copy, manage social | Brand compliance, legal review of claims, A/B testing, sentiment analysis | Refine brand guidelines, update creative templates, optimise media spend | Brand strategy, positioning, portfolio architecture, budget allocation |

In every domain, execution is where the *making* happens --- the iterative, judgment-heavy, flow-state work that practitioners find meaningful. Validation is where that work is *reconciled with the world* --- checked, governed, made safe. Improvement is where the system that does the making and the checking is refined. And governance is where intent is set --- the choice of what to make, and why.

### The Fractal Property

These four purposes recur at every organisational scale. A delivery team has its own execution loop (the sprint), its own validation loop (QA), its own improvement loop (the retrospective), and its own governance loop (backlog prioritisation). The enterprise has the same four loops at a larger scale and slower cadence. The market has them at a larger scale still.

The pattern is self-similar. This is why "loops" feel blurred in the discourse: someone saying "the outer loop" might mean validation at the agent level, governance at the delivery-team level, or improvement at the value-chain level. The terminology is ambiguous because the structure is fractal.

### The Cascade

The governance output of one level becomes the execution input of the level below:

```
Strategic governance     →   "Enter the European market"
    ↓
Value chain execution    →   Formulate GDPR-compliance initiative
    ↓
Value chain governance   →   "Build consent management feature"
    ↓
Delivery team execution  →   Sprint backlog: consent API, consent UI, consent audit
    ↓
Delivery team governance →   "Implement POST /consent endpoint"
    ↓
Agent execution          →   Observe → Plan → Act → Reflect (write the endpoint)
```

Each level receives a governance decision from above, executes against it, validates the output, improves its process, and makes governance decisions for the level below. The cascade is how intent flows downward through an organisation. The harness is the mechanism by which governance at one level is encoded into execution constraints for the level below.

But the cascade also runs in reverse. Execution at each level produces insights that should flow upward and adjust governance at the level above. When an agent builds the consent endpoint, the build reveals things about the API design that the delivery team should know. When the delivery team ships the consent feature, the shipping reveals things about user behaviour that the value chain should know.

> **The upward flow is information; the downward flow is intent. The organisation functions when both flows are healthy --- and when both run at cadences that match.**

This is a restatement, in the language of organisational loops, of a systems principle that has been understood for decades. Russell Ackoff, the pioneer of systems thinking, put it bluntly: *"If you optimize the parts, you sub-optimize the whole."* A system is not the sum of its parts; it is the product of their interactions. Improving one component in isolation --- making execution dramatically faster --- does not improve the system. It strains the interactions that connect execution to validation, to improvement, to governance. The parts accelerate; the interactions break.

This is why AI has not yet delivered the productivity revolution that its raw capability suggests. AI accelerates execution --- one part of one loop at one level. It does not accelerate validation, which runs at the speed of trust. It does not accelerate improvement, which runs at the speed of pattern recognition. It does not accelerate governance, which runs at the speed of deliberation --- and, under acceleration, at the speed of comprehension. Nor does it accelerate the upward cascade of information, which is the only mechanism by which execution insights can reach governance and adjust intent. Local optimisation has deoptimised the whole. It will continue to do so until the surrounding loops --- and the cascade that connects them --- are re-engineered to match the speed that AI has imposed on execution.

### Where AI Disrupts the Cascade

AI enters at the **execution** level first --- the agent loop. This is what the literature survey captures. But the disruption propagates upward:

1. **Agent execution accelerates** → delivery-team validation becomes the bottleneck. This is the three-layer traffic jam described earlier. The team can generate code faster than it can review, understand, and absorb it.
2. **Delivery-team validation accelerates** (through tools like Patched, CircleCI Chunk, Runtime) → value-chain governance becomes the bottleneck. The team can ship faster than the business can decide what to ship.
3. **Value-chain governance accelerates** → strategic governance becomes the bottleneck. The organisation can enter markets faster than leadership can choose which markets to enter.

The traffic jam Zuber identified is level 1. But the same structural problem will appear at levels 2, 3, and beyond, in sequence. Each level's validation bottleneck is the next level's governance bottleneck. The cascade is predictable.

And at every level, the deepest bottleneck is the same: the cognitive absorption rate. Generation accelerates; comprehension does not. The human who must understand what was built, what it means, and whether the goal should change --- that human is the binding constraint at every level of the cascade.

### The Meta-Loop: Governance Under Acceleration

When an AI agent is the executor --- when the inner execution loop is no longer a human activity --- the human's role in the four-purpose framework shifts. The human is no longer in the execution loop. They are not primarily in the validation loop, though they set its thresholds. They operate in the **improvement loop** (refining the harness based on observed failure patterns) and, most importantly, the **governance loop** (defining what the system should do and why).

This is [Kief Morris](https://martinfowler.com/articles/exploring-gen-ai/humans-and-agents.html)'s "humans on the loop" placed into the framework. The human reviews the *harness* --- the tools, gates, guardrails, and goals that shape the agent's execution. When output is wrong, the human does not fix the output. They improve the harness. When strategy shifts, the human does not re-prompt the agent. They change the governance --- the goal, the acceptance criteria, the definition of done.

And then, at the highest level of governance, the human reviews the *trajectory* --- the log of what the agent did, why it did it, and whether the harness caught what it should have caught. In an agentic organisation, the audit trail is not the work product. It is the log of how the work was produced. The artefact is output. The trajectory is the record of judgment.

But the governance loop is not only the periodic, deliberate review of strategy. It is also --- and under AI acceleration, increasingly --- the rapid absorption of what execution reveals about the problem. Every time an agent builds something, the build surfaces insights: an edge case that was not in the specification, a design tension between two requirements, an unstated assumption that turned out to be false. These insights should change the goal. But they can only do so if a human absorbs them, understands their implications, and adjusts intent.

This is the meta-loop in its tightest form: build, review, learn, adjust. It must run at the cadence of the build, not the cadence of the quarterly strategy review. And it is the hardest loop to accelerate, because it runs at the speed of human comprehension --- and human comprehension has not gotten faster.

This is visible across the literature survey. The envref developer did not review 77 iterations of Go code. They set the governance (the goal), let the execution and validation loops run, and then had to understand the architecture that emerged. Anthropic's engineers do not review every line of Claude-authored code. They improve the harness when output is wrong, and they govern the goals when strategy shifts --- but they still have to comprehend what the system produced. The OAuth developer did not hand-debug the token rotation logic. They let the execution loop iterate until the validation gates passed, and then they reviewed the result --- absorbing what three weeks of autonomous iteration revealed about OAuth's complexity.

The governance loop is where human judgment resides in an agentic organisation. It cannot be automated --- not because the technology is insufficient, but because governance is the loop where *intent* is defined and *comprehension* is exercised, and neither is computable from data. You cannot optimise your way to a purpose. You cannot accelerate your way to understanding.

---

## What This Means

The four-purpose framework suggests a reorganisation of human effort that is more profound than "AI makes us faster." It suggests structural changes to how knowledge work is organised, how trust is built, and what skills are valued.

### The Skillset Shift

In every domain, the core human activity shifts from execution to improvement and governance. From writing the code to designing the system that writes the code. From drafting the brief to defining the goals and gates that shape the brief. The skills that matter are no longer primarily domain execution --- syntax, drafting technique, modelling fluency --- though these remain valuable for harness design. What matters is the ability to specify goals precisely enough for an autonomous agent to execute against, to design validation gates that catch errors deterministically, to review trajectories rather than artefacts, and to distinguish between "the system needs improvement" and "the goal needs to change."

This shift will be uncomfortable. The historical pattern [I have written about elsewhere](https://graeme-lockley.github.io/20260314-ai-adoption-patterns/) --- surgeons resisting anaesthesia because it devalued their speed-based skill, master weavers burning power looms because they threatened a lifetime's craft --- is repeating across every domain AI touches. The resistance is not irrational, but neither is it likely to prevent the transition. As [Addy Osmani](https://twitter.com/addyosmani) of Google put it in 2026: *"I'm skeptical, and you absolutely have to be careful."* The question for individual knowledge workers is not whether the transition will happen, but whether they will participate in shaping the harnesses or be shaped by them.

### The Cognitive Absorption Rate Is the Ultimate Constraint

For all the attention paid to model capability, token costs, and validation latency, the deepest constraint in the framework is the one that is least discussed: the speed at which a human can understand what an agent has built and what it means. Generation has accelerated by orders of magnitude. Comprehension has not.

This is not a technical problem. It is a cognitive one. And it means that the organisations that thrive under AI acceleration will not be the ones with the fastest models or the most elaborate harnesses. They will be the ones whose governance structures are designed for rapid comprehension: small batches of work that can be understood quickly, tight feedback loops between building and reviewing, and governance cadences that match the speed of human cognition rather than the speed of the calendar.

The envref developer understood this, perhaps implicitly. Eight hours and forty-one minutes of autonomous execution produced an output that could be reviewed in an afternoon. The batch was sized for comprehension. The OAuth developer understood this. Three weeks of iteration produced a system that could be understood against the RFCs it implemented. The batch was scoped to the problem. In both cases, the governance loop --- review, absorb, adjust --- could close at a human cadence because the batch was human-sized.

### Validation Will Be Re-Engineered, Painfully

Every domain has its version of the validation loop --- the compliance review, the editorial board, the deployment pipeline, the peer review committee --- and every one of them is a bottleneck waiting to be broken. The organisations that re-engineer validation first will absorb the AI acceleration of execution instead of being crushed by it. The ones that do not will generate infinite drafts and ship none of them.

The re-engineering will not be comfortable, because validation is where power resides. Peer review is not just a quality mechanism; it is a gatekeeping function that determines whose work is published and whose is not. Regulatory approval is not just a safety check; it is a barrier to entry that protects incumbents. Editorial sign-off is not just a quality filter; it is a locus of editorial authority. Re-engineering validation means redistributing power, and power is never redistributed willingly.

### The Harness Is a Governance Concept

The harness pattern --- deterministic validation gates, structured goals with verifiable acceptance criteria, human-approval boundaries at dangerous thresholds, trajectory logging for auditability --- is not a software concept. It is a *governance* concept that happened to be discovered in software because software is the domain where execution was accelerated first.

Every domain will need its own version of the harness. Law will need deterministic gates for legal reasoning --- does the brief cite precedent correctly? Does it address the relevant statutes? Medicine will need deterministic gates for diagnosis --- does the proposed treatment align with established guidelines? Does it account for contraindications? Finance will need deterministic gates for modelling --- does the model pass backtesting? Does it comply with regulatory constraints?

The domains that develop governance harnesses first will be the domains where AI adoption is fastest and safest. The domains that do not will oscillate between two failure modes: rejecting AI entirely (and falling behind) or adopting AI without governance (and producing unreliable output at scale). The harness is the middle path, and it is the only path that scales.

### Governance Is Not Improvement

The most important distinction in the framework, and the one most easily lost in practice, is the difference between the improvement loop and the governance loop. Adding a gate is improvement. Changing the goal is governance. Both are necessary. But they are answers to different questions, done at different cadences, by people with different decision rights.

An organisation that treats every failure as an improvement problem --- *"let's add another gate"* --- will eventually build a harness so constraining that the agent cannot execute. An organisation that treats every failure as a governance problem --- *"maybe we're building the wrong thing"* --- will never stabilise long enough to ship. The art of the harness is knowing which loop you are in, and when to switch.

And underlying both is comprehension. You cannot improve what you do not understand. You cannot govern what you have not absorbed. The human who reviews the trajectory, who reads the architecture that emerged from 77 autonomous iterations, who understands what the build revealed about the problem --- that human is not replaceable. The loops that matter most are the ones that cannot be automated.

---

## Conclusion: The Harness Is the Artefact

The discourse around AI-assisted development has spent three years focused on the model --- its capabilities, its limitations, its trajectory. That focus was appropriate when the model was the binding constraint. It is no longer appropriate, in software or anywhere else. The binding constraint is now the system around the model: the tools it can invoke (execution), the gates that verify its output (validation), the process by which the system improves (improvement), the goals that tell it what to do and why (governance) --- and the speed at which humans can comprehend what all of that produces.

The stories surveyed above all point to the same conclusion, and that conclusion generalises. The envref developer did not succeed because of a clever prompt. Altimate Code did not outperform a more expensive model because of a better model. Gus's team did not fail because the model was insufficient. The OAuth developer did not ship in three weeks because the model was a better programmer than he was. Anthropic's 8&times; output gain did not come from a model upgrade. In every case, the difference was the harness --- and the harness is the encoding of governance into execution.

But the harness does not eliminate the need for human comprehension. It channels it. The human who designs the harness, reviews the trajectory, absorbs the insights, and adjusts the goal --- that human is doing something no AI can do. They are exercising judgment. They are setting intent. They are understanding what was built and what it means.

The harness pattern is not about software. It is about how complex human effort is organised when the speed of thought is no longer the binding constraint. The domains that understand this --- that distinguish execution from validation from improvement from governance, that build harnesses for each purpose at each level, that recognise the fractal and cascade properties of the loops they inhabit, and that design for the cognitive absorption rate of the humans who must comprehend what the machines produce --- will thrive. The domains that do not will generate infinite drafts and ship none of them.

The loop is not the point. The harness is. And the harness is the artefact we will all be iterating on --- at every level, in every domain --- for the next decade.
