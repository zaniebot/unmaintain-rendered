```yaml
number: 855
title: "Consider removing `infer_expression_type` query"
type: issue
state: open
author: MichaReiser
labels:
  - performance
  - memory
assignees: []
created_at: 2025-07-20T11:28:57Z
updated_at: 2025-07-20T15:59:49Z
url: https://github.com/astral-sh/ty/issues/855
synced_at: 2026-01-10T02:07:36Z
```

# Consider removing `infer_expression_type` query

---

_Issue opened by @MichaReiser on 2025-07-20 11:28_

The original motivation why we introduced the `infer_expression_type` was because we wanted to avoid that ty ever infers the same expression twice and that this is important for performance. https://github.com/astral-sh/ruff/pull/19436 shows that this might not be the case (and it's a very naive approach at removing the query). Most benchmarks are neutral with the exception of sympy (11% regresssion).

Removing the query is motivated by the fact that the cached query results can take up up to 3GB of memory (struct fields + metadata) in large projects (~10% of overall memory usage) and this is without accounting for the `Expression` ingredients (another 500MB). 



---

_Label `memory` added by @MichaReiser on 2025-07-20 11:28_

---

_Comment by @MichaReiser on 2025-07-20 15:58_

A less extreme variant of this could be to only create standalone expression that are used in reachability constraints, as these tend to be evaluated in fairly hot code and the evaluation of reachability constraints isn't cached itself. 

The alternative is that we try to cache reachability constraint resolution

---

_Label `performance` added by @MichaReiser on 2025-07-20 15:59_

---
