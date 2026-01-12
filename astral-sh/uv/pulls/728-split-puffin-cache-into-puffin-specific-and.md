```yaml
number: 728
title: "Split `puffin-cache` into Puffin-specific and generic utilities"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/split-cache
created_at: 2023-12-25T14:27:50Z
updated_at: 2023-12-25T14:38:57Z
url: https://github.com/astral-sh/uv/pull/728
synced_at: 2026-01-12T16:04:09Z
```

# Split `puffin-cache` into Puffin-specific and generic utilities

---

_@charliermarsh_

This crate started off as generic caching utilities, but we started adding a lot of Puffin-specific stuff (like the cache buckets abstraction that knows about Git vs. direct URL vs. indexes and so on). This PR moves the generic stuff into a new `cache-key` crate.

---

_Label `internal` added by @charliermarsh on 2023-12-25 14:27_

---

_Merged by @charliermarsh on 2023-12-25 14:38_

---

_Closed by @charliermarsh on 2023-12-25 14:38_

---

_Branch deleted on 2023-12-25 14:38_

---
