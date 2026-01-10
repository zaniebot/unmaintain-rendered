---
number: 5633
title: "Document how to resolve `uv.lock` merge conflicts"
type: issue
state: open
author: zanieb
labels:
  - documentation
assignees: []
created_at: 2024-07-30T20:40:18Z
updated_at: 2025-11-25T23:11:49Z
url: https://github.com/astral-sh/uv/issues/5633
synced_at: 2026-01-10T01:23:51Z
---

# Document how to resolve `uv.lock` merge conflicts

---

_Issue opened by @zanieb on 2024-07-30 20:40_

I don't know what best practice is here.

---

_Label `documentation` added by @zanieb on 2024-07-30 20:40_

---

_Comment by @LennyN95 on 2024-11-07 10:12_

I came across this problem. We found that the lock files may differ greatly across branches during active development and that it's usually to complex for automatic conflict resolution. However, it seems more plausible to generate the lock file after merging branches rather than before. 

What first came to my mind was to avoid pushing local lock files and create them as part of the CI pipeline on the main branch before running the tests, and commit the lock file when all tests pass.




---

_Comment by @zanieb on 2024-11-07 20:27_

In brief the current recommendation is:

- Attempt to merge the parent into your branch, encounter a merge conflict in the lockfile
- Checkout the lockfile from the parent `git checkout <parent> -- uv.lock`
- Lock again `uv lock`

---

_Comment by @BenGale93 on 2024-12-12 09:08_

I find that the most common source of conflict is the version number of the package itself. Easy to deal with but generates conflicts when it doesn't really need to. Is there any plan to mitigate this?

---

_Comment by @zanieb on 2024-12-12 14:03_

Are you using a dynamic package version or does it just change a lot?

---

_Comment by @BenGale93 on 2024-12-12 14:26_

Good point, I'm picking up the version number dynamically from the git tags. I assume it's impossible to avoid this issue and keep git tags as the single source of truth?

---

_Comment by @BenGale93 on 2024-12-12 14:34_

Just found https://github.com/astral-sh/uv/issues/7533 which covers it. Sorry for wasting your time @zanieb!

---

_Comment by @zanieb on 2024-12-12 15:21_

👍 yep that's the issue to track. No problem :)

---

_Referenced in [astral-sh/uv#11185](../../astral-sh/uv/issues/11185.md) on 2025-02-03 16:24_

---

_Referenced in [astral-sh/uv#16348](../../astral-sh/uv/issues/16348.md) on 2025-10-17 19:58_

---
