---
name: {{SKILL_NAME}}
description: {{DESCRIPTION}}
{{#IF_ARGUMENTS}}
argument-hint: {{ARGUMENT_HINT}}
{{/IF_ARGUMENTS}}
{{#IF_MANUAL_ONLY}}
disable-model-invocation: true
{{/IF_MANUAL_ONLY}}
{{#IF_AUTO_ONLY}}
user-invocable: false
{{/IF_AUTO_ONLY}}
{{#IF_RESTRICTED_TOOLS}}
allowed-tools: {{TOOL_LIST}}
{{/IF_RESTRICTED_TOOLS}}
{{#IF_FORKED}}
context: fork
agent: {{AGENT_TYPE}}
{{/IF_FORKED}}
---

# {{SKILL_TITLE}}

{{#IF_DYNAMIC_CONTEXT}}
## Context
{{DYNAMIC_COMMANDS}}
{{/IF_DYNAMIC_CONTEXT}}

## Purpose

{{PURPOSE}}

## Steps

{{STEPS}}

## Rules

{{RULES}}

## Output Format

{{OUTPUT_FORMAT}}
