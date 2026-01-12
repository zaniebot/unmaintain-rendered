```yaml
number: 4254
title: "Re-add `aarch64-unknown-linux-gnu` binary to release assets"
type: pull_request
state: merged
author: charliermarsh
labels:
  - releases
assignees: []
merged: true
base: main
head: charlie/libc
created_at: 2024-06-11T23:27:54Z
updated_at: 2024-06-11T23:46:10Z
url: https://github.com/astral-sh/uv/pull/4254
synced_at: 2026-01-12T16:06:06Z
```

# Re-add `aarch64-unknown-linux-gnu` binary to release assets

---

_@charliermarsh_

This PR re-adds the `aarch64-unknown-linux-gnu` binary, which will also add the `manylinux_2_28` wheel for `aarch64` (in addition to the now dual-tagged `musllinux_1_1` and `manylinux_2_217` wheel for `aarch64`). We can consider dropping that _wheel_, but in my assessment removing a release asset should now be treated as a breaking change -- so removing it in a patch release was incorrect.

Closes https://github.com/astral-sh/uv/issues/4122.


---

_Label `release` added by @charliermarsh on 2024-06-11 23:27_

---

_@zanieb approved on 2024-06-11 23:40_

---

_Merged by @charliermarsh on 2024-06-11 23:46_

---

_Closed by @charliermarsh on 2024-06-11 23:46_

---

_Branch deleted on 2024-06-11 23:46_

---
