```yaml
number: 244
title: Adjust line numbers when reporting rules in f-strings
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/f-string
created_at: 2022-09-21T16:56:46Z
updated_at: 2022-09-21T17:42:59Z
url: https://github.com/astral-sh/ruff/pull/244
synced_at: 2026-01-12T15:55:04Z
```

# Adjust line numbers when reporting rules in f-strings

---

_@charliermarsh_

The best we can do until this is fixed in the parser itself: whenever reporting a rule in an f-string, just use the location of the f-string itself. This will be wrong in some cases, but at least it will be close.

Resolves #145.


---

_Merged by @charliermarsh on 2022-09-21 17:42_

---

_Closed by @charliermarsh on 2022-09-21 17:42_

---

_Branch deleted on 2022-09-21 17:42_

---
