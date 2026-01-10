```yaml
number: 12163
title: Add standalone installers to Ruff installation in README
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/installers
created_at: 2024-07-03T04:48:53Z
updated_at: 2024-07-03T19:13:40Z
url: https://github.com/astral-sh/ruff/pull/12163
synced_at: 2026-01-10T21:56:00Z
```

# Add standalone installers to Ruff installation in README

---

_Pull request opened by @zanieb on 2024-07-03 04:48_

Note this is already included in our installation page at `docs/installation.md`

---

_Label `documentation` added by @zanieb on 2024-07-03 04:48_

---

_Review comment by @dhruvmanila on `README.md`:140 on 2024-07-03 05:27_

```suggestion
# With pip.
pip install ruff

# With pipx.
pipx install ruff
```

---

_@dhruvmanila approved on 2024-07-03 05:29_

Do we want to keep the same order as in `docs/installation.md`?

---

_Comment by @zanieb on 2024-07-03 12:36_

I'm not sure, I took the order from uv @charliermarsh 

---

_Merged by @zanieb on 2024-07-03 18:39_

---

_Closed by @zanieb on 2024-07-03 18:39_

---

_Branch deleted on 2024-07-03 18:40_

---

_Comment by @charliermarsh on 2024-07-03 19:13_

Sorry, I missed this, but I would say they go below PyPI. I can change it if you want.

---
