# Proposed Directory Structure - Agent-Specific Subfolders

## Current Structure (What We Have)
```
evals/
├── framework/              # Shared framework
├── opencode/
│   ├── openagent/         # OpenAgent tests
│   └── shared/            # Shared tests
└── results/
```

## Proposed Structure (Cleaner)
```
evals/
├── framework/              # Shared framework (agent-agnostic)
│   ├── src/
│   │   ├── sdk/
│   │   ├── evaluators/
│   │   └── types/
│   └── package.json
│
├── agents/                 # All agent-specific tests
│   ├── openagent/         # OpenAgent-specific
│   │   ├── tests/
│   │   │   ├── developer/
│   │   │   ├── business/
│   │   │   ├── creative/
│   │   │   └── edge-case/
│   │   ├── docs/
│   │   │   ├── RULES.md
│   │   │   └── TEST_SCENARIOS.md
│   │   ├── config/
│   │   │   └── config.yaml
│   │   └── README.md
│   │
│   ├── opencoder/         # OpenCoder-specific (future)
│   │   ├── tests/
│   │   │   ├── developer/
│   │   │   └── refactoring/
│   │   ├── docs/
│   │   │   └── RULES.md
│   │   └── README.md
│   │
│   ├── shared/            # Tests for ANY agent
│   │   ├── tests/
│   │   │   └── common/
│   │   └── README.md
│   │
│   └── README.md          # Guide to agent testing
│
└── results/               # Test results (gitignored)
```

## Benefits of This Structure

1. **Clear Separation**
   - `framework/` = Shared infrastructure
   - `agents/` = All agent-specific content
   - Each agent has its own subfolder

2. **Easy to Find**
   - Want OpenAgent tests? → `agents/openagent/tests/`
   - Want OpenCoder tests? → `agents/opencoder/tests/`
   - Want shared tests? → `agents/shared/tests/`

3. **Scalable**
   - Add new agent: `mkdir -p agents/my-agent/tests/developer`
   - Copy structure from existing agent
   - No confusion about where files go

4. **Consistent Naming**
   - All agents use same structure:
     - `tests/` - Test files
     - `docs/` - Agent-specific documentation
     - `config/` - Agent configuration
     - `README.md` - Agent overview

## Migration Plan

### Option A: Rename `opencode/` to `agents/`
```bash
mv evals/opencode evals/agents
```

### Option B: Create new `agents/` and move content
```bash
mkdir -p evals/agents
mv evals/opencode/openagent evals/agents/
mv evals/opencode/shared evals/agents/
rmdir evals/opencode
```

### Option C: Keep both (transition period)
```bash
# Keep opencode/ for now
# Create agents/ as new structure
# Migrate gradually
```

## Recommended: Option A (Simple Rename)

```bash
cd evals
mv opencode agents
```

Then update documentation to reference `agents/` instead of `opencode/`.

## File Paths After Migration

### Before
```
evals/opencode/openagent/sdk-tests/developer/task-simple-001.yaml
evals/opencode/shared/sdk-tests/common/approval-gate-basic.yaml
```

### After
```
evals/agents/openagent/tests/developer/task-simple-001.yaml
evals/agents/shared/tests/common/approval-gate-basic.yaml
```

## Commands After Migration

### Before
```bash
npm run eval:sdk -- --pattern="opencode/openagent/**/*.yaml"
```

### After
```bash
npm run eval:sdk -- --pattern="agents/openagent/**/*.yaml"
```

## What Needs to Update

1. **Documentation**
   - Update all references from `opencode/` to `agents/`
   - Update all references from `sdk-tests/` to `tests/`

2. **Test Runner** (if it has hardcoded paths)
   - Check `framework/src/sdk/test-runner.ts`
   - Update any hardcoded paths

3. **README files**
   - Update directory structure diagrams
   - Update example commands

## Decision Needed

Which option do you prefer?
- [ ] Option A: Simple rename `opencode/` → `agents/`
- [ ] Option B: Create new `agents/` and move content
- [ ] Option C: Keep current structure (opencode/)
- [ ] Option D: Different structure (please specify)
