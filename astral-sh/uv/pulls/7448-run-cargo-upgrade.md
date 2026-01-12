```yaml
number: 7448
title: "Run `cargo upgrade`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/uip
created_at: 2024-09-17T03:06:39Z
updated_at: 2024-09-17T10:40:00Z
url: https://github.com/astral-sh/uv/pull/7448
synced_at: 2026-01-12T16:07:50Z
```

# Run `cargo upgrade`

---

_@charliermarsh_

## Summary

`cargo upgrade` just updates the listed versions in the TOML files. There's no change to the `Cargo.lock` other than a minor bump to `toml-edit`.


---

_Label `internal` added by @charliermarsh on 2024-09-17 03:06_

---

_Comment by @charliermarsh on 2024-09-17 03:27_

I have absolutely no clue why the trampoline lockfile is trying to update.

---

_Comment by @charliermarsh on 2024-09-17 03:28_

It's not included in the workspace and nothing in that crate changed.

---

_Comment by @konstin on 2024-09-17 09:41_

It was a dev dep from the trampoline crate to uv_fs

---

_Merged by @konstin on 2024-09-17 10:39_

---

_Closed by @konstin on 2024-09-17 10:39_

---

_Branch deleted on 2024-09-17 10:39_

---
