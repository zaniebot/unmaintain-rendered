```yaml
number: 11625
title: Allow use of x86-64 Python on ARM Windows
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/win-arm-x64
created_at: 2025-02-19T16:43:19Z
updated_at: 2025-02-19T18:01:49Z
url: https://github.com/astral-sh/uv/pull/11625
synced_at: 2026-01-10T11:10:38Z
```

# Allow use of x86-64 Python on ARM Windows

---

_Pull request opened by @zanieb on 2025-02-19 16:43_

Closes https://github.com/astral-sh/uv/issues/11493

---

_Label `bug` added by @zanieb on 2025-02-19 16:43_

---

_Renamed from "Allow use fo x86-64 Python on ARM Windows" to "Allow use of x86-64 Python on ARM Windows" by @zanieb on 2025-02-19 16:44_

---

_@zanieb reviewed on 2025-02-19 16:46_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:256 on 2025-02-19 16:46_

Just retaining the hack from https://github.com/astral-sh/uv/pull/10722 for now

See https://github.com/astral-sh/uv/pull/9788

---

_Review requested from @Gankra by @zanieb on 2025-02-19 17:12_

---

_@Gankra approved on 2025-02-19 17:22_

While this will work I'm worried it's a bit of a timebomb for when pbs adds the appropriate builds. That said I guess we statically bake this info so we can just block "update the pbs json" on "implement cascading properly"?

---

_Comment by @zanieb on 2025-02-19 18:01_

Yeah we just need to be careful when we add the builds in the future. We always need to consider ordering when handling new builds anyway.

---

_Merged by @zanieb on 2025-02-19 18:01_

---

_Closed by @zanieb on 2025-02-19 18:01_

---

_Branch deleted on 2025-02-19 18:01_

---
