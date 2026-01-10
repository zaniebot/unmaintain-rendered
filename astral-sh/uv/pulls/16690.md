```yaml
number: 16690
title: "Replace println with Printer for all `dir` commands & workspace dir followups"
type: pull_request
state: merged
author: mikaylathompson
labels:
  - internal
assignees: []
merged: true
base: main
head: mikayla/workspace-dir-quick-followups
created_at: 2025-11-11T20:12:04Z
updated_at: 2025-11-11T20:36:32Z
url: https://github.com/astral-sh/uv/pull/16690
synced_at: 2026-01-10T06:28:12Z
```

# Replace println with Printer for all `dir` commands & workspace dir followups

---

_Pull request opened by @mikaylathompson on 2025-11-11 20:12_

## Summary

1. Discussed in review of #16678 that println should be replaced by using `printer`. The `println` pattern was pretty consistent across all the `dir` commands, so I've updated all of them in this PR (there are some usages of `println` outside of `uv/src/commands` that I didn't touch -- the use cases there seemed more complex and nuanced).
2. I missed two comments in the previous PR before merging, so updates from those are in here as well.

## Test Plan

No behavior changes, existing tests for all commands pass.


---

_Label `internal` added by @zanieb on 2025-11-11 20:14_

---

_@zanieb approved on 2025-11-11 20:14_

Thanks!

---

_Comment by @zanieb on 2025-11-11 20:14_

I wonder if we should ban `println` with Clippy?

---

_Merged by @zanieb on 2025-11-11 20:36_

---

_Closed by @zanieb on 2025-11-11 20:36_

---

_Branch deleted on 2025-11-11 20:36_

---
