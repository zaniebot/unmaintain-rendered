```yaml
number: 17507
title: Add cargo-xwin to the CONTRIBUTING guide
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: claude/setup-cargo-xwin-clippy-Wc4BH
created_at: 2026-01-15T23:50:43Z
updated_at: 2026-01-16T16:22:07Z
url: https://github.com/astral-sh/uv/pull/17507
synced_at: 2026-01-16T16:59:58Z
```

# Add cargo-xwin to the CONTRIBUTING guide

---

_@zanieb_

This is very useful when making Windows changes.

---

_Label `documentation` added by @zanieb on 2026-01-15 23:51_

---

_Renamed from "Add xwin to the CONTRIBUTING guide" to "Add cargo-xwin to the CONTRIBUTING guide" by @zanieb on 2026-01-15 23:54_

---

_Review comment by @konstin on `CONTRIBUTING.md`:152 on 2026-01-16 09:35_

Why do we pin the version?

---

_@konstin approved on 2026-01-16 09:35_

---

_@zanieb reviewed on 2026-01-16 13:08_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:152 on 2026-01-16 13:08_

```
❯ cargo install cargo-xwin
    Updating crates.io index
  Downloaded cargo-xwin v0.20.2
  Downloaded 1 crate (39.4KiB) in 0.16s
  Installing cargo-xwin v0.20.2
    Updating crates.io index
error: failed to compile `cargo-xwin v0.20.2`, intermediate artifacts can be found at `/var/folders/6p/k5sd5z7j31b31pq4lhn0l8d80000gn/T/cargo-installZU3vlq`.
To reuse those artifacts with a future compilation, set the environment variable `CARGO_TARGET_DIR` to that path.

Caused by:
  failed to select a version for the requirement `xwin = "^0.6.6"`
    version 0.6.6 is yanked
    version 0.6.7 is yanked
  location searched: crates.io index
  required by package `cargo-xwin v0.20.2`
```

---

_@zanieb reviewed on 2026-01-16 13:08_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:152 on 2026-01-16 13:08_

I'm not really sure what's going on there

---

_Marked ready for review by @zanieb on 2026-01-16 13:09_

---

_Merged by @zanieb on 2026-01-16 13:16_

---

_Closed by @zanieb on 2026-01-16 13:16_

---

_Review comment by @konstin on `CONTRIBUTING.md`:152 on 2026-01-16 16:21_

That's my fault: https://github.com/Jake-Shadle/xwin/issues/168, https://github.com/rust-cross/cargo-xwin/pull/181

---

_@konstin reviewed on 2026-01-16 16:21_

---

_@konstin reviewed on 2026-01-16 16:22_

---

_Review comment by @konstin on `CONTRIBUTING.md`:152 on 2026-01-16 16:22_

`cargo install cargo-xwin --locked` also works

---
