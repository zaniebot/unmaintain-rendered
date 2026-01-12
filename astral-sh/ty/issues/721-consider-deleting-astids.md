```yaml
number: 721
title: "Consider deleting `AstIds`"
type: issue
state: closed
author: MichaReiser
labels:
  - memory
assignees: []
created_at: 2025-06-28T18:23:11Z
updated_at: 2025-07-02T15:57:33Z
url: https://github.com/astral-sh/ty/issues/721
synced_at: 2026-01-12T15:54:23Z
```

# Consider deleting `AstIds`

---

_@MichaReiser_

We introduced `AstIds` to get IDs that are local to a single scope. This has the benefit that local changes in one scope don't change IDs for other scopes. However, we don't really use the `ScopedExpressionId` in any place where this would be important (e.g. a scope local query result). I think this is mainly because we have `Expression` which accomplishes the same and is often more useful. 

We should explore if we can remove `AstIds` by directly using `ExpressionNodeKey` instead. This should help reduce memory usage a fair amount because expressions are fairly common.

CC: @ibraheemdev 



---

_Label `memory` added by @MichaReiser on 2025-06-28 18:23_

---

_Closed by @sharkdp on 2025-07-02 15:57_

---
