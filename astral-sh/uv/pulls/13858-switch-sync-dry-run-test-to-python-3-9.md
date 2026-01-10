```yaml
number: 13858
title: "Switch `sync_dry_run` test to Python 3.9"
type: pull_request
state: merged
author: mgorny
labels:
  - internal
assignees: []
merged: true
base: main
head: py38-dryrun
created_at: 2025-06-05T10:55:41Z
updated_at: 2025-06-05T13:15:00Z
url: https://github.com/astral-sh/uv/pull/13858
synced_at: 2026-01-10T11:10:42Z
```

# Switch `sync_dry_run` test to Python 3.9

---

_Pull request opened by @mgorny on 2025-06-05 10:55_

## Summary

Switch the `sync_dry_run` test to use Python 3.9 instead of Python 3.8. I didn't previously catch it since it was marked as `python_managed`.

## Test Plan

`cargo test --no-default-features --features git,pypi,python` without Python 3.8 installed.

---

_@konstin approved on 2025-06-05 12:38_

---

_Review requested from @zanieb by @konstin on 2025-06-05 12:38_

---

_Label `internal` added by @konstin on 2025-06-05 12:42_

---

_Merged by @zanieb on 2025-06-05 13:15_

---

_Closed by @zanieb on 2025-06-05 13:15_

---
