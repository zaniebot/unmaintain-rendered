```yaml
number: 14298
title: "Fix `pytest-raises-too-broad (PT011)` to flag `pytest.raises` call with keyword `expected_exception`"
type: pull_request
state: merged
author: harupy
labels:
  - bug
assignees: []
merged: true
base: main
head: PT011-expected_exception
created_at: 2024-11-12T13:33:40Z
updated_at: 2024-11-12T23:51:25Z
url: https://github.com/astral-sh/ruff/pull/14298
synced_at: 2026-01-10T20:50:57Z
```

# Fix `pytest-raises-too-broad (PT011)` to flag `pytest.raises` call with keyword `expected_exception`

---

_Pull request opened by @harupy on 2024-11-12 13:33_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

`pytest-raises-too-broad (PT011)` should be raised when `expected_exception` is provided as a keyword argument.

```python
def test_foo():
    with pytest.raises(ValueError):  # raises PT011
        raise ValueError("Can't divide 1 by 0")

    # This is minor but a valid pytest.raises call
    with pytest.raises(expected_exception=ValueError):  # doesn't raise PT011 but should
        raise ValueError("Can't divide 1 by 0")
```

`pytest.raises` doc: https://docs.pytest.org/en/8.3.x/reference/reference.html#pytest.raises

## Test Plan

<!-- How was it tested? -->

Unit tests

---

_Comment by @github-actions[bot] on 2024-11-12 13:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "Fix `pytest-raises-too-broad (PT011)` to cover `expected_exception`" to "Fix `pytest-raises-too-broad (PT011)` to flag `pytest.raises` with keyword `expected_exception`" by @harupy on 2024-11-12 14:38_

---

_Renamed from "Fix `pytest-raises-too-broad (PT011)` to flag `pytest.raises` with keyword `expected_exception`" to "Fix `pytest-raises-too-broad (PT011)` to flag `pytest.raises` call with keyword `expected_exception`" by @harupy on 2024-11-12 14:38_

---

_@charliermarsh approved on 2024-11-12 19:28_

Thanks!

---

_Label `bug` added by @charliermarsh on 2024-11-12 19:28_

---

_Merged by @charliermarsh on 2024-11-12 19:28_

---

_Closed by @charliermarsh on 2024-11-12 19:28_

---

_Branch deleted on 2024-11-12 23:51_

---
