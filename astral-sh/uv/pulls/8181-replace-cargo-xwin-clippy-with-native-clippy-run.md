```yaml
number: 8181
title: "Replace `cargo xwin clippy` with native clippy run on Windows temporarily"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/xwin-tmp
created_at: 2024-10-14T18:08:11Z
updated_at: 2024-10-14T18:22:06Z
url: https://github.com/astral-sh/uv/pull/8181
synced_at: 2026-01-10T12:54:04Z
```

# Replace `cargo xwin clippy` with native clippy run on Windows temporarily

---

_Pull request opened by @zanieb on 2024-10-14 18:08_

We can't have CI blocked by this. If this doesn't work or is exceptionally slow, I'll remove the job entirely.

See https://github.com/rust-cross/cargo-xwin/issues/127

---

_Label `internal` added by @zanieb on 2024-10-14 18:08_

---

_Merged by @zanieb on 2024-10-14 18:19_

---

_Closed by @zanieb on 2024-10-14 18:19_

---

_Branch deleted on 2024-10-14 18:19_

---

_Comment by @zanieb on 2024-10-14 18:20_

This took 3m 20s which is comparable to the xwin job.

---
