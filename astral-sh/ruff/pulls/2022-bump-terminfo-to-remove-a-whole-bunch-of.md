```yaml
number: 2022
title: Bump terminfo to remove a whole bunch of unnecessary dependencies
type: pull_request
state: merged
author: akx
labels: []
assignees: []
merged: true
base: main
head: bump-terminfo
created_at: 2023-01-20T11:02:06Z
updated_at: 2023-01-20T14:09:02Z
url: https://github.com/astral-sh/ruff/pull/2022
synced_at: 2026-01-12T15:55:07Z
```

# Bump terminfo to remove a whole bunch of unnecessary dependencies

---

_@akx_

See https://github.com/meh/rust-terminfo/commit/6281c6b8f7abb8799e2099174d9f4e4a4215d259

```
$ cargo update -p terminfo
    Updating crates.io index
    Removing cfg-if v0.1.10
    Removing dirs v2.0.2
    Removing getrandom v0.1.16
    Removing phf v0.8.0
    Updating phf_codegen v0.8.0 -> v0.11.1
    Updating phf_generator v0.8.0 -> v0.11.1
    Removing phf_shared v0.8.0
    Removing rand v0.7.3
    Removing rand_chacha v0.2.2
    Removing rand_core v0.5.1
    Removing rand_hc v0.2.0
    Removing rand_pcg v0.2.1
    Updating terminfo v0.7.3 -> v0.7.5
    Removing wasi v0.9.0+wasi-snapshot-preview1
```

---

_Renamed from "Bump p to remove a whole bunch of unnecessary dependencies" to "Bump terminfo to remove a whole bunch of unnecessary dependencies" by @akx on 2023-01-20 11:02_

---

_Marked ready for review by @akx on 2023-01-20 11:07_

---

_Comment by @charliermarsh on 2023-01-20 12:41_

Happy to merge but this may need to be rebased now unfortunately!

---

_Comment by @akx on 2023-01-20 13:40_

@charliermarsh Rebased! 👍 

---

_Merged by @charliermarsh on 2023-01-20 14:09_

---

_Closed by @charliermarsh on 2023-01-20 14:09_

---
