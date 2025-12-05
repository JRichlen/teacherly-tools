---
applyTo: "**/*.prompt.md"
---

# Prompt File Specification

This document defines the **mandatory structure, rules, and style** for all `.prompt.md` files in this workspace.

All prompts must comply with the global rules in `.github/copilot-instructions.md`. This file adds prompt-specific requirements.

---

## 1. File Format

Prompt files use the `.prompt.md` extension and consist of:
1. **YAML frontmatter** (required)
2. **Markdown body** with execution instructions (required)

---

## 2. YAML Frontmatter (Required)

Every prompt MUST include a YAML frontmatter block at the top of the file with these fields:

| Field           | Required | Description                                                                 |
|-----------------|----------|-----------------------------------------------------------------------------|
| `name`          | Yes      | Short, action-focused identifier (e.g., `create-agent`, `refactor-code`)   |
| `description`   | Yes      | What the prompt does in 1–2 sentences                                       |
| `tools`         | Yes      | Array of tools the prompt is allowed to use                                 |
| `argument-hint` | No       | Guidance on how to phrase user input                                        |
| `agent`         | No       | Name of the agent this prompt delegates to                                  |
| `model`         | No       | Override model choice if needed (default: `Auto (copilot)`)                 |

**Example:**
```yaml
---
name: create-agent
description: Design a new custom agent as a `.agent.md` file using the standardized workspace agent specification.
argument-hint: Describe the desired agent's purpose, responsibilities, tools, and neighboring roles.
agent: agent-architect
model: Auto (copilot)
tools: ['edit/createFile', 'search/codebase', 'runSubagent', 'todos']
---
```

---

## 3. Body Structure (Required)

The markdown body MUST include:

### 3.1 Role/Context Statement
A brief statement (1–3 sentences) establishing:
- What execution context the prompt creates.
- Which agent (if any) is being invoked.

### 3.2 Rules Section
Explicit rules the execution must follow:
- References to instruction files (e.g., `.github/instructions/*.instructions.md`).
- References to global instructions (`.github/copilot-instructions.md`).
- Any prompt-specific behavioral rules.
- Mandatory patterns (e.g., subagent pattern for research).

### 3.3 Objectives Section
Clear, numbered list of what the prompt accomplishes:
- Primary output (e.g., "Produce EXACTLY ONE `.agent.md` file").
- Required components of that output.
- Quality requirements.

### 3.4 Ambiguity Handling (Recommended)
Instructions for handling unclear requests:
- When to ask clarifying questions.
- Default behaviors when ambiguity persists.
- Documentation of assumptions made.

### 3.5 Output Specification
Define what the prompt returns:
- Exact deliverable format.
- What NOT to include (e.g., "Return ONLY the file and nothing else").

---

## 4. Mandatory Rules

### 4.1 Single Purpose
- Each prompt has **ONE clear purpose**.
- The `description` field must express this clearly.
- Complex workflows should use agent delegation or multiple prompts.

### 4.2 Tool Discipline
- Only list tools actually needed for execution.
- Include `todos` for multi-step prompts.
- Follow the **mandatory subagent pattern** for research:
  - Reference `runSubagent` in tools when research is needed.
  - Instruct explicit use of the pattern in rules.

### 4.3 Instruction References
- Always reference relevant instruction files by path.
- Do NOT duplicate content from instruction files.
- Reference global instructions when applicable.

### 4.4 Agent Delegation
- When delegating to an agent, use the `agent` field.
- The prompt body should configure execution, not redefine the agent.

### 4.5 Output Clarity
- Explicitly state what the prompt produces.
- Explicitly state what NOT to produce.

---

## 5. Style Guidelines

### 5.1 Naming
- `name`: lowercase, hyphenated, action-oriented (e.g., `create-agent`, `review-pr`, `plan-feature`).
- File name: `<name>.prompt.md` (e.g., `create-agent.prompt.md`).

### 5.2 Language
- Direct and imperative ("Follow these rules", "Produce EXACTLY ONE").
- Concise rules, not verbose explanations.
- Focus on execution instructions, not philosophy.

### 5.3 Formatting
- Use numbered lists for sequential steps.
- Use bullet points for rules and requirements.
- Use code blocks for examples and file references.
- Wrap the body in ` ```prompt ` and ` ``` ` markers.

### 5.4 References
- Use relative paths for workspace files (e.g., `.github/instructions/agents.instructions.md`).
- Reference agents by name as defined in their `name` field.

---

## 6. Template

```prompt
---
name: <prompt-name>
description: <What this prompt does in 1-2 sentences.>
argument-hint: <Optional: how to phrase input.>
agent: <optional-agent-name>
model: Auto (copilot)
tools: ['todos', '<other-tools>']
---

You are executing the **<Agent/Context Name>** to <primary purpose>.

Follow these rules:

1. Use the **<specific> rules** defined in `.github/instructions/<file>.instructions.md`.
2. Use the **global workspace instructions** in `.github/copilot-instructions.md`.
3. Follow the **mandatory runSubagent pattern** for any context gathering:
   - First attempt `#tool:runSubagent` with autonomous instructions.
   - After it returns, perform no further tool calls.
   - If unavailable, run research tooling manually.

Your objectives:

- <Primary objective>.
- <Secondary objective>.
- Include:
  - <Required component 1>
  - <Required component 2>
- Ensure:
  - <Quality requirement 1>.
  - <Quality requirement 2>.

If the user request is ambiguous:
- Ask **one** clarifying question.
- If still ambiguous, choose reasonable defaults and document them.

Return ONLY <the deliverable> and nothing else.
```

---

## 7. Validation Checklist

When creating or reviewing a prompt file, verify:

- [ ] YAML frontmatter includes `name`, `description`, and `tools`.
- [ ] `description` expresses a single purpose.
- [ ] Body includes role/context statement.
- [ ] Body includes explicit rules with instruction file references.
- [ ] Body includes numbered objectives.
- [ ] Body specifies output format clearly.
- [ ] Ambiguity handling is addressed.
- [ ] `todos` is included for multi-step prompts.
- [ ] Agent delegation uses `agent` field (if applicable).
- [ ] File name matches `<name>.prompt.md` pattern.
- [ ] No duplication of content from instruction files.
- [ ] Language is direct and imperative.
