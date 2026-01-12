```yaml
number: 19295
title: "[`pyupgrade`] Make example error out-of-the-box (`UP046`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-3
created_at: 2025-07-11T22:53:31Z
updated_at: 2025-07-12T16:26:08Z
url: https://github.com/astral-sh/ruff/pull/19295
synced_at: 2026-01-12T15:56:36Z
```

# [`pyupgrade`] Make example error out-of-the-box (`UP046`)

---

_@MeGaGiGaGon_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #18972

This PR makes [non-pep695-generic-class (UP046)](https://docs.astral.sh/ruff/rules/non-pep695-generic-class/#non-pep695-generic-class-up046)'s example error out-of-the-box.

[Old example](https://play.ruff.rs/9d51ffc3-6be5-4646-9f6a-4dd57bae7fcb)
```py
from typing import TypeVar

T = TypeVar("T")


class GenericClass(Generic[T]):
    var: T
```

[New example](https://play.ruff.rs/927bd849-4b61-43d0-b121-e51a4d7c40b8)
```py
from typing import Generic, TypeVar

T = TypeVar("T")


class GenericClass(Generic[T]):
    var: T
```

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-07-11 23:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2025-07-12 13:54_

---

_Label `documentation` added by @AlexWaygood on 2025-07-12 13:54_

---

_Merged by @AlexWaygood on 2025-07-12 13:54_

---

_Closed by @AlexWaygood on 2025-07-12 13:54_

---

_Branch deleted on 2025-07-12 16:26_

---
