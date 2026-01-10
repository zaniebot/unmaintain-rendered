```yaml
number: 13780
title: "Update `lock::lock_requires_python` not to require Python 3.8"
type: pull_request
state: merged
author: mgorny
labels: []
assignees: []
merged: true
base: main
head: py38-more
created_at: 2025-06-02T07:56:43Z
updated_at: 2025-06-04T14:53:30Z
url: https://github.com/astral-sh/uv/pull/13780
synced_at: 2026-01-10T11:10:42Z
```

# Update `lock::lock_requires_python` not to require Python 3.8

---

_Pull request opened by @mgorny on 2025-06-02 07:56_

## Summary

Update `lock::lock_requires_python` not to require Python 3.8.

Fixes #13676

## Test Plan

`cargo test --no-default-features --features git,pypi,python` after removing Python 3.8.


---

_@konstin approved on 2025-06-02 08:11_

---

_Merged by @konstin on 2025-06-02 09:10_

---

_Closed by @konstin on 2025-06-02 09:10_

---

_Comment by @zanieb on 2025-06-04 14:53_

Thanks again!

---
