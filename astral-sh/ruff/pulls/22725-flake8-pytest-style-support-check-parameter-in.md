```yaml
number: 22725
title: "[`flake8-pytest-style`] Support `check` parameter in `PT011`"
type: pull_request
state: open
author: harupy
labels: []
assignees: []
base: main
head: pt011-support-check-parameter
created_at: 2026-01-19T15:07:10Z
updated_at: 2026-01-19T15:08:15Z
url: https://github.com/astral-sh/ruff/pull/22725
synced_at: 2026-01-19T15:37:48Z
```

# [`flake8-pytest-style`] Support `check` parameter in `PT011`

---

_@harupy_

## Summary

PT011 (`pytest-raises-too-broad`) now recognizes the `check` parameter introduced in pytest 8.4.0.

Closes #22673

## Test plan

Added test cases for `check` parameter (valid callback and `check=None`).


---
