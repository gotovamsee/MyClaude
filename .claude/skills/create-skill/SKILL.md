---
name: create-skill
description: Create a new Claude Code skill by interviewing the user to extract all required details upfront. Use when the user wants to create, build, or make a new skill, slash command, or custom command for Claude Code.
argument-hint: "[skill-name (optional)]"
disable-model-invocation: true
---

# Skill Creator — Interview-First Approach

You are a skill architect. Your job is to interview the user with structured questions to gather every detail needed to produce a complete, working Claude Code skill — **before writing any code or files**. This eliminates back-and-forth by collecting all requirements in one pass.

## Process

Follow these three phases strictly:

---

### Phase 1: Discovery Interview

Conduct a structured interview by presenting ALL of the following questions to the user in a single message. Group them clearly so the user can answer them all at once.

Present this interview form:

```
I'll help you create a new skill! Please answer these questions so I can build it perfectly in one shot.

**── Core Identity ──**

1. **Skill name**: What should the slash command be called?
   (e.g., "deploy", "review-pr", "explain-code" — lowercase, hyphens only)
   ${IF_ARGUMENT_PROVIDED: Suggested name: `$ARGUMENTS`}

2. **One-line description**: What does this skill do in one sentence?

3. **Detailed purpose**: What problem does this skill solve? What's the typical scenario where someone would use it?

**── Invocation & Triggers ──**

4. **How should it be invoked?**
   - [ ] Manual only — user types the /command explicitly
   - [ ] Auto-detected — Claude uses it automatically when relevant
   - [ ] Both — user can invoke it, and Claude can also auto-detect when it's relevant

5. **Arguments**: Does the skill need any input arguments?
   - If yes, list them with format: `[arg-name]` required or optional
   - Example: `[file-path]` required, `[output-format]` optional

6. **Trigger phrases**: What would a user typically say that should activate this skill?
   (e.g., "deploy to production", "review this PR", "explain how this works")

**── Behavior & Instructions ──**

7. **Step-by-step behavior**: What should Claude do when this skill runs? List the steps in order.
   (e.g., 1. Read the file  2. Analyze for issues  3. Suggest fixes  4. Apply if confirmed)

8. **Output format**: How should the result be presented?
   - [ ] Free-form text
   - [ ] Structured markdown (headings, tables, lists)
   - [ ] Code generation (new files)
   - [ ] Code modification (edit existing files)
   - [ ] Conversational (ask follow-ups as needed)
   - [ ] Other: ___

9. **Constraints or rules**: Any specific dos/don'ts?
   (e.g., "never modify tests directly", "always ask before deleting", "use TypeScript only")

**── Technical Configuration ──**

10. **Tool access**: What tools should this skill be allowed to use?
    - [ ] All tools (default)
    - [ ] Read-only (Read, Grep, Glob only)
    - [ ] Limited: specify which tools ___
    - [ ] Shell commands: specify patterns (e.g., `Bash(npm *)`, `Bash(git *)`)

11. **Execution context**: How should it run?
    - [ ] Inline — in the main conversation (default, good for most skills)
    - [ ] Forked — in an isolated subagent (good for heavy research/analysis)

12. **Dynamic context**: Does the skill need live data injected before running?
    (e.g., current git diff, PR comments, file listing, environment variables)
    If yes, what shell commands should provide this data?

**── Scope & Location ──**

13. **Where should this skill live?**
    - [ ] This project only (`.claude/skills/`)
    - [ ] All my projects — personal (`~/.claude/skills/`)

**── Examples (Optional but Recommended) ──**

14. **Example invocation**: Show me how you'd use this skill.
    (e.g., `/deploy staging`, `/review-pr 42`, `/explain-code src/auth.ts`)

15. **Example output**: What would ideal output look like? (Paste a sample or describe it)

**── Anything Else? ──**

16. **Additional notes**: Anything I missed? Special edge cases, integrations, or preferences?
```

Wait for the user to respond. Do NOT proceed until you have answers.

---

### Phase 2: Confirmation & Gap-Filling

After receiving answers:

1. **Summarize** what you understood in a compact spec table:

```
| Field                  | Value                           |
|------------------------|---------------------------------|
| Name                   | /skill-name                     |
| Description            | ...                             |
| Invocation             | Manual / Auto / Both            |
| Arguments              | [arg1] required, [arg2] opt     |
| Tools                  | All / Read-only / Custom list   |
| Context                | Inline / Forked                 |
| Dynamic data           | None / commands listed          |
| Location               | Project / Personal              |
```

2. **Identify gaps**: If any critical detail is missing or ambiguous, ask ONLY those specific follow-up questions — not the full interview again.

3. **Get confirmation**: Ask the user to confirm or adjust before generating.

---

### Phase 3: Generation

Once confirmed, generate the complete skill:

1. **Create the directory** at the chosen location:
   - Project: `.claude/skills/<skill-name>/`
   - Personal: `~/.claude/skills/<skill-name>/`

2. **Write `SKILL.md`** using this structure:

```yaml
---
name: <skill-name>
description: <description with trigger keywords>
argument-hint: <hint if arguments exist>
# Include these only when needed:
# disable-model-invocation: true     (if manual-only)
# user-invocable: false              (if auto-only)
# allowed-tools: <tool list>         (if restricted)
# context: fork                      (if forked)
# agent: <type>                      (if forked: Explore, Plan, general-purpose)
---

# <Skill Title>

<Clear instructions for Claude to follow when this skill is active>

## Steps
1. ...
2. ...

## Rules
- ...

## Output Format
...
```

3. **Add supporting files** if the skill benefits from them:
   - `template.md` — for skills that generate structured output
   - `examples/` — sample inputs/outputs for complex skills

4. **Test guidance**: After generation, tell the user how to test:
   - "Try it now: `/<skill-name> <example-args>`"
   - "Or ask Claude something that matches the trigger phrases to test auto-invocation"

---

## Quality Standards

When generating the SKILL.md content:

- **Description**: Include natural-language keywords users would say — this drives auto-invocation accuracy
- **Instructions**: Be specific and imperative ("Read the file", "List all issues"), not vague ("Consider looking at...")
- **Constraints**: Encode guardrails directly ("NEVER modify files without confirmation", "ALWAYS show a preview first")
- **Keep SKILL.md under 200 lines** — move reference material to separate files
- **Use $ARGUMENTS** placeholders for dynamic input
- **Use !`command`** syntax for dynamic context injection where the user requested live data
