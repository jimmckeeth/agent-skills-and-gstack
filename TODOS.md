# Review: Building Autonomous Capabilities (agent-skills.md)

Technical presentation for developers with LLM experience but are new to the "Skills" paradigm. Formatted for Reveal.js and markdown import on Slides.com

## Structure & Flow

- [x] Add a title slide with author/date/event info before the first `---`
- [x] Add a brief "Who is this for?" or "Prerequisites" slide after the title to set audience expectations
- [x] Add transition slides between the three parts (Agents, Skills, Lifecycle) — each Part heading now includes a brief transition line
- [x] Added a "Resources / Next Steps" slide at the end

## Content Gaps

- [x] The SKILLS.md slide now lists specific agents: Claude Code, Codex, Copilot, Gemini CLI, Cursor, Windsurf
- [x] Added SKILL.md file structure/frontmatter slide (mirrors the Agent file structure slide)
- [ ] The "Lean SKILL.md" slide mentions progressive disclosure with references/ but doesn't show a SKILL.md frontmatter example the way the Agent slide does — partially addressed by the new Skill File Structure slide above it
- [x] Added "Skill Discovery" slide covering project-level and user-level paths, loading order, and trigger mechanism
- [x] Added "Worked Example: A Deploy Skill" slide walking through the full Identify→Plan→Act→Validate→Iterate loop

## Clarity & Accuracy

- [x] Subtitle changed to "Beyond 'Vibe Coding' — adding structure to AI-assisted development"
- [x] Table now says "Deterministic / Procedural" instead of "Static / Procedural"
- [x] Changed "developed by Anthropic" to "originated from Anthropic" — consistent with the open-standard framing

## New Slides Added

- [x] "Other Important MD Files" slide — lists CLAUDE.md, AGENTS.md, and SKILLS.md with purpose and location

## Presentation Polish

Skip all of these

- [ ] Several slides are text-heavy (e.g., "The Context Window Tax" at line 39) — consider using diagrams or visuals for progressive disclosure and the context window concept
- [ ] The code block on lines 72-85 (Agent file structure) would benefit from syntax highlighting hints or annotations pointing out key fields
- [ ] Add speaker notes (Reveal.js `Note:` blocks) for slides that need verbal context beyond the bullet points
- [ ] Consider adding a real-world demo or screenshot slide — e.g., showing an agent being triggered in a terminal session
