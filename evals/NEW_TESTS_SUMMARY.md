# New Tests Summary - 5 Essential Workflow Tests

**Created:** November 22, 2025  
**Purpose:** Validate OpenAgent follows workflows defined in `openagent.md`  
**Approach:** Simple, focused tests for core workflow compliance

---

## ✅ What We Created

### **5 Essential Tests**

| Test ID | File | Workflow Tested | Status |
|---------|------|----------------|--------|
| `task-simple-001` | `developer/task-simple-001.yaml` | Analyze → Approve → Execute → Validate | ✅ Created |
| `ctx-code-001` | `developer/ctx-code-001.yaml` | Execute → Load Context (code.md) | ✅ Created |
| `ctx-docs-001` | `developer/ctx-docs-001.yaml` | Execute → Load Context (docs.md) | ✅ Created |
| `fail-stop-001` | `developer/fail-stop-001.yaml` | Validate → Stop on Failure | ✅ Created |
| `conv-simple-001` | `business/conv-simple-001.yaml` | Conversational Path (no approval) | ✅ Created |

### **1 Shared Test (Agent-Agnostic)**

| Test ID | File | Purpose | Status |
|---------|------|---------|--------|
| `shared-approval-001` | `shared/tests/common/approval-gate-basic.yaml` | Universal approval gate test | ✅ Created |

### **3 Documentation Files**

| File | Purpose | Status |
|------|---------|--------|
| `evals/agents/shared/README.md` | Shared tests guide | ✅ Created |
| `evals/opencode/AGENT_TESTING_GUIDE.md` | Agent-agnostic architecture guide | ✅ Created |
| `evals/SIMPLE_TEST_PLAN.md` | Simple test plan | ✅ Already exists |

---

## 📊 Test Coverage

### **Before (6 tests)**
- ✅ Business analysis (conversational)
- ✅ Create component
- ✅ Install dependencies (v2)
- ✅ Install dependencies (v1)
- ✅ "Just do it" bypass
- ✅ Negative test (should violate)

### **After (11 tests)**
- ✅ All previous tests (6)
- ✅ Simple bash execution (1)
- ✅ Code with context loading (1)
- ✅ Docs with context loading (1)
- ✅ Stop on failure (1)
- ✅ Conversational path (1)

### **Coverage by Workflow Stage**

| Workflow Stage | Rule | Tests Before | Tests After | Gap Closed |
|----------------|------|--------------|-------------|------------|
| **Analyze** | Path detection | 1 | 2 | +1 |
| **Approve** | Approval gate | 2 | 3 | +1 |
| **Execute → Load Context** | Context loading | 0 | 2 | +2 |
| **Validate** | Stop on failure | 0 | 1 | +1 |
| **Confirm** | Cleanup | 0 | 0 | 0 |

**Progress:** 4/13 gaps closed (31% improvement)

---

## 🎯 Test Details

### **1. task-simple-001 - Simple Bash Execution**
**File:** `developer/task-simple-001.yaml`

**Tests:**
- ✅ Approval gate enforcement
- ✅ Basic task workflow (Analyze → Approve → Execute → Validate)
- ✅ Bash tool usage

**Expected Behavior:**
```
User: "Run npm install"
Agent: "I'll run npm install. Should I proceed?" ← Asks approval
User: [Approves]
Agent: [Executes bash] → Reports result
```

**Rules Tested:**
- Line 64-66: Approval gate
- Line 141-144: Task path

---

### **2. ctx-code-001 - Code with Context Loading**
**File:** `developer/ctx-code-001.yaml`

**Tests:**
- ✅ Context loading for code tasks
- ✅ Approval gate enforcement
- ✅ Execute stage context loading (Step 3.1)

**Expected Behavior:**
```
User: "Create a TypeScript function"
Agent: "I'll create the function. Should I proceed?" ← Asks approval
User: [Approves]
Agent: [Reads .opencode/context/core/standards/code.md] ← Loads context
Agent: [Writes code following standards] → Reports result
```

