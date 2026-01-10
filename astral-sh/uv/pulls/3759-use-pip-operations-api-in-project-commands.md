```yaml
number: 3759
title: "Use pip \"operations\" API in project commands"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/ops
created_at: 2024-05-22T19:22:40Z
updated_at: 2024-05-22T19:43:13Z
url: https://github.com/astral-sh/uv/pull/3759
synced_at: 2026-01-10T14:32:20Z
```

# Use pip "operations" API in project commands

---

_Pull request opened by @charliermarsh on 2024-05-22 19:22_

## Summary

This PR removes most of the code in `project/mod.rs` in favor of the routines exposed in `pip/operations.rs`.

I think we can do a lot more to add more abstraction here and reduce the verbosity, but for now it deduplicates a _ton_ of logic. The remaining logic is just instantiating settings etc.


---

_Comment by @zanieb on 2024-05-22 19:24_

👍 I feel like this doesn't need review just lmk if you want a closer look for anything

---

_Comment by @charliermarsh on 2024-05-22 19:25_

👍 Yeah I'll just keep grinding on it. I've now deduplicated a lot of code but it still feels very under-abstracted (too much argument passing everywhere).

---

_Label `internal` added by @charliermarsh on 2024-05-22 19:25_

---

_@konstin approved on 2024-05-22 19:28_

This will break my workspace branch badly

Nice work on the +196 −579 though, the centralization will make things easier in the future

---

_Comment by @charliermarsh on 2024-05-22 19:43_

Lol I'm really sorry.

---

_Merged by @charliermarsh on 2024-05-22 19:43_

---

_Closed by @charliermarsh on 2024-05-22 19:43_

---

_Branch deleted on 2024-05-22 19:43_

---
