```yaml
number: 2538
title: "No matching overload doesn't find base case"
type: issue
state: open
author: ntjohnson1
labels: []
assignees: []
created_at: 2026-01-16T21:52:42Z
updated_at: 2026-01-16T21:52:42Z
url: https://github.com/astral-sh/ty/issues/2538
synced_at: 2026-01-16T22:14:35Z
```

# No matching overload doesn't find base case

---

_@ntjohnson1_

### Summary

https://play.ty.dev/270322f2-5e01-4d4c-8d43-b13f366fc8de

If I do a trivial wrapper around an overloaded method it doesn't compare to the full function signature and only looks at the overload

Looks related but not the same error as here https://github.com/astral-sh/ty/issues/2383

### Version

ty 0.0.11 (830cb9cc6 2026-01-09)

---
