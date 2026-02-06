# Global OpenCode Instructions - Auto-Loaded for All Sessions

---

## 🔥 QUICK START: LSP-FIRST MANDATE (READ THIS FIRST!)

**BEFORE searching for ANY code, symbols, or definitions:**

### Scenario 1: "Find where class/function X is defined"
```bash
lsp_symbols(scope="workspace", query="ClassName", filePath="<any-file-in-project>")
# Returns: List of all matching symbols with file paths and line numbers
```

### Scenario 2: "Find all places function/class Y is used"
```bash
# Step 1: Find the definition first
lsp_symbols(scope="workspace", query="functionName", filePath="<any-file>")
# Step 2: Use file/line from results
lsp_find_references(filePath="<from-step-1>", line=X, character=Y)
# Returns: All usages across entire codebase
```

### Scenario 3: "Understand file structure/outline"
```bash
lsp_symbols(scope="document", filePath="<target-file>")
# Returns: All classes, functions, imports in that file
```

### Scenario 4: "Check for errors before committing"
```bash
lsp_diagnostics(filePath="<changed-file>", severity="error")
# Returns: Type errors, lint errors, warnings
```

**DEFAULT RULE**: If you're looking for code → START WITH LSP, not grep, not read.

---

## ⛔ BLOCKING GATE: LSP Enforcement

**BEFORE using `read`, `grep`, `glob`, or `bash` to search code, YOU MUST:**

### Step 1: Identify Search Intent (check ONE box)
- [ ] Looking for symbol/class/function definition
- [ ] Looking for all usages of symbol/function
- [ ] Looking for file structure/outline
- [ ] Looking for project-wide symbols matching pattern
- [ ] Need config file content (NOT code symbols)
- [ ] Need to read actual implementation (AFTER finding with LSP)

### Step 2: Use LSP FIRST (unless checking config/non-code)
```bash
# Examples of MANDATORY LSP-first usage:
"Find AuthService class" → lsp_symbols(scope="workspace", query="AuthService")
"Find where login() is used" → lsp_symbols + lsp_find_references
"See what's in user.rb" → lsp_symbols(scope="document")
"Find all *Controller classes" → lsp_symbols(scope="workspace", query="Controller")
```

### Step 3: Document Why LSP Insufficient (if skipping to grep/read)
**You MUST provide ONE of these justifications:**
- [ ] LSP returned no results → [show LSP output]
- [ ] LSP error occurred → [paste error message]
- [ ] Need config file (yml/json/env) - LSP doesn't index these
- [ ] Need file listing, not symbol lookup
- [ ] Already have exact file path from previous LSP result

### Step 4: ONLY THEN Use grep/read

**FAILURE TO FOLLOW THIS GATE = TASK REJECTED**

---

## ⛔ BLOCKING ENFORCEMENT: Token Optimization First

**BEFORE YOU DO ANYTHING ELSE IN THIS SESSION:**

```
╔═════════════════════════════════════════════════════════════╗
║  🚨 MANDATORY FIRST ACTIONS (NON-NEGOTIABLE)               ║
╠═════════════════════════════════════════════════════════════╣
║  1. BEFORE ANY FILE READ:                                   ║
║     ✓ Tried LSP tools first? (symbols/goto/refs)           ║
║     ✓ Tried grep/ast-grep?                                  ║
║     ✓ Verified file < 500 lines?                            ║
║     ✓ Absolutely necessary for task?                        ║
║                                                             ║
║  2. AFTER TASK COMPLETION:                                  ║
║     → Report token efficiency (see §Token Budget)           ║
║     → Report LSP usage vs skipped                           ║
║                                                             ║
║  ❌ VIOLATION = TASK FAILURE                                ║
╚═════════════════════════════════════════════════════════════╝
```

**This applies to:**
- Primary agent (you)
- ALL delegated sub-agents
- ALL exploration/research operations
- ALL file read operations

**No exceptions. No excuses.**

---

## 🚨 CRITICAL: Load Custom Instructions First

**BLOCKING STEP**: At the start of EVERY session, BEFORE any other action, you MUST read these files using your Read tool:

1. **~/.config/opencode/INSTRUCTIONS.md** - Core development workflow (TDD, Git, Documentation)
2. **~/.config/opencode/CONFIG_INDEX.md** - Configuration reference and index

**Why**: These files contain mandatory workflows, quality standards, and project-specific guidelines that govern ALL development work.

**Enforcement**: If you start working without loading these files, your work will NOT follow required standards.

---

## 📋 Conditional Loading (Language-Specific)

**After loading core instructions above**, check project type and load additional checklists:

