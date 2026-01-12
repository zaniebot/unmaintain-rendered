```yaml
number: 1591
title: "Fix PT006 autofix of parametrize name strings like `'   first, ,  second  '`"
type: pull_request
state: merged
author: bluetech
labels: []
assignees: []
merged: true
base: main
head: pt006-whitespace
created_at: 2023-01-03T10:08:53Z
updated_at: 2023-01-03T13:12:09Z
url: https://github.com/astral-sh/ruff/pull/1591
synced_at: 2026-01-12T15:55:06Z
```

# Fix PT006 autofix of parametrize name strings like `'   first, ,  second  '`

---

_@bluetech_

pytest trims and ignores empty names.

Previously the autofix would be to `('   first', ' ', '    second  ')`.

Now it's `('first', 'second')`.

---

_Merged by @charliermarsh on 2023-01-03 13:12_

---

_Closed by @charliermarsh on 2023-01-03 13:12_

---
