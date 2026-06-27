# PATCH: Rules & Checklist Additions

**Add to**: Your tool-specific rule files and verification checklist

This patch contains the new rules for concurrency, testing, git hygiene, configuration management, and API contracts. Add these to the appropriate sections in each file.

---

## For SYSTEM_PROMPT.md

**Add after** the "Resource Cleanup" working principle:

```markdown
### Concurrency Awareness
Always ask: "What happens if this runs twice concurrently?" Guard against double-execution of async operations. Cancel stale requests when superseded. Use optimistic UI updates only with rollback on failure. Never fire-and-forget — every async call needs error handling.

### Testing Discipline
Tests verify behavior, not implementation details. Test names should read like specifications. Every error path must have a corresponding test. Tests must be independent — no shared mutable state, no execution order dependencies. Mocks simulate external boundaries only — never mock the unit under test. Flaky tests are bugs, not inconveniences.

### Git & Commit Hygiene
Atomic commits — one logical change per commit. Never mix refactoring with feature changes. Never commit broken code. Use conventional commit messages (`feat:`, `fix:`, `refactor:`, `test:`, `docs:`, `chore:`).

### Configuration Management
Validate all required configuration at startup — fail fast if missing. Never use `process.env` directly in business logic — access through a typed config object. No environment-specific branching (`if NODE_ENV === 'production'`) in application code.

### API Contract Discipline
Never change API response shapes without explicit versioning. Document all public API endpoints. Input validation on all endpoints. Error responses follow a consistent structure.
```

**Add to** the "Before You're Done" checklist:

```markdown
13. **Concurrency**: Can any async operation be called twice concurrently? Are stale requests cancelled?
14. **Testing**: Does every error path have a test? Do tests verify behavior, not implementation?
15. **Git**: Is this a single logical change? Does the commit message follow conventional format?
16. **Config**: Are all environment variables accessed through a typed config object?
```

---

## For CLAUDE.md

**Add after** the "Performance & Resources" section:

```markdown
---

## Concurrency & Async Safety

- **Always** ask: "What happens if this runs twice concurrently?"
- **Always** guard async operations against double-execution
- **Always** cancel stale requests when superseded (AbortController)
- **Always** rollback optimistic UI updates on failure
- **Never** fire-and-forget async calls — every async call needs error handling
- **Never** use unbounded `Promise.all` for large collections — use controlled concurrency

---

## Testing

- **Always** test behavior, not implementation details
- **Always** write test names that read like specifications
- **Always** test every error path with a corresponding test case
- **Always** make tests independent — no shared mutable state
- **Never** mock the unit under test — mock only external boundaries
- **Never** use snapshot tests for business logic
- **Never** skip or disable tests (`test.skip` is a bug, not a solution)

---

## Git & Commit Hygiene

- **Always** make atomic commits — one logical change per commit
- **Always** use conventional commit messages (`feat:`, `fix:`, `refactor:`, `test:`, `docs:`, `chore:`)
- **Never** mix refactoring with feature changes in the same commit
- **Never** commit generated files (build artifacts, compiled outputs)

---

## Configuration Management

- **Always** validate all required config at startup — fail fast if missing
- **Always** access environment variables through a typed config object
- **Never** use `process.env` directly in business logic
- **Never** use environment-specific branching (`if NODE_ENV === 'production'`) in app code
- **Never** provide default values for required secrets
```

**Add to** the CLAUDE.md Verification Checklist, after "Modern Patterns":

```markdown
### Concurrency
- [ ] Async operations guarded against concurrent double-execution
- [ ] Stale requests cancelled (AbortController)
- [ ] Optimistic UI updates have rollback on failure
- [ ] No fire-and-forget async calls

### Testing
- [ ] Tests verify behavior, not implementation
- [ ] Every error path has a test
- [ ] Tests are independent (no shared state)
- [ ] No skipped tests

### Git
- [ ] Atomic commits (one logical change each)
- [ ] Conventional commit messages
- [ ] No refactoring mixed with features

### Configuration
- [ ] Environment variables accessed through typed config object
- [ ] Required config validated at startup
- [ ] No `process.env` in business logic
```