### Go Projects (if `go.mod` exists OR any `.go` files present):
- **~/.config/opencode/GO_QUALITY_CHECKLIST.md** - 15 Hard Stops + 83 quality checks
- **~/.config/opencode/GO_CHECKLIST_SETUP.md** - Setup and troubleshooting guide

### Metabase Projects (if working with Metabase):
- **~/.config/opencode/METABASE_AGENT_README.md** - Metabase agent guidelines
- **~/.config/opencode/METABASE_MCP_SETUP.md** - MCP setup documentation

---

## ✅ Verification Checklist

After loading instructions, verify you understand:

- [ ] TDD workflow (tests first, then implementation)
- [ ] Manual testing requirements (headless/debug mode)
- [ ] Bug fixing protocol (research → test → fix → document)
- [ ] Git workflow (conventional commits)
- [ ] Documentation standards (README template, banner generation)
- [ ] Task completion checklist (tests pass, linter clean, repo clean)
- [ ] LSP-first mandate for code search

---

## 🎯 Core Principles (Always Apply)

### 1. Tests Before Code
- **NEVER** write implementation before tests exist
- Use black-box E2E testing approach
- Tests define the specification

### 2. Documentation Standards
- Use `~/docs/README.template.md` for README files
- Generate banner with `banner-gen .`
- Place all `.md` files (except README.md and AGENTS.md) in `./docs/` directory
- `./docs/` is gitignored globally (local documentation only)

### 3. Git Workflow
- Conventional commit format: `type(scope): description`
- Types: feat, fix, test, refactor, docs, style, perf, chore, build, ci
- Verify with `git log -1 --stat` after commit

### 4. Code Quality
- Run linter before commit (language-specific)
- Run formatter before commit (language-specific)
- No `console.log()`, `print()`, `fmt.Println()` debug statements in commits
- No temporary files or commented-out code

### 5. Task Completion
- [ ] All tests pass
- [ ] Linter clean
- [ ] Repository clean (no temp files)
- [ ] Manual test performed and documented
- [ ] Commit with conventional message

---

## 🛑 Hard Blocks (NEVER Violate)

| Violation | Consequence |
|-----------|-------------|
| Skip loading INSTRUCTIONS.md | Work will not follow required standards |
| Write code before tests | Violates TDD mandate |
| Commit without conventional format | Breaks Git workflow |
| Leave temp files in commit | Repository pollution |
| Skip linter/formatter | Code quality failure |
| Ignore language-specific checklist | Quality standards violated |
| Use grep/read before LSP for code search | Token waste + slower results |

---

## 🎯 Token Optimization Protocol (MANDATORY - NON-NEGOTIABLE)

**⚠️ BLOCKING ENFORCEMENT**: This section is NOT optional. Violating token optimization rules will result in task failure.

**CRITICAL**: Minimize token consumption by using specialized tools and avoiding raw file operations.

### 🚨 PRE-FLIGHT CHECK (REQUIRED BEFORE ANY RESEARCH/READ OPERATION)

**YOU MUST MENTALLY VERIFY THIS CHECKLIST BEFORE EVERY FILE READ OR RESEARCH ACTION:**

```
┌─────────────────────────────────────────────────────────────┐
│  TOKEN OPTIMIZATION PRE-FLIGHT CHECK (MANDATORY)            │
├─────────────────────────────────────────────────────────────┤
│  Before ANY file read or research operation, verify:        │
│                                                              │
│  [ ] 1. Tried LSP tools (symbols/goto/references) first?    │
│  [ ] 2. Used grep/ast-grep to narrow location?              │
│  [ ] 3. Verified file is < 500 lines (wc -l)?               │
│  [ ] 4. This read is absolutely necessary for task scope?   │
│                                                              │
│  ❌ If ANY box unchecked → ABORT READ OPERATION             │
│  ✅ If ALL boxes checked → Proceed with targeted read       │
└─────────────────────────────────────────────────────────────┘
```

**If you cannot check all boxes, STOP and use the appropriate tool instead.**

### Token Efficiency Hierarchy (Use in This Order)

| Priority | Tool Type | When to Use | Token Cost |
|----------|-----------|-------------|------------|
| **1nd** | **LSP Tools** | Codebase structure, symbols, definitions, references | Very low |
| **2rd** | **Context7 MCP** | Official documentation lookup | Low |
| **3th** | **AST-grep** | Pattern-based code search | Low |
| **4th** | **Grep/ripgrep** | Content search before reading | Medium |
| **7th** | **Targeted Read** | Small chunks only (< 500 lines) | High |
| **FORBIDDEN** | **Raw bash file ops** | `cat`, `head`, `tail`, large file reads | EXTREME |

