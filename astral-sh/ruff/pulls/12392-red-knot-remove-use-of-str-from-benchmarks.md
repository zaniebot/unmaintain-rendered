```yaml
number: 12392
title: "[red-knot] Remove use of `str` from benchmarks"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
base: main
head: alex/redknot-benchmarks
created_at: 2024-07-18T20:20:05Z
updated_at: 2024-07-18T20:33:29Z
url: https://github.com/astral-sh/ruff/pull/12392
synced_at: 2026-01-12T15:55:41Z
```

# [red-knot] Remove use of `str` from benchmarks

---

_@AlexWaygood_

#12390 adds support for resolving types to classes in typeshed's `builtins.pyi` stub file. This causes redknot to crash when attempting to execute this benchmark, as the `str` definition in typeshed is too complex for us to handle right now. `object` is a simpler type definition which we can resolve symbols to without crashing.


---

_Label `red-knot` added by @AlexWaygood on 2024-07-18 20:20_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-07-18 20:20_

---

_Review requested from @carljm by @AlexWaygood on 2024-07-18 20:20_

---

_Comment by @carljm on 2024-07-18 20:28_

Oops -- can we go with https://github.com/astral-sh/ruff/pull/12392 instead since the next PR is already rebased on it? Sorry!

---

_Closed by @carljm on 2024-07-18 20:28_

---

_Branch deleted on 2024-07-18 20:28_

---

_Comment by @github-actions[bot] on 2024-07-18 20:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
