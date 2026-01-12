```yaml
number: 12851
title: Collapse whitespace in python_list tests
type: pull_request
state: merged
author: mgorny
labels:
  - testing
assignees: []
merged: true
base: main
head: list-ws
created_at: 2025-04-12T11:22:58Z
updated_at: 2025-04-14T14:56:04Z
url: https://github.com/astral-sh/uv/pull/12851
synced_at: 2026-01-12T16:10:25Z
```

# Collapse whitespace in python_list tests

---

_@mgorny_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Collapse whitespace into a single space in python_list tests, in order to make them agnostic of padding, and therefore pass both with Python 3.12.9 and Python 3.12.10.

Fixes #12799

## Test Plan
    cargo test --features python --profile=fast-build --no-default-features



---

_@charliermarsh approved on 2025-04-14 12:23_

---

_Merged by @charliermarsh on 2025-04-14 12:23_

---

_Closed by @charliermarsh on 2025-04-14 12:23_

---

_Label `testing` added by @charliermarsh on 2025-04-14 12:23_

---

_Comment by @charliermarsh on 2025-04-14 12:23_

Thank you! Sorry about that.

---

_Branch deleted on 2025-04-14 14:55_

---

_Comment by @mgorny on 2025-04-14 14:56_

Thanks! 

---
