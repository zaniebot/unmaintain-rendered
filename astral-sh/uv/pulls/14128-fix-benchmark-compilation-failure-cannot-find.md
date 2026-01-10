```yaml
number: 14128
title: "Fix benchmark compilation failure: `cannot find attribute clap in this scope`"
type: pull_request
state: merged
author: jtfmumm
labels:
  - bug
assignees: []
merged: true
base: main
head: jtfm/fix-torch-failure
created_at: 2025-06-18T13:45:37Z
updated_at: 2025-06-18T14:30:14Z
url: https://github.com/astral-sh/uv/pull/14128
synced_at: 2026-01-10T11:10:42Z
```

# Fix benchmark compilation failure: `cannot find attribute clap in this scope`

---

_Pull request opened by @jtfmumm on 2025-06-18 13:45_

[Two benchmark jobs](https://github.com/astral-sh/uv/actions/runs/15732775460/job/44337710992?pr=14126) were failing with `error: cannot find attribute clap in this scope` based on #14120. This updates the recently added `#[clap(name = rocm...` lines to use `cfg_attr(feature = "clap",`.


---

_Label `bug` added by @jtfmumm on 2025-06-18 13:45_

---

_Comment by @konstin on 2025-06-18 13:47_

Should those be `#[cfg_attr(feature = "clap", ` instead? Otherwise we still have the implicit required clap dependency.

---

_Renamed from "Handle clap feature in `uv-bench` to prevent CI failure" to "Fix benchmark compilation failure: `cannot find attribute clap in this scope`" by @jtfmumm on 2025-06-18 13:57_

---

_Comment by @jtfmumm on 2025-06-18 14:04_

> Should those be `#[cfg_attr(feature = "clap", ` instead? Otherwise we still have the implicit required clap dependency.

Updated

---

_Review requested from @konstin by @jtfmumm on 2025-06-18 14:16_

---

_@konstin approved on 2025-06-18 14:29_

---

_Merged by @jtfmumm on 2025-06-18 14:30_

---

_Closed by @jtfmumm on 2025-06-18 14:30_

---

_Branch deleted on 2025-06-18 14:30_

---
