```yaml
number: 1861
title: "Validate more usages of `ParamSpec`, `P.args`, `P.kwargs`"
type: issue
state: open
author: dhruvmanila
labels:
  - diagnostics
  - calls
assignees: []
created_at: 2025-12-12T10:13:12Z
updated_at: 2025-12-12T10:13:12Z
url: https://github.com/astral-sh/ty/issues/1861
synced_at: 2026-01-10T01:55:00Z
```

# Validate more usages of `ParamSpec`, `P.args`, `P.kwargs`

---

_Issue opened by @dhruvmanila on 2025-12-12 10:13_

https://github.com/astral-sh/ruff/pull/21445 did not focus much on validating whether `ParamSpec`, `P.args` and `P.kwargs` were used correctly.

This issue is to keep track of the remaining work. Most of these cases should have an existing TODO in [`generics/legacy/paramspec.md`](https://github.com/astral-sh/ruff/blob/2d0681da082b6678f342d768b2c84cac15d829e9/crates/ty_python_semantic/resources/mdtest/generics/legacy/paramspec.md) and [`generics/pep695/paramspec.md`](https://github.com/astral-sh/ruff/blob/2d0681da082b6678f342d768b2c84cac15d829e9/crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md) but if not then new tests should be added.

- [ ] `ParamSpec` is only valid as the first element to `Callable` or the final element to `Concatenate` (requires #1535), we should raise a `invalid-type-form` for other places
- [ ] When both `P.args` and `P.kwargs` aren't defined in the function signature e.g., `def foo(*args: P.args): ...` and `def foo(**kwargs: P.kwargs): ...`
- [ ] When there's an additional parameter between `*args: P.args` and `**kwargs: P.kwargs` e.g., `def foo(*args: P.args, x: int, **kwargs: P.kwargs): ...`
- [ ] When it's used to annotate anything other than `*args` and `**kwargs` like variable, instance or class attribute, etc.
- [ ] On the argument side, we validate when there's a type mismatch between a `ParamSpec` components and the argument type but we also need to enforce that arguments for a `ParamSpec` are required to be passed in certain scenarios like https://github.com/astral-sh/ruff/blob/2d0681da082b6678f342d768b2c84cac15d829e9/crates/ty_python_semantic/resources/mdtest/generics/pep695/paramspec.md?plain=1#L184-L188

---
