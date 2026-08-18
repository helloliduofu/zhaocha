---
name: zhaocha
description: Zhaocha 🔍 - Multi-perspective optimization analysis after task completion to reduce iteration cycles. Auto-triggered after code/design completion.
triggers:
  - /zhaocha
  - /zc
---

# 🔍 Zhaocha - Multi-Perspective Optimization Analysis

> 🪞 Examine your work from every angle, like looking in a mirror

## Trigger Conditions

This skill auto-triggers in the following scenarios:
- After completing code (created or modified files)
- After completing a design/solution
- After implementing a feature
- After fixing a bug

Does NOT trigger for:
- Read-only operations (viewing files, no modifications)
- Simple config changes (e.g., changing a single number)
- User explicitly says "no check needed" or "skip check"

## Core Instructions

**Quick Index**: [1. Analyze](#1-analyze-task-nature) → [2. Perspectives](#2-dynamically-generate-perspectives) → [3. Analysis](#3-multi-perspective-parallel-analysis) → [4. Severity](#4-severity-assessment) → [5. Report](#5-output-report) → [6. Confirm](#6-wait-for-user-confirmation) → [7. Execute](#7-execute-optimizations) → [8. Iterate](#8-iterate)

When triggered, follow this workflow:

### 1. Analyze Task Nature

First, understand the completed work:
- What type of artifact? (code/design/config/docs...)
- What technical domains? (frontend/backend/database/devops...)
- What is the core functionality?
- Who is the target user/audience?

### 2. Dynamically Generate Perspectives

Infer the needed perspectives based on task characteristics. Do NOT use preset roles — generate dynamically based on the situation.

**Principles for generating perspectives**:
- Where is this task most likely to have issues?
- Which dimensions of review are most valuable for this task?
- Are there any easily overlooked angles?

**If no meaningful perspectives can be generated** (task is trivially simple or purely mechanical): skip directly to step 5 and output "Zhaocha complete — no obvious optimization points found". Do not force-generate perspectives.

**Examples** (for reference only, always generate dynamically):
- UI Component → Security + Error Handling + UX + Accessibility + Performance
- API Design → Input Validation + Error Handling + Security + Backward Compatibility + Documentation
- Database → Consistency + Query Performance + Indexing + Migration Compatibility + Scalability
- CLI Tool → CLI Conventions + Error Messages + Help Docs + Pipe Composition + Cross-platform

### 3. Multi-Perspective Parallel Analysis

For each generated perspective, think from that viewpoint:

**Each perspective should answer**:
- What can be improved from this angle?
- What potential issues or risks exist?
- Is there a better implementation approach?
- Are any important scenarios missed?

### 4. Severity Assessment

Rate each finding:
- 🔴 **HIGH**: Must fix — affects functionality/security/correctness
- 🟡 **MEDIUM**: Should fix — affects quality/experience/maintainability
- 🟢 **LOW**: Optional — nice to have

### 5. Output Report

> ⚠️ **Report Language**: Always output the report in the **user's language**. Detect the user's language from the conversation and produce the report accordingly. If the user writes in Chinese, the report is in Chinese. If in English, the report is in English. **Default to English if detection is uncertain.**

Use the following format:

```markdown
## 🔍 Zhaocha Report

**Artifact**: [file path or solution name]
**Perspectives**: [perspective 1] | [perspective 2] | [perspective 3] ...

---

### [[Perspective Name]]
- [severity] [specific suggestion]
- [severity] [specific suggestion]

---

### Optimization Summary

| # | Perspective | Severity | Suggestion |
|---|-------------|----------|------------|
| 1 | [perspective] | 🔴 HIGH | [summary] |
| 2 | [perspective] | 🟡 MEDIUM | [summary] |
| ... | ... | ... | ... |

**Actions**: `y` all | `n` skip | `1,3` specific | `仅HIGH` HIGH only | `HIGH+MEDIUM` HIGH+MID
```

### 6. Wait for User Confirmation

After outputting the report, wait for user selection. Accept both shorthand and natural language:
- `y` / `yes` / `全部` — Execute all optimizations
- `n` / `no` / `skip` / `跳过` — Skip all
- `1,3` — Execute only items 1 and 3 (supports any combination)
- `仅HIGH` / `only HIGH` / `HIGH` — Execute only 🔴 HIGH items
- `HIGH+MEDIUM` / `MEDIUM以上` — Execute 🔴 HIGH + 🟡 MEDIUM items

### 7. Execute Optimizations

Execute selected optimizations in order. For each, briefly explain what was changed.

### 8. Iterate

After execution, enter the next round of zhaocha. Continue until exit conditions are met.

**Flow**:
```
Zhaocha Analysis → Output Report → User Confirms → Execute → Repeat → ... → Goal reached or max iterations
```

**Exit conditions** (any one stops the loop):
- Max iterations reached (default: 10)
- No new optimization points found in current round (quality is sufficient)
- User interrupts

**Per-round output**:
```markdown
### 🔄 Iteration Progress

**Current round**: 3 / 10
**Findings this round**: 2 (down 3 from previous round)
**Cumulative fixes**: 12

| Round | Found | Fixed | Quality Score |
|-------|-------|-------|---------------|
| 1     | 8     | 8     | 60            |
| 2     | 5     | 5     | 75            |
| 3     | 2     | -     | 88            |

Continue? `y` continue | `n` stop
```

**Quality scoring**:
- Start at 100
- Each 🔴 HIGH unresolved: -10
- Each 🟡 MEDIUM unresolved: -5
- Each 🟢 LOW unresolved: -2

## Core Principles

### 🎯 Stability First (Coding)

> ⚠️ **Scope**: This principle applies **only to code artifacts**. For design, config, or documentation artifacts, prioritize clarity and correctness instead.

When analyzing code, this principle takes highest priority:

1. **Minimize changes** — Only change what is strictly necessary. Do not refactor unrelated code, do not touch files that don't need changes, do not introduce unnecessary abstractions or "clean up" code that isn't directly related to the task.
2. **Stability over elegance** — A working, stable solution is always better than a clever but fragile one. Prefer boring, proven patterns over novel approaches.
3. **Preserve existing behavior** — Changes must not break existing functionality. Every modification should be scoped as narrowly as possible to achieve the goal.
4. **No unnecessary dependencies** — Do not introduce new libraries or frameworks unless absolutely required. Leverage what already exists in the project.
5. **One change at a time** — Each change should be atomic and independently verifiable. Avoid bundling unrelated fixes together.

**Checklist before proposing any code change**:
- [ ] Is this change strictly necessary to achieve the goal?
- [ ] Does it risk breaking existing functionality?
- [ ] Can it be made smaller in scope?
- [ ] Is it using existing project patterns and dependencies?

## Special Cases

### Large Changes

If the artifact spans multiple files or has many changes:
- Output an overall report first
- Suggest batch execution for the user
- Avoid too many changes at once (hard to review)

### No Optimization Needed

If analysis finds nothing to improve:
- Output "Zhaocha complete — no obvious optimization points found"
- Do not force-find issues

### User Skips

If user chooses to skip (`n`):
- Do not ask again
- Respect the decision

## Notes

1. **Don't over-analyze**: Focus on truly valuable improvements, not nitpicks
2. **Stay practical**: Suggestions should be concrete and actionable, not vague
3. **Respect context**: Consider the project's tech stack, team conventions, and business context
4. **Limit findings**: No more than 10 optimization points per report to avoid overload
5. **Prioritize HIGH**: Sort by severity so users see the most important issues first

## Examples

### Example 1: React Component

**Artifact**: `LoginForm.tsx`

**Dynamically generated perspectives**:
- Security (form handles sensitive data)
- Error Handling (network requests may fail)
- User Experience (users need clear feedback)
- Accessibility (form must be accessible)

### Example 2: Database Migration

**Artifact**: `migration_add_user_table.sql`

**Dynamically generated perspectives**:
- Data Consistency (foreign keys, transactions)
- Rollback Safety (how to recover from failed migration)
- Performance (locking issues on large tables)
- Backward Compatibility (do old code paths still work?)

### Example 3: CLI Tool

**Artifact**: `deploy.sh`

**Dynamically generated perspectives**:
- Error Handling (how failures are reported)
- Idempotency (safe to run multiple times?)
- Cross-platform (macOS/Linux compatibility)
- Logging (is execution traceable?)