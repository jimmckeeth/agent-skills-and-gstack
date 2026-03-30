# Building Autonomous Capabilities
## Skills for LLM Coding Agents

Technical presentation for developers with LLM experience but are new to the "Skills" paradigm.

---

## SKILLS.md
### Not just for Claude anymore

* The Agentic SKILLS.md standard was developed by Anthropic
* An open standard used by many/most AI coding agents
* [agentskills.io](https://agentskills.io/) | [github.com/agentskills](https://github.com/agentskills/agentskills)
---

## The Core Concept
### Extending Intelligence

* **Agents**: Autonomous experts for complex, multi-step tasks.
* **Skills**: Procedural "onboarding guides" for specialized domains.
* **Goal**: Transform a general AI into a specialized engineering partner.

---

## Agentic Engineering vs. "Vibe Coding"
### This is not a replacement; it's an upgrade.

* **Vibe Coding**: High-level requests, "fingers crossed" results.
* **Agentic Engineering**: Explicitly defined personas, workflows, and standards.
* **The Reality**: Requires engineers who understand the technical domain to define the boundaries and validate the output.

---

## The "Context Window Tax"
### Why we can't just put everything in one prompt.

* **The Problem**: Large codebases + massive prompts = "Memory Fog."
* **The Solution**: **Progressive Disclosure**.
* **How it works**:
  1. **Metadata**: Always loaded (~100 words).
  2. **Instructions**: Loaded when triggered (<5k words).
  3. **Resources**: Loaded only as needed (docs/scripts).

---

# part 1: Agents
## The Autonomous Experts

---

## What is an Agent?
* An autonomous subprocess that handles tasks independently.
* **Autonomous Work** vs. User Actions (Commands).
* Defined by:
  * **Triggering**: When should it start?
  * **Persona**: Who is it?
  * **Process**: How does it work?

---

## Agent File Structure
### `agents/my-agent.md`

```markdown
---
name: agent-identifier
description: Use this agent when... <example>...</example>
model: inherit
color: blue
tools: ["Read", "Write"]
---

# System Prompt
You are an expert...
Your responsibilities...
Your process...
```

---

## Triggering: The "Aha!" Moment
### How the AI knows to call for help.

* **Reactive Triggering**: User explicitly asks for the agent's expertise.
  * *"Hey, can you review this for security?"*
* **Proactive Triggering**: The agent triggers based on context or tool outputs.
  * *AI sees code was written -> Triggers reviewer agent automatically.*
* **Implementation**: Defined in the `description` with `<example>` blocks.

---

## Anatomy of an Example
```markdown
<example>
Context: User just implemented a new feature
user: "I've added the authentication feature"
assistant: "Great! Let me review the code quality."
<commentary>
Code was written, trigger code-quality-reviewer agent.
</commentary>
assistant: "I'll use the code-quality-reviewer agent."
</example>
```

---

## Designing the System Prompt
### The "Expert Persona"

* **Style**: Use **Second Person** ("You are...", "You will...").
* **Why**: Agents need a persona to make decisions and evaluate quality.
* **Core Responsibilities**: Clear boundaries.
* **Process**: Step-by-step workflow.
* **Quality Standards**: What "good" looks like.

---

# part 2: Skills
## Specialized Procedural Knowledge

---

## What are Skills?
* Modular packages providing specialized knowledge and workflows.
* Think of it as a **Playbook** for a specific domain.
* They provide:
  * Specialized workflows.
  * Tool integrations.
  * Bundled resources.

---

## Agents vs. Skills
### Who is doing what?

| Feature | Agents | Skills |
| :--- | :--- | :--- |
| **Role** | The "Expert Player" | The "Playbook" |
| **Writing Style** | Second Person (Persona) | Imperative (Instructions) |
| **State** | Dynamic / Autonomous | Static / Procedural |
| **Use Case** | Multi-step reasoning | Domain-specific reference |

---

## Bundled Resources
### A Concrete Example: `deploy-bot` skill

```text
skills/deploy-bot/
├── SKILL.md          # How to deploy
├── scripts/
│   └── push_to_s3.sh # Reusable deployment logic
├── references/
│   └── aws_iam.json  # Permissions schema
└── examples/
    └── staging.yaml  # Config template
```

---

## Writing Effective Skills
### The "Procedural Instruction"

* **Style**: Use **Imperative/Infinitive** form.
  * *"Validate the input."* vs. *"You should validate..."*
* **Why**: Skills are instructions to be followed, not a persona to embody.
* **Description**: Third-person trigger phrases.
  * *"This skill should be used when the user asks to..."*

---

## The "Lean" SKILL.md
* Target: 1,500 - 2,000 words.
* **Progressive Disclosure in action**:
  * Detailed patterns → `references/patterns.md`
  * Advanced guides → `references/advanced.md`
  * API docs → `references/api-reference.md`

---

# part 3: The Lifecycle
## Build, Test, Iterate

---

## The Development Workflow
1. **Identify**: Notice a repetitive task or a recurring failure.
2. **Plan**: Define the "Playbook" (Skill) and the "Expert" (Agent).
3. **Act**: Implement the surgical code changes and configurations.
4. **Validate**: Run tests and manually verify the triggering behavior.
5. **Iterate**: Refine triggering examples and system prompts.

---

## Summary: Building for Autonomy
1. **Be Specific**: Avoid vague instructions.
2. **Use Examples**: Show, don't just tell.
3. **Context is King**: Use Skills to keep the window lean.
4. **Iterate**: Observe where Claude fails, then patch it with a Skill.

