---
name: specify
description: Walk through the 5 Pillars of Specification Engineering to turn a rough requirement into a complete, airtight specification. Use this skill whenever the user has a task, feature, workflow, or agent behavior they want to fully spec out — whether the spec is intended for an AI agent or a human executor. Trigger on phrases like "help me spec this out", "write a spec for", "I need to specify", "turn this into a spec", "spec this requirement", or when the user describes a task they want an AI agent or team member to execute without real-time oversight.
---

# Specify — Specification Engineering Workflow

This skill guides the user through the 5 Pillars of Specification Engineering to produce a complete, self-contained specification. The goal is a document that gives any executor — AI agent or human — everything they need to do the work correctly without needing to ask clarifying questions or make autonomous judgment calls.

The 5 Pillars are:

1. **Self-Contained Problem Statement** — enough context that the task is plausibly solvable without going out to get more information
2. **Acceptance Criteria** — 3 verifiable sentences an independent observer can use to confirm the output is correct
3. **Constraint Architecture** — what the executor must do, cannot do, should prefer, and should escalate
4. **Decomposition** — sub-tasks that can be executed, tested, and integrated independently
5. **Evaluation Design** — 3-5 test cases with known good outputs to validate correctness now and catch regressions later

---

## Step 0: Intake

Start by getting the raw requirement. The user may have given it already in the chat. If so, extract it and confirm. If not, ask:

> "Give me the requirement — it can be rough. What's the task, feature, or behavior you want to specify?"

Also ask:

> "Who or what is the executor for this spec? An AI agent, a human, or both?"

This shapes how you phrase constraints and acceptance criteria — AI agents need explicit guardrails; humans need procedural clarity.

Once you have the requirement and executor type, summarize what you've understood in 2-3 sentences and ask the user to confirm before continuing.

---

## Step 1: Self-Contained Problem Statement

**Goal:** A clear statement of the problem that includes all context needed to understand and attempt the task — no assumed prior knowledge, no references to external conversations, no gaps that require follow-up.

Ask the user:

1. What is the end state we want to reach? (Not the steps — the outcome.)
2. What inputs or resources does the executor have access to?
3. What does the output look like? (File, action, decision, content — be specific.)
4. Who is the downstream consumer of the output?
5. What background context would a brand new person need to understand why this matters?

After gathering answers, draft the **Problem Statement** section in the spec.

Format:

```
## Problem Statement

[2-4 paragraphs covering: the task goal, the context, the inputs available, and the expected output. Self-contained — no external references required.]
```

Review it with the user. Ask: "If someone had no context from our conversation and only read this, could they understand and attempt the task?" Refine until the answer is yes.

---

## Step 2: Acceptance Criteria

**Goal:** 3 sentences that a completely independent observer — someone who wasn't in this conversation — could use to verify whether the output is correct. Each sentence must be checkable without asking any questions.

Explain this to the user:

> "Acceptance criteria aren't about the process — they're about the output. An independent observer should be able to read the output, read your 3 criteria, and issue a pass/fail with no ambiguity."

Ask the user:

1. What does a correct output look like vs. an incorrect one?
2. Are there any edge cases where the output might look right but be wrong?
3. What are the most common failure modes?

Draft 3 acceptance criteria. Each should:

- Name a specific, verifiable property of the output
- Avoid vague qualifiers ("good", "complete", "appropriate")
- Be checkable without human judgment wherever possible

Format:

```
## Acceptance Criteria

1. [Specific, observable, verifiable condition.]
2. [Specific, observable, verifiable condition.]
3. [Specific, observable, verifiable condition.]
```

Review with the user. Ask: "Could an observer apply each of these criteria without asking you a single follow-up question?" Refine until yes.

---

## Step 3: Constraint Architecture

**Goal:** Define the operating envelope for the executor. This is where oversight gets embedded — not in real-time supervision, but in pre-defined rules that guide behavior before execution starts.

Gather answers across 4 dimensions:

**Must Do:**

- "What are the non-negotiable requirements? Things the executor absolutely has to do, every time?"

**Cannot Do:**

- "What is off-limits? Actions, decisions, or outputs the executor should never take regardless of how reasonable they might seem?"

**Should Prefer:**

- "When multiple valid approaches exist, what should the executor lean toward? (e.g., conservative over aggressive, simple over clever, using existing tools before building new ones)"

**Should Escalate:**

- "What situations should the executor stop and flag rather than attempt to decide autonomously? What's the threshold for 'I shouldn't make this call myself'?"

