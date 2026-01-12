```yaml
number: 224
title: "fix: Use UTF-32 char count for line length (#223)"
type: pull_request
state: merged
author: sgryjp
labels: []
assignees: []
merged: true
base: main
head: fix/utf32-line-length
created_at: 2022-09-18T10:06:10Z
updated_at: 2022-10-28T03:56:00Z
url: https://github.com/astral-sh/ruff/pull/224
synced_at: 2026-01-12T05:48:45Z
```

# fix: Use UTF-32 char count for line length (#223)

---

_Pull request opened by @sgryjp on 2022-09-18 10:06_

This should make ruff compatible with flake8 (pycodestyle) on checking length of a line which contains non-single-byte characters.

---

_Comment by @charliermarsh on 2022-09-18 15:17_

Good catch! Makes sense, I just wanna benchmark before merging.

---

_Merged by @charliermarsh on 2022-09-18 16:45_

---

_Closed by @charliermarsh on 2022-09-18 16:45_

---

_Comment by @charliermarsh on 2022-09-18 16:45_

Thank you!

---

_Branch deleted on 2022-10-28 03:56_

---
