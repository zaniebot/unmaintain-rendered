```yaml
number: 2538
title: "No matching overload doesn't find base case"
type: issue
state: open
author: ntjohnson1
labels: []
assignees: []
created_at: 2026-01-16T21:52:42Z
updated_at: 2026-01-16T22:40:19Z
url: https://github.com/astral-sh/ty/issues/2538
synced_at: 2026-01-16T23:05:37Z
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

_Comment by @MeGaGiGaGon on 2026-01-16 22:40_

This is the expected behavior of how overloads work, the implementation is not considered when doing overload matching:
https://typing.python.org/en/latest/spec/overload.html#overload-call-evaluation
> Only the `overloads` (the `@overload`-decorated signatures) should be considered for matching purposes. The implementation, if provided, should be ignored for purposes of overload matching.

---
