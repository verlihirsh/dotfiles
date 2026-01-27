# Global OpenCode Instructions

## Core Development Workflow (ALL PROJECTS)

### 1. Test-Driven Development (TDD) Approach - MANDATORY

**Philosophy**: Tests define the specification. Code implements the specification.

#### Before Writing Any Code:
1. **Think about the task/stage**: What functionality needs to be implemented?
2. **Design tests first**: What tests would prove this works correctly?
3. **Use black-box E2E approach**: Test behavior, not implementation details
4. **Use common test libraries**: 
   - Go: `testing` package, table-driven tests
   - Python: `pytest`, `unittest`
   - JavaScript/TypeScript: `jest`, `vitest`, `mocha`
   - Rust: built-in `#[test]` framework

#### TDD Workflow:
```
1. Write failing test (RED)
   ↓
2. Implement minimal code to pass (GREEN)
   ↓
3. Refactor while keeping tests green (REFACTOR)
   ↓
4. Repeat
```

**NEVER**: Write implementation code before tests exist (except exploratory spikes, which must be deleted).

---

### 2. Manual Testing Strategy - MANDATORY

During development, ensure you have a way to manually test the application.

#### Implementation Options (choose one):

**Option A: Headless Mode (Recommended)**
- Implement `--headless` or `--non-interactive` flag
- Accept input via stdin/stdout
- Log output to file for verification
- Example: `./app --headless < input.txt > output.log`

**Option B: Test Harness**
- Create `test_runner.sh` or similar script
- Automate input/output scenarios
- Capture and verify results

**Option C: Debug Mode**
- Implement `--debug` flag with verbose logging
- Log all operations to file
- Verify behavior by reading logs

**Requirement**: 
- Document the manual testing approach in AGENTS.md
- Demonstrate manual test before marking task complete
- Manual test should be repeatable and scriptable

---

### 3. Bug Fixing Protocol - MANDATORY

When working on a bugfix, follow this exact sequence:

#### Step 1: Research Phase (REQUIRED)
1. **Understand the bug**: What is the expected vs actual behavior?
2. **Find the context**: What code path leads to this bug?
3. **Identify root cause**: Why does this bug happen? (not just symptoms)
4. **Research approaches**: How can this be fixed? What are the tradeoffs?

#### Step 2: Write Failing Test (REQUIRED)
1. **Create test that reproduces the bug**: This test MUST fail initially
2. **Test should be minimal**: Smallest possible reproduction case
3. **Test should be isolated**: No dependencies on other tests
4. **Run test to confirm failure**: Verify test actually catches the bug

```bash
# Example: Verify test fails before fix
go test -run TestBugFix_Issue123
# Output should show: FAIL
```

#### Step 3: Implement Fix (REQUIRED)
1. **Make minimal changes**: Fix root cause, not symptoms
2. **Run test to verify**: Test should now pass
3. **Run full test suite**: Ensure no regressions
4. **Manual test**: Verify fix works in real scenario

#### Step 4: Failure Recovery Protocol
If unable to fix bug after **reasonable attempts** (2-3 iterations):

1. **Document the investigation**:
   ```markdown
   ## Bug: [Issue Number/Description]
   
   ### Root Cause
   [What causes the bug]
   
   ### Approaches Attempted
   1. [Approach 1] - Failed because [reason]
   2. [Approach 2] - Failed because [reason]
   3. [Approach 3] - Failed because [reason]
   
   ### Remaining Challenges
   [What's blocking the fix]
   
   ### Recommendations
   [Suggestions for next attempt]
   ```

2. **Save to `./docs/bugs/bug-[issue-number].md`**

3. **Create TODO comment in code**:
   ```go
   // TODO(bug-123): Fix race condition in concurrent writes
   // See: ./docs/bugs/bug-123.md for investigation notes
   ```

4. **Move forward**: Leave bug for another agent/human to tackle

