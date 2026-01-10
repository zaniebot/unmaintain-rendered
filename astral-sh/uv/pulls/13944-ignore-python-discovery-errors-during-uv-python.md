```yaml
number: 13944
title: "Ignore Python discovery errors during `uv python pin`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/pin-find-err
created_at: 2025-06-10T12:36:44Z
updated_at: 2025-06-10T14:52:25Z
url: https://github.com/astral-sh/uv/pull/13944
synced_at: 2026-01-10T11:10:42Z
```

# Ignore Python discovery errors during `uv python pin`

---

_Pull request opened by @zanieb on 2025-06-10 12:36_

See https://github.com/astral-sh/uv/issues/13935#issuecomment-2957300516 where we fail to write a pin file because we encounter an unusable interpreter. This is actually a special case where `MissingPython` is not raised because we want to show why we failed to find a usable interpreter, which is useful in commands where you _need_ an interpreter to use, but here we don't actually need it. Here, we just log the failure and move on.

Related https://github.com/astral-sh/uv/pull/13936

---

_Label `bug` added by @zanieb on 2025-06-10 12:36_

---

_Marked ready for review by @zanieb on 2025-06-10 12:43_

---

_Review requested from @Gankra by @zanieb on 2025-06-10 12:47_

---

_Assigned to @Gankra by @zanieb on 2025-06-10 12:47_

---

_@Gankra approved on 2025-06-10 14:12_

---

_Merged by @zanieb on 2025-06-10 14:52_

---

_Closed by @zanieb on 2025-06-10 14:52_

---

_Branch deleted on 2025-06-10 14:52_

---
