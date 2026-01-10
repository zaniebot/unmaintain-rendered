```yaml
number: 16211
title: Fix pylock.toml config conflict error messages
type: pull_request
state: merged
author: ncoghlan
labels: []
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-10-09T17:13:45Z
updated_at: 2025-10-09T17:38:19Z
url: https://github.com/astral-sh/uv/pull/16211
synced_at: 2026-01-10T06:36:15Z
```

# Fix pylock.toml config conflict error messages

---

_Pull request opened by @ncoghlan on 2025-10-09 17:13_

## Summary

When specifying constraints or overrides in combination with `pylock.toml` as an input to `uv pip install`,
the error messages are not currently correct.

## Test Plan

No testing, it's a straightforward error string fix.


---

_@zanieb approved on 2025-10-09 17:23_

---

_Merged by @zanieb on 2025-10-09 17:38_

---

_Closed by @zanieb on 2025-10-09 17:38_

---
