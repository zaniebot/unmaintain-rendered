```yaml
number: 1118
title: "Re-add Cargo's known hosts checking"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/known
created_at: 2024-01-26T03:17:36Z
updated_at: 2024-01-26T03:29:37Z
url: https://github.com/astral-sh/uv/pull/1118
synced_at: 2026-01-10T15:39:03Z
```

# Re-add Cargo's known hosts checking

---

_Pull request opened by @charliermarsh on 2024-01-26 03:17_

## Summary

This ensures that (like Cargo) we don't suffer from https://github.com/advisories/GHSA-r5w3-xm58-jv6j, by way of checking known hosts when fetching via `libgit2`.

The implementation is taken from Cargo itself, modified to remove all configuration, since we don't yet support configuration for known hosts, etc.

Closes #285.

---

_Merged by @charliermarsh on 2024-01-26 03:29_

---

_Closed by @charliermarsh on 2024-01-26 03:29_

---

_Branch deleted on 2024-01-26 03:29_

---
