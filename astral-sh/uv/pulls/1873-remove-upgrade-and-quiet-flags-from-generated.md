```yaml
number: 1873
title: "Remove `--upgrade` and `--quiet` flags from generated output files"
type: pull_request
state: merged
author: yasufumy
labels: []
assignees: []
merged: true
base: main
head: fix/pip-tools-compatibility
created_at: 2024-02-22T15:35:22Z
updated_at: 2024-02-24T05:05:47Z
url: https://github.com/astral-sh/uv/pull/1873
synced_at: 2026-01-12T16:04:45Z
```

# Remove `--upgrade` and `--quiet` flags from generated output files

---

_@yasufumy_

## Summary

Resolve #1814

I changed the behavior of `pip compile` to not display `--upgrade` (`-U`) and `--quiet` (`-q`) for compatibility

---

_@yasufumy reviewed on 2024-02-23 07:16_

---

_Review comment by @yasufumy on `crates/uv/src/commands/pip_compile.rs`:415 on 2024-02-23 07:16_

I'm new to Rust and I didn't come up with a good idea to avoid the clippy warning. I'd appreciate it if you suggest a better way.

---

_Marked ready for review by @yasufumy on 2024-02-23 07:16_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-23 16:05_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-02-23 16:05_

---

_@charliermarsh reviewed on 2024-02-23 18:54_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_compile.rs`:415 on 2024-02-23 18:54_

I think what you have here is ok!Tthough I'm gonna remove the arguments because I think we should just always omit these.

---

_@charliermarsh approved on 2024-02-23 18:56_

---

_Merged by @charliermarsh on 2024-02-23 19:01_

---

_Closed by @charliermarsh on 2024-02-23 19:01_

---

_Comment by @charliermarsh on 2024-02-23 19:01_

Thanks @yasufumy!

---

_Branch deleted on 2024-02-24 04:45_

---

_@yasufumy reviewed on 2024-02-24 05:05_

---

_Review comment by @yasufumy on `crates/uv/src/commands/pip_compile.rs`:415 on 2024-02-24 05:05_

Okay! Thank you for your comment.

---
