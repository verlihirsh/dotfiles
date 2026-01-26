# Token Optimization Quick Reference Card

**🚨 MANDATORY BEFORE EVERY FILE READ 🚨**

## Pre-Flight Check (All Must Be TRUE)

```
┌────────────────────────────────────────────┐
│ [ ] 1. Checked mem0 first?                 │
│ [ ] 2. Tried LSP tools?                    │
│ [ ] 3. Used grep/ast-grep?                 │
│ [ ] 4. File < 500 lines?                   │
│ [ ] 5. Absolutely necessary?               │
│                                            │
│ ❌ ANY FALSE → ABORT READ                  │
└────────────────────────────────────────────┘
```

## Tool Priority Order

| # | Tool | Cost | Use Case |
|---|------|------|----------|
| 1️⃣ | **mem0** | Near-zero | Check cached context FIRST |
| 2️⃣ | **LSP** | Very low | Symbols, definitions, references |
| 3️⃣ | **Context7** | Low | External library docs |
| 4️⃣ | **AST-grep** | Low | Code pattern search |
| 5️⃣ | **grep** | Medium | Content search → targeted read |
| 6️⃣ | **read** | High | Last resort, < 500 lines only |

## LSP Quick Commands

```typescript
lsp_symbols(scope="workspace", query="*Service")  // Find all services
lsp_goto_definition(...)                          // Jump to definition
lsp_find_references(...)                          // Find all usages
lsp_diagnostics(...)                              // Check for errors
```

## Session Protocol

**START**: `mem0_search_memory(query="[context]")`  
**WORK**: Follow pre-flight check  
**END**: `mem0_add_memories(text="[learnings]") + report efficiency`

## File Size Rules

| Lines | Action |
|-------|--------|
| < 200 | OK to read fully |
| 200-500 | Read with offset/limit |
| 500-1000 | grep + targeted read only |
| > 1000 | ❌ FORBIDDEN - use LSP/grep |

## Anti-Patterns (FORBIDDEN)

❌ `bash("cat large-file.ts")`  
❌ Read files to "understand codebase"  
❌ Read entire folders  
❌ Skip mem0 check  
❌ Skip LSP when available  

## Task Completion Report Template

```
✅ Task Complete: [description]

Tools Used:
- mem0_search_memory: [what found]
- lsp_*: [operations]
- grep: [searches]
- read: [X targeted reads, Y lines total]

Token Efficiency:
- Used: ~[X] tokens
- Avoided: ~[Y] tokens (would have read Z files)
- Savings: [%]

Memory Updates:
- [saved learnings]

Scope Discipline:
✓ [confirmations]
```

---

**Remember**: This is NON-NEGOTIABLE. Violations = task failure.
