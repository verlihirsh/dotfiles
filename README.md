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

3. Verify installation:

   ```bash
   dotfiles-doctor
   ```

## What's Included

### Tools (Optional, selected during `chezmoi init`)

| Tool | Description |
|------|-------------|
| **zsh + oh-my-zsh** | Shell with plugins (autosuggestions, syntax highlighting, completions) |
| **Starship** | Modern cross-shell prompt with git status, language versions, etc. |
| **Nerd Fonts** | Patched fonts with icons (JetBrainsMono, FiraCode, Hack) |
| **pyenv** | Python version manager |
| **fnm** | Fast Node.js version manager |
| **Neovim** | Editor with jelvim config |
| **CLI tools** | fzf, ripgrep, fd, bat, eza, jq, htop, uv |
| **tmux** | Terminal multiplexer with sensible config |
| **direnv** | Per-directory environment variables |
| **LSP servers** | Language servers for Python, TypeScript, Go, Lua, Bash, YAML, Docker, Markdown |
| **OpenCode** | AI-powered coding assistant with oh-my-opencode |

### Configurations

- `~/.bashrc` - Common shell config (sourced by both bash and zsh)
- `~/.zshrc` - Zsh-specific config with oh-my-zsh
- `~/.gitconfig` - Git config with useful aliases and SSH-first GitHub access
- `~/.gitignore_global` - Global gitignore
- `~/.tmux.conf` - tmux config (if enabled)
- `~/.config/starship.toml` - Starship prompt config (if enabled)

### Secrets Support

Copy your secrets (SSH keys, API tokens, etc.) by providing a path during setup:

```
secrets-template/
├── .ssh/
├── .aws/
├── .config/gh/
├── .gnupg/
├── .netrc
└── .env
```

## Supported Platforms

- **macOS** (Intel & Apple Silicon) - includes sane system defaults
- **Linux** (Ubuntu, Debian, Fedora, Arch)
- **WSL** (Windows Subsystem for Linux)

## Usage

### Verify installation

```bash
dotfiles-doctor
```

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

Create `~/.zshrc.local` or `~/.bashrc.local` for machine-specific configuration:

```bash
# ~/.zshrc.local
export CUSTOM_VAR="value"
alias myalias='mycommand'
```

### Change tool versions

Edit `~/.config/chezmoi/chezmoi.toml`:

```toml
[data]
    pythonVersion = "3.12"
    nodeVersion = "22"
```

Then run:

```bash
chezmoi apply
```

## Directory Structure

```
.
├── home/
│   ├── .chezmoi.toml.tmpl            # Interactive prompts
│   ├── .chezmoiexternal.toml.tmpl    # External deps (oh-my-zsh, plugins, nvim config)
│   ├── .chezmoiscripts/
│   │   ├── run_onchange_00-copy-secrets.sh.tmpl
│   │   ├── run_onchange_01-install-system-deps.sh.tmpl
│   │   ├── run_once_02-configure-zsh.sh.tmpl
│   │   ├── run_onchange_03-install-pyenv.sh.tmpl
│   │   ├── run_onchange_04-install-fnm.sh.tmpl
│   │   ├── run_onchange_05-configure-neovim.sh.tmpl
│   │   ├── run_onchange_06-install-cli-tools.sh.tmpl
│   │   ├── run_onchange_07-install-lsp-servers.sh.tmpl
│   │   ├── run_onchange_08-install-opencode.sh.tmpl
│   │   ├── run_onchange_09-install-nerdfonts.sh.tmpl
│   │   ├── run_onchange_10-install-starship.sh.tmpl
│   │   └── run_once_11-macos-defaults.sh.tmpl
│   ├── dot_bashrc.tmpl
│   ├── dot_zshrc.tmpl
│   ├── dot_gitconfig.tmpl
│   ├── dot_tmux.conf.tmpl
│   ├── dot_config/
│   │   └── starship.toml.tmpl
│   └── dot_local/
│       └── bin/
│           └── executable_dotfiles-doctor
├── secrets-template/
└── README.md
```

## License

MIT
