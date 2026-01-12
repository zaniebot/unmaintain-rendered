```yaml
number: 3901
title: Initialize multi-progress state before individual bars
type: pull_request
state: merged
author: ibraheemdev
labels:
  - bug
assignees: []
merged: true
base: main
head: jupyter-progress
created_at: 2024-05-29T10:56:11Z
updated_at: 2024-05-29T13:23:44Z
url: https://github.com/astral-sh/uv/pull/3901
synced_at: 2026-01-12T16:05:55Z
```

# Initialize multi-progress state before individual bars

---

_@ibraheemdev_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves https://github.com/astral-sh/uv/issues/3896. Adding progress bars to the `MultiProgress` after configuring them seems to not synchronize the required state fully.

## Test Plan

The repeated output is gone when testing locally.


---

_Review requested from @charliermarsh by @ibraheemdev on 2024-05-29 10:56_

---

_@charliermarsh approved on 2024-05-29 13:23_

---

_Merged by @charliermarsh on 2024-05-29 13:23_

---

_Closed by @charliermarsh on 2024-05-29 13:23_

---

_Label `bug` added by @charliermarsh on 2024-05-29 13:23_

---
