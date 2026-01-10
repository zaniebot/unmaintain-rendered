```yaml
number: 2261
title: Rules ignored with Unknown types
type: issue
state: closed
author: mxrch
labels: []
assignees: []
created_at: 2025-12-29T16:12:08Z
updated_at: 2025-12-29T16:20:18Z
url: https://github.com/astral-sh/ty/issues/2261
synced_at: 2026-01-10T01:56:41Z
```

# Rules ignored with Unknown types

---

_Issue opened by @mxrch on 2025-12-29 16:12_

### Summary

There seems to be an issue if, in the following Playground examples, the variable "output" is Unknown instead of str. If "line" is str, "output" will be str, and the rules will work and report an error, otherwise, it seems to ignore (while Pyright reports the error correctly).

## Example 1 : Non-iterable int
https://play.ty.dev/d8a0c386-7003-45f9-962f-045ff5f1ce42

## Example 2 : Math operations not supported on None
https://play.ty.dev/dc296cdb-7050-404c-90c6-79d563869b7a

## Example 3 : "output" is correctly typed, and the rules are not ignored anymore
https://play.ty.dev/d7bd629c-66a4-4ec3-bd37-1702be0c38f1

### Version

_No response_

---

_Comment by @carljm on 2025-12-29 16:20_

Thanks for the report! This is actually a duplicate of #136. The problem is that `line` is type `Unknown`, which means we infer `Unknown` for the variable `position` as well, even though you've annotated it as `int`. (We currently treat `int` as the "upper bound" on its type, but since `Unknown` is assignable to `int`, we allow an inferred type of `Unknown` for it.) We plan to change this. So with `position` as `Unknown`, `None * position` is valid, since `position` could be some time which implements `__rmul__` to accept `None`.

---

_Closed by @carljm on 2025-12-29 16:20_

---