---

### MANDATORY Rules for Agents

#### Rule 1: LSP Tools Cheat Sheet (USE THESE AGGRESSIVELY)
| Task | Tool | Example |
|------|------|---------|
| Find where symbol is defined | `lsp_goto_definition` | Find `UserService` class definition |
| Find all usages of symbol | `lsp_find_references` | Find all places `authenticate()` is called |
| Get file outline/structure | `lsp_symbols(scope="document")` | Get all classes/functions in file |
| Search project-wide symbols | `lsp_symbols(scope="workspace")` | Find all `*Controller` classes |
| Check for errors | `lsp_diagnostics` | Get type errors, warnings |
| Validate rename | `lsp_prepare_rename` | Check if symbol can be renamed |
| Rename across project | `lsp_rename` | Rename `oldName` → `newName` everywhere |

**When exploring codebase structure:**

```typescript
// CORRECT: LSP-first approach
1. lsp_symbols(scope="workspace", query="AuthService")  // Find symbols
2. lsp_goto_definition(...)                              // Navigate to definition
3. lsp_find_references(...)                              // Find all usages
4. lsp_diagnostics(...)                                  // Check for errors
5. Read ONLY if LSP info insufficient

// WRONG: Raw reading approach
1. bash("find . -name '*.ts'")                          // Token waste
2. read entire files to find symbols                     // Massive token waste
```

#### Rule 2: Use Context7 for External Documentation

```typescript
// CORRECT: Context7 for library docs
1. resolve_library_id(libraryName="react")
2. query_docs(libraryId="/facebook/react", query="useState hook examples")

// WRONG: Web search or reading node_modules
❌ bash("cat node_modules/react/README.md")
❌ webfetch("https://react.dev/...")
```

#### Rule 3: Grep Before Reading

**ALWAYS search before reading files:**

```bash
# CORRECT: Targeted search → small read
1. grep(pattern="class.*Service", include="*.ts")
2. read(filePath="...", offset=50, limit=100)  # Only relevant section

# WRONG: Read entire files hoping to find something
❌ read(filePath="large-file.ts")  # 5000 lines = massive tokens
```

#### Rule 4: NEVER Read Large Files

**Pre-flight file size check:**

```bash
# ALWAYS check size first
bash("wc -l path/to/file.ts")  # Get line count
bash("du -h path/to/file.ts")  # Get file size

# Decision tree
< 200 lines   → OK to read fully
200-500 lines → Read with offset/limit
500-1000 lines → Use grep + targeted read
> 1000 lines  → FORBIDDEN to read raw (use LSP/grep/ast-grep only)
```

#### Rule 5: Stay Scoped to Your Task

**DO NOT:**
- ❌ "Let me understand the entire architecture first..."
- ❌ "Let me read all the controllers to see patterns..."
- ❌ "Let me explore the whole auth module..."

**DO:**
- ✅ "Task: Fix login bug → LSP find login function → Read only that function"
- ✅ "Task: Add validation → LSP find existing validators → Copy pattern"
- ✅ "Task: Refactor UserService → LSP find references → Update callers"

**Scope Discipline:**

| Violation | Token Waste | Correct Approach |
|-----------|-------------|------------------|
| Reading 10 files to understand "the codebase" | 50,000+ tokens | LSP symbols + grep for exact task scope |
| Reading entire folder structure | 20,000+ tokens | LSP workspace symbols or ast-grep pattern search |
| Reading config files "just in case" | 5,000+ tokens | Read only when task explicitly requires it |

---

### Anti-Patterns (Token Waste Examples)

| ❌ WRONG (Token Waste) | ✅ CORRECT (Token Efficient) | Savings |
|------------------------|------------------------------|---------|
| `bash("cat src/**/*.ts")` | `lsp_symbols(scope="workspace")` | 95% |
| Read 10 files to find function | `lsp_symbols` + `grep` → read 1 file | 90% |
| Read entire file for imports | `lsp_symbols(scope="document")` → shows imports | 80% |
| Web search for React docs | `context7_query_docs(libraryId="/facebook/react")` | 70% |
| Read node_modules for API | `context7_resolve_library_id()` + `query_docs()` | 99% |

---

### Enforcement Checklist

Before ANY file read operation, agents MUST verify:

- [ ] **Tried LSP tools first?** (symbols, definitions, references)
- [ ] **Checked Context7 for external docs?**
- [ ] **Used grep/ast-grep to narrow down location?**
- [ ] **Verified file size < 500 lines?**
- [ ] **Reading only specific lines needed for task?**
- [ ] **This read is absolutely necessary for current task scope?**

