```yaml
number: 8282
title: "Fix error leading to out-of-bound panic in `uv-pep508`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
assignees: []
merged: true
base: main
head: main
created_at: 2024-10-17T07:02:35Z
updated_at: 2024-10-17T12:23:06Z
url: https://github.com/astral-sh/uv/pull/8282
synced_at: 2026-01-12T16:08:15Z
```

# Fix error leading to out-of-bound panic in `uv-pep508`

---

_@InSyncWithFoo_

Resolves #8281.

## Summary

[`Cursor.slice()`](https://github.com/astral-sh/uv/blob/d930367f8c5728b5c51a2bbf95d91354046f6efa/crates/uv-pep508/src/cursor.rs#L39) expects a start index and a length, but it is instead [given a start index and an end index](https://github.com/astral-sh/uv/blob/d930367f8c5728b5c51a2bbf95d91354046f6efa/crates/uv-pep508/src/lib.rs#L936).

## Test plan

A new "leading whitespace" test is added to `tests.rs`.


---

_Merged by @charliermarsh on 2024-10-17 12:22_

---

_Closed by @charliermarsh on 2024-10-17 12:22_

---

_Comment by @charliermarsh on 2024-10-17 12:23_

Thank you!

---

_Label `bug` added by @charliermarsh on 2024-10-17 12:23_

---
