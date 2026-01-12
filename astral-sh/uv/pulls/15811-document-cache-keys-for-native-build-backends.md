```yaml
number: 15811
title: Document cache-keys for native build backends
type: pull_request
state: merged
author: konstin
labels:
  - documentation
assignees: []
merged: true
base: main
head: konsti/doc-cache-keys
created_at: 2025-09-12T13:33:25Z
updated_at: 2025-09-14T14:20:18Z
url: https://github.com/astral-sh/uv/pull/15811
synced_at: 2026-01-12T16:11:57Z
```

# Document cache-keys for native build backends

---

_@konstin_

Update note for https://github.com/astral-sh/uv/pull/15705#issuecomment-3285240646

---

_Label `documentation` added by @konstin on 2025-09-12 13:33_

---

_Review comment by @zanieb on `docs/concepts/projects/init.md`:304 on 2025-09-12 13:58_

We should say something after this like... "When creating a project with extension modules, uv configures the cache keys to include source common file types."

---

_@zanieb reviewed on 2025-09-12 13:58_

---

_@zanieb reviewed on 2025-09-12 13:59_

---

_Review comment by @zanieb on `docs/concepts/projects/init.md`:306 on 2025-09-12 13:59_

Maybe we should say, "To force a rebuild, e.g., when changing files not included in the cache keys, use `--reinstall`"? 

Do we need to include this line at all though?

---

_@konstin reviewed on 2025-09-12 14:01_

---

_Review comment by @konstin on `docs/concepts/projects/init.md`:306 on 2025-09-12 14:01_

I would definitely include it, there are some cases we don't cover currently, e.g. additional `CMakeLists` files or changes in system headers.

---

_@zanieb reviewed on 2025-09-12 14:04_

---

_Review comment by @zanieb on `docs/concepts/projects/init.md`:306 on 2025-09-12 14:04_

You're framing it as "Without `tool.uv.cache-keys`, changes to the extension code in `lib.rs` or `main.cpp`" though. If you care about, e.g., CMakeLists, then you should probably say what I suggested plus recommend extending the cache keys.

---

_Review comment by @zanieb on `docs/concepts/projects/init.md`:305 on 2025-09-14 13:34_

```suggestion
    to include common source file types. To force a rebuild, e.g. when changing files outside
```

---

_@zanieb reviewed on 2025-09-14 13:34_

---

_@zanieb approved on 2025-09-14 14:13_

---

_Merged by @konstin on 2025-09-14 14:20_

---

_Closed by @konstin on 2025-09-14 14:20_

---

_Branch deleted on 2025-09-14 14:20_

---
