```yaml
number: 1019
title: "Avoid canonicalizing paths in `requirements-txt`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/norm
created_at: 2024-01-19T21:09:30Z
updated_at: 2024-01-19T21:28:05Z
url: https://github.com/astral-sh/uv/pull/1019
synced_at: 2026-01-10T15:39:03Z
```

# Avoid canonicalizing paths in `requirements-txt`

---

_Pull request opened by @charliermarsh on 2024-01-19 21:09_

## Summary

When you specify an editable that doesn't exist, it should error, but not in the parser -- the error should be downstream.

---

_Label `internal` added by @charliermarsh on 2024-01-19 21:09_

---

_@charliermarsh reviewed on 2024-01-19 21:09_

---

_Review comment by @charliermarsh on `crates/puffin/tests/pip_compile.rs`:3496 on 2024-01-19 21:09_

This is worse, but I'm going to improve it in puffin (rather than in the requirements parser).

---

_Merged by @charliermarsh on 2024-01-19 21:28_

---

_Closed by @charliermarsh on 2024-01-19 21:28_

---

_Branch deleted on 2024-01-19 21:28_

---
