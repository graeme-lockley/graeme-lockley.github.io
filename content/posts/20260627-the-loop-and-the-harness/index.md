---
title: "The Loop and the Harness"
date: 2026-06-27
description: "Why the most important question in AI-assisted development is no longer what to ask, but what system to build."
tags: ["ai", "software-development", "agents", "loop-engineering", "harness"]
draft: false
---

> *"You're not supposed to prompt Claude. You're supposed to build a system that prompts itself."*
>
> — [Boris Cherny](https://twitter.com/bcherny), Head of Claude Code, Anthropic, 2026

This sentence, offered quietly in conversation and widely circulated in the months since, captures something that has become apparent to almost everyone who has tried to scale AI-assisted development beyond a single engineer's workflow. The bottleneck is no longer the model. The bottleneck is the system around the model.

Through the first half of 2026, a new vocabulary has consolidated around this insight. The term *loops* now appears everywhere in the AI software delivery discourse — conference talks, Hacker News front pages, internal memos at firms moving fast and quietly. But the term conceals a duality that matters. There are two distinct ideas at work, and confusing them leads to confusion about what to build.

This essay separates them, surveys the field evidence — where loops have worked, where they have failed, and what the failures reveal — and then does something most of the discourse has not yet done: it constructs a working harness from first principles, line by line, and compares what that construction reveals against the stories in circulation. The aim is not to survey the field. The aim is to understand it well enough to build something.

---

## The Two Meanings of Loops

The first meaning is older, and is not primarily about AI at all.

### Inner and Outer: The Pipeline That Is Out of Sync

Software delivery has long been understood through a pair of nested cycles. The *inner loop* is where developers write, debug, build, and unit-test code. It is fast, creative, high-cadence work — the think-make-check rhythm that defines the experience of writing software. The *outer loop* is everything that surrounds it: requirements gathering, integration testing, compliance review, deployment, monitoring, and the slow return of user feedback. The inner loop runs in seconds or minutes. The outer loop runs in hours, days, or weeks.

This model traces back to continuous integration thinking in the DevOps movement, was refined by the [DORA research programme](https://dora.dev/research/) and Google Cloud's developer productivity teams, and has been adopted widely in platform engineering. For years, the inner and outer loops operated in rough equilibrium. A developer might spend four hours coding and two days waiting for integration test results, security review, and deployment. The ratio was frustrating but stable.

AI has broken the equilibrium. The inner loop has been dramatically accelerated — tools like [Claude Code](https://docs.anthropic.com/en/docs/claude-code), [Codex](https://openai.com/codex/), and [GitHub Copilot](https://github.com/features/copilot) can generate functions, entire modules, or test suites in seconds that would have taken a developer hours. The inner loop has never been faster.

The outer loop has barely moved. Integration tests still take twenty minutes. Compliance reviews still wait for a human in a different timezone. Deployment pipelines still gate on manual approval. Security scanning still runs overnight.

As [Rob Zuber](https://www.linkedin.com/posts/robzuber_the-loops-of-software-delivery-are-out-of-activity-7417973946622402560-olHc), CTO of CircleCI, observed in June 2026: *"The loops of software delivery are out of sync. The inner loop, where developers write, debug, and build, has never been faster. But the outer loop — where code is integrated, tested, secured, deployed, and measured — has barely moved."*

The result is a traffic jam. Code is generated faster than it can be validated, integrated, and deployed. Organisations that poured AI into the inner loop without re-engineering the outer loop discovered, sometimes painfully, that they had accelerated their way into a bottleneck. The bottleneck shifted downstream — from *writing* to *shipping*.

### Observe, Plan, Act, Reflect: The Agent Loop

The second meaning is newer, more radical, and is the one generating the most heat in mid-2026. It describes a paradigm shift in how developers interact with AI systems — from single-shot prompting to autonomous iterative loops.

In this framing, the developer does not write a prompt and accept whatever comes back. The developer designs a *system* that observes the codebase, plans an action, executes it, reflects on the result, and repeats — autonomously, over hours or days, until a verifiable goal is met. The human is not in the middle of each iteration. The human is *on the loop*, watching the system from above, intervening only at pre-defined boundaries.

The intellectual lineage is clear enough. The [ReAct pattern](https://arxiv.org/abs/2210.03629), published by Princeton and Google DeepMind researchers in 2022, demonstrated that interleaving reasoning steps with action steps — think, act, observe, think again — substantially improved model performance on complex tasks. [AutoGPT](https://github.com/Significant-Gravitas/AutoGPT) and [BabyAGI](https://github.com/yoheinakajima/babyagi) in early 2023 proved, crudely and expensively, that chaining LLM calls in autonomous loops could produce results beyond what single prompts could achieve. The community then moved through a sequence of refinements: prompt engineering (2022–2024: *"What should I say?"*), context engineering (2025: *"What information do I provide?"*), and finally harness engineering (2026: *"What system do I build?"*).

The progression is not merely terminological. It marks a genuine shift in where intelligence resides. In prompt engineering, intelligence is in the text. In harness engineering, intelligence is in the *architecture* — the tools, gates, guardrails, and feedback mechanisms that surround the model and shape its behaviour over time. The model becomes a component, not the centre.

---

## A Literature Survey: What Worked, What Didn't, and What the Failures Reveal

Before constructing anything, it is worth surveying the field evidence systematically. The stories that follow are drawn from the public record — Hacker News posts, company blogs, conference talks, and the broader discourse — and they are selected not for novelty but for what each reveals about the conditions under which loop engineering succeeds or fails. The pattern that emerges is more instructive than any individual story.

### 1. Anthropic — Internal Engineering at Scale

[Boris Cherny](https://twitter.com/bcherny), Head of Claude Code, has disclosed in talks and online discussions throughout 2025–2026 that Anthropic engineers using harness engineering ship **eight times more code daily** than before, and that by May 2026 Claude authored **over 80% of merged production code** at the company. The specific pattern Cherny credits is the *self-verification loop*: the agent runs its own output through deterministic checks, observes failures, and self-corrects before a human ever sees the code. He claims a **2–3× quality improvement** from this pattern alone.

| What worked | What didn't | Insight |
|---|---|---|
| Self-verification loop (agent validates own output against deterministic gates) | Single-shot prompting at scale — too many rejections, too much rework | The agent must be able to observe failure and correct itself. Without this, the human becomes the bottleneck reviewing every line. |
| Harness as shared infrastructure across the engineering org | Implicit harnesses that lived in individual engineers' heads | A harness must be explicit and shared to scale beyond a single developer. |

The numbers are not independently verified and come from the organisation with the strongest incentive to make the case. But they are consistent with everything the independent stories demonstrate at smaller scale. The architecture works. The question is not whether — it is what it costs to build well.

### 2. envref — The Purest Public Demonstration

In February 2026, a single developer posted to Hacker News about [envref](https://news.ycombinator.com/item?id=47042109), a Go CLI tool for managing environment variable references across seven secret backends. The developer did not write the tool. They defined a goal, set up an agent loop with Claude Code (Opus 4.6), and walked away.

| Metric | Value |
|---|---|
| Iterations | 77 autonomous agent iterations |
| Tokens consumed | ~192 million |
| API cost | **$170** |
| Compute time | **8 hours 41 minutes** |
| Human intervention after goal definition | Zero |
| Output | Full Go CLI with fuzz tests, schema validation, documentation site, and a plugin protocol |

The loop terminated not because the agent decided it was done, but because every item in the backlog had been implemented and every gate — compiler, linter, test suite — passed.

| What worked | What didn't | Insight |
|---|---|---|
| Verifiable backlog items as termination conditions | — (the run was successful) | The goal *is* the termination condition. Vague goals produce infinite loops. |
| Deterministic gates (Go compiler, linter, test suite) | — | Every gate was a binary check. No human judgment was needed to determine completion. |

What the story does not capture — because it is not visible from the outside — is the harness design work that preceded the run. The goal did not write itself. The backlog was structured for autonomous execution. The verification gates were defined. The human-approval boundaries were set. The eight hours and forty-one minutes of autonomous work were the *output* of a harness; the harness itself required design.

### 3. Altimate Code — The Harness as Empirical Advantage

In March 2026, [Altimate Code](https://news.ycombinator.com/item?id=47438930) — founded by Anand and Pradnesh, who previously built dbt Power User and Datamates VS Code extensions with over 750,000 combined installs — posted a direct A/B test on the [dbt Labs ADE-bench](https://github.com/dbt-labs/ade-bench). The results were striking:

| Setup | ADE-bench Score |
|---|---|
| **Altimate Code** (Sonnet 4.6 + compiled Rust harness) | **74.4%** |
| Cortex Code / Snowflake (Opus 4.6) | 65% |
| Claude Code baseline (Sonnet 4.6, no harness) | ~40% |

The harness includes: deterministic column-level lineage at 0.26 milliseconds per query across a 500,000-row benchmark, 26 SQL anti-pattern rules with zero false positives, local schema validation in 2 milliseconds without touching the warehouse, and engine-level permission enforcement. Every component is deterministic. None involves a model call.

| What worked | What didn't | Insight |
|---|---|---|
| Deterministic tools (compiled Rust, sub-millisecond) | Reliance on model capability alone | A cheaper model with compiled tools outperforms a more expensive model without them. The gap *is* the harness. |
| Engine-level permission enforcement | Trusting the model to respect boundaries | Permissions must be enforced at the harness level, not requested at the model level. |

The Altimate result is the clearest empirical evidence yet that the harness, not the model, is the binding constraint on quality. The implication is uncomfortable for an industry that has organised itself around model capability rather than system design. Returns on harness investment compound; returns on prompt investment do not.

### 4. Gus and Runtime — The Solo-to-Team Wall

[Gus](https://news.ycombinator.com/item?id=48225040), founder of Runtime (YC P26) and previously of Mentum (YC S21), posted in May 2026 about a pattern that has become one of the most-discussed in the loop engineering discourse — because it is a failure story, and the failures are more instructive than the successes.

As a solo developer, Gus shipped **four full-stack products in three months** using coding agents. The workflow was fluid, high-bandwidth, and effective. When he tried to roll the same workflow to his entire team, it collapsed:

- PRs turned into unmergeable "slop" — generated code that passed no shared quality bar
- Context was trapped in one person's head — the implicit understanding of what "good" meant
- Non-engineers could not safely touch a real codebase — no guardrails existed
- The agents produced code at different quality levels because different team members had different implicit standards

| What worked | What didn't | Insight |
|---|---|---|
| Solo developer + agent loop at high velocity | Rolling the same workflow to a team without shared harness | The solo developer's harness was *implicit* — it lived in his head. Brains do not compose. |
| — | Relying on individual judgment for quality and safety | A team-scale harness must make quality standards explicit, deterministic, and enforced. |

The failure directly led to building Runtime as infrastructure for team-scale agent loops. The fix was to externalise the harness — shared context, deterministic validation, permission boundaries, sandboxed execution — from the individual developer's head into a shared, executable system. Runtime now has a fintech unicorn and several YC scaleups live on the platform; one customer built an **on-call inspector agent** wiring PagerDuty, Sentry, and their repo to autonomously find root causes and open PRs with unit tests before anyone gets paged.

### 5. The OAuth Developer — Four Years of Failure, Three Weeks of Shipping

One of the more striking personal narratives in the discourse is the developer who spent [four years trying to build an OAuth 2.0 server](https://news.ycombinator.com/item?id=46867821) — PKCE, DPoP, MFA, WebAuthn/Passkeys, EU sovereign requirements — and never finished it. After discovering agentic coding loops, they shipped it in **three weeks** with **91% test coverage**.

| What worked | What didn't | Insight |
|---|---|---|
| Agent persistence — the loop did not get tired or lose motivation | Hand-coding over four years — attrition, not incompetence, caused abandonment | OAuth 2.0 with modern extensions is complexity that exhausts a single developer's attention. The loop succeeded not because it was smarter, but because it was more persistent. |
| Autonomous iteration until gates passed | Human attention as the binding constraint on complex projects | Some projects fail not because they are too hard but because they are too *long*. Loop engineering addresses the attention bottleneck directly. |

The story controls for the developer — same person, same project, same domain knowledge. The only variable was the technique. Four years of hand-coding produced an unfinished project. Three weeks of loop engineering produced a shipped product. The difference is not a speedup; it is the difference between completion and abandonment.

### 6. CircleCI Chunk — Moving Validation into the Inner Loop

[CircleCI's Chunk sidecars](https://news.ycombinator.com/item?id=48281284), built by Olaf in the CTO office and posted in May 2026, address a specific failure mode: by the time CI catches a failure in agent-generated code, the agent has already moved on and useful context is gone.

Chunk runs scoped "microbuilds" in lightweight Firecracker microVMs that mirror the CI environment, triggering automatically during agent stop/evaluation events. Validation moves *into* the inner loop, so the agent sees failures while context is still fresh.

| Metric | Result |
|---|---|
| Average microbuild compute | **~27 seconds** |
| Equivalent full CI billable compute | ~5 minutes |
| Token usage reduction in retry loops | **3–5× lower** |

| What worked | What didn't | Insight |
|---|---|---|
| Fast, scoped validation during agent evaluation | Waiting for full CI (minutes) before the agent sees failures | Validation latency determines iteration speed. The faster the feedback, the cheaper and more effective the loop. |
| Firecracker microVMs for sandboxing | Running agent code without isolation | Sandboxing is not optional. It is structural. |

### 7. Loki Mode — Multi-Agent, Research-Backed

[Loki Mode](https://news.ycombinator.com/item?id=46547971), posted in January 2026, is an open-source multi-agent system that orchestrates specialised AI agents to take a PRD to a deployed product with zero human intervention. It implements virtually every scientifically-proven pattern from the 2025–2026 AI agent literature:

- Boris Cherny's **self-verification loop** (Anthropic)
- **CONSENSAGENT** (ACL 2025) — blind review + devil's advocate, producing a 30% reduction in false positives
- **GoalAct** — global planning — skill decomposition — local execution, yielding a 12%+ improvement in success rate
- **Multi-Agent Reflexion** — structured debate between Implementer, Skeptic, Advocate, and Synthesizer roles

| What worked | What didn't | Insight |
|---|---|---|
| Separation of concerns across specialised agents | Single-agent loops for very complex, multi-domain tasks | Different agents can catch each other's errors. The proposer/verifier separation is powerful. |
| Research-backed patterns (CONSENSAGENT, GoalAct, Reflexion) | Ad-hoc multi-agent orchestration without structured debate | The academic literature on multi-agent systems is now mature enough to apply directly. |

### 8. MCP Agent Mail — Cross-Agent Coordination Without a Human

[MCP Agent Mail](https://news.ycombinator.com/item?id=45720416), posted in October 2025, demonstrated that multiple coding agents — Claude Code, Codex, and others — could coordinate across separate repositories by sending structured messages to each other, described by the creator as "like Gmail for coding agents." The agents *"take to this system like a fish to water"*, sending detailed messages, coordinating naturally, giving each other feedback, and pushing back on bad ideas. Different frontier models (Claude + GPT + Gemini) were observed to collaborate in complementary ways without a human in the middle.

| What worked | What didn't | Insight |
|---|---|---|
| Structured message-passing between agents across repo boundaries | Human-mediated hand-offs between agent-produced artefacts | Agents can coordinate without humans — if the communication channel is structured and the protocol is explicit. |
| Multi-model collaboration (different frontier models complementing each other) | Single-model, single-agent setups for cross-cutting changes | Different models have different strengths. A harness that orchestrates multiple models can outperform any single one. |

### 9. Patched — Automating the Outer Loop

[Patched](https://news.ycombinator.com/item?id=41082041) (YC S24), launched in October 2024, automates post-code tasks — code reviews, documentation generation, and patches — through customisable, self-hostable workflows. It directly addresses the outer-loop bottleneck that CircleCI's Zuber identified. An example automated PR generated SDK bindings with complex type information for the Stack Auth project.

| What worked | What didn't | Insight |
|---|---|---|
| Automating post-code tasks (reviews, docs, patches) | Leaving the outer loop as a manual process after accelerating the inner loop | The outer loop must be re-engineered alongside the inner loop, or the bottleneck simply shifts downstream. |

### The Pattern Across the Survey

Look across all nine cases and a clear shape emerges, visible only when the stories are placed side by side:

1. **The individual hero story repeats.** Gus shipping four products in three months. The envref developer building a CLI tool autonomously for $170. The OAuth developer finishing a four-year project in three weeks. These are real, verifiable, and structurally similar: one person, one explicit or implicit harness, one goal, autonomous execution.

2. **The team scaling crash is the rule, not the exception.** Every founder who succeeded solo hit a wall when rolling the same workflow to a team — Gus at Runtime, the pattern that Altimate Code's founders observed at Fortune 500 data estates, the implicit standards problem that Anthropic solved by making harnesses explicit and shared. The fix is always the same: externalise the harness from the individual developer's head into a shared, executable system.

3. **Deterministic beats probabilistic, every time.** Altimate Code's A/B test is the cleanest evidence, but the pattern is visible everywhere. envref's Go compiler and test suite. CircleCI's microbuild sidecars. The self-verification loop that Cherny describes. In every case, the highest-leverage investment is not a better model — it is a faster, cheaper, more reliable verification gate.

4. **The infrastructure layer is emerging, fast.** Runtime, CircleCI Chunk, Altimate Code, Patched, Loki Mode, MCP Agent Mail — a Cambrian explosion of tooling all solving different slices of the same problem: how to make autonomous loops safe, reliable, and team-scalable. The market is voting that this is not a fad.

---

## The Architecture of a Harness

If harness engineering is the right frame — and the survey above suggests it is — the next question is practical: what does a harness actually contain? The answer converges on a small set of elements.

{{< note title="A note on the code that follows." >}}
The code blocks in this section are simplified illustrations designed to convey the architecture clearly. The [companion repository](https://github.com/graeme-lockley/loop-harness) contains the full, working implementation --- with async model calls, workspace file walking, progress detection, custom-scoped gates, and operational features not shown here. The code below teaches the concepts; the repo runs them.
{{< /note >}}

### The Goal as Termination Condition

The first and most important element is the *goal* — and the goal must be verifiable. A vague goal — *"make the auth system better"* — produces an infinite loop. A well-formed goal specifies acceptance criteria that a deterministic system can check:

{{< note title="Example: verifiable acceptance criteria" >}}
- Add JWT refresh token rotation.
- All 47 tests in `tests/auth/` must pass.
- ESLint must report zero lint violations.
- `tsc --noEmit` must report zero type errors.
- The `/auth/refresh` endpoint must return 401 for revoked tokens.
- New refresh tokens must have a different `jti` than the old one.
- Access token expiry must not exceed 15 minutes.
{{< /note >}}

Every criterion is binary. Pass or fail. No human judgment is required to determine whether the loop has succeeded — and that is precisely what allows the loop to run autonomously. This is the envref pattern made explicit: the goal *is* the termination condition.

### Deterministic Tools: The Highest-Leverage Investment

The second element is a set of *deterministic tools* — compiled code that the agent can invoke, which runs in milliseconds, costs nothing per invocation, and never hallucinates. A linter. A test runner. A type checker. A grep for leftover debug statements. Git diff. The [Altimate Code](https://news.ycombinator.com/item?id=47438930) A/B test — cheaper model with compiled tools outperforming a more expensive model without — is the empirical foundation: the harness, not the model, is the binding constraint.

This is why every practical harness begins with a tool layer that looks something like the following:

```typescript
// tools.ts — Deterministic tools. These are the bedrock.
// Every tool returns a structured result so the loop engine
// can branch on success/failure without a model call.

import { execSync } from "node:child_process";
import { readFileSync, writeFileSync } from "node:fs";

interface ToolResult {
  ok: boolean;
  data: Record<string, unknown>;
  error?: string;
}

function runLinter(paths: string[] = ["."]): ToolResult {
  // Zero-hallucination linting. Deterministic, fast, free.
  try {
    const stdout = execSync(
      `npx eslint ${paths.join(" ")} --format json`,
      { timeout: 30_000, encoding: "utf-8" }
    );
    const violations = JSON.parse(stdout || "[]");
    return {
      ok: violations.length === 0,
      data: { violations, count: violations.length },
    };
  } catch (err: any) {
    if (err.stdout) {
      const violations = JSON.parse(err.stdout);
      return { ok: false, data: { violations, count: violations.length } };
    }
    return { ok: false, data: {}, error: String(err) };
  }
}

function runTests(testPath: string = "tests/"): ToolResult {
  // Agent validates its own work. No model in the loop.
  try {
    execSync(`npx vitest run ${testPath} --reporter json`, {
      timeout: 120_000, encoding: "utf-8",
    });
    return { ok: true, data: { passed: true } };
  } catch (err: any) {
    return {
      ok: false, data: { passed: false },
      error: err.stderr?.slice(0, 2000) ?? String(err),
    };
  }
}

function checkTypeSafety(): ToolResult {
  // The agent cannot argue with tsc.
  try {
    execSync("npx tsc --noEmit", { timeout: 60_000, encoding: "utf-8" });
    return { ok: true, data: { errors: "" } };
  } catch (err: any) {
    return {
      ok: false,
      data: { errors: err.stdout?.slice(0, 3000) ?? String(err) },
    };
  }
}

function gitDiff(): ToolResult {
  try {
    const stdout = execSync("git diff --stat", {
      timeout: 10_000, encoding: "utf-8",
    });
    return {
      ok: true,
      data: { diffStat: stdout, filesChanged: stdout.split("\n").length },
    };
  } catch (err) {
    return { ok: false, data: {}, error: String(err) };
  }
}

function readFile(path: string): ToolResult {
  try {
    const content = readFileSync(path, "utf-8");
    return { ok: true, data: { content } };
  } catch (err) {
    return { ok: false, data: {}, error: String(err) };
  }
}

function writeFile(path: string, content: string): ToolResult {
  try {
    writeFileSync(path, content, "utf-8");
    return { ok: true, data: { written: true } };
  } catch (err) {
    return { ok: false, data: {}, error: String(err) };
  }
}
```

Each of these is a pure function of the filesystem plus arguments. No model in the loop. No hallucination possible. The agent can call these freely — thousands of times over the course of a multi-hour run — without burning tokens or introducing errors. The cost per invocation is effectively zero. The latency is measured in milliseconds.

A linter that catches an unused variable in 200 milliseconds, deterministically, costs nothing. A model call that might catch the same thing — but might not, and might hallucinate a fix for a problem that does not exist — costs tokens, time, and trust.

### Verification Gates: The Self-Correction Engine

The third element is a set of *verification gates*. Each gate is a fast, deterministic check that the agent runs against its own output after every action. If a gate fails, the failure message is structured to be *actionable* — the agent sees what failed, why, and can self-correct on the next iteration. This is the self-verification loop that [Boris Cherny](https://twitter.com/bcherny) credits with a 2–3× quality improvement at Anthropic.

The loop runs like this:
```
Agent writes code
    ↓
Agent runs: npx eslint .   — deterministic, $0, 200ms
    ↓
FAIL: "line 47: 'tokenId' is assigned a value but never used"
    ↓
Agent sees the failure message, fixes line 47
    ↓
Agent runs: npx eslint . again
    ↓
PASS
    ↓
Agent moves to next gate: npx vitest run tests/auth/
    ↓
...and so on, until ALL gates pass
```

No human in the middle. The agent corrects itself. The human only reviews the *harness* — is this gate catching the right things? Should we add a gate for this class of error we keep seeing?

The gates are assembled into a harness definition:

```typescript
// harness.ts — The thing you actually iterate on.
// When output is bad, fix the harness — not the artefact.

const authRefreshHarness: Harness = {
  name: "auth-refresh-rotation",
  tools: [
    { name: "run_linter",  kind: "deterministic", fn: () => runLinter(["."]) },
    { name: "run_tests",   kind: "deterministic", fn: (p = "tests/") => runTests(p) },
    { name: "check_types", kind: "deterministic", fn: checkTypeSafety },
    { name: "git_diff",    kind: "deterministic", fn: gitDiff },
    { name: "read_file",   kind: "deterministic", fn: readFile },
    { name: "write_file",  kind: "deterministic", fn: writeFile },
  ],
  gates: [
    {
      name: "lint-clean",
      check: () => runLinter(["."]).ok,
      messageOnFail: "ESLint found violations. Fix them.",
      isBlocking: true,
    },
    {
      name: "all-tests-pass",
      check: () => runTests("tests/").ok,
      messageOnFail: "Tests are failing. Fix the code, not the tests.",
      isBlocking: true,
    },
    {
      name: "type-safe",
      check: () => checkTypeSafety().ok,
      messageOnFail: "tsc found type errors.",
      isBlocking: true,
    },
  ],
  requireHumanApprovalFor: ["git push", "npm publish", "database migrate"],
};
```

### The Loop Engine

The fourth element is the loop engine itself — the mechanism that sequences observe, plan, act, and reflect. It is deliberately simple. The intelligence is in the harness; the loop engine is plumbing.

```typescript
// engine.ts — Observe -> Plan -> Act -> Reflect -> Repeat.
// The complexity belongs in the harness. The engine is plumbing.

interface Observation {
  workspaceFiles: string[];
  lastToolResults: Record<string, unknown>;
  currentDiff: string | null;
  gateStatuses: Record<string, boolean>;
  elapsedIterations: number;
  totalCostSoFar: number;
}

interface Plan {
  reasoning: string;
  action: string;
  actionArgs: Record<string, unknown>;
  expectedOutcome: string;
}

interface IterationLog {
  iteration: number;
  observation: Observation;
  plan: Plan;
  actionResult: unknown;
  gateResults: Record<string, boolean>;
  costThisIteration: number;
  timestamp: number;
}

interface LoopResult {
  success: boolean;
  reason: string;
  iterations: number;
  log: IterationLog[];
}

class LoopEngine {
  private log: IterationLog[] = [];
  private totalCost = 0;
  private startTime = Date.now();

  constructor(
    private harness: Harness,
    private goal: Goal,
    private modelInvoke: (prompt: string) => Plan,
    private workspacePath = "."
  ) {}

  run(): LoopResult {
    for (let i = 1; i <= this.goal.maxIterations; i++) {
      // GUARDRAIL: budget check
      if (this.totalCost > this.goal.maxCostDollars) {
        return {
          success: false,
          reason: `Cost limit exceeded: $${this.totalCost.toFixed(2)}`,
          iterations: i,
          log: this.log,
        };
      }

      // 1. OBSERVE
      const obs = this.observe(i);

      // 2. PLAN
      const plan = this.plan(obs);

      // 3. ACT (with human-approval guardrail)
      if (this.harness.requireHumanApprovalFor.includes(plan.action)) {
        const approved = this.requestHumanApproval(plan);
        if (!approved) continue;  // human said no; re-plan
      }
      const result = this.act(plan);

      // 4. REFLECT (run all verification gates)
      const gateResults = this.runGates();

      // Record iteration
      const costThisIteration = this.estimateCost(plan, result);
      this.totalCost += costThisIteration;
      this.log.push({
        iteration: i,
        observation: obs,
        plan,
        actionResult: result,
        gateResults,
        costThisIteration,
        timestamp: Date.now(),
      });

      // 5. CHECK: are we done?
      if (Object.values(gateResults).every(Boolean)) {
        return {
          success: true,
          reason: "All verification gates pass",
          iterations: i,
          log: this.log,
        };
      }
    }

    return {
      success: false,
      reason: `Max iterations (${this.goal.maxIterations}) reached`,
      iterations: this.goal.maxIterations,
      log: this.log,
    };
  }

  private observe(iteration: number): Observation {
    const files: string[] = [];
    // Walk workspace, collect file paths (elided for brevity)
    return {
      workspaceFiles: files.slice(0, 200),
      lastToolResults: {},
      currentDiff: this.runToolSafe("git_diff") as string | null,
      gateStatuses: Object.fromEntries(
        this.harness.gates.map(g => [g.name, g.check()])
      ),
      elapsedIterations: iteration,
      totalCostSoFar: this.totalCost,
    };
  }

  private plan(obs: Observation): Plan {
    const prompt = this.buildPlanningPrompt(obs);
    return this.modelInvoke(prompt);  // returns structured JSON
  }

  private act(plan: Plan): unknown {
    const tool = this.harness.tools.find(t => t.name === plan.action);
    if (!tool) return { ok: false, data: {}, error: `Unknown tool: ${plan.action}` };
    try {
      return tool.fn(plan.actionArgs);
    } catch (err) {
      return { ok: false, data: {}, error: String(err) };
    }
  }

  private runGates(): Record<string, boolean> {
    const results: Record<string, boolean> = {};
    for (const gate of this.harness.gates) {
      try {
        results[gate.name] = gate.check();
        if (!results[gate.name]) {
          console.log(`GATE FAIL [${gate.name}]: ${gate.messageOnFail}`);
        }
      } catch (err) {
        results[gate.name] = false;
        console.log(`GATE ERROR [${gate.name}]: ${err}`);
      }
    }
    return results;
  }

  private buildPlanningPrompt(obs: Observation): string {
    const failedGates = Object.entries(obs.gateStatuses)
      .filter(([, ok]) => !ok)
      .map(([name]) => name);

    return `You are an agent in a self-correcting development loop.

## GOAL
${this.goal.description}

## ACCEPTANCE CRITERIA (ALL must pass)
${this.goal.acceptanceCriteria.map(c => `- ${c}`).join("\n")}

## CURRENT STATE
- Iteration: ${obs.elapsedIterations}/${this.goal.maxIterations}
- Cost spent: $${obs.totalCostSoFar.toFixed(4)}
- Files in workspace: ${obs.workspaceFiles.length}

## FAILED VERIFICATION GATES
${failedGates.length ? failedGates.join(", ") : "(none — all gates passing)"}

## AVAILABLE TOOLS
${this.harness.tools.map(t => `- ${t.name}: ${t.description}`).join("\n")}

## INSTRUCTION
Based on the current state, output ONE next action as JSON:
{
  "reasoning": "why this action",
  "action": "tool_name",
  "actionArgs": {},
  "expectedOutcome": "what you expect to observe after this action"
}

If ALL gates are passing and you believe the goal is achieved,
use action "complete" with empty args.`;
  }

  private requestHumanApproval(plan: Plan): boolean {
    console.log("\n" + "=".repeat(60));
    console.log("HUMAN APPROVAL REQUIRED");
    console.log(`Action: ${plan.action}`);
    console.log(`Args: ${JSON.stringify(plan.actionArgs, null, 2)}`);
    console.log(`Reasoning: ${plan.reasoning}`);
    console.log("=".repeat(60));
    // In production, this would pause and wait for a human response
    return false;  // safe default: deny if unattended
  }

  private runToolSafe(toolName: string): unknown {
    const tool = this.harness.tools.find(t => t.name === toolName);
    if (!tool) return null;
    try { return tool.fn({}); } catch { return null; }
  }

  private estimateCost(_plan: Plan, _result: unknown): number {
    // Deterministic tools cost $0; model calls cost ~$0.02
    const tool = this.harness.tools.find(t => t.name === _plan.action);
    if (tool && tool.kind === "deterministic") return 0;
    return 0.02;  // placeholder
  }
}
```

The engine enforces budgets — cost per iteration, time per iteration, total cost for the run — and logs every iteration for auditability. The planning step is the only point at which the model is called, and its output is structured: not free text, but JSON specifying a tool name and arguments. This keeps the loop on rails. The [envref](https://news.ycombinator.com/item?id=47042109) run — 77 iterations, 192 million tokens, $170 — was only possible because the loop engine enforced budgets and the goal provided a clear termination condition.

### Humans On the Loop

The fifth element is a set of *human-approval boundaries* — pre-defined actions that the loop may not execute without explicit human consent. This is the pattern [Kief Morris](https://martinfowler.com/articles/exploring-gen-ai/humans-and-agents.html) of Thoughtworks, writing on Martin Fowler's site in March 2026, called *"humans on the loop"* as distinct from *"humans in the loop"* (reviewing every line) or *"humans out of the loop"* (vibe-coding with no oversight).

The distinction matters. A human who reviews every line of AI-generated code becomes the bottleneck — precisely the problem the loop is supposed to solve. A human who reviews nothing is abdicating responsibility — the pattern Gus observed when non-engineers couldn't safely touch the codebase. A human who designs the harness, monitors the loop, and intervenes only at dangerous boundaries is occupying the right level of abstraction: system architect, not code reviewer.

The boundaries are simple: never auto-push to main. Never auto-publish a package. Never auto-deploy to production. Never auto-migrate a database. The loop runs freely until it hits one of these walls, at which point it pauses and asks. This is the externalisation of the judgment that [Gus](https://news.ycombinator.com/item?id=48225040) kept in his head — made explicit, shared, and enforceable.

---

## What the Technique Demands

Loop engineering is not free. The technique demands things that the current generation of development practice is not well organised to provide.

### Token Costs Are Real

The envref run consumed 192 million tokens at a cost of $170. That is for one CLI tool. Larger projects will consume more. The economics are favourable compared to developer time — $170 for a complete Go tool is, by any reasonable measure, a bargain — but they are not negligible, and they are variable. A poorly specified goal can produce a loop that spins for hours, consuming tokens without converging. Budget enforcement is not optional. It is structural.

### Vague Goals Produce Infinite Loops

The goal is the load-bearing element of the entire architecture. A goal that cannot be verified deterministically cannot serve as a termination condition. *"Make the code better"* is not a goal. *"All tests pass and lint is clean"* is a goal. The difference is not length; it is falsifiability.

This demands a discipline that many teams do not have. Writing verifiable acceptance criteria is a skill. It is learnable, but it is not widespread. Organisations that adopt loop engineering without investing in this skill will produce loops that run until the budget runs out, not until the work is done. The envref developer succeeded because the backlog was structured for autonomous verification. The OAuth developer succeeded because the goal — an OAuth 2.0 server passing a comprehensive test suite — was inherently verifiable. Gus's team initially failed because the quality bar was implicit and varied from person to person.

### Auditability Shifts from Code to Trajectory

When a human writes code, the audit trail is the version history — diffs, commit messages, pull request discussions. When an agent loop writes code, the audit trail is the *trajectory* — the sequence of observations, plans, actions, and gate results that produced each commit. The code alone does not tell you why a particular design decision was made. The trajectory does.

This shift has implications for review practices, for incident response, and for institutional memory. A team that adopts loop engineering without also adopting trajectory logging is building without a memory — and when something goes wrong, as it eventually will, the absence of a trajectory log will make root cause analysis substantially harder than it needs to be. The envref run logged all 77 iterations. Anthropic's internal harness logs every self-verification cycle. This is not accidental. It is necessary.

### The Outer Loop Remains

Loop engineering solves the inner-loop bottleneck. It does not solve the outer-loop bottleneck. Code that is generated, tested, and verified autonomously still needs to be integrated, compliance-reviewed, deployed, and monitored. Those processes have not been re-engineered for AI speed, and until they are, the traffic jam simply shifts downstream — from writing to shipping, as Rob Zuber identified.

[CircleCI's Chunk sidecars](https://news.ycombinator.com/item?id=48281284) — lightweight microVMs running scoped microbuilds in ~27 seconds — move validation into the inner loop but do not solve the broader outer-loop problem. [Patched](https://news.ycombinator.com/item?id=41082041) automates post-code tasks but does not re-engineer compliance or deployment. [Runtime](https://news.ycombinator.com/item?id=48225040) provides team-scale harness infrastructure but does not touch the deployment pipeline. The full promise of loop engineering materialises only when both loops are re-engineered together, and that work is only beginning.

### Skillset Displacement Is Real

Loop engineering changes what it means to be a developer. The core activity shifts from *writing code* to *designing systems that write code*. The skills that matter are no longer primarily syntax, algorithms, and debugging — though these remain valuable for harness design — but system architecture, verification design, and the ability to specify goals precisely enough for an autonomous agent to execute against.

This shift will be uncomfortable for many developers, particularly those who have invested heavily in the craft of hand-coding. The historical pattern [I have written about elsewhere](https://graeme-lockley.github.io/20260314-ai-adoption-patterns/) — surgeons resisting anaesthesia because it devalued their speed-based skill, master weavers burning power looms because they threatened a lifetime's craft — is repeating. The resistance is not irrational, but neither is it likely to prevent the transition. As [Addy Osmani](https://twitter.com/addyosmani) of Google put it in 2026: *"I'm skeptical, and you absolutely have to be careful."* The question for individual developers is not whether the transition will happen, but whether they will participate in shaping the harnesses or be shaped by them.

---

## Conclusion: The Harness Is the Artefact

The discourse around AI-assisted development has spent three years focused on the model — its capabilities, its limitations, its trajectory. That focus was appropriate when the model was the binding constraint. It is no longer appropriate. The binding constraint is now the system around the model: the tools it can invoke, the gates that verify its output, the guardrails that prevent it from doing harm, and the goals that tell it when to stop.

The stories surveyed above all point to the same conclusion. The [envref](https://news.ycombinator.com/item?id=47042109) developer did not succeed because of a clever prompt. [Altimate Code](https://news.ycombinator.com/item?id=47438930) did not outperform a more expensive model because of a better model. [Gus's](https://news.ycombinator.com/item?id=48225040) team did not fail because the model was insufficient. The [OAuth developer](https://news.ycombinator.com/item?id=46867821) did not ship in three weeks because the model was a better programmer than he was. Anthropic's 8× output gain did not come from a model upgrade. In every case, the difference was the harness.

Building a harness is not a research problem. It is an engineering problem, and the patterns are becoming clear: deterministic tools, verification gates, structured planning, human-approval boundaries, cost and time budgets, trajectory logging. The minimum viable harness — a goal, three deterministic gates, one safety boundary, a loop engine — takes perhaps three hours to construct. It will not be perfect. But it will run, and it will improve with use, because the harness is the artefact you iterate on when output is wrong.

The engineers who will thrive in this transition are not the ones who write the cleverest prompts. They are the ones who design the most elegant, reliable, self-correcting systems. The loop is not the point. The harness is.