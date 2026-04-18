---
name: copilot-brainstorming
description: Use when starting a new project, facing complex architectural decisions, or needing creative solutions while using Copilot CLI. This skill is optimized for Premium Request (PR) efficiency by leveraging the interactive ask_user tool and grounding its logic in the existing codebase context.
---

# Copilot Brainstorming

## Overview
Interactive brainstorming specialized for Copilot CLI. This skill minimizes Premium Requests (PR) by conducting the entire session within a single tool-use loop using `ask_user`. It proactively discovers project context and ensures progress is saved through periodic checkpointing.

**The Golden Rule:** Do NOT finalize the response or exit the brainstorming session until the user explicitly confirms they have all the information they need. Every `ask_user` interaction is "free" of PR cost.

## When to Use
- Starting a new feature or project.
- Solving complex bugs that require architectural shifts.
- Creative blocks or "blank page" syndrome.
- Evaluating multiple technical approaches within an existing codebase.

## Brainstorming Modes
You MUST ask the user to select a mode at the start:

1. **Adaptive (Recommended):** The AI dynamically switches between techniques (Socratic, First Principles, etc.) based on the conversation flow, explaining the shift when it happens.
2. **Socratic Questioning:** Deep dive via probing questions to uncover assumptions.
3. **SCAMPER:** Innovating on existing ideas (Substitute, Combine, Adapt, Modify, Put to use, Eliminate, Reverse).
4. **First Principles:** Breaking down problems to basic elements and building up.
5. **Free Association:** Rapid-fire idea generation without judgment.

## Workflow

### Step 1: Context Discovery & Initialization (The "Pre-flight" Check)
Before starting the loop, you MUST understand the current environment. This ensures you don't brainstorm in a vacuum.
1. **Auto-Discovery:** Use `glob` or `list_directory` to search for `README.md`, `package.json`, `go.mod`, `requirements.txt`, and any `.md` files in `docs/` or `.github/` folders.
2. **Analysis:** Read key files to understand the tech stack and project goals.
3. **Proposal:** In your FIRST `ask_user` call, you MUST (no exceptions):
    - Share a "Context Discovery" summary (e.g., "I've found X and Y files, suggesting a React project using Vite...").
    - Propose the brainstorming modes and ask the user to select one.
**Tools:** `glob`, `list_directory`, `read_file`, `ask_user`

### Step 2: The Interactive Loop & Checkpointing
Engage in the selected mode using `ask_user`.
- **Checkpointing:** Every **5-7 turns**, you MUST write a summary of the session so far to `.brainstorm_cache`. This prevents context loss and allows session recovery. Mention that a checkpoint was saved in your next `ask_user` prompt.
- Ask one or two questions at a time.
- Provide brief suggestions and ask for feedback.
- **CRITICAL:** Always end your turn with an `ask_user` call.

### Step 3: Synthesis & Actionable Next Steps
Once the user says "done" or "generate output":
1. **Summary:** Produce a high-detail `BRAINSTORM_SUMMARY.md`. This MUST include (no exceptions):
    - **Executive Summary:** The core concept decided upon.
    - **Architecture Diagrams:** At least one **Mermaid.js diagram** (e.g., `graph TD` or `sequenceDiagram`) visualizing the system flow, data model, or logic architecture. **This is mandatory even for non-technical brainstorms (use a flowchart).**
    - **Deep-Dive Sections:** Detailed analysis of each sub-topic.
    - **Recommendation:** A specific "default recommendation" with technical rationale.
2. **Transition:** Use the **`writing-plans`** skill to transform the brainstormed ideas into a concrete, executable implementation plan. Do NOT exit until you have confirmed the plan is generated.
3. **Cleanup:** Offer to remove the `.brainstorm_cache` file.

## Common Mistakes
- **Ignoring Codebase:** Brainstorming in a vacuum without checking existing files.
- **Context Bloat:** Forgetting to checkpoint, leading to model confusion in long sessions.
- **Dead Ends:** Ending the skill without providing a clear path to execution (e.g., failing to trigger `writing-plans`).
