```yaml
number: 13215
title: Improve detection of whether a symbol refers to a builtin exception
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - linter
assignees: []
merged: true
base: main
head: alex/stdlib-fns
created_at: 2024-09-02T15:14:06Z
updated_at: 2024-09-05T15:27:07Z
url: https://github.com/astral-sh/ruff/pull/13215
synced_at: 2026-01-10T21:38:32Z
```

# Improve detection of whether a symbol refers to a builtin exception

---

_Pull request opened by @AlexWaygood on 2024-09-02 15:14_

## Summary

This PR makes several minor improvements to functions in the `ruff_python_stdlib::builtins` module. It is a followup to https://github.com/astral-sh/ruff/pull/13172. The following changes are made:
- `is_ipython_builtin` becomes private, and `is_python_builtin` calls `is_ipython_builtin`. We don't have a use case for distinguishing between IPython builtins and regular builtins, and the way we currently have the checks split into two functions seems error-prone, as you could easily forget to call `is_ipython_builtin`, which would lead to incorrect results when checking a notebook
- The `is_exception` function was missing several builtin exceptions added in newer Python versions.
- I made the `is_exception` function take account of the Python version an exception was added in, similar to the change https://github.com/astral-sh/ruff/pull/13172 made to `is_python_builtin`.

## Test Plan

I added some new fixtures to the sole lint rule that uses `is_exception()`. Other than the changes to that function, the changes here aren't particularly user-facing.

---

_Label `linter` added by @AlexWaygood on 2024-09-02 15:14_

---

_Comment by @github-actions[bot] on 2024-09-02 15:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @codspeed-hq[bot] on 2024-09-02 16:00_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex/stdlib-fns)

### Merging #13215 will **not alter performance**

<sub>Comparing <code>alex/stdlib-fns</code> (ae8d6bd) with <code>main</code> (9d51706)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Review comment by @MichaReiser on `crates/ruff_python_stdlib/src/builtins.rs`:207 on 2024-09-03 06:00_

Nit: I would call the variable `is_stub`. The fact that it then must include the `ipython` builtins seems more like an implementation detail (the current variable tells it what it should do rather than what you want)

---

_@MichaReiser approved on 2024-09-03 06:00_

---

_@AlexWaygood reviewed on 2024-09-03 10:29_

---

_Review comment by @AlexWaygood on `crates/ruff_python_stdlib/src/builtins.rs`:207 on 2024-09-03 10:29_

Renamed it to `is_notebook`

---

_Merged by @AlexWaygood on 2024-09-03 10:33_

---

_Closed by @AlexWaygood on 2024-09-03 10:33_

---

_Branch deleted on 2024-09-03 10:33_

---

_Label `internal` added by @dhruvmanila on 2024-09-05 15:27_

---
