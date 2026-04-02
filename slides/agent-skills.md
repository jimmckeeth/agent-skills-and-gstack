# Building Autonomous Capabilities

## Skills for AI/LLM Coding Agents

Jim McKeeth

---

## Who is this for?

### Prerequisites & Audience

- Developers with experience using LLM-based coding tools
- Curious about moving from ad-hoc prompting to **structured, repeatable** agent workflows
- No prior knowledge of Skills or Agents required

---

## SKILLS.md

### Not just for Claude anymore

- The Agent SKILLS.md standard originated from Anthropic
- Now an open standard supported by Claude Code, Codex, Copilot, Gemini CLI, Cursor, Windsurf, and others
- [agentskills.io](https://agentskills.io/) | [github.com/agentskills](https://github.com/agentskills/agentskills)

---

## The Core Concept

### Extending Intelligence

- **Agents**: Autonomous experts for complex, multi-step tasks.
- **Skills**: Procedural "onboarding guides" for specialized domains.
- **Goal**: Transform a general AI into a specialized engineering partner.

---

## Agent Engineering vs. "Vibe Coding"

### Beyond "Vibe Coding" — adding structure to AI-assisted development

- **Vibe Coding**: High-level requests, "fingers crossed" results.
- **Agent Engineering**: Explicitly defined personas, workflows, and standards.
- **The Reality**: Requires engineers who understand the technical domain to define the boundaries and validate the output.

---

## The "Context Window Tax"

### Why we can't just put everything in one prompt

- **The Problem**: Large codebases + massive prompts = "Memory Fog."
- **The Solution**: *Progressive Disclosure*.
- **How it works**:
  1. **Metadata**: Always loaded (~100 words).
  2. **Instructions**: Loaded when triggered (<5k words).
  3. **Resources**: Loaded only as needed (docs/scripts).

---

# Part 1: Agents

## The Autonomous Experts

Now that we understand the "why," let's meet the first building block.

---

## What is an Agent?

- An autonomous subprocess that handles tasks independently.
- **Autonomous Work** vs. User Actions (Commands).
- Defined by:
  - **Triggering**: When should it start?
  - **Persona**: Who is it?
  - **Process**: How does it work?

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

### How the AI knows to call for help

- **Reactive Triggering**: User explicitly asks for the agent's expertise.
  - _"Hey, can you review this for security?"_
- **Proactive Triggering**: The agent triggers based on context or tool outputs.
  - _AI sees code was written -> Triggers reviewer agent automatically._
- **Implementation**: Defined in the `description` with `<example>` blocks.

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

- **Style**: Use **Second Person** ("You are...", "You will...").
- **Why**: Agents need a persona to make decisions and evaluate quality.
- **Core Responsibilities**: Clear boundaries.
- **Process**: Step-by-step workflow.
- **Quality Standards**: What "good" looks like.

---

# Part 2: Skills

## Specialized Procedural Knowledge

Agents are the _who_. Skills are the _how_.

---

## What are Skills?

- Modular packages providing specialized knowledge and workflows.
- Think of it as a **Playbook** for a specific domain.
- They provide:
  - Specialized workflows.
  - Tool integrations.
  - Bundled resources.

---

## Agents vs. Skills

### Who is doing what?

| Feature           | Agents                  | Skills                     |
| :---------------- | :---------------------- | :------------------------- |
| **Role**          | The "Expert Player"     | The "Playbook"             |
| **Writing Style** | Second Person (Persona) | Imperative (Instructions)  |
| **State**         | Dynamic / Autonomous    | Deterministic / Procedural |
| **Use Case**      | Multi-step reasoning    | Domain-specific reference  |

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

- **Style**: Use **Imperative/Infinitive** form.
  - _"Validate the input."_ vs. _"You should validate..."_
- **Why**: Skills are instructions to be followed, not a persona to embody.
- **Description**: Third-person trigger phrases.
  - _"This skill should be used when the user asks to..."_

---

## Skill File Structure

### `skills/my-skill/SKILL.md`

```markdown
---  
name: deploy-bot
description: >-
  Use this skill when the user asks to deploy,
  push to staging, or release to production.
--- 

# Deploy Bot

## Steps

1. Validate the deployment target.
2. Run `scripts/push_to_s3.sh` with the environment flag.
3. Verify the health check endpoint returns 200.

## Constraints

- Never deploy to production without a passing CI build.
```

---

## The "Lean" SKILL.md

- Target: 1,500 - 2,000 words.
- **Progressive Disclosure in action**:
  - Detailed patterns → `references/patterns.md`
  - Advanced guides → `references/advanced.md`
  - API docs → `references/api-reference.md`

---

## Skill Discovery

### How agents find and load skills

- **Project-level**: `.claude/skills/`, `.github/skills/`, `.agents/skills/`
- **User-level**: `~/.claude/skills/`, `~/.copilot/skills/`, `~/.agents/skills/`
- **Loading order**: Project skills override user-level defaults
- **Trigger**: Agent reads skill `description` and activates when context matches

---

# Part 3: The Lifecycle

## Build, Test, Iterate

You have agents and skills. Now let's put them to work.

---

## The Development Workflow

1. **Identify**: Notice a repetitive task or a recurring failure.
2. **Plan**: Define the "Playbook" (Skill) and the "Expert" (Agent).
3. **Act**: Implement the surgical code changes and configurations.
4. **Validate**: Run tests and manually verify the triggering behavior.
5. **Iterate**: Refine triggering examples and system prompts.

---

## Worked Example: A Deploy Skill

### Walking through the lifecycle

1. **Identify**: You notice the team runs the same 5 deploy commands every release.
2. **Plan**: Create a `deploy-bot` skill with a checklist and a reusable shell script.
3. **Act**: Write `SKILL.md` with steps, add `scripts/push_to_s3.sh`, define the trigger description.
4. **Validate**: Ask the agent _"deploy to staging"_ — does it follow the skill?
5. **Iterate**: The agent skipped the health check → add an explicit constraint to `SKILL.md`.

---

## Other Important MD Files

### The broader ecosystem

| File          | Purpose                                                                                                                         | Location            |
| :------------ | :------------------------------------------------------------------------------------------------------------------------------ | :------------------ |
| ~CLAUDE.md~ / **AGENTS.md** | Open standard (Linux Foundation) for agent instructions — supported by Codex, Copilot, Cursor, Gemini CLI, Windsurf, and others | Project root        |
| **SKILLS.md** | Defines reusable skill packages with triggers, workflows, and bundled resources                                                 | `skills/*/SKILL.md` |

- `CLAUDE.md` was Claude-specific, others started to copy, now `AGENTS.md` is the cross-tool standard
- Both are loaded automatically at the start of every agent session
- Skills complement these by providing specialized, on-demand knowledge

---

## Summary: Building for Autonomy

1. **Be Specific**: Avoid vague instructions.
2. **Use Examples**: Show, don't just tell.
3. **Context is King**: Use Skills to keep the window lean.
4. **Iterate**: Observe where Claude fails, then patch it with a Skill.

---

## Resources & Next Steps

- **Agent Skills Standard**: [agentskills.io](https://agentskills.io/) | [github.com/agentskills](https://github.com/agentskills/agentskills)
- **AGENTS.md Standard**: [agents.md](https://agents.md/)
- **Claude Code Docs**: [code.claude.com/docs](https://code.claude.com/docs/en/overview)
- **Start here**: Add a `CLAUDE.md` or `AGENTS.md` to your project, then write your first skill
