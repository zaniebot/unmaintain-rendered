```yaml
number: 34
title: "Use local copy of `install-wheel-rs`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/install-wheel-fix
created_at: 2023-10-07T01:13:09Z
updated_at: 2023-10-07T01:43:56Z
url: https://github.com/astral-sh/uv/pull/34
synced_at: 2026-01-10T15:50:28Z
```

# Use local copy of `install-wheel-rs`

---

_Pull request opened by @charliermarsh on 2023-10-07 01:13_

This PR modifies the `install-wheel-rs` (and a few other crates) to get everything playing nicely. Specifically, CI should pass, and all these crates now use workspace dependencies between one another.

As part of this change, I split out the wheel name parsing into its own `wheel-filename` crate, and the compatibility tag parsing into its own `platform-tags` crate.

---

_Merged by @charliermarsh on 2023-10-07 01:43_

---

_Closed by @charliermarsh on 2023-10-07 01:43_

---

_Branch deleted on 2023-10-07 01:43_

---
