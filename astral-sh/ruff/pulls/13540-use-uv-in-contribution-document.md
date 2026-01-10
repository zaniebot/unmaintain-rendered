```yaml
number: 13540
title: Use uv in contribution document
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/uv-contrib
created_at: 2024-09-27T15:24:59Z
updated_at: 2024-09-30T19:43:01Z
url: https://github.com/astral-sh/ruff/pull/13540
synced_at: 2026-01-10T20:59:36Z
```

# Use uv in contribution document

---

_Pull request opened by @zanieb on 2024-09-27 15:24_

_No description provided._

---

_Label `internal` added by @zanieb on 2024-09-27 15:25_

---

_Review comment by @MithicSpirit on `CONTRIBUTING.md`:40 on 2024-09-27 15:29_

Shouldn't this be `uvx pre-commit install`, as it won't necessarily be the first result for pre-commit on the PATH?

---

_@MithicSpirit reviewed on 2024-09-27 15:29_

---

_@zanieb reviewed on 2024-09-27 15:47_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:40 on 2024-09-27 15:47_

If you `uvx pre-commit install` won't the pre-commit executable be missing during the git hook invocations?

---

_@MithicSpirit reviewed on 2024-09-27 16:38_

---

_Review comment by @MithicSpirit on `CONTRIBUTING.md`:40 on 2024-09-27 16:38_

Just tested it in a clean docker container, and `uvx pre-commit install` works. It uses the python that it was originally executed with, so it doesn't need to be in the PATH (although it falls back to that if the original python is no longer available for some reason).

---

_@zanieb reviewed on 2024-09-30 14:53_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:40 on 2024-09-30 14:53_

I assume this relies on an environment that's in the uv cache though — that's generally bad practice.

---

_Merged by @zanieb on 2024-09-30 19:42_

---

_Closed by @zanieb on 2024-09-30 19:42_

---

_Branch deleted on 2024-09-30 19:43_

---
