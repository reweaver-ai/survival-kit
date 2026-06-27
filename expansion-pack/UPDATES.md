# Vibe Coding Standards — Expansion Pack

**Version**: 1.2  
**Released**: June 2026  
**Requires**: Vibe Coding Standards v1.0

---

## v1.2 — Deeper Coverage (June 2026)

v1.0 covered the most common failure patterns in AI-generated code. As we kept reviewing
AI-written code in the wild, several patterns that v1.0 mentioned only briefly turned out to
deserve full treatment. This release **closes that gap** with three new drop-in deep skills.

### New Skill Files (drop-in, no changes to existing files required)

| File | What It Covers | Was in base kit as… |
|------|---------------|---------------------|
| `SKILL-data-truth.md` | Never hardcode/fake what should come from real data; constants-as-fallbacks are still hardcoded data; render nothing (not a guess) when data is missing; the one legitimate default vs. the masking default | a scattered one-liner ("Truth in data") |
| `SKILL-maintainability.md` | File/function/nesting size limits; `{Purpose}{Domain}{Type}` naming; complexity & fallback-chain limits (max 2–3 strategies, fail loud); no stub code; delete dead code immediately; documentation that stays true (no invented dates/versions/references) | brief notes inside `SKILL-architecture.md` |
| `SKILL-migration.md` | Incremental (never big-bang) legacy migration; bottom-up priority order; add real types/validation/tests every step; forbidden pitfalls (rename-only, `any`, `@ts-ignore`, dropping tests); start-permissive-then-tighten | a passing mention in `SKILL-type-safety.md` |

### Installation

Drop the three `SKILL-*.md` files alongside your existing skills — same format, no config.
If you use `claude-project-prompt.md`, add these to the Skills Available section:

```
- **SKILL-data-truth.md** — Never fabricate data; fail or render nothing when real data is missing
- **SKILL-maintainability.md** — Size/naming/complexity limits, no stub or dead code, truthful docs
- **SKILL-migration.md** — Incremental legacy migration that adds real types and tests each step
```

> Note: themes already covered by v1.1 (testing, concurrency, dependency boundaries, git
> hygiene, structured logging, configuration management) are **not** duplicated here.

---

## v1.1 — What's New

This expansion adds **four areas** that v1.0 didn't cover — patterns we kept encountering in AI-generated code after the original rules were in place.

### New Skill Files (drop-in, no changes to existing files required)

| File | What It Covers |
|------|---------------|
| `SKILL-testing.md` | Testing behavior not implementation, test naming as specs, error path coverage, forbidden patterns (snapshot overuse, mocking the unit under test, test interdependence), test factories and organization |
| `SKILL-concurrency.md` | Race conditions, unguarded concurrent execution, stale closure state, optimistic UI without rollback, controlled parallel execution, AbortController patterns, async mutex, state machines for workflows |

### Integration Patches (additions to your existing v1.0 files)

| Patch File | What It Adds To |
|------------|----------------|
| `PATCH-architecture.md` | Add to `SKILL-architecture.md`: Third-party dependency boundary rules + Git & commit hygiene |
| `PATCH-error-handling.md` | Add to `SKILL-error-handling.md`: Structured logging beyond errors (log levels, structured format, correlation IDs, sensitive data rules) |
| `PATCH-security.md` | Add to `SKILL-security.md`: Environment & configuration management (typed config objects, startup validation, no `process.env` in business logic) |
| `PATCH-rules-additions.md` | Concurrency, testing, git, config, and API contract rules to add to your SYSTEM_PROMPT, CLAUDE.md, cursor-rules, copilot-instructions, claude-project-prompt, and VERIFICATION_CHECKLIST |

---

## Installation

### Step 1: Drop in the new skills

Copy `SKILL-testing.md` and `SKILL-concurrency.md` alongside your existing skill files. No configuration needed — they follow the same format as your v1.0 skills.

### Step 2: Apply the patches

Each `PATCH-*.md` file contains clearly marked content blocks with instructions on where to insert them in your existing files. Copy-paste the blocks into the indicated locations.

### Step 3: Update your skills references

If you use `claude-project-prompt.md`, add these to the Skills Available section:

```
- **SKILL-testing.md** — Test behavior not implementation, test organization, error path coverage
- **SKILL-concurrency.md** — Race conditions, async safety, cancellation, state integrity
```

---

## Why These Four Areas

**Testing**: AI agents are notoriously bad at writing meaningful tests. They generate tests that assert implementation details (spy on internal methods, check call counts) rather than behavior. When you refactor, every test breaks — but not because anything is wrong. The testing skill gives agents a concrete framework for writing tests that actually catch bugs.

**Concurrency**: AI-generated async code almost never considers "what if this runs twice?" Double-click a save button, type while a search is in-flight, unmount a component during a fetch — these are the bugs that make it to production because the code works perfectly in the happy path.

**Dependency boundaries**: AI agents scatter direct library calls (axios, specific ORMs, logging frameworks) across dozens of files. When you need to swap one, you're editing everywhere. The dependency boundary rules teach agents to wrap external dependencies behind internal interfaces — one file to change, not fifty.

**Configuration management**: `if (process.env.NODE_ENV === 'production')` scattered through business logic is a pattern AI loves to generate. It makes code environment-dependent, untestable, and prone to subtle bugs. The config rules enforce typed config objects validated once at startup.
