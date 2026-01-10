```yaml
number: 3511
title: Allow unknown pyproject.toml fields
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/allow-unknown-fields
created_at: 2024-05-10T16:56:46Z
updated_at: 2024-05-10T18:50:25Z
url: https://github.com/astral-sh/uv/pull/3511
synced_at: 2026-01-10T14:37:54Z
```

# Allow unknown pyproject.toml fields

---

_Pull request opened by @konstin on 2024-05-10 16:56_

Fixes #3510, we use typo error messages though.

Tested manually by adding `[tool.uv.pip]`, we should add proper tests for this feature.

---

_Comment by @charliermarsh on 2024-05-10 18:35_

But why is `tool.uv.pip` unknown?

---

_Comment by @charliermarsh on 2024-05-10 18:35_

Oh this is elsewhere.

---

_@charliermarsh approved on 2024-05-10 18:35_

---

_Merged by @charliermarsh on 2024-05-10 18:50_

---

_Closed by @charliermarsh on 2024-05-10 18:50_

---

_Branch deleted on 2024-05-10 18:50_

---
