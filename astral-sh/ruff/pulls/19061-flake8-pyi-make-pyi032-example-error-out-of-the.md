```yaml
number: 19061
title: "[`flake8-pyi`] Make `PYI032` example error out-of-the-box"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-5
created_at: 2025-07-01T00:55:21Z
updated_at: 2025-07-01T07:25:03Z
url: https://github.com/astral-sh/ruff/pull/19061
synced_at: 2026-01-10T18:39:09Z
```

# [`flake8-pyi`] Make `PYI032` example error out-of-the-box

---

_Pull request opened by @MeGaGiGaGon on 2025-07-01 00:55_

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

This PR makes [datetime-min-max (DTZ901)](https://docs.astral.sh/ruff/rules/datetime-min-max/#datetime-min-max-dtz901)'s example error out-of-the-box

[Old example](https://play.ruff.rs/7e825d76-8a68-47fb-97ba-3da08fd46d02)
```py
class Foo:
    def __eq__(self, obj: typing.Any) -> bool: ...
```

[New example](https://play.ruff.rs/1353cb24-efa9-43f6-8bfe-d27007e55fbd)
```py
from typing import Any


class Foo:
    def __eq__(self, obj: Any) -> bool: ...
```

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Review requested from @AlexWaygood by @MeGaGiGaGon on 2025-07-01 00:55_

---

_Comment by @github-actions[bot] on 2025-07-01 01:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2025-07-01 06:50_

---

_Label `documentation` added by @AlexWaygood on 2025-07-01 06:50_

---

_Merged by @AlexWaygood on 2025-07-01 06:50_

---

_Closed by @AlexWaygood on 2025-07-01 06:50_

---

_Branch deleted on 2025-07-01 07:25_

---
