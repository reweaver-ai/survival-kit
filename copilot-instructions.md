# Vibe Coding Standards — GitHub Copilot Instructions
# Place this file at `.github/copilot-instructions.md` in your repository

## Philosophy

Write production-grade code following "Rock Solid, Predictable, Transparent" principles. No shortcuts, no workarounds, no hidden fixes.

## Critical Rules

### Error Handling: Fail Fast, Fail Loud

- NEVER use empty catch blocks or swallow errors silently
- NEVER use `continue` to skip errors in loops
- NEVER return `null`, `{}`, or `[]` when data is missing — throw an error
- NEVER use fallback values to mask missing data
- ALWAYS throw errors with clear, actionable messages including context
- ALWAYS surface errors to the user interface, not just console

```typescript
// ❌ BAD
const value = data.property || defaultValue;

// ✅ GOOD
if (!data.property) {
  throw new Error(`Missing required property. Available: ${Object.keys(data).join(', ')}`);
}
const value = data.property;
```

### Type Safety

- NEVER use `any` without a `// JUSTIFIED: <reason>` comment
- Prefer `unknown` with type guards over `any`
- Define interfaces for all data structures
- Use discriminated unions for state machines
- Enable strict mode in tsconfig.json

```typescript
// ❌ BAD
function process(data: any): any { return data.value; }

// ✅ GOOD
function process(data: unknown): ProcessedResult {
  if (!isValidInput(data)) {
    throw new Error(`Invalid input: expected ValidInput, got ${typeof data}`);
  }
  return data.value;
}
```

### Security

- NEVER use `eval()` or `new Function()`
- NEVER use `innerHTML` with untrusted data — use `textContent`
- NEVER concatenate strings into SQL queries — use parameterized queries
- NEVER hardcode secrets — use environment variables
- ALWAYS validate inputs at system boundaries
- ALWAYS use Content Security Policy compliant code

### Code Quality

- Keep files under 1500 lines (target 300-400)
- Keep functions under 100 lines (target 20-50)
- Maximum 3-4 levels of nesting depth
- No dead code: remove commented code, unused imports, stub functions
- Use single responsibility principle for all classes and functions
- Use dependency injection — depend on abstractions, not concretions

### Data Integrity

- NEVER hardcode design system values (colors, spacing, typography)
- NEVER use constants as fallbacks for missing parsed data
- If data doesn't exist, show an error state — never substitute fake data
- Every displayed value must trace back to a real data source

### Build Hygiene

- NEVER commit TypeScript compiled outputs (`.js`, `.js.map`, `.d.ts`) to source directories
- Use `tsc -p tsconfig.json` — never bare `tsc`
- Output to `dist/` or `build/`, never source directories
- Use `noEmit: true` for apps with bundlers (Vite, Webpack)

### Import Patterns

- Use package exports for cross-module imports
- Use relative imports only for same-module code
- NEVER use deep relative paths like `../../../packages/`

### Performance & Resources

- NEVER optimize without profiling evidence — measure first
- NEVER break correctness for performance
- ALWAYS clean up event listeners, intervals, and subscriptions on teardown
- Cache expensive operations — don't skip them

### Stub Code

- NEVER leave empty functions with TODO/placeholder comments
- NEVER write functions that return hardcoded fake data
- NEVER register handlers for unimplemented features
- Either implement fully or remove completely

### Modern Language Features

- Use const/let (never var), template literals, destructuring, async/await
- Use optional chaining (?.) and nullish coalescing (??)
- Use ES Modules (import/export), not CommonJS unless required by runtime

### Workarounds

- NEVER write defensive checks that skip invalid data — fix the source
- NEVER fix bugs in the wrong layer (parser bugs go in the parser)
- NEVER add "just in case" fallback code — fix the root cause

## Completion Checklist

Before suggesting code is complete:
1. All errors are thrown with context (not just logged)
2. No `any` without justification
3. No security violations (eval, innerHTML, hardcoded secrets)
4. No hardcoded design values
5. No dead code, stubs, or fake data
6. Files/functions within size limits
7. All imports use proper patterns
8. All resources cleaned up (listeners, intervals, subscriptions)
9. No premature optimizations (profiling evidence exists)
10. No workarounds — all fixes at root cause
11. Using modern language features (ES6+, ES Modules)
