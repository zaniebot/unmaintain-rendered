```yaml
number: 3728
title: "Do not discover virtual environment interpreters in `uv venv --python ...`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
  - internal
assignees: []
merged: true
base: main
head: zb/venv-no-venv
created_at: 2024-05-21T23:42:31Z
updated_at: 2024-05-22T01:00:36Z
url: https://github.com/astral-sh/uv/pull/3728
synced_at: 2026-01-10T14:32:20Z
```

# Do not discover virtual environment interpreters in `uv venv --python ...`

---

_Pull request opened by @zanieb on 2024-05-21 23:42_

Otherwise `uv venv --python 3.12` can prefer `.venv/bin/python` over the system Python (which is always used if you don't provide a `--python` flag). I would find this confusing as a user.

---

_Label `bug` added by @zanieb on 2024-05-21 23:42_

---

_Comment by @zanieb on 2024-05-22 00:13_

I tried to write a test for this but it wasn't feasible 🤷‍♀️ 

Looked like https://github.com/astral-sh/uv/commit/a190289984ee4ee7d1a7165e30fc69a33b159da6 — didn't catch a regression.

---

_Marked ready for review by @zanieb on 2024-05-22 00:14_

---

_Review requested from @charliermarsh by @zanieb on 2024-05-22 00:14_

---

_@charliermarsh approved on 2024-05-22 00:19_

---

_Merged by @zanieb on 2024-05-22 01:00_

---

_Closed by @zanieb on 2024-05-22 01:00_

---

_Branch deleted on 2024-05-22 01:00_

---

_Label `internal` added by @zanieb on 2024-05-22 01:00_

---

_Comment by @zanieb on 2024-05-22 01:00_

Omitting from the changelog since it's unreleased

---
