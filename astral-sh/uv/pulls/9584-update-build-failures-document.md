```yaml
number: 9584
title: Update build failures document
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/build-failures
created_at: 2024-12-02T21:59:46Z
updated_at: 2024-12-04T17:05:29Z
url: https://github.com/astral-sh/uv/pull/9584
synced_at: 2026-01-10T12:00:00Z
```

# Update build failures document

---

_Pull request opened by @zanieb on 2024-12-02 21:59_

In preparation for a dedicated "Troubleshooting" section, revitalizes the "Build failures" reference by adding more details, examples, and structure. This will be used as a model for a "Install failures" document.

---

_Label `documentation` added by @zanieb on 2024-12-02 21:59_

---

_@zanieb reviewed on 2024-12-02 22:04_

---

_Review comment by @zanieb on `docs/reference/build_failures.md`:118 on 2024-12-02 22:04_

I don't think it's generally worth showing the users these invocations.

---

_Review requested from @konstin by @zanieb on 2024-12-02 22:20_

---

_Review requested from @charliermarsh by @zanieb on 2024-12-02 23:46_

---

_@charliermarsh reviewed on 2024-12-02 23:59_

---

_Review comment by @charliermarsh on `docs/reference/build_failures.md`:88 on 2024-12-02 23:59_

This piece might be a bit misleading here because they shouldn't actually file an issue with NumPy, I think?

---

_@charliermarsh reviewed on 2024-12-03 00:00_

---

_Review comment by @charliermarsh on `docs/reference/build_failures.md`:4 on 2024-12-03 00:00_

Perhaps mention (after "for many reasons"), something like: "some of which may be unrelated to uv itself."

---

_@charliermarsh approved on 2024-12-03 00:00_

---

_Review comment by @zanieb on `docs/reference/build_failures.md`:88 on 2024-12-03 00:07_

In this example, yeah sort of. I can expand this though, I had a similar caveat.

It's more like.. "read the upstream documentation and file an issue there if you need to"

---

_@zanieb reviewed on 2024-12-03 00:07_

---

_Merged by @zanieb on 2024-12-03 15:27_

---

_Closed by @zanieb on 2024-12-03 15:27_

---

_Branch deleted on 2024-12-03 15:27_

---

_Review comment by @konstin on `docs/reference/build_failures.md`:143 on 2024-12-04 17:01_

While it's a good example for "If X is missing, try `apt install X`", we should consider guiding ubuntu and debian user to `build-essential`, it's true to its name and has everything that you usually need for C/C++ packages

---

_@konstin reviewed on 2024-12-04 17:05_

Looks good

---
