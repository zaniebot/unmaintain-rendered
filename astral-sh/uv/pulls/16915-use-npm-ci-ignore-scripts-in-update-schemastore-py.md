```yaml
number: 16915
title: "Use `npm ci --ignore-scripts` in update_schemastore.py"
type: pull_request
state: merged
author: woodruffw
labels:
  - internal
assignees: []
merged: true
base: main
head: ww/npm
created_at: 2025-12-01T21:25:56Z
updated_at: 2025-12-01T23:36:56Z
url: https://github.com/astral-sh/uv/pull/16915
synced_at: 2026-01-10T05:49:14Z
```

# Use `npm ci --ignore-scripts` in update_schemastore.py

---

_Pull request opened by @woodruffw on 2025-12-01 21:25_

## Summary

This ensures that we fully honor SchemaStore's lockfile when installing, plus that we don't run any build-time scripts (which shouldn't be needed here).

## Test Plan

I ran `uv run update_schemastore.py` locally and confirmed that it created a branch as expected.

---

_Review requested from @charliermarsh by @woodruffw on 2025-12-01 21:25_

---

_Review requested from @zanieb by @woodruffw on 2025-12-01 21:25_

---

_Assigned to @woodruffw by @woodruffw on 2025-12-01 21:25_

---

_Label `internal` added by @woodruffw on 2025-12-01 21:48_

---

_@zanieb approved on 2025-12-01 23:28_

---

_Merged by @woodruffw on 2025-12-01 23:36_

---

_Closed by @woodruffw on 2025-12-01 23:36_

---

_Branch deleted on 2025-12-01 23:36_

---
