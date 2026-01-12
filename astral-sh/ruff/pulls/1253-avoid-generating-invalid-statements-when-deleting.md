```yaml
number: 1253
title: Avoid generating invalid statements when deleting from multi-statement lines
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/semicolon
created_at: 2022-12-15T22:04:25Z
updated_at: 2022-12-16T03:17:32Z
url: https://github.com/astral-sh/ruff/pull/1253
synced_at: 2026-01-12T05:36:31Z
```

# Avoid generating invalid statements when deleting from multi-statement lines

---

_Pull request opened by @charliermarsh on 2022-12-15 22:04_

Resolves #1226.

---

_Comment by @charliermarsh on 2022-12-15 23:23_

I'm probably just going to disable autofix in cases in which we can't be 100% sure that we'll generate valid syntax (like multi-statement lines -- we can handle them in some, but not all cases).

---

_Merged by @charliermarsh on 2022-12-16 03:17_

---

_Closed by @charliermarsh on 2022-12-16 03:17_

---

_Branch deleted on 2022-12-16 03:17_

---
