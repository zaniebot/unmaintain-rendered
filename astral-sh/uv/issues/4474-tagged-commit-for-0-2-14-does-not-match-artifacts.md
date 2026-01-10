---
number: 4474
title: Tagged commit for 0.2.14 does not match artifacts
type: issue
state: closed
author: zanieb
labels:
  - releases
assignees: []
created_at: 2024-06-24T14:23:19Z
updated_at: 2024-06-24T15:29:17Z
url: https://github.com/astral-sh/uv/issues/4474
synced_at: 2026-01-10T01:23:38Z
---

# Tagged commit for 0.2.14 does not match artifacts

---

_Issue opened by @zanieb on 2024-06-24 14:23_

I'm not sure if there's anything we can do here (now that it's happened), but our release tooling tagged https://github.com/astral-sh/uv/commit/f07308823e877690a00141441b07fd0f7dfe9805 instead of https://github.com/astral-sh/uv/commit/e0ad649c7449b7f54c0053e409ac6e7bf18a6f68 during the re-run of the failed 0.2.14 release.

The artifacts were all built for the proper commit, but the git tag points to an incorrect source.

See #4432 for details on the original problem.





---

_Label `release` added by @zanieb on 2024-06-24 14:25_

---

_Referenced in [astral-sh/uv#4475](../../astral-sh/uv/pulls/4475.md) on 2024-06-24 14:48_

---

_Comment by @zanieb on 2024-06-24 15:23_

We're releasing 0.2.15 now to minimize the effect of this.

---

_Closed by @zanieb on 2024-06-24 15:29_

---
