```yaml
number: 9306
title: "[`pyupgrade`] Document more fix unsafety for UP007"
type: pull_request
state: merged
author: hauntsaninja
labels:
  - documentation
assignees: []
merged: true
base: main
head: up007-unsafe
created_at: 2023-12-29T09:38:51Z
updated_at: 2024-01-02T00:38:40Z
url: https://github.com/astral-sh/ruff/pull/9306
synced_at: 2026-01-10T23:07:18Z
```

# [`pyupgrade`] Document more fix unsafety for UP007

---

_Pull request opened by @hauntsaninja on 2023-12-29 09:38_

```
from typing import Optional
x = "asdf"
def foo(a: Optional[x]):
    pass
```

It's not uncommon to see issues like this with multiprocessing, where things are actually methods instead of types

Relates to #8482

---

_Comment by @github-actions[bot] on 2023-12-29 09:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2023-12-29 12:41_

---

_Label `documentation` added by @charliermarsh on 2023-12-29 12:41_

---

_Merged by @charliermarsh on 2023-12-29 12:41_

---

_Closed by @charliermarsh on 2023-12-29 12:41_

---

_Comment by @charliermarsh on 2023-12-29 12:41_

Thanks! 

---

_Branch deleted on 2023-12-29 18:35_

---
