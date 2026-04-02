# **Handbook: Mastering AI Skill Triggers and Descriptions**

## **1\. Foundations: Understanding the "Gatekeeper" of AI Capability**

In the architecture of AI agents, "skills" serve as specialized toolkits that extend an agent's base capabilities. However, an agent cannot load every specialized workflow into its active memory at once without overwhelming the context window and increasing the risk of "model confusion." To manage this, we utilize a pattern of **Progressive Disclosure**.

**Progressive Disclosure:** A context management strategy where the AI system only loads high-density information when it is explicitly determined to be relevant to the current task. This maintains high "token hygiene" and ensures the agent remains fast and accurate.

Effective skill discovery follows a rigorous three-tiered loading hierarchy:

* **Metadata (Level 1):** At startup, the agent loads only the **name** and **description** of every available skill. This is a low-overhead scan of roughly 100 tokens per skill.  
* **Instructions (Level 2):** If the Level 1 description matches the user’s intent, the agent "triggers" the skill, loading the full `SKILL.md` body. This should ideally be kept under **5,000 tokens** (approximately 500 lines) to preserve the context window.  
* **Resources (Level 3):** On demand, the agent accesses deep-tier resources referenced in the instructions, such as executable **scripts**, technical **references**, or static **assets**.

To ensure your skill is discoverable at Level 1, you must first master the mandatory structural architecture of the skill file.

## **2\. Anatomy of a Skill: The SKILL.md Architecture**

Every skill must reside in its own dedicated subdirectory and contain a manifest file named exactly `SKILL.md`. This file uses **YAML frontmatter** to define its identity and discovery logic.

### **Frontmatter Specification**

| Field | Requirement | Constraint | Purpose |
| :---- | :---- | :---- | :---- |
| `name` | **Mandatory** | 1-64 chars; unicode lowercase alphanumeric and hyphens; no consecutive hyphens; no start/end hyphens. | Unique identifier; must match the directory name. |
| `description` | **Mandatory** | Max 1024 characters; non-empty. | The primary "trigger" logic used for Level 1 discovery. |
| `model` | Optional | `inherit` (recommended), `sonnet`, `opus`, or `haiku`. | Specifies which AI model executes the skill. |
| `color` | Optional | `blue`, `green`, `red`, `yellow`, `magenta`, `cyan`. | Visual identifier for the agent in the UI. |
| `license` | Optional | License name or reference. | Specifies the license applied to the skill. |
| `compatibility` | Optional | Max 500 characters. | Defines specific environment or system requirements. |

### **Minimal Valid Frontmatter**

Below is a valid example for a GitHub Actions debugging skill:

\---  
name: github-actions-failure-debugging  
description: Use this skill to diagnose and fix failed GitHub Actions workflow runs, even if the user does not explicitly ask for debugging help.  
model: inherit  
color: red  
\---

\# Instructions  
1\. Analyze the workflow logs to identify the specific failing step.  
2\. Formulate a hypothesis based on the error output...

While structural validity is essential, the `description` field is the functional heart of the skill—it is the only data the agent sees before deciding whether to activate the specialized instructions.

## **3\. The Strategy of the Trigger: Writing Effective Descriptions**

Because the agent uses the description to "gatekeep" context loading, your writing must be strategically tuned. A weak description results in a "dormant skill" that fails to assist the user.

### **Core Principles of Effective Descriptions**

* **Imperative Phrasing:** Frame the description as a direct instruction to the agent. Use verb-first phrases like "Use this skill when..." rather than passive summaries like "This skill helps with..."  
* **Focus on User Intent:** Describe the goal the user is trying to achieve (e.g., "deploy to production") rather than the internal scripts the skill runs.  
* **Be Strategic with "Pushiness":** Explicitly list contexts where the skill applies. Use broad catch-alls such as "Apply this skill when the user mentions data files, even if they don't explicitly name 'CSV'."  
* **Adhere to the Token Budget:** You must stay within the **1024-character hard limit**. A high-impact description is usually two to four sentences.

**Architect's Insight:** AI agents possess significant general knowledge. They will only consult a skill when a task requires "specialized knowledge" or a "domain-specific workflow" that exceeds their default training. An agent can read a simple text file alone; use skills for complex API patterns, project-specific conventions, or multi-step recovery procedures.

To move from theory to implementation, we can use a linguistic transformation framework to turn passive descriptions into active triggers.

## **4\. Crafting the Language: Imperative Phrasing and User Intent**

The most effective triggers use the **Imperative/Infinitive** form. This tells the agent exactly when to reach for the tool.

### **The Transformation Table**

