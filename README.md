<!-- banner-title: 🏠 dotfiles -->
<!-- banner-tagline: chezmoi dotfiles-->
<img src="banner.svg" >

<p align="center"><h1>🏠 dotfiles - Cross-Platform Development Environment</h1></p>

<p align="center">
  <strong>A fully automated, cross-platform development environment setup</strong><br>
  Batteries-included dotfiles with optional tools, LSP servers, and intelligent secrets management
</p>

<p align="center">
  <a href="#installation"><img src="https://img.shields.io/badge/chezmoi-managed-blue?logo=chezmoi" alt="Chezmoi Managed"></a>
  <a href="#features"><img src="https://img.shields.io/badge/platform-macOS%20%7C%20Linux%20%7C%20WSL-green" alt="Platform Support"></a>
  <a href="#features"><img src="https://img.shields.io/badge/shell-zsh%20%7C%20bash-orange" alt="Shell Support"></a>
</p>

<p align="center">
  <a href="#quick-start">Quick Start</a> •
  <a href="#features">Features</a> •
  <a href="#configuration">Configuration</a> •
  <a href="#usage">Usage</a> •
  <a href="#customization">Customization</a>
</p>

---

## Why dotfiles?

A **comprehensive development environment** that works consistently across macOS, Linux, and WSL.

**dotfiles** gives you:
- **🚀 One-liner install**: Complete setup with a single command
- **⚡ Interactive prompts**: Choose only the tools you need
- **🎨 Modern tooling**: Starship prompt, Nerd Fonts, LSP servers, OpenCode
- **📊 Secrets management**: Secure SSH keys and API tokens handling
- **🔧 Cross-platform**: Same environment on macOS, Linux, and WSL

---

## Table of Contents

