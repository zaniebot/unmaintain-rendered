```yaml
number: 1177
title: "Deduplicate `pep440_rs` in dependency tree"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/r
created_at: 2024-01-29T21:04:21Z
updated_at: 2024-01-29T21:11:43Z
url: https://github.com/astral-sh/uv/pull/1177
synced_at: 2026-01-12T16:04:29Z
```

# Deduplicate `pep440_rs` in dependency tree

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/puffin/issues/1176.

## Test Plan

`cargo tree -p puffin -i pep440_rs` runs without error. Previously, it errored due to multiple versions.

---

_Renamed from "Deduplicate pep440_rs in dependency tree" to "Deduplicate `pep440_rs` in dependency tree" by @charliermarsh on 2024-01-29 21:04_

---

_Label `internal` added by @charliermarsh on 2024-01-29 21:04_

---

_Merged by @charliermarsh on 2024-01-29 21:11_

---

_Closed by @charliermarsh on 2024-01-29 21:11_

---

_Branch deleted on 2024-01-29 21:11_

---
