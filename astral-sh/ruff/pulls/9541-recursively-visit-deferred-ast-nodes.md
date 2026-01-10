```yaml
number: 9541
title: Recursively visit deferred AST nodes
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/deferred
created_at: 2024-01-16T01:12:53Z
updated_at: 2024-01-16T01:34:39Z
url: https://github.com/astral-sh/ruff/pull/9541
synced_at: 2026-01-10T22:57:09Z
```

# Recursively visit deferred AST nodes

---

_Pull request opened by @charliermarsh on 2024-01-16 01:12_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR is a more holistic fix for https://github.com/astral-sh/ruff/issues/9534 and https://github.com/astral-sh/ruff/issues/9159.

When we visit the AST, we track nodes that we need to visit _later_ (deferred nodes). For example, when visiting a function, we defer the function body, since we don't want to visit the body until we've visited the rest of the statements in the containing scope.

However, deferred nodes can themselves contain deferred nodes... For example, a function body can contain a lambda (which contains a deferred body). And then there are rarer cases, like a lambda inside of a type annotation.

The aforementioned issues were fixed by reordering the deferral visits to catch common cases. But even with those fixes, we still fail on cases like:

```python
from __future__ import annotations

import re
from typing import cast

cast(lambda: re.match, 1)
```

Since we don't expect lambdas to appear inside of type definitions.

This PR modifies the `Checker` to keep visiting until all the deferred stacks are empty. We _already_ do this for any one kind of deferred node; now, we do it for _all_ of them at a level above.


---

_Renamed from "Charlie/deferred" to "Recursively visit deferred AST nodes" by @charliermarsh on 2024-01-16 01:14_

---

_Label `bug` added by @charliermarsh on 2024-01-16 01:14_

---

_Label `internal` added by @charliermarsh on 2024-01-16 01:14_

---

_Label `internal` removed by @charliermarsh on 2024-01-16 01:14_

---

_Comment by @codspeed-hq[bot] on 2024-01-16 01:26_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/deferred)

### Merging #9541 will **improve performances by 4.55%**

<sub>Comparing <code>charlie/deferred</code> (ef91b4f) with <code>main</code> (da275b8)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/deferred` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[numpy/ctypeslib.py]` | 12.8 ms | 12.2 ms | +4.55% |


---

_Comment by @github-actions[bot] on 2024-01-16 01:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-01-16 01:34_

---

_Closed by @charliermarsh on 2024-01-16 01:34_

---

_Branch deleted on 2024-01-16 01:34_

---