**NEVER**: 
- Leave code in broken state without documentation
- Implement workarounds without understanding root cause
- Skip the failing test step

---

### 4. Task Completion Checklist - MANDATORY

Before marking any task/task-stage as complete, verify ALL of the following:

#### Tests
- [ ] All tests pass (green)
- [ ] New tests added for new functionality
- [ ] Bug fixes have regression tests
- [ ] Run full test suite: `go test ./...` (or equivalent)
- [ ] Race detector clean (Go): `go test -race ./...`

#### Code Quality
- [ ] Linter has no errors
  - Go: `go vet ./...`, `golangci-lint run ./...`
  - Python: `pylint`, `flake8`, `mypy`
  - JavaScript/TypeScript: `eslint`, `tsc`
- [ ] Formatter applied
  - Go: `gofmt -w ./...`
  - Python: `black .`, `isort .`
  - JavaScript/TypeScript: `prettier --write .`

#### Repository Cleanliness
- [ ] No temporary files committed
- [ ] No debug code or commented-out blocks
- [ ] No `console.log()`, `print()`, `fmt.Println()` debug statements
- [ ] No `.DS_Store`, `.swp`, `*.tmp` files
- [ ] Build artifacts in `.gitignore`

#### Manual Verification
- [ ] Manual test performed and documented
- [ ] Application builds successfully
- [ ] No runtime errors in test scenarios

**If ANY checkbox fails, task is NOT complete.**

---

### 5. Git Workflow - MANDATORY

After each task stage or task completion:

#### Step 1: Verify Repository State
```bash
git status
# Should show only intended changes
```

#### Step 2: Stage Changes
```bash
# Stage specific files (preferred)
git add path/to/changed/files

# Or stage all (only if verified clean)
git add .
```

#### Step 3: Commit with Conventional Commit Message
```bash
git commit -m "type(scope): description"
```

**Conventional Commit Types**:
- `feat`: New feature
- `fix`: Bug fix
- `test`: Adding or updating tests
- `refactor`: Code refactoring (no behavior change)
- `docs`: Documentation changes
- `style`: Code style changes (formatting, no logic change)
- `perf`: Performance improvements
- `chore`: Maintenance tasks
- `build`: Build system or dependency changes
- `ci`: CI/CD changes

**Examples**:
```bash
git commit -m "feat(parser): add support for UTF-8 encoding"
git commit -m "fix(auth): prevent race condition in token refresh"
git commit -m "test(api): add E2E tests for user registration"
git commit -m "refactor(database): extract connection pool logic"
```

#### Step 4: Verify Commit
```bash
git log -1 --stat
# Review what was committed
```

**Commit Hygiene**:
- ✅ One logical change per commit
- ✅ All tests pass before committing
- ✅ Linter clean before committing
- ✅ Meaningful commit message
- ❌ Don't commit broken code
- ❌ Don't commit commented-out code
- ❌ Don't commit temporary debug files

---

### 6. Documentation Standards

#### README.md Creation/Updates

**Template Location**: `~/docs/README.template.md`

**Workflow**:
1. **Copy template**:
   ```bash
   cp ~/docs/README.template.md ./README.md
   ```

2. **Fill in project details**:
   - Project title
   - Description
   - Installation instructions
   - Usage examples
   - API documentation (if applicable)
   - Contributing guidelines
   - License information

3. **Add banner metadata** (if project supports banners):
   ```markdown
   <!-- banner-title: Your Project Name -->
   <!-- banner-tagline: Your project description -->
   ```

4. **Generate banner**:
   ```bash
   banner-gen . [theme] [alignment]
   # Default: banner-gen .
   ```

5. **Verify banner exists**:
   ```bash
   ls -la banner.{svg,png}
   ```

6. **Include banner in README.md**:
   ```markdown
   ![Project Banner](banner.png)
   # or
   <img src="banner.png" alt="Project Banner" width="100%">
   ```

