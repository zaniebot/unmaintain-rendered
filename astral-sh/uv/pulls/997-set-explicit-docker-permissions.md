```yaml
number: 997
title: Set explicit Docker permissions
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/perm
created_at: 2024-01-19T02:44:25Z
updated_at: 2024-01-19T05:29:30Z
url: https://github.com/astral-sh/uv/pull/997
synced_at: 2026-01-10T15:39:03Z
```

# Set explicit Docker permissions

---

_Pull request opened by @charliermarsh on 2024-01-19 02:44_

_No description provided._

---

_@zanieb reviewed on 2024-01-19 03:06_

---

_Review comment by @zanieb on `Cargo.toml`:171 on 2024-01-19 03:06_

Do they not support this? Do we lose all the safety guarantees of release won't run if the generated workflow is modified?

---

_@charliermarsh reviewed on 2024-01-19 03:19_

---

_Review comment by @charliermarsh on `Cargo.toml`:171 on 2024-01-19 03:19_

No, yes. Please bear with me, there was a lot of discussion about this in the Axo Discord, I am trying to understand GitHub's permission model. We should not be publishing from a build step (we should be publishing from a publish step), but GitHub doesn't seem to allow us to build a multi-platform image and publish it in a separate step. I spent time trying to figure it out earlier this week.


---

_@zanieb approved on 2024-01-19 03:37_

---

_@charliermarsh reviewed on 2024-01-19 03:39_

---

_Review comment by @charliermarsh on `Cargo.toml`:171 on 2024-01-19 03:39_

(To be clear, I'm testing off this branch, so I'm hoping I won't have to merge this. If I do, it will be temporary as I'll ask Axo team for help.)

---

_Comment by @charliermarsh on 2024-01-19 05:16_

Okay, merging this for now while I work with the Axo team to remove it.

---

_Merged by @charliermarsh on 2024-01-19 05:29_

---

_Closed by @charliermarsh on 2024-01-19 05:29_

---

_Branch deleted on 2024-01-19 05:29_

---
