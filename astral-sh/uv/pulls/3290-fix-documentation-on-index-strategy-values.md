```yaml
number: 3290
title: Fix documentation on --index-strategy values
type: pull_request
state: merged
author: sergeykolosov
labels:
  - documentation
assignees: []
merged: true
base: main
head: index-strategy-docs
created_at: 2024-04-27T19:12:02Z
updated_at: 2024-04-27T22:29:51Z
url: https://github.com/astral-sh/uv/pull/3290
synced_at: 2026-01-10T14:37:54Z
```

# Fix documentation on --index-strategy values

---

_Pull request opened by @sergeykolosov on 2024-04-27 19:12_

## Summary

Dropped the `--` prefix from the values of the `--index-strategy` option mistakenly added to documentation in #3138.

## Test Plan

Verified that actually accepted values of `--index-strategy` don't use a `--` prefix.

---

_@charliermarsh approved on 2024-04-27 20:44_

Thank you.

---

_Label `documentation` added by @charliermarsh on 2024-04-27 20:44_

---

_Merged by @charliermarsh on 2024-04-27 20:44_

---

_Closed by @charliermarsh on 2024-04-27 20:44_

---

_Branch deleted on 2024-04-27 22:29_

---
