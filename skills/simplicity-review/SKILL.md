---
name: simplicity-review
description: Review implemented code specifically for unnecessary complexity and human comprehensibility. Use after implementing a non-trivial feature or refactor and before final code review.
---

# Simplicity Review

Review the implementation for unnecessary complexity.

The goal is NOT to propose a better architecture.

The goal is to determine what can be removed, flattened, renamed, or made
more explicit while preserving behaviour.

## Review

Look for:

- abstractions used only once
- interfaces with only one implementation
- factories that do not provide meaningful value
- wrappers that merely forward calls
- excessive dependency injection
- unnecessary generic types
- speculative extensibility
- managers/coordinators/orchestrators that hide straightforward logic
- unnecessary layers of indirection
- excessive file fragmentation
- terminology that is harder than the underlying concept
- abstractions created for hypothetical future requirements
- duplicated representations of the same concept

For each abstraction introduced by the change, ask:

1. What concrete current requirement requires this?
2. What is the simpler alternative?
3. What would we lose by removing it?

Prefer:

- fewer concepts
- fewer files
- fewer layers
- fewer transformations
- explicit control flow
- existing repository patterns
- domain terminology the team already understands

Do not propose a new abstraction to replace an unnecessary abstraction
unless clearly required.

## Output

Summarize:

1. What is already appropriately simple.
2. What complexity appears necessary.
3. What complexity appears unnecessary.
4. Concrete simplifications worth making.
5. Any terminology or documentation that should be rewritten in plainer language.