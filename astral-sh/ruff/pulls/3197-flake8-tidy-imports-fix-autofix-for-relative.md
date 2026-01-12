```yaml
number: 3197
title: "[`flake8-tidy-imports`] fix autofix for relative imports"
type: pull_request
state: merged
author: sciyoshi
labels: []
assignees: []
merged: true
base: main
head: tid252-fix-incorrect-module
created_at: 2023-02-24T03:27:03Z
updated_at: 2023-11-22T20:21:28Z
url: https://github.com/astral-sh/ruff/pull/3197
synced_at: 2026-01-12T15:55:12Z
```

# [`flake8-tidy-imports`] fix autofix for relative imports

---

_@sciyoshi_

The [previous PR to improve the autofix for TID252](https://github.com/charliermarsh/ruff/pull/2990) contains a bug: notice how `from .. import server` becomes `from my_package.sublib.sublib import server` in the test (`sublib` becomes incorrectly duplicated). This PR fixes that issue, although I admit I don't fully understand the code here.

---

_Merged by @charliermarsh on 2023-02-24 04:40_

---

_Closed by @charliermarsh on 2023-02-24 04:40_

---

_Branch deleted on 2023-11-22 20:21_

---
