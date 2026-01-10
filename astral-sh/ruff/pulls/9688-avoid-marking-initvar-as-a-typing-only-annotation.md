```yaml
number: 9688
title: "Avoid marking `InitVar` as a typing-only annotation"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/init-var
created_at: 2024-01-29T20:36:16Z
updated_at: 2024-01-29T21:27:21Z
url: https://github.com/astral-sh/ruff/pull/9688
synced_at: 2026-01-10T22:57:09Z
```

# Avoid marking `InitVar` as a typing-only annotation

---

_Pull request opened by @charliermarsh on 2024-01-29 20:36_

## Summary

Given:

```python
from dataclasses import InitVar, dataclass

@dataclass
class C:
    i: int
    j: int = None
    database: InitVar[DatabaseType] = None

    def __post_init__(self, database):
        if self.j is None and database is not None:
            self.j = database.lookup('j')

c = C(10, database=my_database)
```

We should avoid marking `InitVar` as typing-only, since it _is_ required by the dataclass at runtime.

Note that by default, we _already_ don't flag this, since the `@dataclass` member is needed at runtime too -- so it's only a problem with `strict` mode.

Closes https://github.com/astral-sh/ruff/issues/9666.

---

_Label `bug` added by @charliermarsh on 2024-01-29 20:38_

---

_Renamed from "Avoid marking InitVar as typing-only" to "Avoid marking `InitVar` as a typing-only annotation" by @charliermarsh on 2024-01-29 20:46_

---

_Comment by @github-actions[bot] on 2024-01-29 20:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @charliermarsh on 2024-01-29 20:54_

Ah wow that's a great ecosystem result -- this implementation actually isn't quite correct.

---

_Merged by @charliermarsh on 2024-01-29 21:27_

---

_Closed by @charliermarsh on 2024-01-29 21:27_

---

_Branch deleted on 2024-01-29 21:27_

---
