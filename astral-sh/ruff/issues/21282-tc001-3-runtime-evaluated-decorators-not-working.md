---
number: 21282
title: TC001-3 runtime-evaluated-decorators not working with generic framework decorators
type: issue
state: open
author: hartungstenio
labels: []
assignees: []
created_at: 2025-11-05T14:17:29Z
updated_at: 2025-11-05T21:47:22Z
url: https://github.com/astral-sh/ruff/issues/21282
synced_at: 2026-01-10T01:23:02Z
---

# TC001-3 runtime-evaluated-decorators not working with generic framework decorators

---

_Issue opened by @hartungstenio on 2025-11-05 14:17_

### Summary

Pydantic AI's `FunctionToolSet` is a `Generic` class providing a `@toolset.tool` decorator.

`runtime-evaluated-decorators` works without providing the generic type: https://play.ruff.rs/5267d988-584d-4a3e-88d9-4b48cac7fbbc
```python
toolset = FunctionToolset()
```

but it is now working when providing the generic type: https://play.ruff.rs/4e5c8e8c-f76a-4e77-91a5-3c89ca0624d1
```python
toolset = FunctionToolset[Deps]()
```



### Version

0.14.3

---

_Comment by @Daverball on 2025-11-05 21:43_

This like #20637 looks like another limitation of the very basic type inference in ruff. IIRC there's been another issue where inference failed in a different way. We can probably make the inference slightly more sophisticated than it currently is to at least work around some of these edge cases. This case definitely seems easier to support than the case in the linked issue.

What always works as a workaround in these cases is to add `toolset` with its fully qualified name to `runtime-evaluated-decorators` in addition to `FunctionToolset`. You would need to do this anyway, if you want `@toolset.tool` to work correctly if you access it from a different module.

---
