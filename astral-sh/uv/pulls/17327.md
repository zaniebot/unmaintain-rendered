```yaml
number: 17327
title: "Use `Cow<str>` for deserialization map keys in `PypiFile` and `PyxFile`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: claude/fix-uv-17221-kmH49
created_at: 2026-01-05T20:17:13Z
updated_at: 2026-01-06T09:51:24Z
url: https://github.com/astral-sh/uv/pull/17327
synced_at: 2026-01-10T05:49:14Z
```

# Use `Cow<str>` for deserialization map keys in `PypiFile` and `PyxFile`

---

_Pull request opened by @zanieb on 2026-01-05 20:17_

Use `Cow<'_, str>` when deserializing map keys. This allows serde to borrow strings directly from the input in the common case (no escaping needed), while still correctly handling cases where de-escaping is required.

Addresses feedback from #17221

---

_Marked ready for review by @zanieb on 2026-01-05 20:54_

---

_Review requested from @woodruffw by @zanieb on 2026-01-05 20:54_

---

_@woodruffw approved on 2026-01-05 20:59_

<img width="800" height="443" alt="Image" src="https://github.com/user-attachments/assets/c373b3e8-7d67-4d0b-87c2-2a5ce864a32c" />

---

_Merged by @woodruffw on 2026-01-05 20:59_

---

_Closed by @woodruffw on 2026-01-05 20:59_

---

_Label `internal` added by @konstin on 2026-01-06 09:51_

---
