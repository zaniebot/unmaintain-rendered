---
number: 3562
title: "Dependent editables cannot be installed with `--only-binary`"
type: issue
state: closed
author: zanieb
labels:
  - bug
assignees: []
created_at: 2024-05-13T20:37:24Z
updated_at: 2024-05-14T00:03:55Z
url: https://github.com/astral-sh/uv/issues/3562
synced_at: 2026-01-10T01:23:29Z
---

# Dependent editables cannot be installed with `--only-binary`

---

_Issue opened by @zanieb on 2024-05-13 20:37_

See test case in https://github.com/astral-sh/uv/pull/3561 prompted by #3513 

---

_Label `bug` added by @zanieb on 2024-05-13 20:37_

---

_Referenced in [astral-sh/uv#3513](../../astral-sh/uv/issues/3513.md) on 2024-05-13 20:37_

---

_Comment by @zanieb on 2024-05-13 20:41_

So... give a setup like: `install -e A -e B --only-binary :all:`

where `B` depends on `A` we fail because `B` does not depend on an editable version of `A`, just `A` at some path. Since `A` is not yet installed, we attempt to build it again and fail. I think we should be reusing the already-built editable `A` for `B`'s requirement, but I'm not sure how we'll go about that.

cc @charliermarsh 

---

_Referenced in [astral-sh/uv#3561](../../astral-sh/uv/pulls/3561.md) on 2024-05-13 20:42_

---

_Comment by @charliermarsh on 2024-05-13 20:45_

I can take a look.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-13 20:45_

---

_Referenced in [astral-sh/uv#3563](../../astral-sh/uv/pulls/3563.md) on 2024-05-13 23:58_

---

_Closed by @charliermarsh on 2024-05-14 00:03_

---
