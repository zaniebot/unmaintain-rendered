```yaml
number: 326
title: "Remove unnecessary Option wrapper from some pyproject::Config fields"
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: pyproject-option
created_at: 2022-10-04T20:07:03Z
updated_at: 2022-10-04T20:29:39Z
url: https://github.com/astral-sh/ruff/pull/326
synced_at: 2026-01-12T15:55:04Z
```

# Remove unnecessary Option wrapper from some pyproject::Config fields

---

_@andersk_

For `extend_exclude`, `ignore`, and `per_file_ignores`, `None` has the same effect as `Some(vec![])`, so there’s no need to encode it separately.

This is not the case for `exclude` and `select`, so I’ve left those alone.


---

_Merged by @charliermarsh on 2022-10-04 20:27_

---

_Closed by @charliermarsh on 2022-10-04 20:27_

---

_Comment by @charliermarsh on 2022-10-04 20:27_

Good stuff.

---

_Branch deleted on 2022-10-04 20:29_

---
