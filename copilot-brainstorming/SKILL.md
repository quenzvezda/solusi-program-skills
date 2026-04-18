---
name: copilot-brainstorming
description: Use when starting a new project, facing complex architectural decisions, or needing creative solutions while using Copilot CLI. This skill is optimized for Premium Request (PR) efficiency by leveraging the interactive ask_user tool.
---

# Copilot Brainstorming

## Overview
Interactive brainstorming specialized for Copilot CLI. This skill minimizes Premium Requests (PR) by conducting the entire session within a single tool-use loop using `ask_user`. 

**The Golden Rule:** Do NOT finalize the response or exit the brainstorming session until the user explicitly confirms they have all the information they need. Every `ask_user` interaction is "free" of PR cost—use them liberally.

## When to Use
- Starting a new feature or project.
- Solving complex bugs that require architectural shifts.
- Creative blocks or "blank page" syndrome.
- Evaluating multiple technical approaches.

## Brainstorming Modes
You MUST ask the user to select a mode at the start of the session:

1. **Socratic Questioning:** A deep dive where the AI asks probing questions to uncover underlying assumptions and refine the problem statement.
2. **SCAMPER:** (Substitute, Combine, Adapt, Modify, Put to another use, Eliminate, Reverse) - A framework for innovating on existing ideas.
3. **First Principles:** Breaking down complex problems into their basic elements and building back up from scratch.
4. **Free Association:** Rapid-fire idea generation without immediate judgment to explore broad possibilities.

## Workflow (The PR-Efficiency Protocol)

### Step 1: Initialization
Propose the four modes to the user and ask for their initial topic.
**Tool:** `ask_user`

### Step 2: The Interactive Loop
Engage in the selected mode. 
- Ask one or two questions at a time.
- Provide brief suggestions and ask for feedback.
- **CRITICAL:** Always end your turn with an `ask_user` call to keep the session alive. Do NOT summarize or "wrap up" until the user says "done" or "generate output".

### Step 3: Final Output Generation
Once the user is satisfied, produce a high-detail `BRAINSTORM_SUMMARY.md`. This document MUST include:
- **Executive Summary:** The core concept decided upon.
- **Deep-Dive Sections:** Detailed analysis of each sub-topic discussed.
- **Multiple Options:** At least 2-3 distinct paths forward for each section.
- **Detailed Recommendations:** A specific "default recommendation" with technical rationale for the best path.

## Example Session
**You:** "Let's brainstorm! Which mode should we use? (Socratic, SCAMPER, First Principles, Free Association)"
**User:** "Socratic. I want to build a local-first task manager."
**You (via ask_user):** "Great. Why is 'local-first' the priority for this specific project?"
**User:** "Privacy and offline speed."
**You (via ask_user):** "Understood. How do you plan to handle synchronization when the user eventually goes back online?"
... (Loop continues) ...
**User:** "Okay, generate the summary."
**You (final turn):** *Creates BRAINSTORM_SUMMARY.md and provides a concluding message.*

## Common Mistakes
- **Exiting too early:** Finishing the turn before the user is satisfied, causing them to send a new PR.
- **Oversimplified summaries:** Giving a "TL;DR" instead of the requested detailed options and recommendations.
- **Mixing modes:** Losing focus on the selected framework (e.g., jumping from Socratic to SCAMPER without user permission).
