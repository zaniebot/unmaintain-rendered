```yaml
number: 16695
title: "Deny stdout/stderr printing in `uv` crate via clippy"
type: pull_request
state: merged
author: terror
labels:
  - internal
assignees: []
merged: true
base: main
head: uv-deny-println
created_at: 2025-11-12T00:36:02Z
updated_at: 2025-11-12T13:54:53Z
url: https://github.com/astral-sh/uv/pull/16695
synced_at: 2026-01-10T06:28:12Z
```

# Deny stdout/stderr printing in `uv` crate via clippy

---

_Pull request opened by @terror on 2025-11-12 00:36_

Follow-up from https://github.com/astral-sh/uv/pull/16690, in `uv` every command should be using `write!(...)/writeln!(...)` with the `Printer` abstraction instead of bypassing control with the standard printing functions. This lint ensures that.

---

_Label `internal` added by @zanieb on 2025-11-12 13:54_

---

_Comment by @zanieb on 2025-11-12 13:54_

Ah interesting, I was looking at banning it ~everywhere but this seems like a good start

---

_Merged by @zanieb on 2025-11-12 13:54_

---

_Closed by @zanieb on 2025-11-12 13:54_

---
