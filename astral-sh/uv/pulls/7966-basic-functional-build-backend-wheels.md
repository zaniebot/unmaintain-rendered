```yaml
number: 7966
title: Basic functional build backend wheels
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: build-backend-6-add-files
created_at: 2024-10-07T11:21:22Z
updated_at: 2024-10-07T17:30:54Z
url: https://github.com/astral-sh/uv/pull/7966
synced_at: 2026-01-10T12:54:00Z
```

# Basic functional build backend wheels

---

_Pull request opened by @konstin on 2024-10-07 11:21_

Add file writing, basic module directory walking, RECORD writing and integration test to write basic functional wheels.

---

_Review requested from @BurntSushi by @konstin on 2024-10-07 11:21_

---

_@BurntSushi approved on 2024-10-07 14:32_

What do you think about using virtual dispatch instead of `impl DirectoryWriter` and `impl Write`? It would likely reduce code size and compile times, and probably not impact overall perf.

---

_Comment by @konstin on 2024-10-07 17:30_

Good idea!

I'm going to merge this and do more pull requests on top to keep their size reasonable.

---

_Label `preview` added by @konstin on 2024-10-07 17:30_

---

_Marked ready for review by @konstin on 2024-10-07 17:30_

---

_Merged by @konstin on 2024-10-07 17:30_

---

_Closed by @konstin on 2024-10-07 17:30_

---

_Branch deleted on 2024-10-07 17:30_

---
