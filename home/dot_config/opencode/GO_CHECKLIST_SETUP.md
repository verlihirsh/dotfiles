# Go Quality Checklist - Setup Summary

## ✅ Installation Complete

The Go quality guidelines have been successfully installed as **global OpenCode instructions** that automatically load only for Go projects.

---

## 📁 Files Created

### 1. Global Instructions File
**Location**: `~/.config/opencode/INSTRUCTIONS.md`
- **Purpose**: Tells OpenCode to conditionally load language-specific guidelines
- **Size**: 1.7 KB
- **Trigger**: Automatically loaded for all OpenCode sessions

### 2. Go Quality Checklist
**Location**: `~/.config/opencode/GO_QUALITY_CHECKLIST.md`
- **Purpose**: Comprehensive Go quality checklist with 15 Hard Stops + 83 checks
- **Size**: 12 KB (314 lines)
- **Source**: https://github.com/verlihirsh/llm-docs/blob/main/golang_guidelines.toon
- **Trigger**: Only loaded when working in Go projects (detected via `go.mod` or `.go` files)

### 3. Project AGENTS.md
**Location**: `/Users/chhlga/work/banner-kit-go/AGENTS.md`
- **Status**: Restored to original (310 lines, project-specific guidelines only)
- **Purpose**: Project-specific build/test/style guidelines for banner-kit-go

---

## 🎯 How It Works

### Automatic Loading
1. OpenCode loads `INSTRUCTIONS.md` globally for every session
2. When you work in a Go project (with `go.mod`), agents automatically load `GO_QUALITY_CHECKLIST.md`
3. For non-Go projects, the checklist is NOT loaded (saves ~12 KB context tokens)

### Detection Triggers
Go checklist is loaded when:
- ✅ Directory contains `go.mod` file
- ✅ Modifying or creating `.go` files
- ✅ Running Go-related commands

### What Agents Must Do
Before marking any Go task complete:
1. ✅ Review ALL 15 Hard Stops (HS01-HS15) - **MANDATORY**
2. ✅ Run: `gofmt -w ./...`, `go test ./...`, `go vet ./...`
3. ✅ Successfully build the project
4. ✅ Provide structured audit report (template provided in checklist)

---

## 📋 Checklist Overview

### 15 Hard Stops (BLOCKING)
- HS01: No ignored errors
- HS02: No panic for normal flow
- HS03: No unbounded goroutine spawning
- HS04: Every goroutine has stop condition
- HS05: No mutable package globals
- HS06: Exported API must not return unexported types
- HS07: Context must be first param
- HS08: Do not leak internal errors
- HS09: No mixed concerns in structs
- HS10: New dependency must be justified
- HS11: No double logging
- HS12: No select with single case
- HS13: No insecure string comparisons for auth
- HS14: No resource leaks
- HS15: No tests with uncontrolled time/randomness

### 10 Quality Categories (83 checks)
- **S0**: Scope & Intent (6 checks)
- **S1**: Project & Packages (8 checks)
- **S2**: Dependencies & Modules (7 checks)
- **S3**: Format, Style & Readability (9 checks)
- **S4**: Errors (9 checks)
- **S5**: Concurrency & Context (12 checks)
- **S6**: HTTP, API & Services (10 checks)
- **S7**: Data & Persistence (9 checks)
- **S8**: Observability (7 checks)
- **S9**: Testing (12 checks)
- **S10**: Security & Performance (10 checks)

---

## 🔍 Verification

### Check Installation
```bash
# Verify files exist
ls -lh ~/.config/opencode/INSTRUCTIONS.md
ls -lh ~/.config/opencode/GO_QUALITY_CHECKLIST.md

# View global instructions
cat ~/.config/opencode/INSTRUCTIONS.md

# View Go checklist
cat ~/.config/opencode/GO_QUALITY_CHECKLIST.md
```

### Test in Go Project
```bash
# Navigate to any Go project
cd /path/to/go-project

# Start OpenCode session
# Agents should automatically load the Go checklist
```

---

## 🚀 Benefits

### ✅ Token Efficiency
- Only loads when needed (Go projects only)
- Saves ~12 KB tokens for non-Go work
- Scales easily to other languages

### ✅ Consistency
- All agents follow the same battle-tested practices
- Prevents common production issues
- Ensures quality across all Go projects

### ✅ Maintainability
- Single source of truth: `~/.config/opencode/GO_QUALITY_CHECKLIST.md`
- Update once, applies to all Go projects globally
- No need to copy-paste guidelines per project

### ✅ Accountability
- Structured audit reports required
- Clear pass/fail criteria
- Traceable compliance with check IDs

---

## 📝 Future Enhancements

You can extend this pattern for other languages:

```bash
# Add Python checklist
~/.config/opencode/PYTHON_QUALITY_CHECKLIST.md

# Add TypeScript checklist
~/.config/opencode/TYPESCRIPT_QUALITY_CHECKLIST.md

# Add Rust checklist
~/.config/opencode/RUST_QUALITY_CHECKLIST.md
```

Then update `INSTRUCTIONS.md` with trigger conditions for each language.

---

## 🔗 Resources

- **Original Source**: https://github.com/verlihirsh/llm-docs/blob/main/golang_guidelines.toon
- **Go Style Guide**: https://google.github.io/styleguide/go/
- **Effective Go**: https://go.dev/doc/effective_go
- **Go Code Review Comments**: https://go.dev/wiki/CodeReviewComments

---

## ❓ Troubleshooting

### Checklist Not Loading?
1. Verify `go.mod` exists in project root
2. Check file exists: `ls -l ~/.config/opencode/GO_QUALITY_CHECKLIST.md`
3. Check global instructions: `cat ~/.config/opencode/INSTRUCTIONS.md`

### Need to Update Guidelines?
Simply edit the file:
```bash
vim ~/.config/opencode/GO_QUALITY_CHECKLIST.md
```

Changes apply immediately to all new OpenCode sessions.

---

**Setup Status**: ✅ Complete

Your OpenCode installation is now configured to automatically enforce Go quality guidelines for all Go projects, while conserving tokens for non-Go work.
