```yaml
number: 11164
title: "[red-knot] Unresolved imports lint rule"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: red-knot-lint-unresolved-imports
created_at: 2024-04-26T12:08:35Z
updated_at: 2024-04-28T10:16:57Z
url: https://github.com/astral-sh/ruff/pull/11164
synced_at: 2026-01-10T22:37:01Z
```

# [red-knot] Unresolved imports lint rule

---

_Pull request opened by @MichaReiser on 2024-04-26 12:08_

## Summary

This PR is based on https://github.com/astral-sh/ruff/pull/11157/ and https://github.com/astral-sh/ruff/pull/11148

It implements a very basic check for detecting unknown imports. It doesn't do anything fancy but it demonstrates the cross-module resolution infrastructure. 

The `lint_semantic` isn't yet correctly retriggered when a dependency changes. That's something we have to follow up with.

## Test Plan

I imported the `test` function from `match.py`. No lint error. I imported the non existing `test2` function from `match.py` and it shows a diagnostic. 


---

_Review comment by @carljm on `crates/red_knot/src/lint.rs`:81 on 2024-04-27 00:40_

What's the advantage of putting these directly in the context, rather than just giving the lint rule the db and letting it ask for what it needs?

---

_Review comment by @carljm on `crates/red_knot/src/lint.rs`:108 on 2024-04-27 00:45_

An `ImportFrom` with `Some(module)` is not necessarily a relative import. Only if `level > 0`. Otherwise it's just like `from foo.bar import baz`, which is an `ImportFrom`, but it's absolute, not relative.

---

_Review comment by @carljm on `crates/red_knot/src/lint.rs`:114 on 2024-04-27 00:47_

The case where `module` is `None` is definitely always a relative import (and must have `level > 0`).

---

_Review comment by @carljm on `crates/red_knot/src/lint.rs`:94 on 2024-04-27 00:50_

On a typical large real-world module, we might have literally thousands of definitions (every assignment statement), out of which tens are imports. Maybe we should index the import definitions separately? That could also unify `definitions` and `dependencies`.


---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:56 on 2024-04-27 00:54_

This change should go along with a TODO in this impl for proper synchronization. (I had put the TODO out on `eval_symbols` (now `infer_symbol_types`) since I hadn't changed its db ref to non-mut yet.)

---

_@carljm approved on 2024-04-27 00:56_

Looks great! Sorry about the rebase pain due to the changes I made to my PR before merge.

---

_Review comment by @MichaReiser on `crates/red_knot/src/lint.rs`:81 on 2024-04-27 09:12_

A large portion of lint rules need access to these struct. Resolving them once reduces the number of hash table lookups and arc clones that are needed.

I also don't want to give lint rules access to `db`. I think we instead want to expose some methods on context (e.g. I don't know if I want lint rules to read the source text of other files). 

---

_@MichaReiser reviewed on 2024-04-27 09:12_

---

_Review comment by @MichaReiser on `crates/red_knot/src/lint.rs`:94 on 2024-04-27 09:13_

That's a good point. Let's solve this when we implement the real rule ;) I thought I was already pretty clever by using definitions instead of iterating over the entire AST again 

---

_@MichaReiser reviewed on 2024-04-27 09:14_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:56 on 2024-04-27 14:44_

Never mind, there's already a TODO here that's sufficient.

---

_@carljm reviewed on 2024-04-27 14:44_

---

_Label `internal` added by @MichaReiser on 2024-04-27 17:30_

---

_Comment by @github-actions[bot] on 2024-04-27 17:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @MichaReiser on 2024-04-28 10:12_

---

_Closed by @MichaReiser on 2024-04-28 10:12_

---

_Branch deleted on 2024-04-28 10:12_

---
