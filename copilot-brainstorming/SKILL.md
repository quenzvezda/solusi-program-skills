---
name: copilot-brainstorming
description: Use when the user wants to brainstorm a new feature, compare technical directions, untangle a fuzzy requirement, or turn rough ideas into a concrete approach while using Copilot CLI. Use it even when the request is informal, such as "diskusi dulu", "brainstorm bareng", "carikan arah", or "mau explore opsi", especially when the user needs an interactive back-and-forth that ends in a written markdown summary.
---

# Copilot Brainstorming

## Overview
Interactive brainstorming specialized for Copilot CLI. The goal is to keep the conversation open long enough to uncover constraints, compare options, and converge on a practical recommendation grounded in the real codebase.

**The Golden Rule:** Do not treat brainstorming like a one-shot answer. Ask at least one real follow-up question before concluding, and do not end the brainstorming flow until you have produced the markdown summary file.

## Runtime Compatibility
Do not hard-code your workflow to specific tool names. Tool availability changes across runtimes and sessions.

- If an interactive question tool such as `ask_user` / 'Asked user' is available, you may use it.
- If it is not available, ask your questions in the normal conversation and wait for the user's reply.
- Use whatever file-discovery and file-reading tools are actually available in the current runtime instead of assuming `list_directory` or `read_file` exist.
- If a referenced companion skill is unavailable, continue the brainstorm inline rather than stopping.

## When to Use
- Starting a new feature or project.
- Solving complex bugs that require architectural shifts.
- Creative blocks or "blank page" syndrome.
- Evaluating multiple technical approaches within an existing codebase.
- Reviewing or simplifying instruction files, agent rules, or prompt scaffolding before implementation.

## Brainstorming Modes
Offer these modes at the start and let the user pick, or recommend **Adaptive** if they do not care:

1. **Adaptive (Recommended):** The AI dynamically switches between techniques (Socratic, First Principles, etc.) based on the conversation flow, explaining the shift when it happens.
2. **Socratic Questioning:** Deep dive via probing questions to uncover assumptions.
3. **SCAMPER:** Innovating on existing ideas (Substitute, Combine, Adapt, Modify, Put to use, Eliminate, Reverse).
4. **First Principles:** Breaking down problems to basic elements and building up.
5. **Free Association:** Rapid-fire idea generation without judgment.

## First-Turn Guardrails
The first turn is where Copilot is most likely to collapse the whole brainstorm into one answer. Prevent that.

- On the first turn, do **not** deliver the full recommendation, full rewrite plan, or a complete keep/remove/move breakdown.
- You may share a brief initial read with at most 2-3 high-signal observations.
- Then stop and ask the user which direction to explore first, or ask the most important clarifying question.
- If the user already provided a lot of context, treat it as enough to form hypotheses, not enough to end the brainstorm.
- If you recommend **Adaptive**, do so briefly and then immediately ask what they want to prioritize.

## Required Output
Every completed brainstorming session must end with a markdown summary file.

- Write the summary to `docs/brainstorming/YYYY-MM-DD-{short-title}.md` the path base on project root relative.
- Create the `docs/brainstorming/` folder if it does not exist.
- Do not consider the session complete until the file exists and you have told the user where it was saved.
- Use the summary as the canonical artifact for the brainstorm, not just as a transcript recap.

## Workflow

### Step 1: Context Discovery & Initialization (The "Pre-flight" Check)
Before starting the loop, understand the current environment so you do not brainstorm in a vacuum.
1. **Auto-discovery:** Search for repo signals such as `README.md`, `package.json`, `go.mod`, `requirements.txt`, and relevant docs in `docs/` or `.github/`.
2. **Analysis:** Read only the key files needed to understand the stack, goals, and vocabulary.
3. **First interaction:** Start with a brief context summary, offer the brainstorming modes, and ask the user to choose a mode or let you choose.
4. **Hard stop after the first question:** Unless the user explicitly asked for a one-shot answer, do not continue into the full recommendation until they answer your first question.
5. **No local files available:** If the user is brainstorming from screenshots or pasted text only, say what context is missing and continue by asking about goals, constraints, and success criteria.

### Step 2: The Interactive Loop & Checkpointing
Engage in the selected mode interactively.
- Ask one or two focused questions at a time.
- After each user answer, briefly synthesize what changed in your understanding before proposing the next question or option set.
- Prefer short option lists with tradeoffs over long lectures.
- If the user asks for ideas immediately, still ask at least one clarifying or prioritization question unless the request is already fully specified.
- For long sessions, save a checkpoint only if writing a local cache file is both allowed and genuinely useful. Otherwise keep a compact running summary in the conversation.
- Do not abruptly conclude after your first recommendation. Keep the loop open until the user wants a summary, recommendation, or plan.
- If the user asks for "diskusi dulu", "brainstorm dulu", "mari diskusi", or similar phrasing, interpret that as a strong signal to stay interactive rather than producing a finished answer immediately.

### Step 3: Synthesis & Markdown Artifact
Once the user says "done", "wrap it up", or asks for output:
1. **Write the markdown summary:** Save a file at `docs/brainstorming/YYYY-MM-DD-{short-title}.md`.
2. **Include the required sections:** Executive Summary, Architecture Diagram or flowchart when useful, Deep-Dive Sections, and Recommendation.
3. **Make it actionable:** Add next steps so the file can be used as a handoff artifact.
4. **Plan handoff:** If the user wants execution planning and the `writing-plans` skill is available, invoke it. Otherwise produce the implementation plan inline.
5. **Close cleanly:** Tell the user the file path and ask whether they want refinement, a written plan, or to stop here.

## Common Mistakes
- **Ignoring Codebase:** Brainstorming in a vacuum without checking existing files.
- **Tool Assumptions:** Treating `ask_user`, `list_directory`, or `read_file` as guaranteed instead of adapting to available capabilities.
- **First-Turn Dumping:** Giving the polished recommendation immediately after reading context instead of pausing for user steering.
- **Premature Closure:** Giving one polished answer and ending the session before the user confirms they are done.
- **Context Bloat:** Collecting too much detail at once instead of asking focused questions and moving step by step.
- **Dead Ends:** Ending without a clear next step, recommendation, or handoff to planning.
