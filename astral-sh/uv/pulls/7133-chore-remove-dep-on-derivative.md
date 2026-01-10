```yaml
number: 7133
title: "chore: Remove dep on derivative"
type: pull_request
state: merged
author: fasterthanlime
labels:
  - internal
assignees: []
merged: true
base: main
head: no-derivative
created_at: 2024-09-06T17:44:09Z
updated_at: 2024-09-06T22:18:51Z
url: https://github.com/astral-sh/uv/pull/7133
synced_at: 2026-01-10T12:53:41Z
```

# chore: Remove dep on derivative

---

_Pull request opened by @fasterthanlime on 2024-09-06 17:44_

(This is part of #5711)

## Summary

@BurntSushi and I spotted that the `derivative` crate is only used for one enum in the entire codebase — however, it's a proc macro, and we pay for the cost of (re)compiling it in many different contexts.

This replaces it with a private `Inner` core which uses the regular std derive macros — inlining and optimizations should make this equivalent to the other implementation, and not too hard to maintain hopefully (versus a manual impl of `PartialEq` and `Hash` which have to be kept in sync.)

## Test Plan

Trust CI?

---

_@zanieb approved on 2024-09-06 17:48_

---

_Label `internal` added by @zanieb on 2024-09-06 17:48_

---

_@BurntSushi approved on 2024-09-06 17:54_

Makes sense to me! Thank you!

---

_Merged by @charliermarsh on 2024-09-06 21:46_

---

_Closed by @charliermarsh on 2024-09-06 21:46_

---

_Comment by @fasterthanlime on 2024-09-06 22:18_

![CleanShot 2024-09-07 at 00 17 44@2x](https://github.com/user-attachments/assets/09917187-676c-4e72-b2d0-b87adc61c292)

Was the windows test suite broken by one of the "temp dir in dev dir" PRs we landed recently?

---
