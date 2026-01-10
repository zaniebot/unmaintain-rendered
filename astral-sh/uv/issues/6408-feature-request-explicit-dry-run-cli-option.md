---
number: 6408
title: "[Feature request] Explicit dry-run cli option"
type: issue
state: closed
author: birdhackor
labels:
  - enhancement
  - projects
assignees: []
created_at: 2024-08-22T04:57:04Z
updated_at: 2024-10-24T03:22:19Z
url: https://github.com/astral-sh/uv/issues/6408
synced_at: 2026-01-10T01:24:00Z
---

# [Feature request] Explicit dry-run cli option

---

_Issue opened by @birdhackor on 2024-08-22 04:57_

If I want to know which packages versions will change when I do a upgradable relock, I might want to do the following command

``` shell
$ uv lock -U --dry-run
```

Currently, there seems to be no direct instruction to achieve a similar effect. The following command will report an error and cannot achieve the effect I want.

``` shell
$ uv lock -U --locked
Resolved 10 packages in 126ms
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
```

---

_Label `enhancement` added by @charliermarsh on 2024-08-22 12:28_

---

_Comment by @charliermarsh on 2024-08-22 12:28_

This makes sense to me.

---

_Comment by @zanieb on 2024-08-22 15:42_

Makes sense. There's also `uv lock -U && git diff && git reset --hard`, but we can do better.

---

_Label `projects` added by @zanieb on 2024-08-22 15:42_

---

_Referenced in [astral-sh/uv#7783](../../astral-sh/uv/pulls/7783.md) on 2024-09-29 18:59_

---

_Comment by @charliermarsh on 2024-10-24 03:22_

Closed by https://github.com/astral-sh/uv/pull/7783.

---

_Closed by @charliermarsh on 2024-10-24 03:22_

---
