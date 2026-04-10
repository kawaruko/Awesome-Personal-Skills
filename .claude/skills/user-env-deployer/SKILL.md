---
name: user-env-deployer
description: Deploy or align a user's local shell environment, especially zsh, oh-my-zsh, tmux, and dotfiles. Use when the user wants terminal setup, dev-container shell parity, or reproducible workstation environment configuration across platforms.
---

# User Env Deployer

## Overview

Use this skill when the user wants an agent to install or align a local terminal environment, especially `zsh`, `oh-my-zsh`, `tmux`, and related dotfiles. The goal is to inspect the current machine, choose the correct package/install path, patch existing shell files safely, verify the result, and leave behind a portable record the user can reuse elsewhere.

Load [references/dotfile-templates.md](./references/dotfile-templates.md) when you need the default `~/.zshrc`, `~/.tmux.conf`, platform package-manager mapping, or verification commands.

## Workflow

### 1. Ground in the current machine

- Detect OS and package manager before proposing commands.
- Inspect existing dotfiles before changing them:
  - `~/.zshrc`
  - `~/.zprofile`
  - `~/.tmux.conf`
  - `~/.oh-my-zsh`
- Check whether `tmux`, `zsh`, and `oh-my-zsh` are already present.
- If the request references another repo or container setup, inspect that source first and extract the actual shell-management pattern instead of guessing.
- For SGLang Docker parity, use `docker/configs/.zshrc` for shell init and `docker/configs/opt/.tmux.conf` for tmux, not the generic fallback template.

### 2. Choose the deployment shape

- Preserve existing user bootstrap lines unless they are clearly obsolete or the user asked for replacement.
- Default to manual tmux launch rather than auto-attaching from every interactive shell.
- If `oh-my-zsh` is absent and the user wants parity with an `oh-my-zsh`-based environment, install it and make `~/.zshrc` load it explicitly.
- Keep shell init conservative:
  - avoid aggressive auto-start logic
  - avoid replacing unrelated aliases or exports
  - avoid moving login-shell behavior into the wrong startup file

### 3. Apply dotfile changes

- Use additive, readable dotfiles.
- For `~/.zshrc`:
  - keep the user’s existing bootstrap lines near the top
  - set `ZSH="$HOME/.oh-my-zsh"` when `oh-my-zsh` is installed
  - if matching SGLang, prefer `robbyrussell` with `git z zsh-autosuggestions zsh-syntax-highlighting`
  - add only the aliases and history settings that the referenced environment actually uses
  - do not add tmux auto-start or helper functions unless the user asked for them
- For `~/.tmux.conf`:
  - if matching SGLang, mirror its pane/status styling, backtick prefix, vi copy mode, and `-`/`=` split bindings
  - if matching SGLang, note that its copy binding expects a `yank` helper on `PATH`
  - otherwise use a conservative fallback with mouse support, history limit, low escape time, and cwd-preserving splits

### 4. Verify behavior

- Confirm installs with version or presence checks.
- Start an interactive `zsh` and verify the expected helper function or alias exists.
- Create a disposable tmux session, load the config, list sessions, and clean up.
- If verification fails inside a restricted sandbox, rerun the relevant checks unsandboxed and record the sandbox caveat.

### 5. Export a reusable record

- When the user wants portability, write a Markdown file that captures:
  - platform and package manager
  - source inspiration, if any
  - deployment plan
  - commands actually run
  - final dotfile contents
  - verification commands
  - portability notes for Linux and Windows/WSL

## Guardrails

- Never overwrite user dotfiles blindly without first reading them.
- Do not assume Homebrew, `apt`, `dnf`, or `pacman`; detect what is available.
- Treat `oh-my-zsh` as optional unless the user explicitly wants it or the reference environment depends on it.
- Prefer the referenced environment's plugin set. Do not add extra prompt frameworks or plugin managers beyond the user request.
- If tmux socket creation fails in sandboxed verification, treat it as an environment constraint, not immediately as a config failure.

## Expected Outputs

- Updated shell dotfiles with minimal unrelated churn.
- Working `tmux` installation and helper command.
- A short verification summary.
- Optional Markdown handoff document for reproducing the environment on other machines.
