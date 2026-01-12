```yaml
number: 8410
title: Use production builds of Ruff for pre-commit
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zanie/pre-commit
created_at: 2023-11-01T16:51:19Z
updated_at: 2023-11-01T21:33:11Z
url: https://github.com/astral-sh/ruff/pull/8410
synced_at: 2026-01-10T23:40:55Z
```

# Use production builds of Ruff for pre-commit

---

_Pull request opened by @zanieb on 2023-11-01 16:51_

Requiring `cargo build` per commit is way too slow. Instead, we use the production Ruff version. Additionally, Black is replaced with the Ruff formatter.

---

_Label `internal` added by @zanieb on 2023-11-01 16:51_

---

_Comment by @zanieb on 2023-11-01 16:52_

This has the downside of not being particularly easy to bump (not automated, can't be done until after we finish a release). See #8411 

---

_@charliermarsh approved on 2023-11-01 21:27_

Great, makes sense to me.

---

_Merged by @zanieb on 2023-11-01 21:33_

---

_Closed by @zanieb on 2023-11-01 21:33_

---

_Branch deleted on 2023-11-01 21:33_

---