7. **Commit README and banner**:
   ```bash
   git add README.md banner.svg banner.png
   git commit -m "docs: add README with project banner"
   ```

#### Documentation File Organization

**Rules**:
- ✅ `README.md` → Project root (always)
- ✅ `AGENTS.md` → Project root (agent instructions)
- ✅ All other `.md` files → `./docs/` directory
- ✅ `./docs/` is in global `.gitignore` (documentation is local-only)

**Examples**:
```
project-root/
├── README.md              ✅ Root level (committed)
├── AGENTS.md              ✅ Root level (committed)
├── docs/                  ✅ Local documentation (gitignored)
│   ├── architecture.md
│   ├── api-design.md
│   ├── bugs/
│   │   ├── bug-123.md
│   │   └── bug-456.md
│   └── investigation-notes.md
├── banner.svg             ✅ Generated banner (committed)
└── banner.png             ✅ Generated banner (committed)
```

**When creating documentation**:
```bash
# Always create in docs/ directory
mkdir -p docs/bugs
echo "# Bug Investigation" > docs/bugs/bug-123.md

# Never create at root level (except README.md and AGENTS.md)
```

---

## Language-Specific Quality Guidelines

### Go Projects

**Trigger Condition**: When working in a directory that contains a `go.mod` file OR when modifying/creating `.go` files.

**Action**: Automatically load and follow the comprehensive Go Quality Checklist from:
```
~/.config/opencode/GO_QUALITY_CHECKLIST.md
```

**Why**: This checklist contains 15 Hard Stops and 83+ quality checks across 10 categories (Scope, Packages, Dependencies, Format/Style, Errors, Concurrency, HTTP/API, Data/Persistence, Observability, Testing, Security/Performance).

**Mandatory**: 
- Review ALL Hard Stops (HS01-HS15) before marking task complete
- Run minimum commands: `gofmt`, `go test`, `go vet`, and successful build
- Provide structured audit report using template in the checklist

**Token Optimization**: This checklist is ONLY loaded for Go projects to conserve context tokens in non-Go work.

---

## Future Language Guidelines

Add similar conditional loading for other languages as needed:
- Python projects (when `requirements.txt`, `pyproject.toml`, or `setup.py` present)
- TypeScript/JavaScript projects (when `package.json` present)
- Rust projects (when `Cargo.toml` present)
- etc.

---

## Summary: Agent Workflow Checklist

For every task, follow this sequence:

### Planning Phase
- [ ] Understand the task requirements
- [ ] Design tests that prove correctness (TDD)
- [ ] Plan manual testing approach

### Implementation Phase
- [ ] Write failing tests first (RED)
- [ ] Implement minimal code to pass tests (GREEN)
- [ ] Refactor while keeping tests green (REFACTOR)
- [ ] Perform manual testing

### Completion Phase
- [ ] All tests pass (green)
- [ ] Linter clean (no errors)
- [ ] Repository clean (no temp files)
- [ ] Manual test documented/performed
- [ ] Stage changes: `git add`
- [ ] Commit with conventional message: `git commit -m "type(scope): message"`

### Bug Fixing Phase
- [ ] Research root cause
- [ ] Write failing test that reproduces bug
- [ ] Implement fix
- [ ] Verify test passes + no regressions
- [ ] If unable to fix: document in `./docs/bugs/` and move on

### Documentation Phase
- [ ] Use `~/docs/README.template.md` for README
- [ ] Generate banner: `banner-gen .`
- [ ] Verify banner.png in README.md
- [ ] Place all other .md files in `./docs/` directory

---

## Enforcement

These instructions are **MANDATORY** for all agents working in any project. Failure to follow this workflow may result in:
- Broken builds
- Flaky tests
- Production bugs
- Technical debt
- Inconsistent documentation

**When in doubt**: Follow the checklist. If unsure about a step, ask before proceeding.
