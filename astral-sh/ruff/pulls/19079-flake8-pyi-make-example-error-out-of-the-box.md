```yaml
number: 19079
title: "[`flake8-pyi`] Make example error out-of-the-box (`PYI062`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-5
created_at: 2025-07-01T22:29:30Z
updated_at: 2025-07-02T15:44:39Z
url: https://github.com/astral-sh/ruff/pull/19079
synced_at: 2026-01-12T15:56:31Z
```

# [`flake8-pyi`] Make example error out-of-the-box (`PYI062`)

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

This PR makes [duplicate-literal-member (PYI062)](https://docs.astral.sh/ruff/rules/duplicate-literal-member/#duplicate-literal-member-pyi062)'s example error out-of-the-box

[Old example](https://play.ruff.rs/6b00b41c-c1c5-4421-873d-fc2a143e7337)
```py
foo: Literal["a", "b", "a"]
```

[New example](https://play.ruff.rs/1aea839b-9ae8-4848-bb83-2637e1a68ce4)
```py
from typing import Literal

foo: Literal["a", "b", "a"]
```

Imports were also added to the "use instead" section.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Review requested from @AlexWaygood by @MeGaGiGaGon on 2025-07-01 22:29_

---

_Comment by @github-actions[bot] on 2025-07-01 22:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2025-07-02 07:21_

---

_Label `documentation` added by @AlexWaygood on 2025-07-02 07:21_

---

_Merged by @AlexWaygood on 2025-07-02 07:21_

---

_Closed by @AlexWaygood on 2025-07-02 07:21_

---

_Branch deleted on 2025-07-02 15:44_

---
