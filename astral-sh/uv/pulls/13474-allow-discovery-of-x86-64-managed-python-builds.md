```yaml
number: 13474
title: Allow discovery of x86-64 managed Python builds on macOS
type: pull_request
state: closed
author: zanieb
labels:
  - enhancement
assignees: []
base: zb/arch-mode
head: zb/macos-86
created_at: 2025-05-15T17:38:07Z
updated_at: 2025-05-29T19:06:35Z
url: https://github.com/astral-sh/uv/pull/13474
synced_at: 2026-01-10T11:10:41Z
```

# Allow discovery of x86-64 managed Python builds on macOS

---

_Pull request opened by @zanieb on 2025-05-15 17:38_

_No description provided._

---

_Label `enhancement` added by @zanieb on 2025-05-15 17:38_

---

_Assigned to @Gankra by @zanieb on 2025-05-28 18:32_

---

_Marked ready for review by @zanieb on 2025-05-28 19:04_

---

_@zanieb reviewed on 2025-05-28 19:17_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:1484 on 2025-05-28 19:17_

Unfortunately this is wrong. We'll need to change the ordering of the managed distributions per target platform so we prefer the native builds.

---

_@zanieb reviewed on 2025-05-28 19:18_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:1484 on 2025-05-28 19:18_

I think this hints at a different design for #13701 

---

_Review comment by @Gankra on `crates/uv-python/src/platform.rs`:160 on 2025-05-28 19:59_

I will note that isn't actually strictly true for macOS: Rosetta2 is not default-installed on Apple Silicon macOS. It *is* auto-installed the first time you run an intel app... but only if it's a full GUI App. If it's just a random CLI binary it actually won't bother!

So if we want to be fully bullet-proof we would have to check if Rosetta2 is already on the user's system :(

---

_@Gankra reviewed on 2025-05-28 20:20_

---

_@Gankra reviewed on 2025-05-28 20:21_

---

_Review comment by @Gankra on `crates/uv-python/src/platform.rs`:160 on 2025-05-28 20:21_

In practice it tends to be fine though. Especially since we're finding these things on their system.

---

_@zanieb reviewed on 2025-05-28 20:25_

---

_Review comment by @zanieb on `crates/uv-python/src/platform.rs`:160 on 2025-05-28 20:25_

Oh dear goodness. That's... good to know. I will at least add some commentary about that.

---

_@zanieb reviewed on 2025-05-28 20:48_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:1484 on 2025-05-28 20:48_

Well, it might be separate? I fixed this in https://github.com/astral-sh/uv/pull/13709

I worry that #13701 may still be wrong though. I'll look at it again tomorrow.

---

_Closed by @zanieb on 2025-05-29 19:06_

---
