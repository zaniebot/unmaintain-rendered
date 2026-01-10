```yaml
number: 3396
title: List and uninstall legacy editables
type: pull_request
state: closed
author: hauntsaninja
labels: []
assignees: []
draft: true
base: charlie/egg
head: sj/egg
created_at: 2024-05-06T03:30:02Z
updated_at: 2024-05-06T21:56:24Z
url: https://github.com/astral-sh/uv/pull/3396
synced_at: 2026-01-10T14:37:54Z
```

# List and uninstall legacy editables

---

_Pull request opened by @hauntsaninja on 2024-05-06 03:30_

No worries if this isn't something uv wants to support! Note this PR is currently based on top of #3380 , so shouldn't be merged till that one is merged

---

_Closed by @charliermarsh on 2024-05-06 13:47_

---

_Comment by @charliermarsh on 2024-05-06 13:48_

Oops, I think this got closed because my upstream branch got deleted, maybe?

---

_Comment by @charliermarsh on 2024-05-06 13:48_

Are you able to reopen? I'm fine with supporting this!

---

_@charliermarsh reviewed on 2024-05-06 13:49_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/uninstall.rs`:240 on 2024-05-06 13:49_

This is my only concern. We might want to use an advisory file-lock on `easy-install.pth`? Or, actually, we could even use a mutex? Since we already have a lock on the environment itself, so in theory we only need to lock within this process.

---

_Branch deleted on 2024-05-06 21:46_

---

_Comment by @hauntsaninja on 2024-05-06 21:56_

Couldn't re-open, needed to make a new PR! https://github.com/astral-sh/uv/pull/3415 I added a mutex, but lmk if I should do flock stuff instead.

---
