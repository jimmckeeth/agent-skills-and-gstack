# Project Overview: Agent Skills and Gstack

This project is centered on the **Agent Skills** specification and the **Gstack** framework. It includes a presentation on autonomous engineering, the official Agent Skills specification (as a submodule), and a reference implementation for validating and using skills.

## Core Components

### 1. Presentation & Strategy
*   **`agent-skills.md`**: A comprehensive Markdown-based slide deck explaining the "Agentic Engineering" paradigm, the "Context Window Tax," and the distinction between Agents (personas) and Skills (playbooks).
*   **`The_Autonomous_Engineering_Blueprint.pdf/pptx`**: High-level strategy documents for building autonomous capabilities.
*   **`VibeVsAgent.png` / `infographic1.png`**: Visual aids illustrating the transition from "vibe coding" to structured agentic workflows.
*   **`TODOS.md`**: A list of feedback and improvement tasks for the presentation materials.

### 2. Agent Skills Standard (`standard/` Submodule)
The `standard` directory is a submodule (linking to `agentskills/agentskills`) containing the official specification.
*   **`standard/docs/`**: Documentation for the Agent Skills format, likely managed via **Mintlify**.
*   **`standard/skills-ref/`**: A Python-based reference implementation (`skills-ref`).
    *   **Purpose**: Validates skill directories, reads properties, and generates XML prompt blocks for AI models.
    *   **Installation**: Uses `uv` or `pip` (`pip install -e standard/skills-ref`).
    *   **CLI Tools**: `skills-ref validate`, `skills-ref read-properties`, `skills-ref to-prompt`.

### 3. Gstack Integration
While primarily documentation-focused, the project refers to **Gstack** as a framework for implementing these agentic patterns.

## Key Concepts

*   **Progressive Disclosure**: Managing context by only loading metadata and instructions when triggered, saving token space.
*   **Agents vs. Skills**:
    *   **Agents**: "The Expert Player" – Dynamic, autonomous, written in second person (persona).
    *   **Skills**: "The Playbook" – Static, procedural knowledge, written in imperative form (instructions).
*   **Triggering**: Using `<example>` blocks in descriptions to define when Claude should activate specific agents or skills.

## Usage & Development

### Working with the Presentation
The `agent-skills.md` file is the primary source for the presentation. Revisions and updates should align with the feedback in `TODOS.md`.

### Working with the Specification
If modifying the Agent Skills format, work within the `standard/` directory. Use the `skills-ref` tool to validate any new skills created:
```bash
# From standard/skills-ref
uv run skills-ref validate /path/to/my-skill
```

### Documentation
Documentation can be previewed using Mintlify:
```bash
# Inside standard/docs
mint dev
```
