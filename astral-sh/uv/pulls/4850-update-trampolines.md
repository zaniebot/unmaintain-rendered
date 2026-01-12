```yaml
number: 4850
title: Update trampolines
type: pull_request
state: merged
author: konstin
labels:
  - windows
assignees: []
merged: true
base: main
head: konsti/update-trampolines
created_at: 2024-07-06T20:59:16Z
updated_at: 2024-07-07T20:18:10Z
url: https://github.com/astral-sh/uv/pull/4850
synced_at: 2026-01-12T16:06:29Z
```

# Update trampolines

---

_@konstin_

Follow-up to https://github.com/astral-sh/uv/pull/4722

---

_Label `windows` added by @konstin on 2024-07-06 20:59_

---

_Merged by @konstin on 2024-07-06 21:06_

---

_Closed by @konstin on 2024-07-06 21:06_

---

_Branch deleted on 2024-07-06 21:06_

---

_Comment by @samypr100 on 2024-07-07 20:09_

@konstin Some of these might still be the old binaries? I noticed only the i686 seem correct / new. Running `strings` still shows `GetEnvironmentVariableA` reference on the other ones and previous `nightly-2024-05-27`  toolchain.

---

_Comment by @konstin on 2024-07-07 20:15_

Odd, does https://github.com/astral-sh/uv/pull/4864 fix that?

---

_Comment by @samypr100 on 2024-07-07 20:18_

It does

---
