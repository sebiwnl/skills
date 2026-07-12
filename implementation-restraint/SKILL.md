---
name: implementation-restraint
description: Codex implementation discipline for existing codebases. Use whenever Codex is asked to implement, change, fix, or refactor production code, or add or alter tests. Keep the diff to the smallest justified behavior change; avoid redundant guards, speculative abstractions, and tests of implementation history. Do not use for pure explanation, planning, or mechanical generated-file updates explicitly requested by the user.
---

# Implementation Restraint

Implement the smallest complete change that preserves the codebase's existing design. This is not an instruction to avoid tests or error handling. Add them when they protect an observable contract or an untrusted boundary; do not add them merely because they are possible.

## Before Editing

Read the relevant code and trace direct callers to the nearest shared boundary. For non-trivial changes, state this compact change card before writing code:

```text
Outcome: [observable behavior that changes]
Boundary: [where invalid/optional/external state first enters, or "none"]
Smallest change: [one sentence]
Expected files: [short list]
Tests: [behavioral claim, or "none—existing coverage is sufficient"]
```

Do not infer a broader redesign from one local request. If the smallest correct change is unclear, inspect more of the call chain or ask one focused question.

## Keep Invariants at Their Boundary

1. Validate an input at the earliest common boundary that can receive invalid or external state.
2. Let downstream code rely on that validated invariant.
3. Repeat a check only when a different untrusted entry point exists, the value can change between checks, or a local recovery policy is genuinely different.

Do not add `nil` checks, default values, boolean fallbacks, or error swallowing solely to make an impossible internal state appear safe. Preserve the existing failure mode or return a contextual error when the caller can recover. A new defensive check must have a named source of invalidity: external input, optional data, a concurrent/state transition, a deserialization boundary, or a distinct caller that does not establish the invariant.

## Add Tests for Behavior, Not History

Add or change a test when it proves one of these:

- a new or changed observable contract;
- a real bug reproduced at a stable seam;
- a known edge case at an input boundary; or
- behavior that could regress without the test.

Do not add a test merely to prove that a private helper, branch, call, or deleted implementation no longer exists. If deleting code does not change observable behavior, normally remove or adjust obsolete tests and rely on the relevant existing coverage.

Test the behavior once at the lowest stable seam. Do not recreate the same assertion through every function in a call chain. Prefer a small table only when multiple distinct cases reveal different behavior; do not turn one case into test scaffolding.

## Spend Complexity Deliberately

Use existing types, control flow, and seams unless the task proves they are insufficient.

- Add an abstraction, interface, option, configuration path, or generic helper only for two concrete current consumers or variations.
- Add a new layer only when it creates a real ownership or boundary separation.
- Keep unrelated clean-ups out of the change. Name them separately if they are worth doing.
- Prefer a direct local expression over a helper whose only purpose is hiding a simple operation.
- Do not add comments that narrate obvious code. Comment a non-obvious invariant, compatibility rule, or external constraint.

## Review the Diff Before Finishing

Read the complete diff and answer silently:

1. Does every changed file serve the requested outcome?
2. Can any guard be removed because an earlier boundary already establishes the invariant?
3. Does every new test assert behavior a caller can observe rather than an implementation detail?
4. Did any new abstraction solve only one current use case?
5. Would a senior maintainer understand the change with fewer moving parts?

Remove unjustified code before running checks. Run the narrowest relevant checks first, then broader checks only when the change or repository instructions require them. Do not run rate-limited integration tests unless explicitly requested.

## Handoff

Report the behavior changed, the validation run, and any intentionally omitted complexity only when that omission is non-obvious. Do not pad the handoff with a diff recap.
