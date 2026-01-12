```yaml
number: 4506
title: More precise locking with --prefix option
type: pull_request
state: merged
author: ericmarkmartin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: precise-prefix-locking
created_at: 2024-06-25T04:05:02Z
updated_at: 2024-06-25T10:47:52Z
url: https://github.com/astral-sh/uv/pull/4506
synced_at: 2026-01-12T16:06:16Z
```

# More precise locking with --prefix option

---

_@ericmarkmartin_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

In #4085, support was implemented for the `--prefix` option. When using this option, however, a lock is either acquired on the virtualenv or globally, preventing multiple installs to different `--prefix`s from the same interpreter.

In this change, acquire the lock on just the prefix in question.

## Test Plan

Ran a `uv pip install` with `--prefix` and `RUST_LOG=trace` and observed that the lock was acquired in the prefix.


---

_Label `enhancement` added by @zanieb on 2024-06-25 04:30_

---

_@zanieb approved on 2024-06-25 04:30_

Thanks for contributing! This seems reasonable to me.

---

_Review requested from @charliermarsh by @zanieb on 2024-06-25 04:30_

---

_Comment by @charliermarsh on 2024-06-25 10:47_

Thanks! Oversight on my part.

---

_Merged by @charliermarsh on 2024-06-25 10:47_

---

_Closed by @charliermarsh on 2024-06-25 10:47_

---
