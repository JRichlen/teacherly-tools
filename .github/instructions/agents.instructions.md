---
applyTo: "**/*.agent.md"
---

# Agent File Specification

This document defines the **mandatory structure, rules, and style** for all `.agent.md` files in this workspace.

All agents must comply with the global rules in `.github/copilot-instructions.md`. This file adds agent-specific requirements.

---

## 1. File Format

Agent files use the `.agent.md` extension and consist of:
1. **YAML frontmatter** (required)
2. **Markdown body** with structured sections (required)

---

## 2. YAML Frontmatter (Required)

Every agent MUST include a YAML frontmatter block at the top of the file with these fields:

| Field           | Required | Description                                                                 |
|-----------------|----------|-----------------------------------------------------------------------------|
| `name`          | Yes      | Short, role-focused identifier in snake_case (e.g., `orchestrator`, `implementer_planner`, `api_implementer`) |
| `description`   | Yes      | Single responsibility in 1–2 sentences                                      |
| `tools`         | Yes      | Array of tools the agent is allowed to use                                  |
| `argument-hint` | No       | Guidance on how to phrase user requests to this agent                       |
| `model`         | No       | Override model choice if needed (default: `Auto (copilot)`)                 |
| `handoffs`      | No       | List of agent names this agent can hand off work to                         |

**Example:**
```yaml
---
name: orchestrator
description: Coordinates multiple agents to complete complex multi-agent tasks.
argument-hint: Describe the task you want completed.
tools: ['search/codebase', 'runSubagent', 'todos']
model: Auto (copilot)
handoffs:
  - label: planner
    agent: orchestration_planner
    prompt: "Plan multi-agent orchestration workflow"
---
```

---

## 3. Body Sections (Required Order)

The markdown body MUST include these sections **in this exact order**:

### 3.1 Role Statement
A brief paragraph (2–4 sentences) stating:
- What the agent is.
- Its single primary responsibility.
- What it explicitly does NOT do.

### 3.2 `<stopping_rules>`
Define when the agent MUST stop or refuse work:
- Requests outside the agent's responsibility.
- Work that belongs to other agents.
- Conditions that trigger a handoff.

### 3.3 `<workflow>`
Ordered steps (3–7 recommended) the agent follows:
- Numbered list format.
- Each step should be concrete and actionable.
- Clearly indicate when tools are used.
- Include stopping/handoff points.

### 3.4 `<tool_policy>`
Specify how the agent uses tools:
- **Allowed tools** with their purposes.
- **Mandatory subagent pattern** (if research is needed).
- **Forbidden actions** (e.g., editing files for read-only agents).
- Always include `todos` for multi-step workflows.

### 3.5 `<context_policy>`
Define context gathering rules:
- What to look for first (high-level → specific).
- When to stop gathering (~80% confidence rule).
- What NOT to do (e.g., paste large blobs, over-fetch).

### 3.6 `<research_guidelines>` (Optional)
Include ONLY if the agent performs research. Specify:
- What to look for.
- Where to look.
- How deep to go.
- When to stop.

Omit this section entirely for agents that do not perform research.

### 3.7 `<style_guide>`
Define output format and style:
- Structure requirements (headings, lists, etc.).
- Language tone (direct, technical, etc.).
- Specific formatting rules for this agent's outputs.

### 3.8 `<self_check>`
A checklist the agent runs before responding:
1. Role adherence.
2. Workflow completion.
3. Tool/context discipline.
4. Style compliance.

---

## 4. Mandatory Rules

### 4.1 Single Responsibility
- Each agent has **ONE primary job**.
- The `description` field must express this clearly.
- If a request spans multiple responsibilities, the agent must:
  - Do only its part.
  - Hand off the rest via `handoffs` or explicit suggestion.

### 4.2 Tool Discipline
- Only list tools the agent actually needs.
- Always include `todos` for agents with multi-step workflows.
- Follow the **mandatory subagent pattern** for research:
  - Use `runSubagent` when available.
  - Fall back to manual research via `<research_guidelines>` when not.
  - Do NOT run additional tools after `runSubagent` returns.

