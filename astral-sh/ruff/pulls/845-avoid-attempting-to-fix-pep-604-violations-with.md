```yaml
number: 845
title: Avoid attempting to fix PEP 604 violations with deferred annotations
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/U007
created_at: 2022-11-21T02:41:49Z
updated_at: 2022-11-21T02:41:56Z
url: https://github.com/astral-sh/ruff/pull/845
synced_at: 2026-01-12T15:55:05Z
```

# Avoid attempting to fix PEP 604 violations with deferred annotations

---

_@charliermarsh_

We now avoid fixing or flagging annotations like `Union["Foo", "Bar"]`. We _also_ avoid fixing or flagging annotations like `"Union[Foo, Bar]"`. These are safer behaviors and also mimic `pyupgrade`.

Resolves #826.

---

_Merged by @charliermarsh on 2022-11-21 02:41_

---

_Closed by @charliermarsh on 2022-11-21 02:41_

---

_Branch deleted on 2022-11-21 02:41_

---
