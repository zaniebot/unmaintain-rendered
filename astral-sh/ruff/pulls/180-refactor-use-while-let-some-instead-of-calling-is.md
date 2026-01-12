```yaml
number: 180
title: "refactor: Use `while let Some` instead of calling `is_empty`"
type: pull_request
state: merged
author: Stranger6667
labels: []
assignees: []
merged: true
base: main
head: dd/while-let-some
created_at: 2022-09-13T12:32:40Z
updated_at: 2022-09-13T14:35:32Z
url: https://github.com/astral-sh/ruff/pull/180
synced_at: 2026-01-12T15:55:04Z
```

# refactor: Use `while let Some` instead of calling `is_empty`

---

_@Stranger6667_

This feels a bit more concise & should be a bit faster as one length check is gone

---

_Comment by @charliermarsh on 2022-09-13 14:06_

Good call, also avoids the unwrap.

---

_Merged by @charliermarsh on 2022-09-13 14:06_

---

_Closed by @charliermarsh on 2022-09-13 14:06_

---

_Branch deleted on 2022-09-13 14:35_

---
