# MyClaude — Custom Skills Repository

This project contains custom Claude Code skills.

## Skills

### /create-skill
Interview-driven skill creator. Asks structured questions to gather all requirements upfront, then generates a complete, working Claude Code skill in one pass — no back-and-forth.

**Usage:**
```
/create-skill
/create-skill my-skill-name
```

## Project Structure

```
.claude/skills/
  create-skill/
    SKILL.md              — Main skill instructions (interview flow)
    template.md           — Template structure for generated skills
    examples/
      sample-generated-skill.md — Example of a skill produced by /create-skill
```

## Conventions

- Skills live in `.claude/skills/<skill-name>/SKILL.md`
- Skill names: lowercase, hyphens, max 64 characters
- Keep SKILL.md files under 200 lines — use supporting files for detailed reference