**If ANY checkbox is unchecked → READ OPERATION REJECTED**

---

### Token Budget Awareness

Agents should track and report token usage:

```
Token Budget Report:
- LSP operations: ~500 tokens
- grep searches: ~300 tokens
- Targeted reads (3 files, 200 lines each): ~6,000 tokens
- TOTAL: ~6,900 tokens

Avoided:
- Raw reading 10 full files would have cost: ~50,000 tokens
- Token savings: 86%
```

---

### Quick Decision Matrix

```
Need to:                          → Use:
─────────────────────────────────────────────────────────
Find where something is defined   → lsp_goto_definition
Find all usages of something      → lsp_find_references  
Understand file structure         → lsp_symbols(document)
Find symbols across project       → lsp_symbols(workspace)
Search for code patterns          → ast-grep or grep
Look up library documentation     → context7 (resolve + query)
Read specific code sections       → LSP/grep THEN targeted read
Need to read large file (>500 ln) → DON'T - use LSP/grep instead
```

---

### Reporting & Accountability

At task completion, agents MUST report:

1. **Tools Used**: List of all tools and their purpose
2. **LSP Usage**: Which LSP tools used, or justification if skipped
3. **Token Efficiency**: Estimate of tokens saved by avoiding raw reads
4. **Scope Discipline**: Confirmation that research stayed within task boundaries

Example:
```
✅ Task Complete: Fixed login validation bug

Tools Used:
- lsp_symbols: Located AuthService class ✅ LSP USED
- lsp_find_references: Found all login calls (23 locations) ✅ LSP USED
- grep: Searched for validation logic (only after LSP)
- read: 2 targeted reads (150 lines total)

LSP Adoption: ✅ USED EXTENSIVELY
- lsp_symbols: 2 calls
- lsp_find_references: 1 call
- Skipped: 0 times

Token Efficiency:
- Avoided reading 8 full files (~40,000 tokens)
- Used LSP + grep instead (~1,200 tokens)
- Savings: 97%

Scope Discipline:
✓ Stayed focused on login validation
✓ Did not explore unrelated modules
✓ Did not read "for understanding"
```

---

## 📖 Quick Reference

### File Locations
- Core instructions: `~/.config/opencode/INSTRUCTIONS.md`
- Config index: `~/.config/opencode/CONFIG_INDEX.md`
- README template: `~/docs/README.template.md`
- Global gitignore: `~/.gitignore_global`

### Common Commands
```bash
# README workflow
cp ~/docs/README.template.md ./README.md
banner-gen .
git add README.md banner.{svg,png}
git commit -m "docs: add README with banner"

# Git workflow
git status
git add <files>
git commit -m "type(scope): description"
git log -1 --stat

# Quality checks (example for Go)
gofmt -w ./...
go vet ./...
go test ./...
```

---

## 🔄 Session Start Workflow

**Every new session MUST follow this sequence**:

1 **Read core instructions**:
   - `~/.config/opencode/INSTRUCTIONS.md`
   - `~/.config/opencode/CONFIG_INDEX.md`

2. **Understand user request**:
   - Read user message completely
   - Identify task type (feature, bug, refactor, docs)
   - Check for ambiguity (ask if needed)

3. **Begin work WITH TOKEN DISCIPLINE AND LSP-FIRST**:
   - **BEFORE ANY FILE READ**: Run pre-flight check (see Token Optimization Protocol)
   - **BEFORE ANY CODE SEARCH**: Use LSP tools first (see LSP Enforcement Gate)
   - Follow TDD workflow
   - Apply quality standards
   - Use conventional commits
   - Complete task checklist

---

## 📞 Support & Updates

**If instructions conflict**:
1. `INSTRUCTIONS.md` takes precedence (core workflow)
2. Language-specific checklist applies for that language
3. Project `AGENTS.md` overrides global rules (if exists)

**If instructions unclear**:
- Refer to `~/.config/opencode/CONFIG_INDEX.md`
- Check setup guides in `~/.config/opencode/`
- Ask user for clarification

**Last Updated**: 2026-01-26 (Session: LSP-first enforcement + reorganization for maximum visibility)

**Key Changes in This Version**:
- ✅ LSP Quick Start moved to top (lines 5-45) - impossible to miss
- ✅ LSP Enforcement Gate added as blocking step (lines 47-80)
- ✅ Enhanced reporting requirements (LSP adoption tracking)
- ✅ Hard block added for skipping LSP on code searches
- ✅ Token hierarchy reordered with LSP prominence

---

**This file is automatically loaded by OpenCode at every session start.**
