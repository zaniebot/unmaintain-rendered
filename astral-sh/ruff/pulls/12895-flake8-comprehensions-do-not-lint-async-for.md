```yaml
number: 12895
title: "[flake8-comprehensions] Do not lint `async for` comprehensions in `unnecessary-comprehension-in-call (C419)`"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
assignees: []
merged: true
base: main
head: c419-no-async
created_at: 2024-08-14T17:27:48Z
updated_at: 2024-08-15T02:27:40Z
url: https://github.com/astral-sh/ruff/pull/12895
synced_at: 2026-01-10T21:38:32Z
```

# [flake8-comprehensions] Do not lint `async for` comprehensions in `unnecessary-comprehension-in-call (C419)`

---

_Pull request opened by @dylwil3 on 2024-08-14 17:27_

List and set comprehensions using `async for` cannot be replaced with underlying generators; this PR modifies C419 to skip such comprehensions. 

Closes #12891.


---

_Label `bug` added by @MichaReiser on 2024-08-14 17:39_

---

_Comment by @github-actions[bot] on 2024-08-14 17:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @charliermarsh by @charliermarsh on 2024-08-15 00:52_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-15 00:52_

---

_@charliermarsh approved on 2024-08-15 00:55_

Awesome, thanks so much! Really appreciating your contributions.

---

_Merged by @charliermarsh on 2024-08-15 01:00_

---

_Closed by @charliermarsh on 2024-08-15 01:00_

---

_Branch deleted on 2024-08-15 02:27_

---
