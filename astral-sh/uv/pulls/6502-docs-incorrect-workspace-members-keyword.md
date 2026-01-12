```yaml
number: 6502
title: "docs: Incorrect workspace members keyword"
type: pull_request
state: merged
author: aborgna-q
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-08-23T09:41:34Z
updated_at: 2024-08-23T10:59:45Z
url: https://github.com/astral-sh/uv/pull/6502
synced_at: 2026-01-12T16:07:23Z
```

# docs: Incorrect workspace members keyword

---

_@aborgna-q_

## Summary

Small keyword fix. In the `concepts/dependencies` documentation, the workspace example listed members under an invalid `tool.uv.workspace.include` field.

This PR changes the key to [`tool.uv.workspace.members`](https://docs.astral.sh/uv/reference/settings/#workspace_members) instead.

---

_@konstin approved on 2024-08-23 10:16_

---

_Label `documentation` added by @konstin on 2024-08-23 10:16_

---

_Merged by @konstin on 2024-08-23 10:16_

---

_Closed by @konstin on 2024-08-23 10:16_

---

_Branch deleted on 2024-08-23 10:59_

---
