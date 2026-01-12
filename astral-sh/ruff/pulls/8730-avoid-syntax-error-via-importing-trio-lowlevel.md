```yaml
number: 8730
title: Avoid syntax error via importing trio.lowlevel
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/trio-lowlevel
created_at: 2023-11-17T01:03:18Z
updated_at: 2023-11-17T01:15:05Z
url: https://github.com/astral-sh/ruff/pull/8730
synced_at: 2026-01-10T23:40:55Z
```

# Avoid syntax error via importing trio.lowlevel

---

_Pull request opened by @charliermarsh on 2023-11-17 01:03_

We ended up with a syntax error here via `from trio import lowlevel.checkpoint`. The new solution avoids that error, but does miss cases like:

```py
from trio.lowlevel import Timer
```

Where it could insert `from trio.lowlevel import Timer, checkpoint`. Instead, it'll add `from trio import lowlevel`.

See: https://github.com/astral-sh/ruff/issues/8402#issuecomment-1810838129



---

_Label `bug` added by @charliermarsh on 2023-11-17 01:04_

---

_Merged by @charliermarsh on 2023-11-17 01:08_

---

_Closed by @charliermarsh on 2023-11-17 01:08_

---

_Branch deleted on 2023-11-17 01:08_

---

_Comment by @github-actions[bot] on 2023-11-17 01:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
