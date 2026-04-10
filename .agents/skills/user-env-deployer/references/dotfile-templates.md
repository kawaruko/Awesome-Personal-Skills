# Dotfile Templates

## SGLang-style `~/.zshrc`

Use this when the user wants parity with the SGLang Docker development image. Preserve any existing bootstrap line such as `. "$HOME/.local/bin/env"` above this block, and change the `ZSH` path from `/root/.oh-my-zsh` to `$HOME/.oh-my-zsh` on a local machine.

```zsh
export ZSH="$HOME/.oh-my-zsh"
ZSH_THEME="robbyrussell"
plugins=(
    git
    z
    zsh-autosuggestions
    zsh-syntax-highlighting
)

source "$ZSH/oh-my-zsh.sh"

alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
alias vi='vim'

HISTSIZE=10000
SAVEHIST=10000
setopt HIST_IGNORE_ALL_DUPS
setopt HIST_FIND_NO_DUPS
setopt INC_APPEND_HISTORY
```

## SGLang-style `~/.tmux.conf`

```tmux
# .tmux.conf
# Pane border styling
set -g pane-border-style fg='#742727',bg=black
set -g pane-active-border-style fg=red,bg=black

# Status bar styling
set -g status-style bg='#0C8A92',fg=black

# Change prefix key to backtick
set-option -g prefix `
unbind C-b
bind-key ` send-prefix

# Split panes using - and = with current path
unbind '"'
bind - splitw -v -c '#{pane_current_path}'
unbind '%'
bind = splitw -h -c '#{pane_current_path}'

# Vi mode settings
bind-key -T copy-mode-vi Y send-keys -X copy-pipe 'yank > #{pane_tty}'
set-window-option -g mode-keys vi

# Other settings
set-option -g escape-time 0
set-option -g base-index 1
set-window-option -g mouse on
set -g history-limit 100000
```

Note: this upstream config expects a `yank` executable for the `copy-mode-vi` `Y` binding. In SGLang's Docker image it is installed separately at `/usr/local/bin/yank`.

## Platform Package Mapping

- macOS:
  - `brew install tmux zsh-autosuggestions zsh-syntax-highlighting`
  - install `oh-my-zsh` by cloning `https://github.com/ohmyzsh/ohmyzsh.git` into `~/.oh-my-zsh`
- Ubuntu/Debian:
  - `sudo apt install tmux zsh git zsh-autosuggestions zsh-syntax-highlighting autojump`
- Fedora:
  - `sudo dnf install tmux zsh git zsh-autosuggestions zsh-syntax-highlighting autojump-zsh`
- Arch:
  - `sudo pacman -S tmux zsh git zsh-autosuggestions zsh-syntax-highlighting autojump`
- Windows:
  - prefer WSL, then follow the Linux path inside WSL

## Verification Commands

```bash
tmux -V
test -d ~/.oh-my-zsh && echo "oh-my-zsh installed"
zsh -i -c 'echo $ZSH_THEME; alias ll; print -l -- $plugins'
zsh -i -c 'tmux new-session -d -s verify; tmux source-file ~/.tmux.conf; tmux list-sessions; tmux kill-session -t verify'
```

Expected results:

- `tmux -V` prints a version.
- `oh-my-zsh installed` is printed when applicable.
- `zsh` shows the expected theme, aliases, and plugins.
- tmux creates and lists the `verify` session without config errors.

## Sandbox Note

Some restricted environments block tmux socket creation under `/tmp` or `/private/tmp`. If tmux verification fails only in the sandbox, rerun verification in a normal local shell before changing the config.
