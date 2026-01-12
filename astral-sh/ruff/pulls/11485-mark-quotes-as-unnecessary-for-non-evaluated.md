```yaml
number: 11485
title: Mark quotes as unnecessary for non-evaluated annotations
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/runtime-annotations
created_at: 2024-05-21T20:05:03Z
updated_at: 2024-05-22T19:44:32Z
url: https://github.com/astral-sh/ruff/pull/11485
synced_at: 2026-01-12T15:55:38Z
```

# Mark quotes as unnecessary for non-evaluated annotations

---

_@charliermarsh_

## Summary

Similar to #11414, this PR extends `UP037` to flag quoted annotations that are located in positions that won't be evaluated at runtime.

For example, the quotes on `Tuple` are unnecessary in:

```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from typing import Tuple


def foo():
    x: "Tuple[int, int]" = (0, 0)

foo()
```

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-05-21 20:05_

---

_Label `rule` added by @charliermarsh on 2024-05-21 20:05_

---

_Merged by @charliermarsh on 2024-05-22 19:44_

---

_Closed by @charliermarsh on 2024-05-22 19:44_

---

_Branch deleted on 2024-05-22 19:44_

---
