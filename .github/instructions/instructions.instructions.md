---
applyTo: "**/*.instructions.md"
---

# Instructions File Specification

This document defines the **mandatory structure, rules, and style** for all `.instructions.md` files in this workspace.

All instruction files must complement (not duplicate) the global rules in `.github/copilot-instructions.md`.

---

## 1. File Format

Instruction files use the `.instructions.md` extension and consist of:
1. **YAML frontmatter** (required)
2. **Markdown body** with structured specification (required)

---

## 2. YAML Frontmatter (Required)

Every instruction file MUST include a YAML frontmatter block with:

| Field     | Required | Description                                                              |
|-----------|----------|--------------------------------------------------------------------------|
| `applyTo` | Yes      | Glob pattern specifying which files these instructions apply to          |

**Examples:**
```yaml
---
applyTo: "**/*.agent.md"
---

---
applyTo: "**/*.prompt.md"
---

---
applyTo: "src/**/*.ts"
---

---
applyTo: "**/*.test.ts"
---
```

---

## 3. Body Structure (Required)

The markdown body MUST include:

### 3.1 Title and Purpose
- H1 heading with the specification name.
- Brief paragraph stating:
  - What this specification defines.
  - What file types it applies to.
  - Relationship to global instructions.

### 3.2 File Format Section
- Extension and naming conventions.
- Required file components (frontmatter, body, etc.).

### 3.3 Frontmatter Specification (If Applicable)
- Table of fields with Required/Optional status.
- Field descriptions and examples.
- Example frontmatter block.

### 3.4 Body/Content Specification
- Required sections in order.
- What each section must contain.
- Formatting requirements.

### 3.5 Mandatory Rules
- Core requirements that MUST be followed.
- Numbered subsections for distinct rule categories.
- Clear, enforceable statements.

### 3.6 Style Guidelines
- Naming conventions.
- Language requirements.
- Formatting standards.

### 3.7 Template
- Complete example file showing all requirements.
- Placeholder text for customizable parts.
- Proper formatting markers.

### 3.8 Validation Checklist
- Checkbox list for verification.
- Covers all mandatory rules.
- Enables consistent review.

---

## 4. Mandatory Rules

### 4.1 Scope Clarity
- The `applyTo` glob pattern must be specific and accurate.
- Instructions apply ONLY to matching files.
- Avoid overly broad patterns that create conflicts.

### 4.2 Complementary, Not Duplicative
- Do NOT restate rules from `.github/copilot-instructions.md`.
- Reference global instructions when relevant.
- Add only file-type-specific requirements.

### 4.3 Actionable Requirements
- Every rule must be enforceable.
- Avoid vague guidance ("consider", "try to").
- Use imperative language ("MUST", "Do NOT").

### 4.4 Complete Specification
- Include all sections listed in Body Structure.
- Provide a complete, usable template.
- Include a validation checklist.

### 4.5 Hierarchical Consistency
- Instructions must not contradict global rules.
- More specific instructions can add constraints.
- More specific instructions cannot remove global constraints.

---

## 5. Style Guidelines

### 5.1 Naming
- File name: `<scope>.instructions.md` (e.g., `agents.instructions.md`, `prompts.instructions.md`).
- Scope should match the primary target (e.g., "agents" for `*.agent.md`).

### 5.2 Language
- Direct and prescriptive.
- Use "MUST", "MUST NOT", "Do NOT" for requirements.
- Use "Recommended", "Prefer" for guidelines.
- Avoid passive voice.

### 5.3 Formatting
- Use H2 (`##`) for major sections.
- Use H3 (`###`) for subsections.
- Use tables for field specifications.
- Use numbered subsections for rules (e.g., `### 4.1 Rule Name`).
- Use checkbox lists for validation checklists.
- Use code blocks for examples and templates.
- Wrap templates in appropriate language markers (` ```yaml `, ` ```chatagent `, etc.).

### 5.4 Section Numbering
- Number major sections sequentially (1, 2, 3...).
- Number subsections hierarchically (4.1, 4.2, 5.1...).
- Keep numbering consistent throughout.

### 5.5 Cross-References
- Reference other instruction files by relative path.
- Reference global instructions explicitly when adding to them.

---

## 6. Template

```instructions
---
applyTo: "<glob-pattern>"
---

# <Scope> File Specification

This document defines the **mandatory structure, rules, and style** for all `.<extension>` files in this workspace.

All <scope> files must comply with the global rules in `.github/copilot-instructions.md`. This file adds <scope>-specific requirements.

---

## 1. File Format

<Scope> files use the `.<extension>` extension and consist of:
1. **YAML frontmatter** (required/optional)
2. **<Body type>** (required)

---

## 2. YAML Frontmatter (<Required/Optional>)

Every <scope> file MUST/MAY include a YAML frontmatter block with these fields:

| Field    | Required | Description                          |
|----------|----------|--------------------------------------|
| `<field>`| Yes/No   | <Description>                        |

**Example:**
```yaml
---
<field>: <value>
---
```

---

## 3. Body Structure (Required)

The body MUST include:

### 3.1 <Section Name>
<What this section contains.>

### 3.2 <Section Name>
<What this section contains.>

---

## 4. Mandatory Rules

### 4.1 <Rule Category>
- <Rule 1>.
- <Rule 2>.

### 4.2 <Rule Category>
- <Rule 1>.
- <Rule 2>.

---

## 5. Style Guidelines

### 5.1 Naming
- <Naming convention>.

### 5.2 Language
- <Language requirements>.

### 5.3 Formatting
- <Formatting requirements>.

---

## 6. Template

```<language>
---
<frontmatter>
---

<Body template with placeholders>
```

---

## 7. Validation Checklist

When creating or reviewing a <scope> file, verify:

- [ ] <Requirement 1>.
- [ ] <Requirement 2>.
- [ ] <Requirement 3>.
```

---

## 7. Validation Checklist

When creating or reviewing an instruction file, verify:

- [ ] YAML frontmatter includes accurate `applyTo` glob pattern.
- [ ] Title clearly identifies the specification scope.
- [ ] Purpose statement references relationship to global instructions.
- [ ] File format section defines extension and components.
- [ ] Frontmatter specification includes field table (if applicable).
- [ ] Body structure lists all required sections in order.
- [ ] Mandatory rules are actionable and enforceable.
- [ ] Style guidelines cover naming, language, and formatting.
- [ ] Template is complete and usable.
- [ ] Validation checklist covers all mandatory rules.
- [ ] No duplication of global rules from `copilot-instructions.md`.
- [ ] File name matches `<scope>.instructions.md` pattern.
- [ ] Section numbering is consistent.
- [ ] Language is direct and prescriptive.
