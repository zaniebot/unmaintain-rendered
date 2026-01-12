```yaml
number: 2810
title: "[`numpy`] deprecated type aliases"
type: pull_request
state: merged
author: sbrugman
labels: []
assignees: []
merged: true
base: main
head: numpy-specific-rules
created_at: 2023-02-12T12:25:57Z
updated_at: 2023-02-14T23:47:03Z
url: https://github.com/astral-sh/ruff/pull/2810
synced_at: 2026-01-12T15:55:11Z
```

# [`numpy`] deprecated type aliases

---

_@sbrugman_

Closes https://github.com/charliermarsh/ruff/issues/2455

Used `NPY` as prefix code as agreed in the issue.

---

_Comment by @charliermarsh on 2023-02-13 02:31_

Yeah, putting this under `numpy` makes sense -- as does removing any link to PyPI or similar.

---

_Converted to draft by @sbrugman on 2023-02-13 15:28_

---

_Renamed from "[`flake8-numpy`] deprecated type aliases" to "[`numpy`] deprecated type aliases" by @sbrugman on 2023-02-13 15:29_

---

_Marked ready for review by @sbrugman on 2023-02-13 15:30_

---

_Comment by @sbrugman on 2023-02-13 15:37_

This is the first rule that does fit in `RUF` and any other existing plugin. Used the same structure: Numpy-specific rules. Only a minor change to the macros was needed.

Caught a minimal 'bug' in docs generation

---

_Merged by @charliermarsh on 2023-02-14 23:45_

---

_Closed by @charliermarsh on 2023-02-14 23:45_

---

_Branch deleted on 2023-02-14 23:47_

---
