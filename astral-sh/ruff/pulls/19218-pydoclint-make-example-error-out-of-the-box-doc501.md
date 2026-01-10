```yaml
number: 19218
title: "[`pydoclint`] Make example error out-of-the-box (`DOC501`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-07-08T21:27:02Z
updated_at: 2025-07-09T17:28:21Z
url: https://github.com/astral-sh/ruff/pull/19218
synced_at: 2026-01-10T18:33:12Z
```

# [`pydoclint`] Make example error out-of-the-box (`DOC501`)

---

_Pull request opened by @MeGaGiGaGon on 2025-07-08 21:27_

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

This PR makes [docstring-missing-exception (DOC501)](https://docs.astral.sh/ruff/rules/docstring-missing-exception/#docstring-missing-exception-doc501)'s example error out-of-the-box. Since the exceptions in the function body need to undergo name resolution to figure out if one of them is `NotImplementedError`, `DOC501` won't lint if the raised name is not defined. This could be considered a limitation, but should be fine since `F821` already covers undefined names. I did discover a different edge case, but it's not relevant to the example.

[Old example](https://play.ruff.rs/d213e87d-e5c7-49d8-a908-931f61f06055)
```py
def calculate_speed(distance: float, time: float) -> float:
    """Calculate speed as distance divided by time.

    Args:
        distance: Distance traveled.
        time: Time spent traveling.

    Returns:
        Speed as distance divided by time.
    """
    try:
        return distance / time
    except ZeroDivisionError as exc:
        raise FasterThanLightError from exc
```

[New example](https://play.ruff.rs/cb41e0b7-b950-4fa0-842d-cecab9c8e842)
```py
class FasterThanLightError(ArithmeticError): ...


def calculate_speed(distance: float, time: float) -> float:
    """Calculate speed as distance divided by time.

    Args:
        distance: Distance traveled.
        time: Time spent traveling.

    Returns:
        Speed as distance divided by time.
    """
    try:
        return distance / time
    except ZeroDivisionError as exc:
        raise FasterThanLightError from exc
```

The "Use instead" section was also updated similarly.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-07-08 21:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @ntBre on 2025-07-09 16:57_

---

_@ntBre approved on 2025-07-09 16:59_

---

_Merged by @ntBre on 2025-07-09 16:59_

---

_Closed by @ntBre on 2025-07-09 16:59_

---

_Branch deleted on 2025-07-09 17:28_

---
