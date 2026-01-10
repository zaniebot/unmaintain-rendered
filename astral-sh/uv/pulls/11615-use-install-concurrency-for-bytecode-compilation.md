```yaml
number: 11615
title: Use install concurrency for bytecode compilation too
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/use-install-concurrency-for-installation-too
created_at: 2025-02-19T09:04:57Z
updated_at: 2025-02-20T11:23:43Z
url: https://github.com/astral-sh/uv/pull/11615
synced_at: 2026-01-10T11:10:38Z
```

# Use install concurrency for bytecode compilation too

---

_Pull request opened by @konstin on 2025-02-19 09:04_

Instead of always using all available threads for bytecode compilation, respect `UV_CONCURRENT_INSTALLS`, so the parallelism is configurable instead of hardcoded. We reuse the install limit since bytecode compilation only runs after install.

---

_Review requested from @BurntSushi by @konstin on 2025-02-19 09:04_

---

_@BurntSushi approved on 2025-02-19 12:11_

Makes sense to me!

---

_Comment by @zanieb on 2025-02-19 15:57_

"for installation" in the title being.. "for bytecode compilation"?

---

_Renamed from "Use install concurrency for installation too" to "Use install concurrency for bytecode compilation too" by @konstin on 2025-02-20 11:23_

---

_Merged by @konstin on 2025-02-20 11:23_

---

_Closed by @konstin on 2025-02-20 11:23_

---

_Branch deleted on 2025-02-20 11:23_

---
