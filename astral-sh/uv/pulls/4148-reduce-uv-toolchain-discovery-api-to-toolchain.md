```yaml
number: 4148
title: "Reduce `uv-toolchain` discovery API to `Toolchain`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/uv-toolchain-pub
created_at: 2024-06-07T21:15:09Z
updated_at: 2024-06-07T21:56:26Z
url: https://github.com/astral-sh/uv/pull/4148
synced_at: 2026-01-12T16:06:03Z
```

# Reduce `uv-toolchain` discovery API to `Toolchain`

---

_@zanieb_

Drops `find_toolchain`, `find_best_toolchain`, etc. in favor of `Toolchain::find_...`

We can change this in the future, but there should only be one "right" way to do it not two redundant ways in the public interface.

---

_@charliermarsh approved on 2024-06-07 21:54_

---

_Merged by @zanieb on 2024-06-07 21:56_

---

_Closed by @zanieb on 2024-06-07 21:56_

---

_Branch deleted on 2024-06-07 21:56_

---
