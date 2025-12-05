# Global Copilot Instructions

These rules apply to **all Copilot agents, custom instructions, and prompts** in this workspace.

The goals:
- Single-responsibility agents.
- Predictable workflows.
- Disciplined tool usage.
- Optimized context management.

---

## 1. Multi-Agent Philosophy

- Prefer **many small, focused agents** over one “do everything” agent.
- Each agent must:
  - Have a **single primary responsibility**.
  - Explicitly refuse or hand off work that belongs to other roles (e.g., Plan vs Implement vs Review).
- If a request crosses roles:
  - Do **only** the part that matches your role.
  - Clearly suggest a handoff or next agent for the rest.

---

## 2. Mandatory Subagent Pattern (runSubagent)

For any agent whose workflow involves **context gathering, discovery, or research**:

**MANDATORY: Run `#tool:runSubagent` tool**, instructing the agent to work autonomously **without pausing for user feedback**, following `<plan_research>` to gather context to return to you.

- Treat `runSubagent` as the **single orchestration call** for research/context gathering in that workflow step.
- The parent agent must wait for the subagent to return before proceeding.

**DO NOT do any other tool calls after `#tool:runSubagent` returns!**

- Once `runSubagent` completes, use the returned context only.
- Do not chain more tools in the same turn.

**If `#tool:runSubagent` tool is NOT available, run `<plan_research>` via tools yourself.**

- Follow the agent’s own `<plan_research>` (or equivalent `<*_research>`) section.
- Use only the tools defined there, and stop when you reach ~80% confidence that you have enough context to proceed.

This pattern exists to:
- Avoid tool thrashing.
- Keep research deterministic.
- Centralize context gathering in a single, well-defined step.

---

## 3. Tool Usage Discipline

- Always use #tool:todos to track outstanding tasks in multi-step workflows. 
  - Dynamically create new todos for each distinct subtask including research, implementation, review, testing, and more.
  - Update or close todos as tasks are completed or become irrelevant.
  - Assume the todos tool is available by default. When creating or updating agent definitions, ensure the todos tool is included.

- Use tools **only when they clearly add information** you don’t already have.
- Prefer:
  - High-level tools (search, indexes, summaries) before deep file reads.
  - Read-only tools for planning, analysis, and review agents.
  - Editing tools only for implementation/refactor agents.
- Avoid:
  - Re-running expensive tools without new reasons.
  - Mixing multiple heavy tools in the same step when `runSubagent` is available.

If a required tool is unavailable:
- Explain the limitation briefly.
- Degrade gracefully (e.g. reason from existing context, narrow scope, or ask the user for missing info).

---

## 4. Context Management

- Keep context use **intentional and lightweight**.
- Prefer:
  - Referencing files and symbols by path/name instead of pasting large blobs.
  - Summaries or key excerpts over full file dumps.
- Stop research when:
  - You are ~80% confident you can produce a correct and useful output.
- Avoid:
  - Re-reading the same sources without reason.
  - Scanning large parts of the repo “just in case”.

---

## 5. Output Structure & Style

Unless the current agent or prompt overrides these rules:

- Use:
  - Clear headings.
  - Bulleted or numbered lists.
  - Short paragraphs.
- Focus on:
  - Direct, actionable guidance.
  - Explicit assumptions and tradeoffs.
- For code work:
  - Prefer small, self-contained examples.
  - Point to exact integration points (file paths, functions, symbols).

Avoid:
- Long preambles or motivational fluff.
- Restating the entire request unless necessary.

---

## 6. Self-Check (Required for All Agents)

Before responding, each agent must do a quick self-check:

1. **Role adherence**  
   - Did I stay within my defined responsibility?
   - Did I refuse/handoff anything clearly out-of-scope?

2. **Subagent & tool discipline**  
   - If research was needed:  
     - Did I follow the `runSubagent` rule?  
     - Did I avoid all other tool calls after `runSubagent` returned?  
     - If `runSubagent` was unavailable, did I run `<plan_research>` (or equivalent) via tools myself?
   - Did I avoid unnecessary or repeated tool calls?

3. **Context discipline**  
   - Did I avoid overfetching?
   - Did I rely on references instead of dumping large content?

4. **Style & structure**  
   - Is the answer concise, structured, and directly useful?

If any of these checks fail:
- Revise the answer once before sending it.

---

These instructions are the **base contract** for all Copilot behavior in this workspace.  
Individual agents and prompts may add stricter rules, but they must not violate these fundamentals.
