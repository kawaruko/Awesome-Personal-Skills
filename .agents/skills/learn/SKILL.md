---
name: learn
description: Plan, write, review, and index Chinese ML systems learning articles using the preserved .learn workflow. Use when the user asks for learn-plan, learn-write, learn-review, learn-add, or ML systems learning content.
---

# Learn

This skill wraps the preserved `.learn/` workflow as a standard Agent Skill.

Use this skill when the user wants to:

- generate a learning plan (`learn-plan`)
- write a Chinese ML systems learning article (`learn-write`)
- review or optionally translate an article (`learn-review`)
- add a published article to the knowledge graph (`learn-add`)

## Canonical Resources

The `.learn/` directory is intentionally preserved. Do not move it while using this skill.

Load these files as needed:

- `.learn/skill.md`: full workflow definition and command semantics
- `.learn/config.md`: agent preferences and adjustable behavior
- `.learn/index/style-guide.md`: writing style and quality guidance
- `.learn/templates/`: article templates for paper reading, tutorials, system design, and code walkthroughs
- `.learn/example/knowledge-graph.json`: published article metadata and relationships, but do not use this, as this is the original author's knowledge graph and cannot apply directly to you. Instead, please use it as a reference for how to structure your own knowledge graph.

## Workflow

1. Read `.learn/skill.md` first to identify the relevant subcommand workflow.
2. Route the task by intent:
   - planning work follows the `learn-plan` flow
   - writing work follows the `learn-write` flow
   - review or translation work follows the `learn-review` flow
   - knowledge graph updates follow the `learn-add` flow
3. Load only the supporting `.learn/` files required by that flow.
4. Preserve the hard constraints from `.learn/skill.md`, especially Chinese-first writing, excluding `[Pending Review]` articles as references, and requiring commit hashes for external source-code citations.

## Compatibility Notes

Claude Code can discover this skill through `.claude/skills/learn/SKILL.md`, which is only a wrapper. OpenAI local shell usage should pass this directory, `.agents/skills/learn`, as the skill path.
