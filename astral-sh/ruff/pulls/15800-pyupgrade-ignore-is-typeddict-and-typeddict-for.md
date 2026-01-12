```yaml
number: 15800
title: "[`pyupgrade`] Ignore `is_typeddict` and `TypedDict` for `deprecated-import` (`UP035`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels: []
assignees: []
merged: true
base: main
head: UP035
created_at: 2025-01-29T00:51:55Z
updated_at: 2025-01-29T18:27:47Z
url: https://github.com/astral-sh/ruff/pull/15800
synced_at: 2026-01-12T15:55:52Z
```

# [`pyupgrade`] Ignore `is_typeddict` and `TypedDict` for `deprecated-import` (`UP035`)

---

_@InSyncWithFoo_

## Summary

Resolves #15780.

`is_typeddict` and `TypedDict` are now listed as known exceptions to the rule. The former will presumably stay until 3.9 reaches its end-of-life circa this October.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-29 00:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-01-29 09:54_

---

_@AlexWaygood reviewed on 2025-01-29 11:09_

Thanks! Could you also move `"TypedDict"` out of the `TYPING_EXTENSIONS_TO_TYPING_313` list? Even on Python 3.13, `typing_extensions.TypedDict` is different to `typing.TypedDict` in the latest release of `typing_extensions`. (It provides an implementation of a version of PEP 728, which hasn't yet been accepted, and therefore hasn't yet been incorporated into `typing.TypedDict`)

---

_Renamed from "[`pyupgrade`] Ignore `is_typeddict` (`UP035`)" to "[`pyupgrade`] Ignore `is_typeddict` and `TypedDict` (`UP035`)" by @InSyncWithFoo on 2025-01-29 13:22_

---

_@AlexWaygood approved on 2025-01-29 18:05_

Thanks!

---

_Renamed from "[`pyupgrade`] Ignore `is_typeddict` and `TypedDict` (`UP035`)" to "[`pyupgrade`] Ignore `is_typeddict` and `TypedDict` for `deprecated-import` (`UP035`)" by @AlexWaygood on 2025-01-29 18:05_

---

_Merged by @AlexWaygood on 2025-01-29 18:05_

---

_Closed by @AlexWaygood on 2025-01-29 18:05_

---

_Branch deleted on 2025-01-29 18:27_

---
