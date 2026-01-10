```yaml
number: 10532
title: "Fix `PT014` autofix for last item in list"
type: pull_request
state: merged
author: augustelalande
labels:
  - bug
assignees: []
merged: true
base: main
head: pt014-fix-bug
created_at: 2024-03-23T04:27:49Z
updated_at: 2024-03-23T15:03:10Z
url: https://github.com/astral-sh/ruff/pull/10532
synced_at: 2026-01-10T22:47:02Z
```

# Fix `PT014` autofix for last item in list

---

_Pull request opened by @augustelalande on 2024-03-23 04:27_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This error was found browsing https://github.com/qarmin/Automated-Fuzzer/actions/runs/8396966850. Which failed when trying to autofix the PT014 violation in the following code:
```python
@pytest.mark.parametrize('data, spec', [(1.0, 1.0), (1.0, 1.0)])
def test_numbers(data, spec):
    ...
```

Investigation revealed that the implementation was not properly tested, when the duplicate value was also the last in the list. In particular the following function, which is in charge of finding the comma following an element to create the suggested fix,
https://github.com/astral-sh/ruff/blob/0a99bd84ce90564b10dbb7c3cf51c70922f4daed/crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs#L647-L651
would find the next comma even if it was outside the list itself leading to a lot of code being deleted.

This PR fixes that.

## Test Plan

Added misbehaving code to the test fixture.


---

_Comment by @github-actions[bot] on 2024-03-23 04:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh approved on 2024-03-23 13:26_

---

_Label `bug` added by @charliermarsh on 2024-03-23 13:26_

---

_Renamed from "Fix `PT014` invalid fix" to "Fix `PT014` autofix for last item in list" by @charliermarsh on 2024-03-23 13:26_

---

_Merged by @charliermarsh on 2024-03-23 13:26_

---

_Closed by @charliermarsh on 2024-03-23 13:26_

---

_Branch deleted on 2024-03-23 15:03_

---
