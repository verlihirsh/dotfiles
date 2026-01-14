# dotfiles

Cross-platform development environment managed with [chezmoi](https://www.chezmoi.io/).

## Quick Start

### One-liner Install

```bash
sh -c "$(curl -fsLS get.chezmoi.io)" -- init --apply YOUR_GITHUB_USERNAME/dotfiles
```

### Manual Install

1. Install chezmoi:

   ```bash
   # macOS
   brew install chezmoi

   # Linux (any distro)
   sh -c "$(curl -fsLS get.chezmoi.io)"

   # Or with package manager
   # Arch: pacman -S chezmoi
   # Debian/Ubuntu: apt install chezmoi (if available)
   ```

2. Initialize and apply:

   ```bash
   chezmoi init YOUR_GITHUB_USERNAME/dotfiles
   chezmoi apply
   ```

## What's Included

### Tools (Optional, selected during `chezmoi init`)

| Tool | Description |
|------|-------------|
| **zsh + oh-my-zsh** | Shell with plugins (autosuggestions, syntax highlighting, completions) |
| **pyenv** | Python version manager |
| **fnm** | Fast Node.js version manager |
| **Neovim** | Editor with jelvim config |
| **CLI tools** | fzf, ripgrep, fd, bat, eza, jq, htop |
| **tmux** | Terminal multiplexer with sensible config |
| **direnv** | Per-directory environment variables |

### Configurations

- `~/.zshrc` - Shell config with tool integrations
- `~/.gitconfig` - Git config with useful aliases
- `~/.gitignore_global` - Global gitignore
- `~/.tmux.conf` - tmux config (if enabled)

## Supported Platforms

- **macOS** (Intel & Apple Silicon)
- **Linux** (Ubuntu, Debian, Fedora, Arch)
- **WSL** (Windows Subsystem for Linux)

## Usage

### Update dotfiles

```bash
chezmoi update
```

### Edit a managed file

```bash
chezmoi edit ~/.zshrc
chezmoi apply
```

### See what would change

```bash
chezmoi diff
```

### Re-run prompts

```bash
chezmoi init
```

### Force re-run of scripts

```bash
# Re-run all run_once_ scripts
chezmoi state delete-bucket --bucket=scriptState

# Then apply
chezmoi apply
```

## Customization

### Local overrides

Create `~/.zshrc.local` for machine-specific configuration that won't be tracked:

```bash
# ~/.zshrc.local
export CUSTOM_VAR="value"
alias myalias='mycommand'
```

### Change tool versions

Edit `~/.config/chezmoi/chezmoi.toml`:

```toml
[data]
    pythonVersion = "3.11"
    nodeVersion = "20"
```

Then run:

```bash
chezmoi apply
```

## Directory Structure

```
.
├── .chezmoi.toml.tmpl          # Interactive prompts
├── .chezmoiexternal.toml.tmpl  # External dependencies (oh-my-zsh, plugins)
├── .chezmoiignore              # Files to ignore
├── .chezmoiroot                # Points to home/ subdirectory
├── home/
│   ├── .chezmoiscripts/        # Installation scripts
│   │   ├── run_onchange_01-install-system-deps.sh.tmpl
│   │   ├── run_once_02-configure-zsh.sh.tmpl
│   │   ├── run_onchange_03-install-pyenv.sh.tmpl
│   │   ├── run_onchange_04-install-fnm.sh.tmpl
│   │   ├── run_onchange_05-configure-neovim.sh.tmpl
│   │   └── run_onchange_06-install-cli-tools.sh.tmpl
│   ├── dot_zshrc.tmpl          # Shell config
│   ├── dot_gitconfig.tmpl      # Git config
│   ├── dot_gitignore_global    # Global gitignore
│   └── dot_tmux.conf.tmpl      # tmux config
└── README.md
```

## License

MIT