### 4.3 Context Discipline
- Reference files by path, not by pasting content.
- Stop gathering context at ~80% confidence.
- Avoid over-fetching or repeated reads.

### 4.4 Stopping Rules
- Every agent MUST have explicit stopping rules.
- Stopping rules prevent scope creep and role violations.

### 4.5 Self-Check Requirement
- Every agent MUST include `<self_check>`.
- The agent must run this check before every response.
- If any check fails, revise once before responding.

---

## 5. Style Guidelines

### 5.1 Naming
- `name`: snake_case (e.g., `api_implementer`, `code_reviewer`, `orchestrator`).
- File name: `<name>.agent.md` (e.g., `orchestrator.agent.md`, `api_implementer.agent.md`).

### 5.2 Language
- Direct and operational, not philosophical.
- Use imperative mood ("Do X", not "The agent should do X").
- Avoid fluff, preambles, and restating global rules.

### 5.3 Formatting
- Use bullet points and numbered lists.
- Keep sections concise.
- Use code blocks for examples.
- Wrap the body in ` ```chatagent ` and ` ``` ` markers.

### 5.4 Section Tags
- Use XML-style tags for sections: `<stopping_rules>`, `<workflow>`, etc.
- These tags enable parsing and tooling.

---

## 6. Template

```chatagent
---
name: <agent_name>
description: <Single responsibility in 1-2 sentences.>
argument-hint: <Optional: how to phrase requests.>
tools: ['todos', '<other-tools>']
model: Auto (copilot)
handoffs:
  - label: <handoff_label>
    agent: <agent_name>
    prompt: <handoff_prompt>
---

You are a <ROLE NAME>.

Your sole responsibility is to <PRIMARY RESPONSIBILITY>.
You NEVER <EXPLICITLY FORBIDDEN ACTIONS>.

<stopping_rules>
- STOP if <condition 1>.
- STOP if <condition 2>.
- Hand off to <agent> when <condition>.
</stopping_rules>

<workflow>
1. <First step>.
2. <Second step>.
3. <Third step with tool usage>.
4. <Fourth step>.
5. <Final step or handoff>.
</workflow>

<tool_policy>
- Allowed tools:
  - `todos`: Track multi-step workflow progress.
  - `<tool>`: <purpose>.

- Mandatory subagent pattern:
  - Use `runSubagent` for research when available.
  - Do NOT run other tools after `runSubagent` returns.

- Forbidden:
  - <Forbidden action 1>.
  - <Forbidden action 2>.
</tool_policy>

<context_policy>
- Start with: <high-level sources>.
- Look for: <specific targets>.
- Stop when: ~80% confident.
- Do NOT: <forbidden context actions>.
</context_policy>

<research_guidelines>
(Include only if agent performs research)
- Look for: <targets>.
- Where: <locations>.
- Stop when: <condition>.
</research_guidelines>

<style_guide>
- Output format: <format requirements>.
- Structure: <structure requirements>.
- Tone: <tone requirements>.
</style_guide>

<self_check>
Before responding, verify:
1. Did I stay within my single responsibility?
2. Did I follow the workflow steps?
3. Did I use tools appropriately (subagent pattern, todos)?
4. Did I avoid over-fetching context?
5. Does my output match the style guide?

If any check fails, revise once before responding.
</self_check>
```

---

## 7. Validation Checklist

When creating or reviewing an agent file, verify:

- [ ] YAML frontmatter includes `name`, `description`, and `tools`.
- [ ] `description` expresses a single responsibility.
- [ ] All required body sections are present in correct order.
- [ ] `<stopping_rules>` explicitly define when to stop/refuse.
- [ ] `<workflow>` has 3–7 concrete, ordered steps.
- [ ] `<tool_policy>` includes allowed tools and forbidden actions.
- [ ] `todos` is included for multi-step agents.
- [ ] `<context_policy>` includes the ~80% confidence rule.
- [ ] `<self_check>` covers role, workflow, tools, context, and style.
- [ ] File name matches `<name>.agent.md` pattern.
- [ ] Language is direct and operational.
- [ ] No duplication of global rules from `copilot-instructions.md`.