**Rules Tested:**
- Line 162-193: Context loading (MANDATORY)
- Line 179: "Code tasks → code.md (MANDATORY)"

---

### **3. ctx-docs-001 - Docs with Context Loading**
**File:** `developer/ctx-docs-001.yaml`

**Tests:**
- ✅ Context loading for docs tasks
- ✅ Approval gate enforcement
- ✅ Execute stage context loading (Step 3.1)

**Expected Behavior:**
```
User: "Update README with installation steps"
Agent: "I'll update the README. Should I proceed?" ← Asks approval
User: [Approves]
Agent: [Reads .opencode/context/core/standards/docs.md] ← Loads context
Agent: [Edits README following standards] → Reports result
```

**Rules Tested:**
- Line 162-193: Context loading (MANDATORY)
- Line 180: "Docs tasks → docs.md (MANDATORY)"

---

### **4. fail-stop-001 - Stop on Test Failure**
**File:** `developer/fail-stop-001.yaml`

**Tests:**
- ✅ Stop on failure rule
- ✅ Report → Propose → Approve → Fix workflow
- ✅ NEVER auto-fix

**Expected Behavior:**
```
User: "Run the test suite"
Agent: "I'll run the tests. Should I proceed?" ← Asks approval
User: [Approves]
Agent: [Runs tests] → Tests fail
Agent: STOPS ← Does NOT auto-fix
Agent: "Tests failed with X errors. Here's what I found..." ← Reports
Agent: "I can propose a fix if you'd like." ← Waits for approval
```

**Rules Tested:**
- Line 68-70: "STOP on test fail/errors - NEVER auto-fix"
- Line 71-73: "REPORT→PROPOSE FIX→REQUEST APPROVAL→FIX"

**Note:** This test requires a project with failing tests to properly validate.

---

### **5. conv-simple-001 - Conversational Path**
**File:** `business/conv-simple-001.yaml`

**Tests:**
- ✅ Conversational path detection
- ✅ No approval for read-only operations
- ✅ Direct answer without approval

**Expected Behavior:**
```
User: "What does the main function do?"
Agent: [Reads src/index.ts] ← No approval needed
Agent: "The main function does X, Y, Z..." ← Answers directly
```

**Rules Tested:**
- Line 136-139: "Conversational path: Answer directly - no approval needed"
- Line 141-144: Task path vs conversational path

---

## 🏗️ Agent-Agnostic Architecture

### **How It Works**

1. **Framework Layer (Agent-Agnostic)**
   - Test runner works with any agent
   - Evaluators check generic behaviors
   - Universal test schema

2. **Agent Layer (Per Agent)**
   - Tests organized by agent: `opencode/{agent}/tests/`
   - Agent-specific rules: `opencode/{agent}/docs/`
   - Shared tests: `agents/shared/tests/`

3. **Test Specifies Agent**
   ```yaml
   agent: openagent  # Routes to OpenAgent
   ```

### **Directory Structure**

```
evals/
├── framework/              # SHARED - Works with any agent
│   ├── src/sdk/           # Test runner
│   └── src/evaluators/    # Generic evaluators
│
├── opencode/
│   ├── openagent/         # OpenAgent-specific tests
│   │   ├── tests/
│   │   │   ├── developer/
│   │   │   │   ├── task-simple-001.yaml      ← NEW
│   │   │   │   ├── ctx-code-001.yaml         ← NEW
│   │   │   │   ├── ctx-docs-001.yaml         ← NEW
│   │   │   │   └── fail-stop-001.yaml        ← NEW
│   │   │   └── business/
│   │   │       └── conv-simple-001.yaml      ← NEW
│   │   └── docs/
│   │       └── OPENAGENT_RULES.md
│   │
│   ├── opencoder/         # OpenCoder tests (future)
│   │   └── tests/
│   │
│   └── shared/            # Tests for ANY agent
│       ├── tests/
│       │   └── common/
│       │       └── approval-gate-basic.yaml  ← NEW
│       └── README.md                         ← NEW
│
└── AGENT_TESTING_GUIDE.md                    ← NEW
```

