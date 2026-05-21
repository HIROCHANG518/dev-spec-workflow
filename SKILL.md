---
name: dev-spec-workflow
description: Codespec — enforces disciplined software development workflow integrating Software Company multi-role collaboration, SuperPowers TDD/debugging/branch management, Karpathy behavioral guidelines, and Matt Pocock ADR/shared-language/architecture-review. Use this skill when building any feature, fixing bugs, refactoring, or writing production code. Triggers on coding tasks that require TDD, architecture design, debugging, or code review. Do NOT use for throwaway prototypes, README edits, or trivial non-code changes.
agent_created: true
---

# Codespec — 软件开发工作流约束

## Overview

This skill enforces a rigorous, four-source development discipline. It should be loaded at the start of any coding session and treated as mandatory — not suggestions. The full specification lives in `references/DEV_SPEC.md`.

**Four integrated sources:**

| Source | Provides |
|--------|----------|
| Software Company (software-company) | Multi-role collaboration: PM → Architect → Engineer → QA |
| SuperPowers | TDD iron law, systematic debugging 4-step, branch finishing |
| Karpathy Guidelines | Think first, simplicity first, surgical changes, goal-driven execution |
| Matt Pocock | CONTEXT.md glossary, ADR decisions, periodic architecture review |

## When to Use

Load this skill whenever the task involves:

- Building a new feature or application
- Fixing a bug (apply systematic debugging)
- Refactoring or modifying existing code
- Designing system architecture
- Writing production-quality code

Skip for: throwaway prototypes, README edits, simple configuration changes, or purely conversational tasks.

## Core Workflow (5 Phases)

```
Phase 1: Requirements → Phase 2: Architecture → Phase 3: TDD Implementation → Phase 4: QA Verification → Phase 5: Branch Finish
```

### Phase 1 — Requirements (Product Manager)
- Write PRD with user stories, P0/P1/P2 priorities, UI mockups, and open questions
- GATE: all open questions resolved before proceeding

### Phase 2 — Architecture (Architect)
- Output: approach + file list + class diagrams + sequence diagrams + task list (ordered, with dependencies) + dependency list + shared conventions
- GATE: all pending clarifications resolved

### Phase 3 — TDD Implementation (Engineer)

**Iron Law: NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST.**

Apply Karpathy behavioral constraints during coding:

1. **Think Before Coding** — State assumptions explicitly. If uncertain, stop and ask. Present multiple interpretations and tradeoffs. Don't silently guess.
2. **Simplicity First** — Minimum code that solves the problem. No unrequested features, no speculative abstractions, no error handling for impossible scenarios. If 200 lines could be 50, rewrite.
3. **Surgical Changes** — Touch only what you must. Don't "improve" adjacent code. Don't refactor unbroken things. Match existing style. Every changed line must trace to the user's request.
4. **Goal-Driven Execution** — Transform commands into verifiable goals with explicit verify points. "Add validation" → "Write tests for invalid inputs, make them pass." "Fix bug" → "Write reproduction test, make it pass."

**Red-Green-Refactor cycle per task:**
1. 🔴 RED — Write minimal failing test
2. ✅ Verify RED — Confirm failure is correct (missing feature, not syntax error)
3. 🟢 GREEN — Write minimal code to pass
4. ✅ Verify GREEN — Full test suite passes
5. 🔵 REFACTOR — Remove duplication, improve names (keep green)

**Global consistency review** after all files complete → IS_PASS: YES/NO (max 2 retries).

### Phase 4 — QA Verification (QA Engineer)
- Write tests for core modules
- **Smart routing**: source bug → back to engineer / test bug → fix yourself / all pass → done
- Max 2 test rounds

### Phase 5 — Branch Finish
- Confirm all tests pass
- Choose: merge locally / push+PR / keep / discard

## Systematic Debugging

**Hard gate: No fix code until root cause is identified.**

**Standard flow (4-step):** Root cause → Pattern analysis → Hypothesis + verification → Fix + regression

**Hard bug upgrade (6-phase Matt Pocock /diagnose):**
1. Build feedback loop (fast, deterministic pass/fail signal — this is the skill)
2. Reproduce
3. Hypothesise (3-5 ranked, each falsifiable)
4. Instrument (one variable at a time, tagged debug logs)
5. Fix + regression test (write test before fix)
6. Cleanup + post-mortem

Trigger the 6-phase upgrade when: hard to reproduce, performance regression, cross-module coupling, or standard 4-step doesn't find the root cause.

## Architecture & Shared Language (Matt Pocock)

### CONTEXT.md — Project Glossary
- Location: project root `CONTEXT.md`
- Purpose: pure glossary, no implementation details, no specs
- Create lazily when first term is crystallized
- Update whenever fuzzy or overloaded terms appear

### ADR — Architecture Decision Records
- Location: `docs/adr/NNNN-short-name.md`
- Create only when: hard to reverse + surprising without context + genuine trade-off
- Format: Status → Background → Decision → Alternatives → Consequences

### Periodic Architecture Review
- Trigger: after completing a Phase or accumulating 10+ files
- Goal: deepen shallow modules (more behavior behind smaller interfaces)
- Use deletion test: would deleting this module concentrate complexity or just move it?

## Document Conventions

| Type | Path |
|------|------|
| Product plan | `PLAN_v4.md` (product-level only) |
| Glossary | `CONTEXT.md` |
| PRD | `docs/prd/YYYY-MM-DD-feature.md` |
| Architecture | `docs/arch/YYYY-MM-DD-feature-arch.md` |
| Implementation plan | `docs/plans/YYYY-MM-DD-feature-plan.md` |
| ADR | `docs/adr/NNNN-decision-name.md` |

**Rule:** Technical/engineering docs live outside the product plan document.

## Prohibited

- ❌ Writing code before a failing test (TDD iron law)
- ❌ Fixing bugs without root cause analysis
- ❌ Skipping commit hooks (`--no-verify`)
- ❌ Over-engineering — 200-line solutions for 50-line problems (Karpathy)
- ❌ "Improving" adjacent code during targeted fixes (Karpathy surgical)
- ❌ Silent guessing when uncertain — stop and ask (Karpathy think-first)
- ❌ Multi-step tasks without declared verify points (Karpathy goal-driven)
- ❌ Fuzzy terms without checking CONTEXT.md (Matt Pocock)
- ❌ Irreversible architecture decisions without ADR (Matt Pocock)
- ❌ Skipping global consistency review before delivery

## References

- Full specification: `references/DEV_SPEC.md` — contains all detailed constraints, examples, and anti-patterns
- Load `references/DEV_SPEC.md` when detailed workflow rules, debugging procedures, or Phase 0 special rules are needed
