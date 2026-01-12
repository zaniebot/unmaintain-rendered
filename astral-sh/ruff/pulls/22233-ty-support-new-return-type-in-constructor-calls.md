```yaml
number: 22233
title: "[ty] support __new__ return type in constructor calls"
type: pull_request
state: open
author: kitagry
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: fix-constructor-typevar-specialization
created_at: 2025-12-28T10:57:39Z
updated_at: 2025-12-29T09:16:19Z
url: https://github.com/astral-sh/ruff/pull/22233
synced_at: 2026-01-12T15:57:44Z
```

# [ty] support __new__ return type in constructor calls

---

_@kitagry_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

fix https://github.com/astral-sh/ty/issues/281

When a class defines a `__new__` method with a custom return type annotation, that return type is now used when calling the class constructor.

## Test Plan

Added test cases.

---

_Review requested from @carljm by @kitagry on 2025-12-28 10:57_

---

_Review requested from @AlexWaygood by @kitagry on 2025-12-28 10:57_

---

_Review requested from @sharkdp by @kitagry on 2025-12-28 10:57_

---

_Review requested from @dcreager by @kitagry on 2025-12-28 10:57_

---

_Label `ty` added by @AlexWaygood on 2025-12-28 11:25_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-28 11:25_

---

_Comment by @astral-sh-bot[bot] on 2025-12-28 11:26_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-28 11:38_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results

**Failing projects**:

| Project | Old Status | New Status | Old Return Code | New Return Code |
|---------|------------|------------|-----------------|------------------|
| `pandas` | success | abnormal exit | `1` | `-6` |

**Diagnostic changes:**

| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 351 | 7 | 46 |
| `invalid-argument-type` | 234 | 11 | 15 |
| `unsupported-operator` | 195 | 0 | 0 |
| `possibly-missing-attribute` | 135 | 0 | 5 |
| `not-iterable` | 104 | 0 | 0 |
| `not-subscriptable` | 73 | 0 | 1 |
| `type-assertion-failure` | 46 | 5 | 15 |
| `no-matching-overload` | 61 | 0 | 0 |
| `invalid-return-type` | 55 | 0 | 2 |
| `invalid-assignment` | 37 | 1 | 2 |
| `unused-ignore-comment` | 11 | 6 | 0 |
| `call-non-callable` | 6 | 0 | 0 |
| **Total** | **1,308** | **30** | **86** |


**[Full report with detailed diff](https://76f5236c.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://76f5236c.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @MichaReiser on 2025-12-29 09:16_

Thank you for working on this. It seems there are some compile errors and there's a new stack overflow on pandas.

However, it might make sense to first take a look at https://github.com/astral-sh/ruff/pull/22124 because it changes constructor inference significantly (and it might make sense to wait until that PR is merged). 

---