### **Running Tests Per Agent**

```bash
# Run ALL OpenAgent tests
npm run eval:sdk -- --pattern="openagent/**/*.yaml"

# Run specific category
npm run eval:sdk -- --pattern="openagent/developer/*.yaml"

# Run shared tests for OpenAgent
npm run eval:sdk -- --pattern="shared/**/*.yaml" --agent=openagent

# Run single test
npx tsx src/sdk/show-test-details.ts openagent/developer/task-simple-001.yaml
```

### **Adding a New Agent**

```bash
# 1. Create directory
mkdir -p evals/opencode/my-agent/tests/developer

# 2. Copy shared tests
cp evals/agents/shared/tests/common/*.yaml \
   evals/opencode/my-agent/tests/developer/

# 3. Update agent field
sed -i 's/agent: openagent/agent: my-agent/g' \
  evals/opencode/my-agent/tests/developer/*.yaml

# 4. Run tests
npm run eval:sdk -- --pattern="my-agent/**/*.yaml"
```

---

## 📝 Next Steps

### **Immediate (Ready to Run)**

1. **Run the new tests**
   ```bash
   cd evals/framework
   npm run eval:sdk -- --pattern="openagent/developer/task-simple-001.yaml"
   npm run eval:sdk -- --pattern="openagent/developer/ctx-code-001.yaml"
   npm run eval:sdk -- --pattern="openagent/developer/ctx-docs-001.yaml"
   npm run eval:sdk -- --pattern="openagent/business/conv-simple-001.yaml"
   ```

2. **Run all new tests together**
   ```bash
   npm run eval:sdk -- --pattern="openagent/**/*.yaml"
   ```

3. **Check results**
   - Review evaluator output
   - Verify workflow compliance
   - Fix any issues

### **Short-Term (Next Week)**

1. **Add remaining tests** (8 more to reach 17 total)
   - More conversational path tests
   - More context loading tests
   - Cleanup confirmation test
   - Edge case tests

2. **Create test fixtures**
   - Project with failing tests (for fail-stop-001)
   - Sample code files
   - Sample documentation

3. **Refine evaluators**
   - Add StopOnFailureEvaluator
   - Add CleanupConfirmationEvaluator
   - Improve context loading detection

### **Long-Term (Future)**

1. **Add OpenCoder tests**
   - Copy shared tests
   - Add OpenCoder-specific tests
   - Compare behaviors

2. **Expand shared tests**
   - More universal tests
   - Cross-agent validation
   - Benchmark tests

---

## 🎓 Key Learnings

### **1. Keep It Simple**
- ✅ Focus on workflow compliance
- ✅ Test one thing at a time
- ✅ Clear expected behaviors

### **2. Agent-Agnostic Design**
- ✅ Framework works with any agent
- ✅ Tests specify which agent to use
- ✅ Evaluators check generic behaviors

### **3. Clear Organization**
- ✅ Agent-specific tests in `opencode/{agent}/`
- ✅ Shared tests in `agents/shared/`
- ✅ Easy to find and manage

### **4. Workflow-Focused**
- ✅ Test workflow stages (Analyze → Approve → Execute → Validate)
- ✅ Test critical rules (approval, context, stop-on-failure)
- ✅ Test both paths (conversational vs task)

---

## 📊 Summary

**Created:**
- ✅ 5 essential workflow tests
- ✅ 1 shared test (agent-agnostic)
- ✅ 3 documentation files
- ✅ Agent-agnostic architecture

**Coverage:**
- ✅ 31% improvement in workflow coverage
- ✅ 11 total tests (was 6)
- ✅ 4/13 gaps closed

**Ready to:**
- ✅ Run tests with free model (no costs)
- ✅ Validate workflow compliance
- ✅ Add more tests easily
- ✅ Test multiple agents

**Next:**
- Run the new tests
- Review results
- Iterate and improve
