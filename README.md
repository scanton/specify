# specify — Specification Engineering for Claude

A Claude Cowork skill that walks you through the **5 Pillars of Specification Engineering** to turn a rough requirement into a complete, airtight specification. Works for AI agent specs, human task briefs, product requirements, and any workflow you want to delegate reliably.

<img width="2752" height="1536" alt="image" src="https://github.com/user-attachments/assets/e18e4bf8-d779-42cb-a7fd-aca49d5a44ff" />

---

## What It Does

Most AI task failures aren't model failures. They're specification failures. When you hand an agent (or a person) a vague brief, you get a vague result. This skill fixes that by treating specification writing as its own discipline.

`specify` guides you through a structured conversation. You bring a rough requirement. The skill asks the right questions, drafts each section, and reviews it with you before moving on. The output is a complete `.md` specification file that any executor — AI agent or human — can use as a standalone brief without needing to ask clarifying questions.

---

## The 5 Pillars

**1. Self-Contained Problem Statement**
State the problem with enough context that the task is plausibly solvable without going out to get more information. A useful test: if you handed this to someone who had never heard of your company, could they understand and attempt the task?

**2. Acceptance Criteria**
Three sentences that a completely independent observer could use to verify whether the output is correct, without asking a single clarifying question. Not process steps — observable properties of the output.

**3. Constraint Architecture**
The operating envelope for the executor. What it must do, what it cannot do, what it should prefer when multiple valid approaches exist, and what it should escalate rather than decide autonomously. This is where oversight gets embedded before execution starts.

**4. Decomposition**
Large tasks broken into sub-tasks that can be executed independently, tested independently, and integrated predictably. The third criterion (predictably integrable) is the one people forget.

**5. Evaluation Design**
Three to five test cases with known good outputs. Run them periodically, especially after model updates. This is what catches regressions before they reach production.

---

## Installation

### Requirements

- [Claude Desktop](https://claude.ai/download)
- Cowork mode enabled (beta feature)
- A workspace folder selected in Cowork

### Steps

1. Clone or download this repository:

```bash
git clone https://github.com/[your-username]/claude-skills.git
```

2. In your Claude Cowork workspace folder, create the skills directory if it doesn't already exist:

```bash
mkdir -p /path/to/your/workspace/.skills/skills
```

3. Copy the `specify` skill folder into the skills directory:

```bash
cp -r claude-skills/specify /path/to/your/workspace/.skills/skills/
```

Your workspace structure should look like this:

```
your-workspace/
└── .skills/
    └── skills/
        └── specify/
            └── SKILL.md
```

4. Open (or reopen) your workspace folder in Claude Desktop's Cowork mode. The skill will be automatically detected and available.

### Triggering the Skill

Once installed, the skill activates on natural language. Try any of these:

- "Help me spec this out"
- "Write a spec for [requirement]"
- "I need to specify this workflow"
- "Turn this into a spec"
- "Spec this requirement"

Or just describe a task you want an AI agent or team member to execute without real-time oversight, and the skill will recognize the intent.

---

## Output

The skill produces a structured `.md` file saved to your workspace. The file follows this format:

```
# Specification: [Task Name]

Executor: [AI agent / human / both]
Version: [date]
Author: [your name]

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

---

## Background

This skill comes out of a broader framework for thinking about AI adoption called Specification Engineering. The core insight is that real-time oversight of an AI model doesn't work for long-running agents. The oversight has to be embedded upfront, in the specification itself. The spec is the supervision.

Read more: https://medium.com/@satoricanton/the-spec-is-the-system-2492d38a4b75

---

## File Structure

```
specify/
├── SKILL.md      # The skill definition — loaded by Cowork
└── README.md     # This file
```

---

## License

MIT
