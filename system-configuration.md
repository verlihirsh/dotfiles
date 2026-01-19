# System Configuration Reference

> Comprehensive documentation of shell, editor, and tool configurations  
> Managed by chezmoi | Last updated: 2026-01-16

---

## Table of Contents

1. [Overview & Philosophy](#1-overview--philosophy)
2. [Shell Configuration](#2-shell-configuration)
3. [Starship Prompt](#3-starship-prompt)
4. [tmux Configuration](#4-tmux-configuration)
5. [Neovim Configuration](#5-neovim-configuration)
6. [Git Configuration](#6-git-configuration)
7. [WTF Dashboard](#7-wtf-dashboard)
8. [OpenCode AI Assistant](#8-opencode-ai-assistant)
9. [Other Tools](#9-other-tools)

---

## 1. Overview & Philosophy

### Configuration Management
- **Tool**: chezmoi (dotfile manager)
- **Theme**: Catppuccin Mocha (consistent across all tools)
- **Terminal Workflow**: tmux-first approach
- **Editor**: Neovim as primary editor
- **Fuzzy Finding**: fzf integrated everywhere

### Directory Structure
```
~/.config/
├── nvim/           # Neovim configuration
├── starship.toml   # Prompt configuration
├── wtf/            # Dashboard configuration
├── opencode/       # AI assistant configuration
├── htop/           # Process viewer
└── neofetch/       # System info display

~/.bashrc           # Common shell config
~/.zshrc            # Zsh-specific config
~/.zshrc.local      # Local overrides
~/.tmux.conf        # tmux configuration
~/.gitconfig        # Git configuration
```

---

## 2. Shell Configuration

### Files
| File | Purpose | Lines |
|------|---------|-------|
| `~/.bashrc` | Common shell config (bash/zsh) | 237 |
| `~/.zshrc` | Zsh-specific + sources bashrc | 58 |
| `~/.zshrc.local` | Local overrides, Proton Pass, dashboard | 258 |

### Environment Variables

```bash
# Editor
export EDITOR="nvim"
export VISUAL="nvim"

# Locale
export LANG="en_US.UTF-8"
export LC_ALL="en_US.UTF-8"

# XDG Base Directory
export XDG_CONFIG_HOME="$HOME/.config"
export XDG_DATA_HOME="$HOME/.local/share"
export XDG_CACHE_HOME="$HOME/.cache"

# PATH additions
export PATH="$HOME/.local/bin:$PATH"
export PATH="$HOME/.opencode/bin:$HOME/.bun/bin:$PATH"
```

### Tool Initialization

| Tool | Purpose | Init |
|------|---------|------|
| Homebrew | Package manager | `eval "$(/opt/homebrew/bin/brew shellenv)"` |
| direnv | Per-directory environments | `eval "$(direnv hook zsh)"` |
| fzf | Fuzzy finder | `source ~/.fzf.zsh` |
| zoxide | Smart cd | `eval "$(zoxide init zsh)"` |
| starship | Prompt | `eval "$(starship init zsh)"` |

### fzf Configuration

```bash
# Use fd as backend (faster, respects .gitignore)
export FZF_DEFAULT_COMMAND='fd --type f --hidden --follow --exclude .git'
export FZF_CTRL_T_COMMAND="$FZF_DEFAULT_COMMAND"
export FZF_ALT_C_COMMAND='fd --type d --hidden --follow --exclude .git'

# UI options
export FZF_DEFAULT_OPTS=" \
    --style=full \
    --tmux=center,80%,60% \
    --preview 'fzf-preview.sh {}' \
    --bind 'focus:transform-header:file'"
```

### CLI Tool Replacements

| Original | Replacement | Config |
|----------|-------------|--------|
| `cat` | `bat` | Theme: Catppuccin Mocha, `--paging=never` |
| `ls` | `eza` | Aliases: `ll`, `la`, `lt` (tree) |
| `cd` | `zoxide` | Smart directory jumping |

### Aliases

```bash
# Git shortcuts
alias g='git'
alias gs='git status'
alias gd='git diff'
alias gc='git commit'
alias gp='git push'
alias gl='git pull'

# Safety nets
alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'

# Editor
alias vim='nvim'
alias vi='nvim'

# Misc
alias c='clear'
alias h='history'
alias j='jobs -l'
```

### Zsh-Specific Settings

```bash
# History
HISTSIZE=50000
SAVEHIST=50000
HISTFILE="$HOME/.zsh_history"

setopt HIST_IGNORE_DUPS
setopt HIST_IGNORE_SPACE
setopt HIST_FIND_NO_DUPS
setopt SHARE_HISTORY
setopt EXTENDED_HISTORY
setopt AUTO_CD
```

### Zsh Plugins

| Plugin | Purpose | Source |
|--------|---------|--------|
| zsh-autosuggestions | Ghost text from history | Homebrew |
| zsh-syntax-highlighting | Command highlighting | Homebrew |
| zsh-history-substring-search | History search with Up/Down | Homebrew |

### Proton Pass Integration

#### Functions
```bash
pp-get-password <item-title> [vault]  # Get password
pp-get-username <item-title> [vault]  # Get username
pp-get-field <item-title> <field> [vault]  # Get any field
pp-copy-password <item-title> [vault]  # Copy password to clipboard
pp-copy-username <item-title> [vault]  # Copy username to clipboard
pp-select [vault]  # Interactive fzf picker with preview
pp-copy  # Interactive copy with fzf
```

#### Keybindings
| Key | Action |
|-----|--------|
| `Ctrl+Alt+P` | Insert password at cursor |
| `Ctrl+Alt+U` | Insert username at cursor |

#### Aliases
```bash
alias pp='pass-cli'
alias ppls='pass-cli item list "$PP_VAULT"'
alias ppv='pass-cli item view "$PP_VAULT"'
alias ppc='pp-copy'
alias ppcp='pp-copy-password'
alias ppcu='pp-copy-username'
```

### tmux Helpers

```bash
# Session management
alias ta='tmux attach-session -t'
alias tl='tmux list-sessions'
alias tn='tmux new-session -s'
alias tk='tmux kill-session -t'
alias ts='tmux switch-client -t'

# Functions
tcd [name]   # Quick session from current directory
tpick        # fzf session/window picker with preview
tnew [name]  # Create new session with picker fallback
```

### Utility Functions

```bash
mkcd <dir>     # Create directory and cd into it
extract <file> # Extract various archive formats (.tar.gz, .zip, .7z, etc.)
```

### tmux-First Terminal

On terminal start, automatically attaches to 'main' tmux session:
- If session exists: creates new window and attaches
- If no session: creates 'main' session
- Skipped when: inside vim, emacs, or dumb terminal

### Dashboard on Startup

Shows WTF dashboard on first pane of first window in new tmux sessions.

---

## 3. Starship Prompt

### File
`~/.config/starship.toml` (330 lines)

### Theme
Catppuccin Mocha palette

### Prompt Format
```
[OS][User] [Directory] [Git Branch][Git Status] [Languages] [Docker/Conda] [Time] [Duration]
❯ (or ❮ in vim mode)
```

### Segments

| Segment | Style | Details |
|---------|-------|---------|
| OS | bg:blue fg:crust | macOS icon  |
| Username | bg:blue fg:crust | Always shown |
| Directory | bg:sky fg:crust | Truncated to 3 levels |
| Git Branch | bg:yellow | Symbol:  |
| Git Status | bg:yellow | All status + ahead/behind |
| Languages | bg:green | Node, Rust, Go, Python, PHP, Java, Kotlin, Haskell, C |
| Docker/Conda | bg:sapphire | Context name |
| Time | bg:lavender | HH:MM format |
| Duration | - | Shows for commands > 0ms |

### Directory Substitutions
```toml
"Documents" = "󰈙 "
"Downloads" = " "
"Music" = "󰝚 "
"Pictures" = " "
"Developer" = "󰲋 "
```

### Language Symbols
```
Node.js:  | Rust:  | Go:  | Python: 
PHP:  | Java:   | Kotlin:  | Haskell: 
C:   | Ruby:   | Docker: 
```

### Vim Mode Indicators
| Mode | Symbol | Color |
|------|--------|-------|
| Normal | `❯` | green |
| Insert | `❯` | green |
| Command | `❮` | green |
| Replace | `❮` | lavender |
| Visual | `❮` | yellow |

### Command Duration
- Shows milliseconds
- Desktop notification for commands > 45 seconds

---

## 4. tmux Configuration

### File
`~/.tmux.conf` (119 lines)

### General Settings

```bash
# True color support
set -g default-terminal "screen-256color"
set -ga terminal-overrides ",*256col*:Tc"

# Mouse support
set -g mouse on

# History
set -g history-limit 50000

# Faster command sequences
set -sg escape-time 0

# Window indexing from 1
set -g base-index 1
setw -g pane-base-index 1
set -g renumber-windows on
```

### Key Bindings

| Key | Action |
|-----|--------|
| `` ` `` (backtick) | Prefix key |
| `prefix + \|` | Split horizontal |
| `prefix + -` | Split vertical |
| `prefix + h/j/k/l` | Navigate panes (vim-style) |
| `prefix + H/J/K/L` | Resize panes (repeatable) |
| `prefix + r` | Reload config |
| `prefix + c` | New window (in current path) |
| `prefix + f` | fzf session/window picker |

### Status Bar

```bash
# Style
set -g status-style 'bg=#333333 fg=#ffffff'
set -g status-position bottom

# Left: Session name
set -g status-left '#[fg=#00ff00,bold]#S #[fg=#ffffff]| '

# Right: Date and time
set -g status-right '#[fg=#888888]%Y-%m-%d #[fg=#ffffff]%H:%M'

# Window format
setw -g window-status-format ' #I:#W '
setw -g window-status-current-format '#[fg=#00ff00,bold] #I:#W '
```

### Pane Border
```bash
set -g pane-border-style 'fg=#444444'
set -g pane-active-border-style 'fg=#00ff00'
```

### Plugins (via TPM)

| Plugin | Purpose |
|--------|---------|
| tpm | Plugin manager |
| tmux-sensible | Sensible defaults |
| tmux-resurrect | Persist sessions across restarts |
| tmux-continuum | Auto-restore sessions |

```bash
# Auto-restore enabled
set -g @continuum-restore 'on'
```

---

## 5. Neovim Configuration

### Files

| File | Purpose |
|------|---------|
| `~/.config/nvim/init.lua` | Entry point, lazy.nvim setup |
| `~/.config/nvim/lua/options.lua` | Editor options |
| `~/.config/nvim/lua/keymaps.lua` | Which-key mappings |
| `~/.config/nvim/lua/dashboard.lua` | Alpha dashboard |
| `~/.config/nvim/lua/plugins/init.lua` | Plugin configurations |
| `~/.config/nvim/lua/plugins/lspconfig.lua` | LSP setup |
| `~/.config/nvim/lua/plugins/blink.lua` | Completion |
| `~/.config/nvim/lua/plugins/catppuccin.lua` | Theme |
| `~/.config/nvim/lua/plugins/neotree.lua` | File explorer |
| `~/.config/nvim/lua/plugins/noice.lua` | UI enhancements |

### Leader Keys
```lua
vim.g.mapleader = " "       -- Space
vim.g.maplocalleader = "\\" -- Backslash
```

### Editor Options

```lua
-- Display
vim.opt.number = true
vim.opt.relativenumber = true
vim.opt.cursorline = true
vim.opt.wrap = false
vim.opt.showmatch = true
vim.opt.termguicolors = true
vim.opt.fillchars = { eob = " " }  -- Hide tildes

-- Indentation
vim.opt.expandtab = true
vim.opt.shiftwidth = 2
vim.opt.tabstop = 2
vim.opt.smartindent = true

-- Search
vim.opt.ignorecase = true
vim.opt.smartcase = true
vim.opt.hlsearch = true
vim.opt.incsearch = true

-- UX
vim.opt.hidden = true
vim.opt.updatetime = 300
vim.opt.scrolloff = 8
vim.opt.sidescrolloff = 8
vim.opt.splitbelow = true
vim.opt.splitright = true

-- Files
vim.opt.backup = false
vim.opt.writebackup = false
vim.opt.swapfile = false
vim.opt.undofile = true
vim.opt.fileencoding = "utf-8"

-- Clipboard (disabled integration, manual shortcuts)
vim.opt.clipboard = ""
```

### Clipboard Shortcuts
| Key | Mode | Action |
|-----|------|--------|
| `ytc` | n, v | Copy to system clipboard |
| `pfc` | n, v | Paste from system clipboard |

### Russian Keyboard Layout Support
Full langmap configuration for Russian layout.

### Theme & UI

- **Colorscheme**: Catppuccin Mocha
- **Statusline**: Lualine with pomo timer integration
- **Bufferline**: Tabline with diagnostics, slant separators
- **File Explorer**: Neo-tree with multi-source support
- **Command UI**: Noice with floating command palette
- **Notifications**: nvim-notify with slide animation

### LSP Configuration

#### Configured Languages

| Language | Server | Features |
|----------|--------|----------|
| Ruby | ruby-lsp | Full (via asdf) |
| Lua | lua_ls | Full with Neovim runtime |
| Go | gopls | Full with hints, codelens |
| TypeScript/JavaScript | ts_ls | Full with inlay hints |
| Svelte | svelteserver | Full |
| Vue | vue_ls | Full with hybrid mode |
| Python | pyright | Full with type checking |
| Ruby (lint) | rubocop | Linting only |

#### LSP Features
- Inlay hints (auto-enabled)
- Code lens (auto-refresh)
- Semantic tokens
- Auto-diagnostics on cursor hold (1s delay)

#### Diagnostics Configuration
```lua
vim.diagnostic.config({
  virtual_text = false,
  float = {
    source = "always",
    border = "rounded",
    focusable = false,
  },
  signs = true,
  underline = true,
  severity_sort = true,
})
```

### Key Plugins

| Plugin | Purpose |
|--------|---------|
| lazy.nvim | Plugin manager |
| blink.cmp | Completion with Copilot |
| conform.nvim | Formatting (rubocop) |
| gitsigns | Git decorations |
| vim-fugitive | Git commands |
| diffview.nvim | Git diff viewer |
| which-key | Keybinding discovery |
| persistence.nvim | Session management |
| pomo.nvim | Pomodoro timer |
| opencode.nvim | AI assistance |
| nvim-treesitter | Syntax highlighting |
| Comment.nvim | Commenting (`<leader>/`) |
| nvim-surround | Surround operations |
| nvim-autopairs | Auto-close brackets |
| indent-blankline | Indent guides |
| neoscroll.nvim | Smooth scrolling |
| todo-comments | TODO highlighting |
| vim-sleuth | Auto-detect indent |

### Completion (blink.cmp)

```lua
sources = {
  default = { 'lsp', 'path', 'snippets', 'buffer', 'copilot' },
}
```

- Copilot integration with score offset 100 (prioritized)
- Documentation auto-show with 500ms delay
- Rust fuzzy matcher

### Leader Key Mappings

#### File Operations (`<leader>f`)
| Key | Action |
|-----|--------|
| `ff` | Find file (Snacks picker) |
| `fg` | Grep text |
| `fn` | New file |

#### Buffer Management (`<leader>b`)
| Key | Action |
|-----|--------|
| `bd` | Delete buffer |
| `bp` | Toggle pin |
| `bf` | Find buffer |
| `bb` | Pick buffer |
| `bD` | Delete all buffers |
| `bco` | Close other buffers |
| `bcr` | Close buffers to right |
| `bcl` | Close buffers to left |

#### Git Operations (`<leader>g`)
| Key | Action |
|-----|--------|
| `gg` | Git status |
| `gf` | File history |
| `gbb` | List branches |
| `gbn` | New branch |
| `gll` | Blame line |
| `glf` | Blame file |
| `gch` | Commit history |
| `gcam` | Add all & commit |
| `gcama` | Add all & commit using LLM |
| `gss` | Stash |
| `gsp` | Stash pop |
| `gpp` | Push |
| `gpsup` | Push & set upstream |
| `gds` | Changed files |
| `gdv` | Diffview open |
| `gdh` | File history |
| `gdH` | Branch history |
| `gdq` | Close diffview |

#### Code Actions (`<leader>c`)
| Key | Action |
|-----|--------|
| `ca` | Code action |
| `cr` | Rename symbol |
| `cf` | Format |
| `cl` | Run codelens |
| `cL` | Refresh codelens |
| `ch` | Toggle inlay hints |
| `co` | Organize imports |
| `cd` | Insert binding.irb (Ruby) |
| `csd` | Document symbols |
| `csw` | Workspace symbols |

#### Diagnostics (`<leader>l`)
| Key | Action |
|-----|--------|
| `ld` | Line diagnostic |
| `lb` | Buffer diagnostics |
| `lw` | Workspace diagnostics |

#### AI (OpenCode) (`<leader>a`)
| Key | Action |
|-----|--------|
| `aa` | Ask about this (n, v) |
| `ae` | Select action |
| `at` | Toggle panel |
| `aou` | Half page up |
| `aod` | Half page down |

#### Explorer (`<leader>e`)
| Key | Action |
|-----|--------|
| `ee` | Toggle Explorer |
| `ef` | Focus Files |
| `eb` | Focus Buffers |
| `eg` | Focus Git |
| `es` | Focus Symbols |
| `em` | Focus Marks |
| `el` | Toggle left panel |
| `er` | Toggle right panel |
| `en` | Cycle windows |
| `et` | Toggle Terminal |

#### Timer (Pomodoro) (`<leader>t`)
| Key | Action |
|-----|--------|
| `ts2` | 2h focus session |
| `ts1` | 1h focus session |
| `tsr` | Rest session (30m) |
| `tts` | Stop timer |
| `ttp` | Pause timer |
| `ttr` | Resume timer |
| `ttc` | Custom timer |

#### Pickers (`<leader>p`)
| Key | Action |
|-----|--------|
| `pf` | Files |
| `pr` | Recent files |
| `ps` | Smart find |
| `pb` | Buffers |
| `pp` | Projects |
| `pe` | Explorer |
| `pz` | Zoxide dirs |
| `pg` | Grep |
| `pG` | Grep buffers |
| `pw` | Grep word |
| `pl` | Buffer lines |
| `pvs` | Git status |
| `pvb` | Git branches |
| `pvl` | Git log |
| `pvf` | Git file log |
| `pvS` | Git stash |
| `pvd` | Git diff |
| `pnc` | Commands |
| `pnh` | Command history |
| `pns` | Search history |
| `pnk` | Keymaps |
| `pna` | Autocmds |
| `pnH` | Highlights |
| `pnC` | Colorschemes |
| `pno` | Options |
| `phh` | Help tags |
| `phm` | Man pages |
| `pm` | Marks |
| `p'` | Registers |
| `pj` | Jumps |
| `pq` | Quickfix |
| `pL` | Location list |
| `pu` | Undo history |
| `pi` | Icons |
| `pS` | Spelling |
| `pN` | Notifications |
| `pT` | TODOs |
| `pY` | Lazy plugins |
| `p.` | Resume last picker |

#### Session/Quit (`<leader>q`)
| Key | Action |
|-----|--------|
| `qq` | Quit without saving |
| `qwq` | Save all and quit |

### LSP Go-to Mappings (on LspAttach)

| Key | Action |
|-----|--------|
| `gd` | Go to definition |
| `gr` | References |
| `gi` | Implementations |
| `gy` | Type definition |
| `gD` | Go to declaration |
| `gci` | Incoming calls |
| `gco` | Outgoing calls |
| `K` | Hover |
| `<C-k>` (insert) | Signature help |
| `[d` | Previous diagnostic |
| `]d` | Next diagnostic |

### Dashboard (Alpha)

Shows on startup:
- ASCII art header
- Project name
- Git branch
- Changed files count
- LSP status
- Date & time
- Lazy.nvim stats

Quick actions:
- New File
- Find File
- Grep Text
- Projects
- Restore Session
- Config
- Quit

### Pomodoro Sessions

```lua
default1h = {
  { name = "Work", duration = "20m" },
  { name = "Short Break", duration = "5m" },
  { name = "Work", duration = "20m" },
}

default2h = {
  { name = "Work", duration = "20m" },
  { name = "Short Break", duration = "5m" },
  { name = "Work", duration = "20m" },
  { name = "Long Break", duration = "15m" },
  { name = "Work", duration = "20m" },
  { name = "Short Break", duration = "5m" },
  { name = "Work", duration = "20m" },
}

rest = {
  { name = "Long Break", duration = "30m" },
}
```

---

## 6. Git Configuration

### File
`~/.gitconfig` (83 lines)

### Core Settings

```ini
[user]
    name = "Olga"
    email = "user@example.com"

[init]
    defaultBranch = main

[core]
    editor = nvim
    autocrlf = input
    excludesfile = ~/.gitignore_global
    pager = delta

[pull]
    rebase = true

[push]
    autoSetupRemote = true
    default = current

[fetch]
    prune = true

[diff]
    colorMoved = zebra

[merge]
    conflictstyle = diff3

[rebase]
    autoStash = true
```

### Aliases

```bash
# Shortcuts
co = checkout
br = branch
ci = commit
st = status

# Useful operations
unstage = reset HEAD --
last = log -1 HEAD
amend = commit --amend --no-edit
undo = reset --soft HEAD~1

# Pretty logs
lg = log --oneline --graph --decorate -20
lga = log --oneline --graph --decorate --all -20
ll = log --pretty=format:'%C(yellow)%h%Creset %s %C(cyan)<%an>%Creset %C(green)(%cr)%Creset' -20

# Contributors
contributors = shortlog -sn --no-merges

# Recent branches
recent = branch --sort=-committerdate --format='%(HEAD) %(color:yellow)%(refname:short)%(color:reset) - %(color:green)%(committerdate:relative)%(color:reset) - %(contents:subject)'

# Stash
sl = stash list
sp = stash pop
ss = stash save
```

### Delta (Better Diff)

```ini
[delta]
    navigate = true
    light = false
    side-by-side = true
    line-numbers = true

[interactive]
    diffFilter = delta --color-only
```

### URL Shortcuts

```ini
[url "git@github.com:"]
    insteadOf = gh:
    insteadOf = https://github.com/

[url "git@gitlab.com:"]
    insteadOf = gl:
    insteadOf = https://gitlab.com/
```

---

## 7. WTF Dashboard

### File
`~/.config/wtf/config.yml` (205 lines)

### Layout
Grid: 4 columns x 5 rows

### Modules

| Row | Position | Module | Description |
|-----|----------|--------|-------------|
| 0 | 0 | resourceusage | CPU, Memory, Swap |
| 0 | 1 | cmdrunner | Disk usage (custom script) |
| 0 | 2 | power | Battery status |
| 0 | 3 | clocks | Local, Iceland, Krasnoyarsk |
| 1 | 0-1 | docker | Container status |
| 1 | 2-3 | cmdrunner | tmux sessions |
| 2 | 0-1 | cmdrunner | Proton Pass status |
| 3 | 0-1 | cmdrunner | Recent commands history |
| 4 | 0-1 | todo | Todo list |

### World Clocks
- Local
- Iceland
- Asia/Krasnoyarsk

### Keybindings
- `q` or `Enter`: Exit dashboard
- `?`: Show keybindings (in todo widget)

### Refresh Intervals

| Module | Interval |
|--------|----------|
| CPU/Memory | 2s |
| Disk | 60s |
| Power | 30s |
| Clocks | 15s |
| Docker | 5s |
| tmux | 5s |
| Proton Pass | 300s (5m) |
| History | 30s |
| Todo | 10s |

---

## 8. OpenCode AI Assistant

### File
`~/.config/opencode/opencode.json` (182 lines)

### Plugins
- oh-my-opencode
- opencode-openai-codex-auth

### MCP Servers

| Server | Type | Description |
|--------|------|-------------|
| mem0 | Remote | Memory server at localhost:8765 |
| sequential-thinking | Local | @modelcontextprotocol/server-sequential-thinking |
| metabase | Local | Metabase MCP (Docker) |

### Provider Configuration

```json
{
  "provider": {
    "openai": {
      "name": "OpenAI",
      "options": {
        "reasoningEffort": "medium",
        "reasoningSummary": "auto",
        "textVerbosity": "medium"
      }
    }
  }
}
```

### Models

| Model | Context | Output | Variants |
|-------|---------|--------|----------|
| gpt-5.2 | 272K | 128K | none, low, medium, high, xhigh |
| gpt-5.2-codex | 272K | 128K | low, medium, high, xhigh |
| gpt-5.1-codex-max | 272K | 128K | low, medium, high, xhigh |

### Reasoning Effort Levels

| Level | Summary |
|-------|---------|
| none | No reasoning |
| low | auto summary |
| medium | auto summary |
| high | detailed summary |
| xhigh | detailed summary |

### Metabase Configuration
- URL: `https://mb.chhlga.me`
- API Key: [REDACTED]

---

## 9. Other Tools

### htop

**File**: `~/.config/htop/htoprc`

**Settings**:
- Sort by: CPU%
- Hide kernel threads: Yes
- Header layout: Two columns (50/50)
- Left column: CPUs, Memory, Swap
- Right column: CPUs, Tasks, Load Average, Uptime
- Mouse support: Enabled
- Update delay: 15 (1.5s)

**Display columns**: PID, USER, PRIORITY, NICE, M_VIRT, M_RESIDENT, STATE, PERCENT_CPU, PERCENT_MEM, TIME, Command

### neofetch

**File**: `~/.config/neofetch/config.conf` (865 lines)

**Displayed Info**:
- OS (with architecture)
- Host
- Kernel
- Uptime
- Packages (with manager names)
- Shell (with version)
- Resolution
- DE
- WM
- WM Theme
- Theme
- Icons
- Terminal
- Terminal Font
- CPU
- GPU
- Memory

**Settings**:
- Image backend: ASCII
- Image source: auto
- Kernel shorthand: on
- Uptime shorthand: on
- CPU brand: on
- GPU brand: on
- Bold text: on
- Color scheme: distro-based

---

## Quick Reference

### Most Used Commands

```bash
# Terminal
tpick          # Switch tmux sessions with fzf
tcd            # New session from current directory
dashboard      # Show WTF dashboard

# Password Manager
ppc            # Copy password with fzf
ppcp <item>    # Copy specific password
Ctrl+Alt+P     # Insert password at cursor

# Navigation
z <dir>        # zoxide jump
ll             # List files with git status
lt             # Tree view (2 levels)

# Git
g lg           # Pretty log
g recent       # Branches by last commit
g amend        # Quick amend
g undo         # Undo last commit (soft)
```

### Neovim Quick Actions

```
Space e e      # Toggle file explorer
Space f f      # Find file
Space f g      # Grep text
Space p .      # Resume last picker
Space a t      # Toggle AI panel
Space g g      # Git status
Space /        # Toggle comment
```

### tmux Quick Actions

```
`|             # Split horizontal
`-             # Split vertical
`f             # fzf picker
`r             # Reload config
`hjkl          # Navigate panes
```
