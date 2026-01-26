# OpenCode Configuration - Master Index

## 📋 Overview

Your OpenCode installation is now configured with comprehensive development workflows and quality guidelines that apply automatically based on project type.

---

## 📁 Configuration Files

### 1. Core Instructions (ALWAYS LOADED)
**File**: `~/.config/opencode/INSTRUCTIONS.md`  
**Size**: 11 KB (393 lines)  
**Scope**: ALL projects, ALL agents, ALL sessions

**Contains**:
- ✅ TDD approach (tests first, black-box E2E)
- ✅ Manual testing strategy (headless mode, debug mode)
- ✅ Bug fixing protocol (research → test → fix → document)
- ✅ Task completion checklist (tests, lint, clean repo)
- ✅ Git workflow (conventional commits)
- ✅ Documentation standards (README template, banner generation)
- ✅ Language-specific loading rules (Go, Python, JS, Rust)

**When Loaded**: Every OpenCode session

---

### 2. Go Quality Checklist (CONDITIONAL)
**File**: `~/.config/opencode/GO_QUALITY_CHECKLIST.md`  
**Size**: 12 KB (314 lines)  
**Scope**: ONLY Go projects (detected via `go.mod` or `.go` files)

**Contains**:
- ✅ 15 Hard Stops (blocking issues)
- ✅ 83 quality checks across 10 categories:
  - S0: Scope & Intent (6 checks)
  - S1: Project & Packages (8 checks)
  - S2: Dependencies & Modules (7 checks)
  - S3: Format, Style & Readability (9 checks)
  - S4: Errors (9 checks)
  - S5: Concurrency & Context (12 checks)
  - S6: HTTP, API & Services (10 checks)
  - S7: Data & Persistence (9 checks)
  - S8: Observability (7 checks)
  - S9: Testing (12 checks)
  - S10: Security & Performance (10 checks)
- ✅ Mandatory commands: `gofmt`, `go test`, `go vet`, `go build`
- ✅ Agent report template

**When Loaded**: Only when working in Go projects (token optimization)

**Source**: https://github.com/verlihirsh/llm-docs/blob/main/golang_guidelines.toon

---

### 3. Setup Documentation

#### Go Checklist Setup Guide
**File**: `~/.config/opencode/GO_CHECKLIST_SETUP.md`  
**Size**: 5.3 KB  
**Purpose**: Installation guide, troubleshooting, verification steps for Go checklist

#### Workflow Setup Guide
**File**: `~/.config/opencode/WORKFLOW_SETUP.md`  
**Size**: 11 KB  
**Purpose**: Complete guide to TDD, manual testing, bug fixing, git workflow, documentation

#### This File
**File**: `~/.config/opencode/CONFIG_INDEX.md`  
**Purpose**: Master index of all configuration files

---

## 🎯 How It Works

### Automatic Loading Strategy

```
Agent starts in project
    ↓
Loads: INSTRUCTIONS.md (ALWAYS)
    ↓
Detects project type
    ↓
Go project (go.mod exists)?
    ├─ YES → Load GO_QUALITY_CHECKLIST.md (12 KB)
    └─ NO  → Skip (saves tokens)
    ↓
Apply all loaded guidelines
```

### Token Optimization

| Project Type | Files Loaded | Total Size |
|--------------|--------------|------------|
| Go project | INSTRUCTIONS.md + GO_QUALITY_CHECKLIST.md | ~23 KB |
| Non-Go project | INSTRUCTIONS.md only | ~11 KB |

**Benefit**: Save ~12 KB tokens when not working with Go

---

## 📚 Complete Workflow Reference

### 1. TDD Approach (MANDATORY)
```
Think → Design Tests → Write Failing Test → Implement → Refactor → Repeat
```

### 2. Manual Testing (MANDATORY)
- Implement headless mode OR debug mode
- Document approach in AGENTS.md
- Demonstrate before task completion

### 3. Bug Fixing (MANDATORY)
```
Research → Write Failing Test → Fix → Verify → (If stuck: Document & Move On)
```

### 4. Task Completion (MANDATORY)
- [ ] All tests pass
- [ ] Linter clean
- [ ] Repository clean
- [ ] Manual test performed
- [ ] Commit with conventional message

### 5. Git Workflow (MANDATORY)
```bash
git status → git add → git commit -m "type(scope): message" → git log -1
```

### 6. Documentation (MANDATORY)
- Use `~/docs/README.template.md`
- Generate banner: `banner-gen .`
- Place other .md files in `./docs/`

### 7. Go Projects (CONDITIONAL)
- Review 15 Hard Stops
- Run: `gofmt`, `go test`, `go vet`
- Provide audit report

---

## 🔍 Quick Verification

### Check All Files Exist
```bash
ls -lh ~/.config/opencode/{INSTRUCTIONS,GO_QUALITY_CHECKLIST,GO_CHECKLIST_SETUP,WORKFLOW_SETUP,CONFIG_INDEX}.md
```

### View Any File
```bash
# Core instructions (always loaded)
cat ~/.config/opencode/INSTRUCTIONS.md

# Go checklist (conditional)
cat ~/.config/opencode/GO_QUALITY_CHECKLIST.md

# Setup guides
cat ~/.config/opencode/WORKFLOW_SETUP.md
cat ~/.config/opencode/GO_CHECKLIST_SETUP.md
```

### Verify README Template
```bash
ls -la ~/docs/README.template.md
```

---

## 📖 Documentation Structure

