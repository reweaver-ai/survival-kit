# Four more things...
You downloaded the Vibe Coding Survival Kit for keeping AI-generated code production-grade. Hopefully, the files we shared are working for you. Silent error swallowing, untyped parameters, hardcoded design values — the guardrails should catch these reliably. Please let us know if they worked for you (or if they didn’t). Feel free to email me directly (~[jonathan@reweaver.ai](mailto:jonathan@reweaver.ai?subject=Vibe%20Coding%20Standards%20Feedback)~).  **Here’s a few additional areas where we think we can help.**

**Concurrency blind spots.** AI-generated async code almost never asks “What if this runs twice?” Race conditions, stale closures, optimistic UI updates without rollback—these bugs work perfectly in the happy path and break under real user behavior.

**Tests that test nothing.** AI agents write tests that spy on internal methods, assert implementation details, and break on every refactor — without catching a single real bug. Coverage goes up, confidence stays at zero.

**Library lock-in by default.** AI scatters direct calls to third-party libraries across dozens of files. Need to swap Axios? Edit fifty files. The dependency boundary rules teach agents to wrap external libraries behind internal interfaces.

**Environment spaghetti.** if(process.env.NODE_ENV === ‘production’) scattered through business logic makes code environment-dependent, untestable, and prone to subtle bugs that only surface in staging or prod.
# Introducing the Expansion Pack
**What’s inside:** Seven files. Two new skill documents. Four integration patches for your existing rules. 

**NEW SKILLS — DROP IN**

| **NEW**
**SKILL-testing.md**
Test behavior not implementation. Test naming as specifications. Error path coverage. Forbidden patterns: snapshot overuse, mocking the unit under test, test interdependence. Test factories and organization. |
 
| **NEW**
**SKILL-concurrency.md**
The five-question concurrency check. Guarding against double-execution. Stale closure state. Optimistic UI with rollback. AbortController patterns. Async mutex. State machines for workflows. |
 
**INTEGRATION PATCHES**

| **PATCH**
**PATCH-architecture.md**
Third-party dependency boundaries + git & commit hygiene. Add to your existing SKILL-architecture.md. |
 
| **PATCH**
**PATCH-error-handling.md**
Structured logging: log levels, structured format, correlation IDs, sensitive data rules. Add to your existing SKILL-error-handling.md. |
 
| **PATCH**
**PATCH-security.md**
Environment & configuration management: typed config objects, startup validation, no process.env in business logic. Add to your existing SKILL-security.md. |
 
| **PATCH**
**PATCH-rules-additions.md**
Pre-formatted blocks for every tool config file (Cursor, Copilot, Claude, System Prompt) and the verification checklist. Copy-paste where indicated. |
 
The *Vibe Coding Survival Kit* addressed how AI writes code. This expansion addresses how AI-written code *behaves* — under concurrency, under test, under real configuration, and under the long-term maintenance burden of library coupling.
Same philosophy. Same format. Four fewer blind spots.

***— The ReWeaver AI team***
