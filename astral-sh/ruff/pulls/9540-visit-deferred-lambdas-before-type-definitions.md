```yaml
number: 9540
title: Visit deferred lambdas before type definitions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/cast
created_at: 2024-01-16T00:46:29Z
updated_at: 2024-01-16T01:08:41Z
url: https://github.com/astral-sh/ruff/pull/9540
synced_at: 2026-01-12T15:55:29Z
```

# Visit deferred lambdas before type definitions

---

_@charliermarsh_

## Summary

This is effectively the same problem as https://github.com/astral-sh/ruff/pull/9175. And this just papers over it again, though I'm gonna try a more holistic fix in a follow-up PR. The _real_ fix here is that we need to continue to visit deferred items until they're exhausted since, e.g., we still get this case wrong (flagging `re` as unused):

```python
import re

cast(lambda: re.match, 1)
```

Closes https://github.com/astral-sh/ruff/issues/9534.


---

_Label `bug` added by @charliermarsh on 2024-01-16 00:46_

---

_Comment by @codspeed-hq[bot] on 2024-01-16 00:53_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/cast)

### Merging #9540 will **degrade performances by 4.35%**

<sub>Comparing <code>charlie/cast</code> (5c424f0) with <code>main</code> (b983ab1)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/cast)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/cast` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 12.2 ms | 12.8 ms | -4.35% |


---

_Comment by @github-actions[bot] on 2024-01-16 00:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-01-16 01:08_

---

_Closed by @charliermarsh on 2024-01-16 01:08_

---

_Branch deleted on 2024-01-16 01:08_

---
