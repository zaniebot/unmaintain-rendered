```yaml
number: 6457
title: "Avoid applying `PYI055` to runtime-evaluated annotations"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/PYI030
created_at: 2023-08-09T20:36:09Z
updated_at: 2023-08-09T20:48:44Z
url: https://github.com/astral-sh/ruff/pull/6457
synced_at: 2026-01-12T15:55:21Z
```

# Avoid applying `PYI055` to runtime-evaluated annotations

---

_@charliermarsh_

## Summary

The use of `|` as a union operator is not always safe, if a type annotation is evaluated in a runtime context. For example, this code errors at runtime:

```python
import httpretty
import requests_mock

item: type[requests_mock.Mocker | httpretty] = requests_mock.Mocker
```

However, it's fine in a `.pyi` file, with `__future__` annotations`, or if the annotation is in a non-evaluated context, like:

```python
def func():
    item: type[requests_mock.Mocker | httpretty] = requests_mock.Mocker
```

This PR modifies the rule to avoid enforcing in those invalid, runtime-evaluated contexts.

Closes https://github.com/astral-sh/ruff/issues/6455.


---

_Label `bug` added by @charliermarsh on 2023-08-09 20:36_

---

_Merged by @charliermarsh on 2023-08-09 20:46_

---

_Closed by @charliermarsh on 2023-08-09 20:46_

---

_Branch deleted on 2023-08-09 20:46_

---

_Comment by @github-actions[bot] on 2023-08-09 20:48_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
