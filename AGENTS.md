# Repository Instructions

## Skills Layout

- Canonical cross-client skills live in `.agents/skills/<skill-name>/SKILL.md`.
- Claude Code compatibility wrappers live in `.claude/skills/<skill-name>/SKILL.md`.
- Keep Claude wrappers thin: they should preserve discoverability metadata, then point to the canonical `.agents/skills/` instructions.
- Do not duplicate full workflows in wrappers.

## Learn Workflow

- `.learn/` is intentionally preserved as the learn skill's resource directory.
- `.agents/skills/learn/SKILL.md` is the standard skill entrypoint.
- `.learn/skill.md`, `.learn/config.md`, `.learn/index/`, and `.learn/templates/` remain the canonical learn workflow resources.

## OpenAI And Codex Usage

- Do not create a `.openai/` directory for skills; there is no repository auto-discovery convention for it here.
- For OpenAI API local shell mode, pass `.agents/skills/<skill-name>` explicitly as the skill path.
- For Codex work in this repository, inspect `.agents/skills/` when a task mentions one of these skills.
