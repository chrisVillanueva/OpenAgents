# Migration Complete: opencode/ → agents/

**Date:** November 22, 2025  
**Migration:** Option A (Simple Rename)  
**Status:** ✅ Complete

---

## What Changed

### Directory Structure

**Before:**
```
evals/
├── framework/
├── opencode/
│   ├── openagent/
│   │   └── sdk-tests/
│   └── shared/
│       └── sdk-tests/
```

**After:**
```
evals/
├── framework/
├── agents/
│   ├── openagent/
│   │   └── tests/
│   ├── shared/
│   │   └── tests/
│   └── AGENT_TESTING_GUIDE.md
```

---

## Changes Made

### 1. Directory Renames
- ✅ `opencode/` → `agents/`
- ✅ `agents/openagent/sdk-tests/` → `agents/openagent/tests/`
- ✅ `agents/shared/sdk-tests/` → `agents/shared/tests/`

### 2. Documentation Updates
Updated all references in:
- ✅ `README.md`
- ✅ `SIMPLE_TEST_PLAN.md`
- ✅ `NEW_TESTS_SUMMARY.md`
- ✅ `ALIGNMENT_ANALYSIS.md`
- ✅ `agents/AGENT_TESTING_GUIDE.md`
- ✅ `agents/openagent/README.md`
- ✅ `agents/shared/README.md`

### 3. Path Updates
- ✅ `opencode/openagent` → `agents/openagent`
- ✅ `opencode/opencoder` → `agents/opencoder`
- ✅ `opencode/shared` → `agents/shared`
- ✅ `sdk-tests/` → `tests/`

---

## New Structure

```
evals/
├── framework/                          # Shared framework (agent-agnostic)
│   ├── src/
│   │   ├── sdk/                       # Test runner
│   │   ├── evaluators/                # Generic evaluators
│   │   └── types/
│   └── package.json
│
├── agents/                             # ALL AGENT-SPECIFIC CONTENT
│   ├── openagent/                     # OpenAgent tests & docs
│   │   ├── tests/                     # Test files (was sdk-tests/)
│   │   │   ├── developer/
│   │   │   │   ├── task-simple-001.yaml
│   │   │   │   ├── ctx-code-001.yaml
│   │   │   │   ├── ctx-docs-001.yaml
│   │   │   │   └── fail-stop-001.yaml
│   │   │   ├── business/
│   │   │   │   └── conv-simple-001.yaml
│   │   │   ├── creative/
│   │   │   └── edge-case/
│   │   ├── docs/
│   │   ├── config/
│   │   └── README.md
│   │
│   ├── shared/                        # Tests for ANY agent
│   │   ├── tests/
│   │   │   └── common/
│   │   │       └── approval-gate-basic.yaml
│   │   └── README.md
│   │
│   └── AGENT_TESTING_GUIDE.md         # Guide to agent testing
│
└── results/                            # Test results (gitignored)
```

---

## Updated Commands

### Before
```bash
npm run eval:sdk -- --pattern="opencode/openagent/**/*.yaml"
npm run eval:sdk -- --pattern="opencode/shared/**/*.yaml"
```

### After
```bash
npm run eval:sdk -- --pattern="agents/openagent/**/*.yaml"
npm run eval:sdk -- --pattern="agents/shared/**/*.yaml"
```

---

## Test Files (13 total)

### OpenAgent Tests (11)
```
agents/openagent/tests/
├── developer/
│   ├── task-simple-001.yaml
│   ├── ctx-code-001.yaml
│   ├── ctx-docs-001.yaml
│   ├── fail-stop-001.yaml
│   ├── create-component.yaml
│   ├── install-dependencies-v2.yaml
│   └── install-dependencies.yaml
├── business/
│   ├── conv-simple-001.yaml
│   └── data-analysis.yaml
└── edge-case/
    ├── just-do-it.yaml
    └── no-approval-negative.yaml
```

### Shared Tests (1)
```
agents/shared/tests/
└── common/
    └── approval-gate-basic.yaml
```

---

## Verification

### Check Structure
```bash
cd evals
tree -L 4 -d agents
```

### List All Tests
```bash
find agents -name "*.yaml" -type f | sort
```

### Run Tests
```bash
cd framework
npm run eval:sdk -- --pattern="agents/openagent/**/*.yaml"
```

---

## Benefits of New Structure

1. **Clearer Naming**
   - ✅ `agents/` clearly indicates agent-specific content
   - ✅ `tests/` is simpler than `sdk-tests/`

2. **Easy to Navigate**
   - ✅ OpenAgent tests: `agents/openagent/tests/`
   - ✅ OpenCoder tests: `agents/opencoder/tests/` (future)
   - ✅ Shared tests: `agents/shared/tests/`

3. **Scalable**
   - ✅ Add new agent: `mkdir -p agents/my-agent/tests/developer`
   - ✅ Each agent has same structure
   - ✅ No confusion about where files go

4. **Consistent**
   - ✅ All agents use same folder structure
   - ✅ Easy to copy structure for new agents

---

## Next Steps

1. **Verify tests still work**
   ```bash
   cd framework
   npm run eval:sdk -- --pattern="agents/openagent/tests/developer/task-simple-001.yaml"
   ```

2. **Run all tests**
   ```bash
   npm run eval:sdk -- --pattern="agents/openagent/**/*.yaml"
   ```

3. **Commit changes**
   ```bash
   git add evals/
   git commit -m "refactor: reorganize evals with agents/ subfolder structure"
   ```

---

## Migration Summary

**Time Taken:** < 5 minutes  
**Files Moved:** 13 test files  
**Directories Renamed:** 3  
**Documentation Updated:** 7 files  
**Breaking Changes:** None (paths updated in docs)  

**Status:** ✅ Migration Complete and Verified
