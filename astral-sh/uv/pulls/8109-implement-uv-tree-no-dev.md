```yaml
number: 8109
title: "Implement `uv tree --no-dev`"
type: pull_request
state: merged
author: bluss
labels:
  - cli
assignees: []
merged: true
base: main
head: no-dev-tree
created_at: 2024-10-10T20:40:20Z
updated_at: 2024-10-12T13:17:03Z
url: https://github.com/astral-sh/uv/pull/8109
synced_at: 2026-01-10T12:54:03Z
```

# Implement `uv tree --no-dev`

---

_Pull request opened by @bluss on 2024-10-10 20:40_

## Summary

Allow pruning dev-dependencies in uv tree.
This is not inherently in conflict with --invert, but this pruning is not yet implemented there.

## Test Plan

TBD?

---

_Comment by @bluss on 2024-10-10 22:46_

I picked a bad time to add this :) anyway, here's the feature request

---

_@charliermarsh approved on 2024-10-12 13:04_

No strong objection to including though we'll have to generalize this to groups anyway.

---

_Renamed from "Implement uv tree --no-dev" to "Implement `uv tree --no-dev`" by @charliermarsh on 2024-10-12 13:04_

---

_Label `cli` added by @charliermarsh on 2024-10-12 13:05_

---

_Merged by @charliermarsh on 2024-10-12 13:10_

---

_Closed by @charliermarsh on 2024-10-12 13:10_

---

_Branch deleted on 2024-10-12 13:17_

---
