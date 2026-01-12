```yaml
number: 23
title: Allow overwrite the branch name
type: pull_request
state: closed
author: karpetrosyan
labels:
  - enhancement
assignees: []
base: main
head: add-branch-name-variable
created_at: 2024-03-23T16:52:04Z
updated_at: 2025-09-08T13:12:08Z
url: https://github.com/zanieb/rooster/pull/23
synced_at: 2026-01-12T16:14:18Z
```

# Allow overwrite the branch name

---

_@karpetrosyan_

Hey @zanieb!

This is a small PR that allows us to overwrite the branch name which is 'main' by default.

---

_@zanieb reviewed on 2024-03-24 16:35_

---

_Review comment by @zanieb on `src/rooster/_git.py`:56 on 2024-03-24 16:35_

Thanks for the pull requests!

Could we read this from the `Config` instead of using an environment variable?

https://github.com/zanieb/rooster/blob/main/src/rooster/_config.py

---

_Label `enhancement` added by @zanieb on 2024-03-24 16:49_

---

_Comment by @zanieb on 2025-09-08 13:12_

We just use the current branch now

---

_Closed by @zanieb on 2025-09-08 13:12_

---
