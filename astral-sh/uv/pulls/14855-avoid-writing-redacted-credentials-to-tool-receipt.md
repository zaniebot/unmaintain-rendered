```yaml
number: 14855
title: Avoid writing redacted credentials to tool receipt
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/tool-index
created_at: 2025-07-23T19:48:17Z
updated_at: 2025-07-23T20:01:12Z
url: https://github.com/astral-sh/uv/pull/14855
synced_at: 2026-01-12T16:11:27Z
```

# Avoid writing redacted credentials to tool receipt

---

_@charliermarsh_

## Summary

Right now, we write index URLs to the tool receipt with redacted credentials (i.e., a username, and `****` in lieu of a password). This is always wrong and unusable. Instead, this PR drops them entirely.

Part of https://github.com/astral-sh/uv/issues/14806.


---

_Label `bug` added by @charliermarsh on 2025-07-23 19:48_

---

_Marked ready for review by @charliermarsh on 2025-07-23 19:48_

---

_Merged by @charliermarsh on 2025-07-23 20:01_

---

_Closed by @charliermarsh on 2025-07-23 20:01_

---

_Branch deleted on 2025-07-23 20:01_

---
