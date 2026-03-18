# AI CODING SURVIVAL KIT : A GOVERNANCE FRAMEWORK FOR AI-ASSISTED DEVELOPMENT
Battle-tested strategies to keep humans in control of AI-generated code quality

**Version**: 1.0  
**Purpose**: A portable, tool-agnostic set of prompts, rules, and skills that help AI coding assistants (Claude, Cursor, Copilot, etc.) produce production-grade software — not just code that "works."

## The Problem

"Vibe coding" — telling an AI to build something and accepting whatever compiles — produces fragile, insecure, unmaintainable software. The code *looks* right. It *runs*. But it silently swallows errors, hardcodes values, skips validation, and accumulates technical debt faster than any human could.

## The Solution

This system distills 20+ battle-tested engineering guardrails (born from real production cleanup of thousands of lines of code) into **three layers** that any AI assistant can consume:

```
┌─────────────────────────────────────────────┐
│  Layer 1: SYSTEM PROMPT (always active)     │
│  Core philosophy + absolute prohibitions    │
├─────────────────────────────────────────────┤
│  Layer 2: RULES FILES (per-tool configs)    │
│  Cursor rules, Copilot instructions, etc.   │
├─────────────────────────────────────────────┤
│  Layer 3: SKILLS / REFERENCES (on demand)   │
│  Deep guidance for specific tasks           │
└─────────────────────────────────────────────┘
```

## Files in This Package

| File | What It Is | Where to Use It |
|------|-----------|-----------------|
| `SYSTEM_PROMPT.md` | The core prompt — universal philosophy + rules | Paste into any AI system prompt |
| `cursor-rules.md` | Cursor IDE `.cursorrules` file | `.cursorrules` in project root |
| `copilot-instructions.md` | GitHub Copilot instructions | `.github/copilot-instructions.md` |
| `claude-project-prompt.md` | Claude Projects system prompt | Claude.ai Project instructions |
| `SKILL-error-handling.md` | Deep skill: Error handling patterns | Claude skill / reference doc |
| `SKILL-type-safety.md` | Deep skill: TypeScript type safety | Claude skill / reference doc |
| `SKILL-security.md` | Deep skill: Security patterns | Claude skill / reference doc |
| `SKILL-architecture.md` | Deep skill: Code architecture | Claude skill / reference doc |
| `SKILL-production-readiness.md` | Deep skill: Production checklist | Claude skill / reference doc |
| `VERIFICATION_CHECKLIST.md` | Run-before-done checklist | Attach to any task completion |
| `CLAUDE.md` | Claude Code / CLAUDE.md format | Root of repo for Claude Code |
| `SKILL-workarounds.md` | Deep skill: Root-cause fixing | Claude skill / reference doc |
| `SKILL-performance.md` | Deep skill: Optimization & resources | Claude skill / reference doc |
| `SKILL-multi-agent.md` | Deep skill: Multi-agent file safety | Claude skill / reference doc |

## Quick Start

### For Claude Code
Copy `CLAUDE.md` → `CLAUDE.md` in your project root. Claude Code reads this automatically.

### For Cursor
Copy `cursor-rules.md` → `.cursorrules` in your project root.

### For GitHub Copilot
Copy `copilot-instructions.md` → `.github/copilot-instructions.md`.

### For Claude (Projects)
Paste `claude-project-prompt.md` into your Claude Project instructions.

### For Any AI Tool
Paste `SYSTEM_PROMPT.md` into the system prompt or context window.

## Philosophy

**Rock Solid. Predictable. Transparent.**

1. **Fail fast, fail loud, fail transparent** — No silent errors, no hidden fallbacks
2. **Fix root causes, not symptoms** — Zero tolerance for workarounds
3. **Comprehensive, not partial** — Fix ALL issues, not just "critical" ones
4. **Measure before optimizing** — Never sacrifice correctness for perceived performance
5. **Truth in data** — Never hardcode what should come from real data structures
6. **Security by default** — Every line of code is secure, not just the "security" code

## Origin

This kit was built from systematic analysis of vibe-coded repositories on GitHub — projects built primarily with AI coding assistants. The same failure patterns appeared across frameworks, languages, and team sizes: silent fallbacks masking errors, untyped parameters erasing type safety, compiled outputs committed to source, god-object files, and cascading error-masking chains. Every rule addresses a pattern observed repeatedly. None are theoretical.
