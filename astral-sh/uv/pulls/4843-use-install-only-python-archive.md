```yaml
number: 4843
title: "Use `install_only` python archive"
type: pull_request
state: merged
author: j178
labels: []
assignees: []
merged: true
base: main
head: install-only
created_at: 2024-07-06T08:29:15Z
updated_at: 2024-07-11T15:08:12Z
url: https://github.com/astral-sh/uv/pull/4843
synced_at: 2026-01-12T16:06:29Z
```

# Use `install_only` python archive

---

_@j178_

## Summary

Resolves #4834

## Test Plan

```sh
# 3.12.3 is a `install_only` archive
$ cargo run -- python install --preview --force 3.12.3

# 3.9.4 has only `full` archive
$ cargo run -- python install --preview --force 3.9.4
```


---

_Marked ready for review by @j178 on 2024-07-06 08:39_

---

_@j178 reviewed on 2024-07-06 08:42_

---

_Review comment by @j178 on `crates/uv-python/src/managed.rs`:255 on 2024-07-06 08:42_

Do we need to keep compatibility with the previously installed toolchain that has a `install` subdirectory?

---

_@charliermarsh reviewed on 2024-07-06 19:04_

---

_Review comment by @charliermarsh on `crates/uv-python/src/managed.rs`:255 on 2024-07-06 19:04_

I think we might as well retain compatibility there, yeah. Can we just check if `install` is a dir?

---

_@charliermarsh reviewed on 2024-07-06 19:05_

---

_Review comment by @charliermarsh on `crates/uv-python/src/downloads.rs`:521 on 2024-07-06 19:05_

I might suggest just checking these directly? We know where they would be, right? We shouldn't need to do a full directory scan.

---

_@charliermarsh reviewed on 2024-07-06 19:23_

---

_Review comment by @charliermarsh on `crates/uv-python/src/downloads.rs`:527 on 2024-07-06 19:23_

Do we need to check both install and build?

---

_@charliermarsh reviewed on 2024-07-06 19:34_

---

_Review comment by @charliermarsh on `crates/uv-python/src/downloads.rs`:521 on 2024-07-06 19:34_

(As in: `p.join("install").is_dir()`.)

---

_@charliermarsh approved on 2024-07-07 02:39_

---

_Merged by @charliermarsh on 2024-07-07 02:43_

---

_Closed by @charliermarsh on 2024-07-07 02:43_

---

_Branch deleted on 2024-07-11 15:08_

---
