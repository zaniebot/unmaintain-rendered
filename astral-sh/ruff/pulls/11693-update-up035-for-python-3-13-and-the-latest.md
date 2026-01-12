```yaml
number: 11693
title: Update UP035 for Python 3.13 and the latest version of typing_extensions
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
  - python313
assignees: []
merged: true
base: main
head: typing-extensions
created_at: 2024-06-02T14:18:22Z
updated_at: 2024-06-02T21:59:49Z
url: https://github.com/astral-sh/ruff/pull/11693
synced_at: 2026-01-12T15:55:38Z
```

# Update UP035 for Python 3.13 and the latest version of typing_extensions

---

_@AlexWaygood_

## Summary

Fixes #11413. This PR:
- Stops Ruff from suggesting to import the following things from `typing` on Python 3.12 and lower (`typing_extensions` backports new features from Python 3.13):
  - `Generator`
  - `AsyncGenerator`
  - `ContextManager`
  - `AsyncContextManager`
  - `TypedDict`
  - `runtime_checkable`
  - `Protocol`
- Adds rewrites for `TypeVar`, `ParamSpec` and `TypeVarTuple` on py313+
- Adds rewrites for the following features which are new in py313:
  - `warnings.deprecated`
  - `typing.TypeIs`
  - `typing.ReadOnly`
  - `typing.NoDefault`
  - `typing.is_protocol`
  - `typing.get_protocol_members`
  - `types.CapsuleType`

## Test Plan

Added new fixtures and tested using `cargo test` / `cargo insta review`. I also manually checked that the new rewrites are not offered if `--target-version=py312` is passed on the command line. (I considered adding explicit tests for this, but we already have similar tests, so it seemed somewhat duplicative to do so.)


---

_Label `rule` added by @AlexWaygood on 2024-06-02 14:18_

---

_Label `python313` added by @AlexWaygood on 2024-06-02 14:18_

---

_Comment by @github-actions[bot] on 2024-06-02 14:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-06-02 17:43_

---

_Merged by @AlexWaygood on 2024-06-02 21:59_

---

_Closed by @AlexWaygood on 2024-06-02 21:59_

---

_Branch deleted on 2024-06-02 21:59_

---
