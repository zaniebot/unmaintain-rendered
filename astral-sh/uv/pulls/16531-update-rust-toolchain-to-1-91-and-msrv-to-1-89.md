```yaml
number: 16531
title: Update Rust toolchain to 1.91 and MSRV to 1.89
type: pull_request
state: merged
author: samypr100
labels: []
assignees: []
merged: true
base: main
head: rust-191
created_at: 2025-10-31T03:01:46Z
updated_at: 2025-10-31T03:39:44Z
url: https://github.com/astral-sh/uv/pull/16531
synced_at: 2026-01-12T16:12:18Z
```

# Update Rust toolchain to 1.91 and MSRV to 1.89

---

_@samypr100_

## Summary

Updates Rust Toolchain to [1.91](https://blog.rust-lang.org/2025/10/30/Rust-1.91.0/) and bumps MSRV to [1.89](https://blog.rust-lang.org/2025/08/07/Rust-1.89.0/) per versioning policy. New clippy rule [implicit clone](https://rust-lang.github.io/rust-clippy/master/index.html#implicit_clone) resulted in some minor changes (some with improvements).

Updates trampoline to `nightly-2025-06-23` which is roughly 1.89~. The trampoline binaries do not need to be regenerated as there should be no changes.


---

_Marked ready for review by @samypr100 on 2025-10-31 03:32_

---

_Comment by @zanieb on 2025-10-31 03:34_

Heck yeah, thank you!

---

_@zanieb approved on 2025-10-31 03:34_

---

_Merged by @zanieb on 2025-10-31 03:35_

---

_Closed by @zanieb on 2025-10-31 03:35_

---

_Branch deleted on 2025-10-31 03:39_

---