| Weak (Implementation-Focused) | Strong (Intent-Focused) |
| :---- | :---- |
| "This skill contains a script for analyzing CSV files and generating charts." | "Use this skill to analyze data, calculate summary statistics, or generate charts from CSV/TSV files, even if the user doesn't mention 'analysis'." |
| "I can help with debugging GitHub Actions if you show me the logs." | "Apply this skill when a user needs to find why a build failed or requires diagnosis of a failed GitHub Actions workflow run." |
| "A skill for rolling dice." | "Use this skill whenever the user requests random number generation or specifically asks to 'roll a die'." |

### **Step-by-Step Method for Extracting a Trigger**

To capture a trigger from a real-world task, follow this extraction sequence:

1. **Perform the task manually:** Conduct the workflow in a live session with an agent.  
2. **Identify successful patterns:** Note the specific sequence of actions that produced the desired result.  
3. **Capture "Steering Moments":** Identify when you had to correct the agent.  
   * *Example:* "No, don't use the third-party library; use the standard library for this specific project." These corrections are your trigger's "gold."  
4. **Define the boundaries:** Note the project-specific facts the agent didn't know initially.  
5. **Draft the trigger:** Combine these into an imperative instruction: "Use this skill when \[User Intent\] requires \[Project-Specific Context\]."

## **5\. The Optimization Loop: Evaluating Trigger Accuracy**

To ensure reliability, you must build a **Trigger Eval Set**—a collection of roughly 20 queries used to verify the description’s triggering logic.

### **Designing the Eval Set**

* **Should-Trigger:** Casual and formal queries where the skill is needed (e.g., "Make me a chart" for a CSV skill).  
* **Should-Not-Trigger:** "Near-misses" that share keywords but require different tools.  
  * *Example:* A "CSV Analysis" skill should **not** trigger for "Update the formulas in my Excel spreadsheet."

### **Checklist for Realism**

* \[ \] **File paths:** Include paths like `~/Downloads/data_v2.csv`.  
* \[ \] **Personal context:** Use phrases like "my manager asked for a report."  
* \[ \] **Casual language:** Include typos and common abbreviations.  
* \[ \] **Complexity:** Mix short, direct requests with long, context-heavy messages.

### **The 5-Step Optimization Loop**

1. **Evaluate:** Run your queries and compute a "trigger rate" (percentage of correct activations).  
2. **Identify Failures:** Use only your **Train Set** (60% of queries) to pinpoint failures and "near-miss" false positives.  
3. **Revise:** Adjust the description. Broaden it if it's too narrow; add specific "do not use for X" constraints if it's too broad.  
4. **Repeat:** Re-run the evaluation on the Train Set.  
5. **Select & Validate:** Choose the best version, then run it against the **Validation Set** (the remaining 40% of queries).

**Warning on Overfitting:** Never look at the Validation Set failures until the final selection. If you optimize against the validation queries, you are "cheating" the test and the description will likely fail when it encounters new, real-world users.

## **6\. Copilot CLI & GStack: Managing Skills in the Wild**

In professional environments like the GitHub Copilot CLI or the GStack ecosystem, skills are managed through specific commands and structured directory paths.

### **Essential Slash Commands**

* `/skills list`: Displays all currently discovered and available skills.  
* `/skills info`: Shows detailed metadata for a skill, including its filesystem location.  
* `/skills reload`: Force-refreshes the agent's memory if you have modified a `SKILL.md` during an active session.

### **Standard Directory Paths**

Skills are categorized by their scope:

* **Project Skills:** Specific to a repository.  
  * `.github/skills/`  
  * `.claude/skills/`  
  * `.agents/skills/`  
* **Personal Skills:** Shared across all projects on your machine.  
  * `~/.copilot/skills/`  
  * `~/.claude/skills/`  
  * `~/.agents/skills/`

### **GStack Proactive Mode**

In the GStack environment, the **Proactive Mode** toggle allows the agent to auto-invoke skills. Rather than waiting for a direct command, the agent scans the **entire conversation context** to suggest or trigger a skill (e.g., suggesting `/qa` when it detects the user saying "this feature is ready for testing").

## **7\. Summary: The "Boil the Lake" Philosophy**

Effective skill triggering is the prerequisite for the **"Boil the Lake"** principle. In an era where AI makes the marginal cost of labor near-zero, we do not settle for half-measures. If an agent can trigger a specialized skill to provide 100% test coverage or a perfect architectural audit in minutes, we choose the complete implementation over the shortcut.

To maintain this standard of excellence, adhere to the ultimate rule of AI-assisted engineering:

**The Iron Law of Debugging:** No fixes without investigation. If an agent triggers a skill to fix a bug, it must first trace the data flow and test hypotheses. Three failed fixes mean it's time to stop and reassess the strategy.

By mastering triggers, you ensure your "virtual engineering team" activates with the right expertise at exactly the right moment.

