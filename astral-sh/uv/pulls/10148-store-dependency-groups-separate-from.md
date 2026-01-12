```yaml
number: 10148
title: Store dependency groups separate from dependencies in lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/store-dep-groups
created_at: 2024-12-24T22:17:09Z
updated_at: 2024-12-24T22:32:31Z
url: https://github.com/astral-sh/uv/pull/10148
synced_at: 2026-01-12T16:09:09Z
```

# Store dependency groups separate from dependencies in lockfile

---

_@charliermarsh_

## Summary

This is necessary for some future improvements to non-`[project]` workspaces and PEP 723 scripts. It's not "breaking", but it will invalidate lockfiles for non-`[project]` workspaces. I think that's okay, since we consider those legacy right now, and they're really rare.

---

_Label `internal` added by @charliermarsh on 2024-12-24 22:17_

---

_Merged by @charliermarsh on 2024-12-24 22:32_

---

_Closed by @charliermarsh on 2024-12-24 22:32_

---

_Branch deleted on 2024-12-24 22:32_

---
