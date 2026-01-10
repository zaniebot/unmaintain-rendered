```yaml
number: 1778
title: "Consider specializing multiple `ParamSpec` using a common behavioral supertype"
type: issue
state: open
author: dhruvmanila
labels:
  - typing semantics
assignees: []
created_at: 2025-12-05T15:00:13Z
updated_at: 2025-12-05T15:01:14Z
url: https://github.com/astral-sh/ty/issues/1778
synced_at: 2026-01-10T01:56:41Z
```

# Consider specializing multiple `ParamSpec` using a common behavioral supertype

---

_Issue opened by @dhruvmanila on 2025-12-05 15:00_

> Just as with traditional `TypeVars`, a user may include the same `ParamSpec` multiple times in the arguments of the same function, to indicate a dependency between multiple arguments. In these cases a type checker may choose to solve to a common behavioral supertype (i.e. a set of parameters for which all of the valid calls are valid in both of the subtypes), but is not obligated to do so.
>
> Ref: https://typing.python.org/en/latest/spec/generics.html#semantics

https://github.com/astral-sh/ruff/pull/21445 does not add this functionality but it seems good to have.

---

_Comment by @dhruvmanila on 2025-12-05 15:00_

The spec clearly states that the type checkers are not obligated to implement this so I'm not adding this to Stable.

---

_Label `typing semantics` added by @AlexWaygood on 2025-12-05 15:01_

---
