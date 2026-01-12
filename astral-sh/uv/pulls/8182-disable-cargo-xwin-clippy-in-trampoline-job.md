```yaml
number: 8182
title: "Disable `cargo xwin clippy` in trampoline job"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/disable-xin-trampoline
created_at: 2024-10-14T18:21:53Z
updated_at: 2024-10-14T18:32:25Z
url: https://github.com/astral-sh/uv/pull/8182
synced_at: 2026-01-12T16:08:11Z
```

# Disable `cargo xwin clippy` in trampoline job

---

_@zanieb_

See https://github.com/rust-cross/cargo-xwin/issues/127

---

_Label `internal` added by @zanieb on 2024-10-14 18:21_

---

_Merged by @zanieb on 2024-10-14 18:29_

---

_Closed by @zanieb on 2024-10-14 18:29_

---

_Branch deleted on 2024-10-14 18:29_

---

_Comment by @samypr100 on 2024-10-14 18:32_

@zanieb Could it be this https://github.com/rust-cross/cargo-xwin/issues/127#issuecomment-2411965490

I think we should bump the timeout first to let it refresh the Windows SDK download (which can take 10+ minutes uncached)

---