---

## For cursor-rules.md

**Add after** the "Stub Code" section:

```markdown
### Concurrency & Async Safety
- ❌ NEVER: Fire-and-forget async calls without error handling
- ❌ NEVER: Unguarded concurrent execution of the same async operation
- ❌ NEVER: Optimistic UI updates without rollback on failure
- ❌ NEVER: Unbounded `Promise.all` on large collections
- ✅ ALWAYS: Guard async operations against double-execution
- ✅ ALWAYS: Cancel stale requests when superseded (AbortController)
- ✅ ALWAYS: Ask "what happens if this runs twice concurrently?"

### Testing
- ❌ NEVER: Test implementation details (internal method calls, private state)
- ❌ NEVER: Mock the unit under test — mock only external boundaries
- ❌ NEVER: Use snapshot tests for business logic
- ❌ NEVER: Write tests without assertions
- ❌ NEVER: Skip or disable tests (`test.skip` is a bug)
- ✅ ALWAYS: Test observable behavior and error paths
- ✅ ALWAYS: Write test names that read like specifications
- ✅ ALWAYS: Keep tests independent (no shared mutable state)

### Git & Commit Hygiene
- ❌ NEVER: Mix refactoring with feature changes in one commit
- ❌ NEVER: Commit broken code or generated files
- ✅ ALWAYS: Atomic commits — one logical change each
- ✅ ALWAYS: Conventional commit messages (`feat:`, `fix:`, `refactor:`, etc.)

### Configuration
- ❌ NEVER: Use `process.env` directly in business logic
- ❌ NEVER: Environment-specific branching in application code
- ❌ NEVER: Default values for required secrets
- ✅ ALWAYS: Typed config object validated at startup
- ✅ ALWAYS: Fail fast on missing required configuration
```

**Add to** the Pre-Completion Checklist:

```markdown
- [ ] Async operations guarded against concurrent double-execution
- [ ] Tests verify behavior, not implementation; error paths tested
- [ ] Atomic commits with conventional messages
- [ ] Config accessed through typed object, validated at startup
```

---

## For copilot-instructions.md

**Add after** the "Workarounds" section:

```markdown
### Concurrency & Async Safety

- NEVER fire-and-forget async calls — every async call needs error handling
- NEVER allow unguarded concurrent execution of the same operation
- NEVER optimistically update UI without rollback on failure
- ALWAYS guard async operations against double-execution
- ALWAYS cancel stale requests when superseded (AbortController)
- ALWAYS ask: "What happens if this runs twice concurrently?"

### Testing

- NEVER test implementation details — test observable behavior
- NEVER mock the unit under test — mock only external boundaries
- NEVER use snapshot tests for business logic
- NEVER write tests without assertions or skip tests
- ALWAYS write test names that read like specifications
- ALWAYS test every error path
- ALWAYS keep tests independent (no shared mutable state)

### Git & Commit Hygiene

- NEVER mix refactoring with feature changes in one commit
- NEVER commit broken code or generated files
- ALWAYS make atomic commits — one logical change per commit
- ALWAYS use conventional commit messages (`feat:`, `fix:`, `refactor:`, etc.)

### Configuration Management

- NEVER use `process.env` directly in business logic
- NEVER use environment-specific branching in application code
- ALWAYS access config through a typed config object validated at startup
- ALWAYS fail fast on missing required configuration
```

**Add to** the Completion Checklist:

```markdown
12. Async operations guarded against concurrent double-execution
13. Tests verify behavior, not implementation; error paths tested
14. Atomic commits with conventional messages
15. Config accessed through typed object, validated at startup
```

---

## For VERIFICATION_CHECKLIST.md

**Add after** the "Stub Code & Modernization" section:

