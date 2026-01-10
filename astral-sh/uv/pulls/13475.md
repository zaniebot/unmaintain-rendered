```yaml
number: 13475
title: Add Windows x86-32 emulation support to interpreter architecture checks
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/windows-86
created_at: 2025-05-15T17:43:54Z
updated_at: 2025-10-30T14:50:29Z
url: https://github.com/astral-sh/uv/pull/13475
synced_at: 2026-01-10T06:28:12Z
```

# Add Windows x86-32 emulation support to interpreter architecture checks

---

_Pull request opened by @zanieb on 2025-05-15 17:43_

Closes https://github.com/astral-sh/uv/issues/13471

Refactors the logic there a bit too.

---

_Label `enhancement` added by @zanieb on 2025-05-15 17:43_

---

_Marked ready for review by @zanieb on 2025-05-15 21:16_

---

_Assigned to @Gankra by @zanieb on 2025-05-28 18:32_

---

_Comment by @zanieb on 2025-05-30 21:43_

@Gankra would you do me a Windows-favor and write a test like the macOS one 🥺 I don't have it in my to copy-update snapshots from CI

---

_@bparzella reviewed on 2025-06-06 07:01_

---

_Review comment by @bparzella on `crates/uv-python/src/platform.rs`:147 on 2025-06-06 07:01_

Does this include running x86_32 on aarch64?

According to [Microsoft](https://learn.microsoft.com/en-us/windows/win32/winprog64/running-32-bit-applications), aarch64 should be able to run x86_32 binaries to.

A quick test reveals this works: ![Screenshot](https://github.com/user-attachments/assets/137bd9c8-e5a7-4991-bf26-4ffc3770a3cf)


---

_Merged by @Gankra on 2025-10-30 14:50_

---

_Closed by @Gankra on 2025-10-30 14:50_

---

_Branch deleted on 2025-10-30 14:50_

---