Draft the **Constraints** section:

```
## Constraint Architecture

### Must Do
- [Requirement 1]
- [Requirement 2]

### Cannot Do
- [Prohibition 1]
- [Prohibition 2]

### Prefer When Multiple Approaches Are Valid
- [Preference 1]
- [Preference 2]

### Escalate Rather Than Decide
- [Escalation trigger 1]
- [Escalation trigger 2]
```

Note: for AI agent specs, escalation should include how to signal a stop (e.g., write a flag file, return a specific error, surface a message to a human-in-the-loop). Ask the user what the escalation mechanism is if it's an agent spec.

---

## Step 4: Decomposition

**Goal:** Break the full task into sub-tasks that satisfy three properties:

1. **Independently executable** — each sub-task can be done without waiting for another to complete
2. **Independently testable** — each sub-task has its own success criteria that can be checked in isolation
3. **Predictably integrable** — combining the completed sub-tasks produces the whole output without re-work

Ask:

1. What are the major phases or chunks of this task?
2. Are there any dependencies between phases? (Some may be sequential — that's fine, but identify them explicitly.)
3. What does "done" look like for each chunk?

Draft the **Decomposition** section. For each sub-task, include:

- Name / label
- What it does
- Input it requires
- Output it produces
- How to test it in isolation

Format:

```
## Decomposition

### Sub-task 1: [Name]
**Does:** [what this component accomplishes]
**Input:** [what it needs to start]
**Output:** [what it produces when done]
**Test:** [how to verify this sub-task in isolation]

### Sub-task 2: [Name]
...
```

If the task is simple enough that decomposition would be forced or artificial (under ~3 steps), say so and write a minimal linear sequence instead. Don't decompose for its own sake.

---

## Step 5: Evaluation Design

**Goal:** Define how we know the output is good — not just for this run, but systematically. Build 3-5 test cases with known good outputs that can be run against the spec periodically, especially after model updates or workflow changes.

Explain to the user:

> "These aren't edge cases — they're representative examples. We want inputs where we already know what a correct output looks like. This lets us verify the spec is being followed correctly now, and catch regressions if something changes later."

Ask:

1. What are 3-5 realistic inputs we could use to test this?
2. For each input, what does a correct output look like? Be specific.
3. Are there any inputs that would be especially revealing — cases where a bad executor would fail but a good one would succeed?

Draft the **Evaluation Design** section:

```
## Evaluation Design

### Test Case 1: [Descriptive Name]
**Input:** [specific input]
**Expected Output:** [specific, verifiable expected output]
**Why this tests well:** [what failure mode this catches]

### Test Case 2: [Descriptive Name]
...
```

Aim for 3-5 test cases. If the user can't think of 3, help them generate plausible examples from the problem statement.

---

## Final Assembly

Once all 5 pillars are drafted, assemble the full specification document and save it as a `.md` file. Name it descriptively based on the task (e.g., `spec-card-generation-agent.md`, `spec-onboarding-email-workflow.md`).

Save to the user's workspace folder at `/sessions/nice-wonderful-hawking/mnt/claude-cowork/` unless they specify another location.

Full document structure:

```
# Specification: [Task Name]

**Executor:** [AI agent / human / both]
**Version:** [date]
**Author:** [user name]

---

## Problem Statement
...

## Acceptance Criteria
...

## Constraint Architecture
...

## Decomposition
...

## Evaluation Design
...
```

After saving, review the full spec with a fresh perspective. Ask:

> "If an executor had only this document — no prior conversation, no context from us — would they be able to complete the task correctly?"

If the answer is no for any section, identify the gaps and offer to fix them.

Then present the file to the user.

---

## Guidance Notes

**Don't rush the pillars.** Each pillar catches a different class of failure. A spec with weak acceptance criteria will fail on ambiguous outputs. A spec without constraint architecture will fail on edge cases. All five matter.

**Identify gaps honestly.** If a pillar can't be completed because the user doesn't yet know the answer, say so explicitly in the spec (e.g., `[TBD — escalation mechanism not yet defined]`). An honest incomplete spec is better than a fabricated complete one.

**For AI agent specs specifically:** Emphasize precision in constraint architecture and evaluation design. Agents don't ask questions mid-task. Every ambiguity becomes a risk. Every missing test case is a blind spot.

**For human specs:** Acceptance criteria and escalation triggers matter most. Humans will fill in gaps with judgment — the spec's job is to define the judgment calls that shouldn't be improvised.
