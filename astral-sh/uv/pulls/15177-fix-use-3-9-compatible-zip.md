```yaml
number: 15177
title: "fix: Use 3.9 compatible zip"
type: pull_request
state: merged
author: anticorrelator
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2025-08-08T22:22:46Z
updated_at: 2025-08-08T23:45:14Z
url: https://github.com/astral-sh/uv/pull/15177
synced_at: 2026-01-12T16:11:37Z
```

# fix: Use 3.9 compatible zip

---

_@anticorrelator_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Uses a <3.10-compatible version of `zip` since the `strict` argument was [added in 3.10](https://docs.python.org/3.10/library/functions.html#zip)

## Test Plan

I executed the `_matching_parents` function in a local 3.9 environment 


---

_Comment by @zanieb on 2025-08-08 23:37_

Hey Dustin! Hope you're doing well <3

Thanks for the patch.

---

_@zanieb approved on 2025-08-08 23:43_

---

_Merged by @zanieb on 2025-08-08 23:45_

---

_Closed by @zanieb on 2025-08-08 23:45_

---