```markdown
## 🟡 Concurrency & Async Safety

- [ ] Async operations guarded against concurrent double-execution
- [ ] Stale requests cancelled when superseded (AbortController)
- [ ] No read-then-write race conditions on shared state
- [ ] Optimistic UI updates have rollback on failure
- [ ] No fire-and-forget async calls — all have error handling
- [ ] Parallel operations use controlled concurrency (not unbounded Promise.all)
- [ ] Component teardown cancels in-flight async operations

## 🟡 Testing

- [ ] Tests verify behavior, not implementation details
- [ ] Test names read like specifications (describe what's expected)
- [ ] Every error path has a corresponding test
- [ ] Tests are independent — no shared mutable state or execution order dependency
- [ ] Mocks only simulate external boundaries (network, FS, DB)
- [ ] No snapshot tests for business logic
- [ ] No tests without assertions
- [ ] No skipped or disabled tests (`test.skip` is a bug)

## 🟡 Git & Commit Hygiene

- [ ] Each commit represents one logical change
- [ ] Refactoring not mixed with feature changes
- [ ] Commit messages use conventional format (feat/fix/refactor/test/docs/chore)
- [ ] No generated files committed (build artifacts, compiled outputs)

## 🟡 Configuration

- [ ] All required environment variables validated at startup
- [ ] No `process.env` accessed directly in business logic
- [ ] Typed config object used throughout application
- [ ] No environment-specific branching in application code
- [ ] No default values for required secrets

## 🟡 API Contracts

- [ ] API response shapes documented and stable
- [ ] Input validation on all endpoints
- [ ] Error responses follow consistent structure
- [ ] Breaking changes explicitly versioned
```

**Add to** the Quick Commands section:

```bash
# Check for fire-and-forget async calls (missing await)
grep -rn "^\s*[a-zA-Z].*\.then(" --include="*.ts" --include="*.tsx" | grep -v "await\|return"

# Check for unguarded concurrent async operations
grep -rn "async function\|async (" --include="*.ts" --include="*.tsx" \
  | grep -v "test\|spec\|mock" | head -20
# Review each: can it be called twice concurrently?

# Check for process.env in business logic (should be in config only)
grep -rn "process\.env\." --include="*.ts" --include="*.tsx" \
  | grep -v "config\|Config\|\.config\.\|env\.d\.ts\|test\|spec"

# Check for environment-specific branching
grep -rn "NODE_ENV\|process\.env\.NODE_ENV" --include="*.ts" --include="*.tsx" \
  | grep -v "config\|Config\|\.config\."

# Check for tests without assertions
grep -rn "test(\|it(" --include="*.test.*" --include="*.spec.*" -l \
  | xargs grep -L "expect\|assert"

# Check for skipped tests
grep -rn "test\.skip\|it\.skip\|xit(\|xdescribe(" --include="*.test.*" --include="*.spec.*"
```

---

## For claude-project-prompt.md

**Add to** the Skills Available section:

```markdown
- **SKILL-testing.md** — Test behavior not implementation, test organization, error path coverage
- **SKILL-concurrency.md** — Race conditions, async safety, cancellation, state integrity
```

**Add to** the Verification Checklist:

```markdown
- [ ] **Concurrency**: No unguarded concurrent calls, stale requests cancelled
- [ ] **Testing**: Error paths tested, tests verify behavior not implementation
- [ ] **Git**: Atomic commits, conventional messages, no mixed concerns
- [ ] **Config**: Environment variables accessed through typed config object
```

---

## For SKILL-production-readiness.md

**Add** two new sections to your production-readiness checklist:

```markdown
---

## Concurrency & Async Safety

- [ ] Async operations guarded against concurrent double-execution
- [ ] Stale requests cancelled when superseded
- [ ] No read-then-write race conditions on shared state
- [ ] Optimistic UI updates have rollback on failure
- [ ] No fire-and-forget async calls
- [ ] Controlled concurrency for batch operations
- [ ] Component teardown cancels in-flight operations

---

## Configuration

- [ ] All required environment variables validated at startup
- [ ] Typed config object used throughout (no direct `process.env` in business logic)
- [ ] No environment-specific branching in application code
- [ ] No default values for required secrets
- [ ] Sensitive config not logged
```

If your checklist tracks a section count, increment it to include these two new sections.
