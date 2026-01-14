# Secrets Template

This folder is a template for the secrets directory used by chezmoi dotfiles.

## Usage

1. Copy this folder to a secure location (e.g., encrypted drive, cloud storage with encryption)
2. Replace placeholder files with your actual secrets
3. During `chezmoi init`, provide the path when prompted for "Path to secrets folder"

## Structure

```
secrets/
├── .ssh/
│   ├── id_ed25519          # Private SSH key
│   ├── id_ed25519.pub      # Public SSH key
│   └── config              # SSH config
├── .config/
│   └── gh/
│       └── hosts.yml       # GitHub CLI auth
├── .aws/
│   ├── config              # AWS config
│   └── credentials         # AWS credentials
├── .gnupg/
│   └── (GPG keyring files)
├── .netrc                  # API tokens for various services
└── .env                    # Environment variables
```

## Files

### SSH (`~/.ssh/`)

Your SSH keys and config. The script preserves permissions:
- Private keys: `600`
- Public keys: `644`
- Directories: `700`

### GitHub CLI (`~/.config/gh/`)

GitHub CLI authentication. Get this after running `gh auth login`.

### AWS (`~/.aws/`)

AWS CLI credentials and config.

### GPG (`~/.gnupg/`)

GPG keyring. Export with:
```bash
gpg --export-secret-keys --armor > secrets/.gnupg/private-keys.asc
```

### .netrc

API tokens for services like:
```
machine api.github.com
  login YOUR_USERNAME
  password YOUR_TOKEN

machine api.openai.com
  login apikey
  password sk-...
```

### .env

Environment variables (API keys, etc.):
```bash
ANTHROPIC_API_KEY=sk-ant-...
OPENAI_API_KEY=sk-...
```

## Security Notes

- **Never commit this folder to git** (it's in .gitignore)
- Store on encrypted storage
- Use proper file permissions (600 for sensitive files)
- Consider using a password manager or encrypted volume