```
~/.config/opencode/
├── INSTRUCTIONS.md                    # Core workflow (11 KB, always loaded)
├── GO_QUALITY_CHECKLIST.md            # Go guidelines (12 KB, conditional)
├── GO_CHECKLIST_SETUP.md              # Go setup guide (5.3 KB, reference)
├── WORKFLOW_SETUP.md                  # Workflow guide (11 KB, reference)
└── CONFIG_INDEX.md                    # This file (master index)

~/docs/
└── README.template.md                 # README template for all projects

<project-root>/
├── README.md                          # Project documentation (from template)
├── AGENTS.md                          # Project-specific agent instructions
├── banner.svg                         # Generated banner (if applicable)
├── banner.png                         # Generated banner (if applicable)
└── docs/                              # Local documentation (gitignored)
    ├── architecture.md
    ├── api-design.md
    └── bugs/
        ├── bug-123.md                 # Bug investigation notes
        └── bug-456.md
```

---

## 🚀 Common Commands

### TDD Workflow
```bash
# Write test → Run (should fail)
go test -run TestNewFeature ./...

# Implement → Run (should pass)
go test -run TestNewFeature ./...

# Full suite
go test ./...
go test -race ./...
```

### Code Quality
```bash
gofmt -w ./...                  # Format
go vet ./...                    # Static analysis
golangci-lint run ./...         # Full lint (if available)
```

### Git Workflow
```bash
git status                      # Check state
git add .                       # Stage changes
git commit -m "feat: message"   # Commit
git log -1 --stat               # Verify
```

### Documentation
```bash
cp ~/docs/README.template.md ./README.md    # Copy template
banner-gen .                                 # Generate banner
git add README.md banner.{svg,png}          # Stage
git commit -m "docs: add README"            # Commit
```

### Bug Investigation
```bash
mkdir -p docs/bugs
vim docs/bugs/bug-123.md        # Document investigation
```

---

## 🎓 Learning Path

### For New Agents
1. Read: `INSTRUCTIONS.md` (core workflow)
2. Read: `WORKFLOW_SETUP.md` (detailed guide)
3. If Go project: Read `GO_QUALITY_CHECKLIST.md`
4. Refer to: `GO_CHECKLIST_SETUP.md` (troubleshooting)

### For Humans (Reviewing Agent Work)
1. Check agent followed TDD (tests exist?)
2. Check commit messages (conventional format?)
3. Check repository cleanliness (no temp files?)
4. If Go: Check audit report provided
5. Verify manual testing documented

---

## 🔧 Customization

### Add New Language Checklist
1. Create: `~/.config/opencode/PYTHON_QUALITY_CHECKLIST.md`
2. Update: `INSTRUCTIONS.md` with loading rules
3. Define trigger: `requirements.txt`, `pyproject.toml`, etc.

### Modify Workflow
Edit: `~/.config/opencode/INSTRUCTIONS.md`

Changes apply immediately to all new OpenCode sessions.

### Update Go Checklist
Edit: `~/.config/opencode/GO_QUALITY_CHECKLIST.md`

Changes apply immediately to Go projects.

---

## 🆘 Troubleshooting

### Agent Not Following Workflow?
1. Check file exists: `ls -l ~/.config/opencode/INSTRUCTIONS.md`
2. Check file size: Should be ~11 KB
3. Restart OpenCode session
4. View file contents: `cat ~/.config/opencode/INSTRUCTIONS.md`

### Go Checklist Not Loading?
1. Verify project has `go.mod`: `ls -l go.mod`
2. Check file exists: `ls -l ~/.config/opencode/GO_QUALITY_CHECKLIST.md`
3. Check file size: Should be ~12 KB
4. Restart OpenCode session in Go project

### Banner Generation Fails?
1. Check `banner-gen` installed: `which banner-gen`
2. Check README has metadata: `grep "banner-title" README.md`
3. Check template exists: `ls -la ~/docs/README.template.md`

### README Template Missing?
1. Check location: `ls -la ~/docs/README.template.md`
2. If not found, ask user for correct path
3. Update `INSTRUCTIONS.md` with correct path

---

## 📊 Statistics

### Total Configuration Size
- Core workflow: 11 KB (always loaded)
- Go checklist: 12 KB (conditionally loaded)
- Documentation: ~21 KB (reference only, not loaded)
- **Total active context**: 11-23 KB depending on project type

### Coverage
- ✅ TDD workflow defined
- ✅ Manual testing strategy defined
- ✅ Bug fixing protocol defined
- ✅ Git workflow defined
- ✅ Documentation standards defined
- ✅ Go quality guidelines (15 Hard Stops + 83 checks)
- ✅ Token optimization (conditional loading)
- ✅ Extensibility (easy to add new languages)

---

## ✅ Setup Status: Complete

Your OpenCode configuration is now fully operational with:

### Core Features
- ✅ TDD approach (tests first, black-box E2E)
- ✅ Manual testing strategy (headless/debug mode)
- ✅ Bug fixing protocol (research → test → fix → document)
- ✅ Task completion checklist (comprehensive)
- ✅ Git workflow (conventional commits)
- ✅ Documentation standards (README template, banner generation)

### Language-Specific
- ✅ Go quality checklist (15 Hard Stops + 83 checks)
- ✅ Conditional loading (token optimization)
- ✅ Extensible (easy to add Python, JS, Rust, etc.)

### Documentation
- ✅ Setup guides for all components
- ✅ Troubleshooting documentation
- ✅ Quick reference commands
- ✅ This master index

**All configuration files are in place and will be automatically loaded by OpenCode agents.** 🎉

---

## 📞 Support

If you need to modify any configuration:
1. Edit the relevant file in `~/.config/opencode/`
2. Changes take effect immediately (no restart required for new sessions)
3. Refer to this index to understand file relationships
4. Check setup guides for troubleshooting

**Last Updated**: 2026-01-19