- [Installation](#installation)
- [Quick Start](#quick-start)
- [Features](#features)
- [Configuration](#configuration)
- [Usage](#usage)
- [Customization](#customization)
- [Architecture](#architecture)
- [Contributing](#contributing)
- [License](#license)

---

## Installation

### Option 1: One-liner Install (Recommended)

```bash
sh -c "$(curl -fsLS get.chezmoi.io)" -- init --apply YOUR_GITHUB_USERNAME/dotfiles
```

### Option 2: Manual Install

**Requirements**: Git and curl

```bash
# Step 1: Install chezmoi
# macOS
brew install chezmoi

# Linux (any distro)
sh -c "$(curl -fsLS get.chezmoi.io)"

# Or with package manager
# Arch: pacman -S chezmoi
# Debian/Ubuntu: apt install chezmoi (if available)

# Step 2: Initialize and apply
chezmoi init YOUR_GITHUB_USERNAME/dotfiles
chezmoi apply

# Step 3: Verify installation
dotfiles-doctor
```

### Option 3: Fork and Customize

1. Fork this repository
2. Customize the configuration in `home/` directory
3. Follow Option 1 or 2 with your fork URL

---

## Quick Start

### 1. Setup/Prerequisites

The installer will prompt you to select optional tools. All tools are optional except chezmoi itself.

```bash
# Initialize with prompts
sh -c "$(curl -fsLS get.chezmoi.io)" -- init --apply YOUR_GITHUB_USERNAME/dotfiles
```

### 2. Basic Usage

```bash
# Update dotfiles from repository
chezmoi update

# Edit a managed file
chezmoi edit ~/.zshrc
chezmoi apply

# See what would change
chezmoi diff
```

### 3. Verify Installation

Run the built-in health check:

```bash
dotfiles-doctor
```

**Press `Ctrl+C` to quit if hanging.**

---

## Features

### 🚀 Core Features

- **Interactive installation**: Choose tools during setup with interactive prompts
- **Auto-configured environment**: Zsh and tmux automatically set as default when selected
- **Cross-platform support**: Works on macOS (Intel & Apple Silicon), Linux (Ubuntu, Debian, Fedora, Arch), and WSL
- **Secrets management**: Secure copy of SSH keys, API tokens, and credentials
- **Idempotent scripts**: Re-run safely without breaking existing setup

### 🎨 Optional Tools (Selected during `chezmoi init`)

| Tool | Description |
|------|-------------|
| **zsh + oh-my-zsh** | Shell with plugins (autosuggestions, syntax highlighting, completions). **Auto-set as default shell** |
| **Starship** | Modern cross-shell prompt with git status, language versions, etc. |
| **Nerd Fonts** | Patched fonts with icons (JetBrainsMono, FiraCode, Hack) |
| **uv** | Fast Python package and version manager (replaces pyenv, pip, poetry, pipx) |
| **fnm** | Fast Node.js version manager |
| **Neovim** | Editor with jelvim config |
| **CLI tools** | Multi-select from 13 tools: fzf, ripgrep, fd, bat, eza, jq, htop, tree, zoxide, neofetch, glow, fx, lazysql |
| **tmux** | Terminal multiplexer with sensible config |
| **direnv** | Per-directory environment variables |
| **LSP servers** | Language servers for Python, TypeScript, Go, Lua, Bash, YAML, Docker, Markdown |
| **OpenCode** | AI-powered coding assistant with oh-my-opencode |

### 🤖 Configuration Files Included

- `~/.bashrc` - Common shell config (sourced by both bash and zsh)
- `~/.zshrc` - Zsh-specific config with oh-my-zsh
- `~/.gitconfig` - Git config with useful aliases and SSH-first GitHub access
- `~/.gitignore_global` - Global gitignore
- `~/.tmux.conf` - tmux config (if enabled)
- `~/.config/starship.toml` - Starship prompt config (if enabled)

---

## Configuration

Configuration is managed through **chezmoi's template system** and can be customized at multiple levels:

1. **Interactive prompts** (during `chezmoi init`)
2. **Config file** (`~/.config/chezmoi/chezmoi.toml`)
3. **Local overrides** (`~/.zshrc.local`, `~/.bashrc.local`)
4. **Re-run prompts** (`chezmoi init`)

### Config File Location

Default: `~/.config/chezmoi/chezmoi.toml`

This file is generated during `chezmoi init` based on your answers to interactive prompts.

### Example Configuration

Edit `~/.config/chezmoi/chezmoi.toml` to change tool versions or settings:

```toml
[data]
    # Tool versions
    pythonVersion = "3.11 3.12 3.13"  # Space or comma separated
    nodeVersion = "22"
    
    # Feature flags
    installZsh = true
    installStarship = true
    installNerdFonts = true
    installPython = true
    installNode = true
    installNeovim = true
    installTmux = true
    installDirenv = true
    installLspServers = true
    installOpencode = true
    
    # CLI Tools (multi-select - use chezmoi init to change)
    selectedCliTools = ["fzf - fuzzy finder", "ripgrep - fast grep", "bat - cat with syntax highlighting"]
    
    # Secrets
    secretsSourcePath = "/path/to/secrets-template"
```

### Re-run Interactive Prompts

To change your configuration choices:

```bash
chezmoi init
```

### Supported Platforms

Platform-specific behavior is automatically detected:

- **macOS** (Intel & Apple Silicon) - includes sane system defaults via `run_once_11-macos-defaults.sh`
- **Linux** (Ubuntu, Debian, Fedora, Arch) - uses appropriate package managers
- **WSL** (Windows Subsystem for Linux) - optimized for WSL environment

---

## Usage

### Common Commands

```bash
# Verify installation and check for issues
dotfiles-doctor

# Update dotfiles from repository
chezmoi update

# See what would change without applying
chezmoi diff

# Edit a managed file
chezmoi edit ~/.zshrc

# Apply changes after editing
chezmoi apply

# Re-run interactive prompts to change configuration
chezmoi init
```

### Command Reference

#### Update Dotfiles

Pull latest changes from your dotfiles repository and apply them.

```bash
chezmoi update
```

**What it does:**
- Fetches latest changes from your fork
- Re-runs any `run_onchange_` scripts if templates changed
- Applies all file updates

#### Edit Managed Files

Edit files managed by chezmoi (never edit the target files directly).

```bash
chezmoi edit ~/.zshrc
chezmoi apply
```

**Options:**
- `--apply`: Automatically apply after editing
- `--diff`: Show diff before applying

**Example:**
```bash
# Edit and apply in one command
chezmoi edit --apply ~/.zshrc
```

#### Force Re-run Scripts

If you need to re-run installation scripts:

```bash
# Re-run all run_once_ scripts
chezmoi state delete-bucket --bucket=scriptState

# Re-run all run_onchange_ scripts (delete cache)
chezmoi state delete-bucket --bucket=entryState

# Then apply
chezmoi apply
```

### Examples

#### Example 1: Add Custom Shell Aliases

```bash
# Create local override file (not managed by chezmoi)
echo 'alias myalias="mycommand"' >> ~/.zshrc.local

# Source immediately
source ~/.zshrc
```

#### Example 2: Change Python Version

```bash
# Edit configuration
chezmoi edit ~/.config/chezmoi/chezmoi.toml

# Change: pythonVersion = "3.12" to pythonVersion = "3.11 3.12 3.13"
# (uv supports multiple versions)

# Apply changes (will re-run uv installation with new versions)
chezmoi apply

# Pin a version for your project directory
cd ~/my-project
uv python pin 3.11
```

#### Example 3: Add Secrets

```bash
# Organize your secrets in a directory
mkdir -p ~/secrets-template/.ssh
cp ~/.ssh/id_rsa ~/secrets-template/.ssh/

# During chezmoi init, provide the path when prompted:
# Secrets source path: /Users/yourname/secrets-template

# Apply to copy secrets
chezmoi apply
```

---

## Customization

### Local Overrides

Create machine-specific configuration that won't be committed:

```bash
# ~/.zshrc.local
export CUSTOM_VAR="value"
alias myalias='mycommand'

# Add custom PATH entries
export PATH="$HOME/custom/bin:$PATH"

# Machine-specific oh-my-zsh plugins
plugins+=(docker kubectl)
```

```bash
# ~/.bashrc.local
export CUSTOM_VAR="value"
alias myalias='mycommand'
```

These files are sourced automatically but not managed by chezmoi.

### Change Tool Versions

Edit `~/.config/chezmoi/chezmoi.toml`:

```toml
[data]
    pythonVersion = "3.11 3.12 3.13"  # Change to desired versions
    nodeVersion = "22"                 # Change to desired version
```

Then apply:

```bash
chezmoi apply
```

The `run_onchange_` scripts will detect the change and reinstall with the new version.

### Add Custom Scripts

Add your own installation scripts to `home/.chezmoiscripts/`:

```bash
# Create a new script (use run_once_ or run_onchange_ prefix)
chezmoi edit ~/.local/share/chezmoi/home/.chezmoiscripts/run_once_99-custom-setup.sh.tmpl
```

Script naming conventions:
- `run_once_*.sh.tmpl` - Runs once (tracked in chezmoi state)
- `run_onchange_*.sh.tmpl` - Runs when template content changes
- Numbers (00-99) control execution order

### Secrets Management

Create a secrets directory with this structure:

```
secrets-template/
├── .ssh/                # SSH keys
│   ├── id_rsa
│   └── id_rsa.pub
├── .aws/                # AWS credentials
│   └── credentials
├── .config/gh/          # GitHub CLI token
│   └── hosts.yml
├── .gnupg/              # GPG keys
├── .netrc               # API credentials
└── .env                 # Environment secrets
```

Provide the path during `chezmoi init` when prompted for "secrets source path".

---

## Architecture

### High-Level Design

```
┌─────────────────────────────────────────────────────────┐
│                      dotfiles                           │
│                                                         │
│  ┌──────────────┐          ┌──────────────┐           │
│  │  Templates   │          │   Scripts    │           │
│  │  (.tmpl)     │──────────│ (.chezmoiscripts/) │     │
│  └──────────────┘          └──────────────┘           │
│         │                         │                    │
│         └──────────┬──────────────┘                    │
│                    │                                   │
│         ┌──────────▼──────────┐                        │
│         │   chezmoi engine    │                        │
│         │   (apply/update)    │                        │
│         └──────────┬──────────┘                        │
│                    │                                   │
└────────────────────┼───────────────────────────────────┘
                     │
                     │ File writes
                     │
          ┌──────────▼──────────┐
          │   Target System     │
          │   (~/, ~/.config/)  │
          └─────────────────────┘
```

### Directory Structure

```
dotfiles/
├── home/
│   ├── .chezmoi.toml.tmpl            # Interactive prompts
│   ├── .chezmoiexternal.toml.tmpl    # External deps (oh-my-zsh, plugins, nvim)
│   ├── .chezmoiscripts/              # Installation scripts
│   │   ├── run_onchange_00-copy-secrets.sh.tmpl
│   │   ├── run_onchange_01-install-system-deps.sh.tmpl
│   │   ├── run_once_02-configure-zsh.sh.tmpl
│   │   ├── run_onchange_03-install-uv.sh.tmpl
│   │   ├── run_onchange_04-install-fnm.sh.tmpl
│   │   ├── run_onchange_05-configure-neovim.sh.tmpl
│   │   ├── run_onchange_06-install-cli-tools.sh.tmpl
│   │   ├── run_onchange_07-install-lsp-servers.sh.tmpl
│   │   ├── run_onchange_08-install-opencode.sh.tmpl
│   │   ├── run_onchange_09-install-nerdfonts.sh.tmpl
│   │   ├── run_onchange_10-install-starship.sh.tmpl
│   │   └── run_once_11-macos-defaults.sh.tmpl
│   ├── dot_bashrc.tmpl               # Bash configuration
│   ├── dot_zshrc.tmpl                # Zsh configuration
│   ├── dot_gitconfig.tmpl            # Git configuration
│   ├── dot_tmux.conf.tmpl            # tmux configuration
│   ├── dot_config/
│   │   └── starship.toml.tmpl        # Starship prompt config
│   └── dot_local/
│       └── bin/
│           └── executable_dotfiles-doctor  # Health check script
├── secrets-template/                 # (User-provided) Secrets directory
└── README.md                         # This file
```

### Key Design Decisions

1. **chezmoi templates**: All configuration files use Go templates for platform-specific and conditional logic
2. **Run-once vs run-onchange**: `run_once_` scripts execute once, `run_onchange_` re-run when template content changes (enables version updates)
3. **Optional installation**: Interactive prompts allow cherry-picking tools, minimizing installation time and disk usage
4. **Secrets isolation**: Secrets are never committed; copied from external directory during setup
5. **Local overrides**: `.local` files enable machine-specific customization without modifying managed files

### Script Execution Flow

```
chezmoi init
    ↓
Interactive prompts → Generate .chezmoi.toml
    ↓
chezmoi apply
    ↓
Process templates → Write files to ~/
    ↓
Run scripts (00 → 11 in order)
    ↓
    ├─ Secrets (00)
    ├─ System deps (01)
    ├─ Zsh setup (02)
    ├─ Language managers (03-04)
    ├─ Tools (05-08)
    ├─ Fonts & prompt (09-10)
    └─ macOS defaults (11)
    ↓
Complete
```

---

## Contributing

Contributions are welcome! Please follow these guidelines:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Test with `chezmoi apply --dry-run --verbose`
5. Commit your changes (`git commit -m 'Add amazing feature'`)
6. Push to the branch (`git push origin feature/amazing-feature`)
7. Open a Pull Request

### Code Review Process

- All PRs require review before merging
- Test on at least one platform (macOS, Linux, or WSL)
- Follow shell scripting best practices (shellcheck)
- Update documentation as needed

### Adding New Tools

To add a new optional tool:

1. Add a prompt in `home/.chezmoi.toml.tmpl`
2. Create a new script in `home/.chezmoiscripts/` (use appropriate prefix and number)
3. Use conditional logic: `{{- if .installYourTool }}`
4. Update README.md features table
5. Test installation and verify with `dotfiles-doctor`

---

## License

This project is licensed under the [MIT License](LICENSE) - see the LICENSE file for details.

---

## Acknowledgments

Built using:
- [chezmoi](https://github.com/twpayne/chezmoi) - Dotfiles manager
- [oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh) - Zsh framework
- [Starship](https://github.com/starship/starship) - Cross-shell prompt
- [jelvim](https://github.com/jellydn/jelvim) - Neovim configuration
- [oh-my-opencode](https://github.com/code-yeongyu/oh-my-opencode) - OpenCode integration

Special thanks to the open-source community and all contributors.

---

## Troubleshooting

### Zsh Not Set as Default Shell

If `chezmoi` shows a message about failing to set zsh as default:

**The fallback auto-switch is already configured** - zsh will automatically launch when you open a new terminal, even if `chsh` failed.

If you want to permanently set zsh as your login shell:

```bash
# Manually run the shell change command
chsh -s $(which zsh)

# Then log out and log back in
```

**Why `chsh` might fail:**
- PAM authentication required (some corporate systems)
- User doesn't have permission to change shell
- WSL environment restrictions
- System security policies

### Tmux Auto-Start Not Working

If tmux doesn't automatically start:

**Check your config:**
```bash
# View your chezmoi config
cat ~/.config/chezmoi/chezmoi.toml | grep installTmux
```

**Skip tmux in specific environments:**
- VSCode integrated terminal: Already handled automatically
- SSH with X11 forwarding: Already handled automatically
- Custom scenarios: Set `TERM_PROGRAM=vscode` to skip auto-start

### Force Re-run zsh Configuration

If you need to retry the shell setup:

```bash
# Delete the run_once state for zsh configuration
chezmoi state delete-bucket --bucket=scriptState

# Re-apply
chezmoi apply
```

---

## Support

- **Issues**: [GitHub Issues](https://github.com/YOUR_USERNAME/dotfiles/issues)
- **Discussions**: [GitHub Discussions](https://github.com/YOUR_USERNAME/dotfiles/discussions)
- **Documentation**: See this README and inline comments in templates

---

<p align="center">
  <strong>Build once, run everywhere 🚀</strong>
</p>
