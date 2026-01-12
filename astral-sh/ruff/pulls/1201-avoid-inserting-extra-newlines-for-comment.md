```yaml
number: 1201
title: Avoid inserting extra newlines for comment-delimited import blocks
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/blanks
created_at: 2022-12-11T22:09:57Z
updated_at: 2022-12-11T22:13:11Z
url: https://github.com/astral-sh/ruff/pull/1201
synced_at: 2026-01-12T15:55:05Z
```

# Avoid inserting extra newlines for comment-delimited import blocks

---

_@charliermarsh_

Tested on the `zulip` codebase.

Resolves #1193.


---

_Review comment by @charliermarsh on `src/isort/helpers.rs`:38 on 2022-12-11 22:10_

Not very happy with this but it's conceptually very difficult to support without some kind of text parsing as long as we're operating on the AST.

---

_@charliermarsh reviewed on 2022-12-11 22:10_

---

_Merged by @charliermarsh on 2022-12-11 22:13_

---

_Closed by @charliermarsh on 2022-12-11 22:13_

---

_Branch deleted on 2022-12-11 22:13_

---
