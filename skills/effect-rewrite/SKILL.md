---
name: effect-rewrite
description: Effect-rewrite-pass over existing code — simplifying and improving the structure and readability of functions.
disable-model-invocation: true
---

Can we rewrite the way these functions are laid-out etc to be more aesthetically pleasing?

- Use idiomatic `effect` patterns and functions from effect when relevant.
- Remove pointless hoisting of methods to the root scope if they're only used once.
- Simplify implementation and verbosity where it makes sense.
- Add proper spacing or grouping of related methods.
- improve conciseness and consistency
- fix logical bugs
- eliminate violations of the Rule of Three: deduplicating before the third clear repetition appears.
- eliminate premature DRY - eg: defining explicit variables for simple constants that are referred to less than 3 times. 
- eliminate Over-abstraction: adding indirection that costs readability more than it saves duplication.
- eliminate defensive assumptions in code.
- add clarity to existing code with comments ( only when other improvement opportunities are not available )