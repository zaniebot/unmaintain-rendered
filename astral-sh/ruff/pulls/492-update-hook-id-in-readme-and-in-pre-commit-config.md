```yaml
number: 492
title: Update hook id in README and in .pre-commit-config.yaml
type: pull_request
state: merged
author: tgross35
labels: []
assignees: []
merged: true
base: main
head: patch-1
created_at: 2022-10-27T21:23:14Z
updated_at: 2022-10-27T21:32:53Z
url: https://github.com/astral-sh/ruff/pull/492
synced_at: 2026-01-12T05:48:45Z
```

# Update hook id in README and in .pre-commit-config.yaml

---

_Pull request opened by @tgross35 on 2022-10-27 21:23_

_No description provided._

---

_Comment by @tgross35 on 2022-10-27 21:25_

I'm not sure if there was a reason the pre-commit hook was at 0.0.40, let me know if that needs to be reverted.

I'm also not sure if you're manually updating the example precommit version in the readme or if there's a script to do it that might have to change (I haven't found one)

---

_Comment by @charliermarsh on 2022-10-27 21:31_

👍 Totally fine to update from 0.40. There's no Python library code in the repo so it more serves as an example than anything else. (And I manually bump the versions everywhere prior to a release.)

---

_Merged by @charliermarsh on 2022-10-27 21:32_

---

_Closed by @charliermarsh on 2022-10-27 21:32_

---
