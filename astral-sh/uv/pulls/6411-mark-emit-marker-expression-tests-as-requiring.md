```yaml
number: 6411
title: "Mark emit_marker_expression* tests as requiring python-patch"
type: pull_request
state: merged
author: mgorny
labels:
  - internal
assignees: []
merged: true
base: main
head: expr-patch
created_at: 2024-08-22T05:36:24Z
updated_at: 2024-08-22T08:50:35Z
url: https://github.com/astral-sh/uv/pull/6411
synced_at: 2026-01-12T16:07:21Z
```

# Mark emit_marker_expression* tests as requiring python-patch

---

_@mgorny_

## Summary

Mark the new tests requiring Python 3.12.1 specifically as requiring python-patch feature.  This makes the test suite pass again on systems not having this specific version (and disabling the feature).

## Test Plan

`cargo test` on Gentoo :-).

---

_@konstin approved on 2024-08-22 08:37_

---

_Merged by @konstin on 2024-08-22 08:37_

---

_Closed by @konstin on 2024-08-22 08:37_

---

_Label `internal` added by @konstin on 2024-08-22 08:37_

---

_Branch deleted on 2024-08-22 08:50_

---

_Comment by @mgorny on 2024-08-22 08:50_

Thank you!

---
