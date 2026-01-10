```yaml
number: 397
title: "Remove `__pycache__` directories when uninstalling"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pycache
created_at: 2023-11-10T18:26:11Z
updated_at: 2023-11-10T19:55:34Z
url: https://github.com/astral-sh/uv/pull/397
synced_at: 2026-01-10T15:50:28Z
```

# Remove `__pycache__` directories when uninstalling

---

_Pull request opened by @charliermarsh on 2023-11-10 18:26_

According to the [packaging documentation](https://packaging.python.org/en/latest/specifications/binary-distribution-format/#binary-distribution-format), "uninstallers should be smart enough to remove .pyc even if it is not mentioned in RECORD". Previously, we weren't handling this case, so if you installed via Puffin, then imported a file (to trigger bytecode compilation), then uninstalled, we'd leave spare `__pycache__` directories around.

Closes https://github.com/astral-sh/puffin/issues/395.

---

_Label `bug` added by @charliermarsh on 2023-11-10 18:26_

---

_Merged by @charliermarsh on 2023-11-10 19:55_

---

_Closed by @charliermarsh on 2023-11-10 19:55_

---

_Branch deleted on 2023-11-10 19:55_

---
