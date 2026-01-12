```yaml
number: 17448
title: "[red-knot] Add some narrowing for assignment expressions"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: narrowing-for-assignment-expressions
created_at: 2025-04-17T12:27:32Z
updated_at: 2025-04-19T13:59:12Z
url: https://github.com/astral-sh/ruff/pull/17448
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Add some narrowing for assignment expressions

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #14866
Fixes #17437

## Test Plan

Update mdtests in `narrow/`


---

_Review requested from @carljm by @MatthewMckee4 on 2025-04-17 12:27_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-04-17 12:27_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-04-17 12:27_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-04-17 12:27_

---

_Renamed from "Add narrowing for assignment expressions for bool and call" to "[red-knot] Add narrowing for assignment expressions for bool and call" by @MatthewMckee4 on 2025-04-17 12:27_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-17 12:28_

---

_Comment by @MatthewMckee4 on 2025-04-17 12:28_

There is still more to cover regarding named expressions. Such as:
```py
def f() -> bool: ...

if x := f():
    reveal_type(x)  # revealed: Literal[True]
else:
    reveal_type(x)  # revealed: Literal[False]
```

---

_Comment by @github-actions[bot] on 2025-04-17 12:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
psycopg (https://github.com/psycopg/psycopg)
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/rows.py:215:12: Operator `<` is not supported for types `None` and `int`, in comparing `int | None` with `Literal[1]`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/sql.py:376:28: Attribute `pgconn` on type `@Todo(map_with_boundness: intersections with negative contributions) | None` is possibly unbound
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/types/datetime.py:77:16: Return type does not match returned value: Expected `timedelta`, found `timedelta | None`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/_conninfo_utils.py:81:31: Attribute `envvar` on type `ParamDef | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/psycopg/psycopg_pool/psycopg_pool/sched_async.py:66:24: Attribute `time` on type `@Todo(map_with_boundness: intersections with negative contributions) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/psycopg/psycopg_pool/psycopg_pool/sched_async.py:69:33: Attribute `time` on type `@Todo(map_with_boundness: intersections with negative contributions) | None` is possibly unbound
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/_conninfo_attempts.py:101:15: Argument to this function is incorrect: Expected `bytes | str | int | None`, found `str & ~AlwaysFalsy | ParamDef & ~AlwaysTruthy & ~AlwaysFalsy | @Todo(map_with_boundness: intersections with negative contributions) & ~AlwaysFalsy`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/generators.py:282:13: Object of type `None` is not callable
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/generators.py:293:13: Object of type `None` is not callable
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/_conninfo_attempts_async.py:105:19: Argument to this function is incorrect: Expected `bytes | str | int | None`, found `str & ~AlwaysFalsy | ParamDef & ~AlwaysTruthy & ~AlwaysFalsy | @Todo(map_with_boundness: intersections with negative contributions) & ~AlwaysFalsy`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/psycopg/psycopg_pool/psycopg_pool/sched.py:70:24: Attribute `time` on type `@Todo(map_with_boundness: intersections with negative contributions) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/psycopg/psycopg_pool/psycopg_pool/sched.py:73:33: Attribute `time` on type `@Todo(map_with_boundness: intersections with negative contributions) | None` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/_conninfo_attempts.py:101:15: Argument to this function is incorrect: Expected `bytes | str | int | None`, found `str | None | ParamDef & ~AlwaysTruthy & ~AlwaysFalsy | @Todo(map_with_boundness: intersections with negative contributions) & ~AlwaysFalsy`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/_conninfo_attempts_async.py:105:19: Argument to this function is incorrect: Expected `bytes | str | int | None`, found `str | None | ParamDef & ~AlwaysTruthy & ~AlwaysFalsy | @Todo(map_with_boundness: intersections with negative contributions) & ~AlwaysFalsy`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/pq/pq_ctypes.py:636:17: Attribute `contents` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/generators.py:256:33: Attribute `status` on type `PGresult | None` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/generators.py:282:35: Argument to this function is incorrect: Expected `PGnotify`, found `PGnotify | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/generators.py:293:35: Argument to this function is incorrect: Expected `PGnotify`, found `PGnotify | None`
- Found 1598 diagnostics
+ Found 1588 diagnostics

```
</details>


---

_Comment by @MatthewMckee4 on 2025-04-17 12:32_

mypy_primer looks good

---

_Renamed from "[red-knot] Add narrowing for assignment expressions for bool and call" to "[red-knot] Add some narrowing for assignment expressions" by @MatthewMckee4 on 2025-04-17 12:34_

---

_@MatthewMckee4 reviewed on 2025-04-17 12:44_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/narrow.rs`:484 on 2025-04-17 12:44_

I'm not sure if its worth it to try to duplicate less code, here. passing `is_positive` into `evaluate_named_expr_compare` seems pointless and after that theres nothing else we can really do.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:484 on 2025-04-17 20:16_

I'm OK with this level of duplicated code.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:368 on 2025-04-17 20:19_

This method name doesn't seem right -- there's nothing `NamedExpr` specific about this method, and it is used for both named exprs and simple names. I suggest `evaluate_expr_compare_op`

---

_@carljm reviewed on 2025-04-17 20:24_

Looks really good, thank you!

> There is still more to cover regarding named expressions.

Not required if you don't have time, but it would be really cool if this PR could just establish full parity, since it should be generally true that anywhere we would narrow on a `Name`, we should also be able to narrow on a `NamedExpr`. (If we don't reach parity, then I wouldn't want to close the associated issue yet.) And it doesn't seem like we are very far from that? It looks like a `NamedExpr` version of `evaluate_name_expr` (to intersect with `AlwaysFalsy` or `AlwaysTruthy`) would just be like five or six lines?

---

_Comment by @MatthewMckee4 on 2025-04-17 20:44_

Okay, if you think I should do that in this PR, then I'll get that done

---

_@MatthewMckee4 reviewed on 2025-04-17 20:46_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/narrow.rs`:368 on 2025-04-17 20:46_

Okay sounds good

---

_Comment by @MatthewMckee4 on 2025-04-18 00:10_

Im not sure what else i need to cover

---

_@carljm approved on 2025-04-18 00:26_

Looks to me like you got everything! Thank you!!

---

_Comment by @carljm on 2025-04-18 00:27_

I also re-checked the updated ecosystem results and they look good. The new errors are not a regression, they are just cases where other missing features mean there is still a (different) error on the same line that previously had an error.

---

_Merged by @carljm on 2025-04-18 00:28_

---

_Closed by @carljm on 2025-04-18 00:28_

---

_Branch deleted on 2025-04-19 13:59_

---
