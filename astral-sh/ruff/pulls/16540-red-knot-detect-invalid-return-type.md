```yaml
number: 16540
title: "[red-knot] detect invalid return type"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: 16248-return-type-checking
created_at: 2025-03-06T17:48:36Z
updated_at: 2025-03-12T02:01:07Z
url: https://github.com/astral-sh/ruff/pull/16540
synced_at: 2026-01-12T15:55:55Z
```

# [red-knot] detect invalid return type

---

_@mtshiba_

## Summary

This PR closes #16248.

If the return type of the function isn't assignable to the one specified, an `invalid-return-type` error occurs.
I thought it would be better to report this as a different kind of error than the `invalid-assignment` error, so I defined this as a new error.

## Test Plan

All type inconsistencies in the test cases have been replaced with appropriate ones.


---

_Review requested from @carljm by @mtshiba on 2025-03-06 17:48_

---

_Review requested from @MichaReiser by @mtshiba on 2025-03-06 17:48_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-03-06 17:48_

---

_Review requested from @sharkdp by @mtshiba on 2025-03-06 17:48_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-06 17:49_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:279 on 2025-03-06 18:09_

Would it be possible to retrieve the return type from the current scope instead of storing it as an extra field on the context?

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/getattr_static.md`:15 on 2025-03-06 18:11_

Minor: Found that as well today, I simply changed it to
```suggestion
        return "a"
```

---

_@sharkdp reviewed on 2025-03-06 18:11_

---

_@sharkdp reviewed on 2025-03-06 18:19_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:1152 on 2025-03-06 18:19_

The invalid-return-type diagnostics are currently attached to the whole function. I think we should aim to attach them to individual `return` statements. Ideally, it would be a multi-span diagnostic where we highlight the expression that is being returned as the primary span, and the actual return type of the function as a secondary span (the idea being that it's "typically" the `return`ed expression that is wrong, and not the annotated return type).

---

_@sharkdp reviewed on 2025-03-06 18:20_

Thank you!

I appreciate that you fixed all existing instances of this diagnostic (it's fascinating how much wrong code we've written :smile:). But we should also write some tests where we can actually see how this new diagnostic works. But I think it makes sense to reconsider the general approach first — see my other comment regarding attaching diagnostics to individual return statements and not to the whole function.

---

_@sharkdp reviewed on 2025-03-06 18:24_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:1152 on 2025-03-06 18:24_

For example:
```py
#        this could be the secondary span
#                    vvv
def f(flag: bool) -> int:
    if flag:
        return 1  # nothing is wrong here
    else:
        return "foo"
#              ^^^^^
#     this should be the primary
#       span for the diagnostic
```

Even if we don't want to work on multi-span diagnostics right away, I think we should still aim to attach the diagnostic to the "foo" expression, and not to the whole function. 

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:1152 on 2025-03-06 18:25_

For comparison, this is what we currently get on your branch (for a slightly modified example):
```
error: lint:invalid-return-type
 --> /home/shark/playground/test.py:1:1
  |
1 | / def f(flag: bool, flag2: bool) -> int:
2 | |     if flag:
3 | |         return 1  # nothing is wrong here
4 | |     elif flag2:
5 | |         return "foo"
6 | |
7 | |     return 2  # nothing wrong here either
  | |____________^ Object of type `Literal[1, "foo", 2]` is not assignable to return type `int`
  |
```

---

_@sharkdp reviewed on 2025-03-06 18:25_

---

_Comment by @github-actions[bot] on 2025-03-06 18:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:159 on 2025-03-07 17:52_

```suggestion
    @property
    def name(self) -> str:
        # TODO: No diagnostic should be emitted here
        # error: [invalid-return-type]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/function/return_type.md`:11 on 2025-03-07 17:54_

Let's use a different error type here; this test is technically fine but it's needlessly close to `return NotImplemented`, a common idiom which will probably need special handling, a different error type works just as well for this test and avoids that potential confusion
```suggestion
    raise ValueError()
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:122 on 2025-03-07 17:55_

I realize this test is kind of passing for the wrong reason, since we don't actually understand these type vars as type vars yet. But the result is still correct, so I don't think we need a TODO here.

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/function/return_type.md`:1 on 2025-03-07 17:57_

These tests look good, but we usually split our tests up a bit more and intersperse a bit more prose commentary to help make the purpose of each test clearer to the reader.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:656 on 2025-03-07 17:58_

```suggestion
        return ""
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:267 on 2025-03-07 17:59_

```suggestion
        summary: "detects a returned value that can't be assigned to the function's annotated return type",
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1059 on 2025-03-07 18:02_

The name `set_return_type` suggests a single value we are setting, not pushing to a vector. I'd suggest `push_return_type` or `record_return_type`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1142 on 2025-03-07 18:12_

Three comments here:

a) I don't see any test for the "suite has only ellipsis" case in the added tests in this PR?

b) I think we should only apply this special case in stub files (and in overload definitions and protocol method definitions, but we don't support either of these yet so don't need to handle them in this PR), not generally for all functions. A normal function in a `.py` file should error if annotated to return some non-None type but its body consists only of ellipsis. (The behavior I suggest matches mypy, but not pyright.)

c) We can do this check only in the case that `self.types.return_types_and_ranges` is empty, which should be cheaper to check for than walking all statements in the body.

---

_Review requested from @sharkdp by @mtshiba on 2025-03-07 18:21_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1154 on 2025-03-07 18:21_

I don't think we should construct a union type for a single initial assignability check. Assignability checking for a union generally means iterating all its member types anyway, and this way we also pay the cost to construct the union type. Why not iterate all possible return types and ranges (after adding "end-of-body `None`" to the list) and check each one for assignability to the annotated return type (that is, what we end up doing here in the error case anyway), without first doing a union assignability check?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1146 on 2025-03-07 18:33_

I don't think this logic is quite right. Consider a function like this:

```py
def f(cond: bool) -> int:
    if cond:
        return 1
```

This function will not have an empty `return_type_and_ranges`, but it is still possible for control flow to fall off the end of the function (implicit return `None`).

I think the best way to check for this is to query the final state of the use-def map builder to see whether `scope_start_visibility` is `ALWAYS_FALSE`. This will require recording one new field onto `UseDefMap` (we can call it `can_implicit_return: bool` and add a public method `UseDefMap::can_implicit_return()`) based on checking `scope_start_visibility` in `UseDefMapBuilder::finish`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1175 on 2025-03-07 18:37_

Even for this case, reporting an error on the entire function definition node is not a good user experience, it leads to massive blocks of red squiggly lines in an editor. Pyright reports this error on the function return annotation node, I think that's a good choice.

Also, I think we should use a distinctive error message for this case. Something like "Function can implicitly return `None`, which is not assignable to return type {}"

---

_@carljm reviewed on 2025-03-07 18:38_

Looking good, thank you! A few comments.

---

_Review requested from @dhruvmanila by @mtshiba on 2025-03-08 19:11_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/function/return_type.md`:1 on 2025-03-10 08:48_

Let's add at least one test where we snapshot the entire diagnostic. Ideally, we'd have multiple tests:

* One where a specific return returns a not-assignable value
* One where at least one branch has no return statement but the function has a non optional return type


---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:265 on 2025-03-10 08:50_

We should write at least a basic documentation for new lint rules. I mainly created #14899 because I didn't wanted to document all existing lint rules when I introduced `declare_lint` but we should avoid accumulating the documentation-debt for new lints

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:288 on 2025-03-10 08:55_

Would it be possible to move this field to the `TypeInferenceBuilder`. Or are there cases where a return statement is inferred as part of another type-inference region that then needs to be aggregated at the function-region-level?

I'm asking because including any sort of range at the `TypeInference` level is problematic because it means that the `infer_types` salsa query invalidates (the returned value!) on every single change in that file. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1142 on 2025-03-10 08:59_

NIt
```suggestion
                match &*suite {
                	[Stmt::Expr(StmtExpr { value, .. }), ..] if value.is_ellipsis_literal_expr() => value.is_ellipsis_literal_expr(),
                	[Stmt::Expr(StmtExpr { value: first, ..}, StmtExpr(StmtExpr { value: second, .. }, ..] if first.is_string_literal_expr() => second.is_ellispsis_literal_expr(),
                	_ => false
              	}
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1165 on 2025-03-10 09:01_

We have to account for the handlers here too

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1185 on 2025-03-10 09:02_

```suggestion
            let is_overloaded = function.decorator_list.iter().any(|decorator| {
            		let decorator_type = self.file_expression_type(&decorator.expression);
            		
                decorator_type
                    .into_function_literal()
                    .is_some_and(|f| f.is_known(self.db(), KnownFunction::Overload))
            });
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1127 on 2025-03-10 09:03_

Nit: `is_stub_suite`

---

_@MichaReiser reviewed on 2025-03-10 09:03_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/function/return_type.md`:24 on 2025-03-11 00:28_

This test and `reveal_type` seem to be testing something about function parameters, not about return types. What is this test demonstrating about function return values?

---

_Comment by @github-actions[bot] on 2025-03-11 14:00_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
git-revise (https://github.com/mystor/git-revise)
+ error: lint:invalid-return-type
+   --> /tmp/mypy_primer/projects/git-revise/gitrevise/todo.py:25:20
+    |
+ 23 |     def parse(instr: str) -> StepKind:
+ 24 |         if "pick".startswith(instr):
+ 25 |             return StepKind.PICK
+    |                    ^^^^^^^^^^^^^ Object of type `Unknown | Literal["pick"]` is not assignable to return type `StepKind`
+ 26 |         if "fixup".startswith(instr):
+ 27 |             return StepKind.FIXUP
+    |
+   ::: /tmp/mypy_primer/projects/git-revise/gitrevise/todo.py:23:30
+    |
+ 22 |     @staticmethod
+ 23 |     def parse(instr: str) -> StepKind:
+    |                              -------- info: Return type is declared here as `StepKind`
+ 24 |         if "pick".startswith(instr):
+ 25 |             return StepKind.PICK
+    |
+ 
+ error: lint:invalid-return-type
+   --> /tmp/mypy_primer/projects/git-revise/gitrevise/todo.py:27:20
+    |
+ 25 |             return StepKind.PICK
+ 26 |         if "fixup".startswith(instr):
+ 27 |             return StepKind.FIXUP
+    |                    ^^^^^^^^^^^^^^ Object of type `Unknown | Literal["fixup"]` is not assignable to return type `StepKind`
+ 28 |         if "squash".startswith(instr):
+ 29 |             return StepKind.SQUASH
+    |
+   ::: /tmp/mypy_primer/projects/git-revise/gitrevise/todo.py:23:30
+    |
+ 22 |     @staticmethod
+ 23 |     def parse(instr: str) -> StepKind:
+    |                              -------- info: Return type is declared here as `StepKind`
+ 24 |         if "pick".startswith(instr):
+ 25 |             return StepKind.PICK
+    |
+ 
+ error: lint:invalid-return-type
+   --> /tmp/mypy_primer/projects/git-revise/gitrevise/todo.py:29:20
+    |
+ 27 |             return StepKind.FIXUP
+ 28 |         if "squash".startswith(instr):
+ 29 |             return StepKind.SQUASH
+    |                    ^^^^^^^^^^^^^^^ Object of type `Unknown | Literal["squash"]` is not assignable to return type `StepKind`
+ 30 |         if "reword".startswith(instr):
+ 31 |             return StepKind.REWORD
+    |
+   ::: /tmp/mypy_primer/projects/git-revise/gitrevise/todo.py:23:30
+    |
+ 22 |     @staticmethod
+ 23 |     def parse(instr: str) -> StepKind:
+    |                              -------- info: Return type is declared here as `StepKind`
+ 24 |         if "pick".startswith(instr):
+ 25 |             return StepKind.PICK
+    |
+ 
+ error: lint:invalid-return-type
+   --> /tmp/mypy_primer/projects/git-revise/gitrevise/todo.py:31:20
+    |
+ 29 |             return StepKind.SQUASH
+ 30 |         if "reword".startswith(instr):
+ 31 |             return StepKind.REWORD
+    |                    ^^^^^^^^^^^^^^^ Object of type `Unknown | Literal["reword"]` is not assignable to return type `StepKind`
+ 32 |         if "cut".startswith(instr):
+ 33 |             return StepKind.CUT
+    |
+   ::: /tmp/mypy_primer/projects/git-revise/gitrevise/todo.py:23:30
+    |
+ 22 |     @staticmethod
+ 23 |     def parse(instr: str) -> StepKind:
+    |                              -------- info: Return type is declared here as `StepKind`
+ 24 |         if "pick".startswith(instr):
+ 25 |             return StepKind.PICK
+    |
+ 
+ error: lint:invalid-return-type
+   --> /tmp/mypy_primer/projects/git-revise/gitrevise/todo.py:33:20
+    |
+ 31 |             return StepKind.REWORD
+ 32 |         if "cut".startswith(instr):
+ 33 |             return StepKind.CUT
+    |                    ^^^^^^^^^^^^ Object of type `Unknown | Literal["cut"]` is not assignable to return type `StepKind`
+ 34 |         if "index".startswith(instr):
+ 35 |             return StepKind.INDEX
+    |
+   ::: /tmp/mypy_primer/projects/git-revise/gitrevise/todo.py:23:30
+    |
+ 22 |     @staticmethod
+ 23 |     def parse(instr: str) -> StepKind:
+    |                              -------- info: Return type is declared here as `StepKind`
+ 24 |         if "pick".startswith(instr):
+ 25 |             return StepKind.PICK
+    |
+ 
+ error: lint:invalid-return-type
+   --> /tmp/mypy_primer/projects/git-revise/gitrevise/todo.py:35:20
+    |
+ 33 |             return StepKind.CUT
+ 34 |         if "index".startswith(instr):
+ 35 |             return StepKind.INDEX
+    |                    ^^^^^^^^^^^^^^ Object of type `Unknown | Literal["index"]` is not assignable to return type `StepKind`
+ 36 |         raise ValueError(
+ 37 |             f"step kind '{instr}' must be one of: pick, fixup, squash, reword, cut, or index"
+    |
+   ::: /tmp/mypy_primer/projects/git-revise/gitrevise/todo.py:23:30
+    |
+ 22 |     @staticmethod
+ 23 |     def parse(instr: str) -> StepKind:
+    |                              -------- info: Return type is declared here as `StepKind`
+ 24 |         if "pick".startswith(instr):
+ 25 |             return StepKind.PICK
+    |
+ 
- Found 52 diagnostics
+ Found 58 diagnostics

black (https://github.com/psf/black)
+    |
+ 
+ error: lint:invalid-return-type
+   --> /tmp/mypy_primer/projects/black/src/blib2to3/pytree.py:83:20
+    |
+ 81 |         """
+ 82 |         if self.__class__ is not other.__class__:
+ 83 |             return NotImplemented
+    |                    ^^^^^^^^^^^^^^ Object of type `_NotImplementedType` is not assignable to return type `bool`
+ 84 |         return self._eq(other)
+    |
+   ::: /tmp/mypy_primer/projects/black/src/blib2to3/pytree.py:76:37
+    |
+ 74 |         return object.__new__(cls)
+ 75 |
+ 76 |     def __eq__(self, other: Any) -> bool:
+    |                                     ---- info: Return type is declared here as `bool`
+ 77 |         """
+ 78 |         Compare two nodes for equality.
+ 
+    |
+    |
+    |
+    |
+ error: lint:invalid-return-type
+   --> /tmp/mypy_primer/projects/black/src/blackd/middlewares.py:35:12
+ 33 |         return resp
+ 34 |
+ 35 |     return impl
+    |            ^^^^ Object of type `Literal[impl]` is not assignable to return type `Middleware`
+   ::: /tmp/mypy_primer/projects/black/src/blackd/middlewares.py:11:43
+ 11 | def cors(allow_headers: Iterable[str]) -> Middleware:
+    |                                           ---------- info: Return type is declared here as `Middleware`
+ 12 |     @middleware
+ 13 |     async def impl(request: Request, handler: Handler) -> StreamResponse:
- Found 296 diagnostics
+ Found 298 diagnostics

arrow (https://github.com/arrow-py/arrow)
+ 
+ 
+ 
+ 
+ 
+ 
+ 
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+      |
+ error: lint:invalid-return-type
+     --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1721:16
+ 1719 |             return self.fromdatetime(self._datetime + other, self._datetime.tzinfo)
+ 1720 |
+ 1721 |         return NotImplemented
+      |                ^^^^^^^^^^^^^^ Object of type `_NotImplementedType` is not assignable to return type `Arrow`
+ 1722 |
+ 1723 |     def __radd__(self, other: Union[timedelta, relativedelta]) -> "Arrow":
+     ::: /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1717:38
+ 1715 |     # math
+ 1716 |
+ 1717 |     def __add__(self, other: Any) -> "Arrow":
+      |                                      ------- info: Return type is declared here as `Arrow`
+ 1718 |         if isinstance(other, (timedelta, relativedelta)):
+ 1719 |             return self.fromdatetime(self._datetime + other, self._datetime.tzinfo)
+ error: lint:invalid-return-type
+     --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1744:16
+ 1742 |             return self._datetime - other._datetime
+ 1743 |
+ 1744 |         return NotImplemented
+      |                ^^^^^^^^^^^^^^ Object of type `_NotImplementedType` is not assignable to return type `timedelta | Arrow`
+ 1745 |
+ 1746 |     def __rsub__(self, other: Any) -> timedelta:
+     ::: /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1734:38
+ 1732 |         pass  # pragma: no cover
+ 1733 |
+ 1734 |     def __sub__(self, other: Any) -> Union[timedelta, "Arrow"]:
+      |                                      ------------------------- info: Return type is declared here as `timedelta | Arrow`
+ 1735 |         if isinstance(other, (timedelta, relativedelta)):
+ 1736 |             return self.fromdatetime(self._datetime - other, self._datetime.tzinfo)
+ error: lint:invalid-return-type
+     --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1750:16
+ 1748 |             return other - self._datetime
+ 1749 |
+ 1750 |         return NotImplemented
+      |                ^^^^^^^^^^^^^^ Object of type `_NotImplementedType` is not assignable to return type `timedelta`
+ 1751 |
+ 1752 |     # comparisons
+     ::: /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1746:39
+ 1744 |         return NotImplemented
+ 1745 |
+ 1746 |     def __rsub__(self, other: Any) -> timedelta:
+      |                                       --------- info: Return type is declared here as `timedelta`
+ 1747 |         if isinstance(other, dt_datetime):
+ 1748 |             return other - self._datetime
+ error: lint:invalid-return-type
+     --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1768:20
+ 1766 |     def __gt__(self, other: Any) -> bool:
+ 1767 |         if not isinstance(other, (Arrow, dt_datetime)):
+ 1768 |             return NotImplemented
+      |                    ^^^^^^^^^^^^^^ Object of type `_NotImplementedType` is not assignable to return type `bool`
+ 1769 |
+ 1770 |         return self._datetime > self._get_datetime(other)
+     ::: /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1766:37
+ 1764 |         return not self.__eq__(other)
+ 1765 |
+ 1766 |     def __gt__(self, other: Any) -> bool:
+      |                                     ---- info: Return type is declared here as `bool`
+ 1767 |         if not isinstance(other, (Arrow, dt_datetime)):
+ 1768 |             return NotImplemented
+ error: lint:invalid-return-type
+     --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1774:20
+ 1772 |     def __ge__(self, other: Any) -> bool:
+ 1773 |         if not isinstance(other, (Arrow, dt_datetime)):
+ 1774 |             return NotImplemented
+      |                    ^^^^^^^^^^^^^^ Object of type `_NotImplementedType` is not assignable to return type `bool`
+ 1775 |
+ 1776 |         return self._datetime >= self._get_datetime(other)
+     ::: /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1772:37
+ 1770 |         return self._datetime > self._get_datetime(other)
+ 1771 |
+ 1772 |     def __ge__(self, other: Any) -> bool:
+      |                                     ---- info: Return type is declared here as `bool`
+ 1773 |         if not isinstance(other, (Arrow, dt_datetime)):
+ 1774 |             return NotImplemented
+ error: lint:invalid-return-type
+     --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1780:20
+ 1778 |     def __lt__(self, other: Any) -> bool:
+ 1779 |         if not isinstance(other, (Arrow, dt_datetime)):
+ 1780 |             return NotImplemented
+      |                    ^^^^^^^^^^^^^^ Object of type `_NotImplementedType` is not assignable to return type `bool`
+ 1781 |
+ 1782 |         return self._datetime < self._get_datetime(other)
+     ::: /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1778:37
+ 1776 |         return self._datetime >= self._get_datetime(other)
+ 1777 |
+ 1778 |     def __lt__(self, other: Any) -> bool:
+      |                                     ---- info: Return type is declared here as `bool`
+ 1779 |         if not isinstance(other, (Arrow, dt_datetime)):
+ 1780 |             return NotImplemented
+ error: lint:invalid-return-type
+     --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1786:20
+ 1784 |     def __le__(self, other: Any) -> bool:
+ 1785 |         if not isinstance(other, (Arrow, dt_datetime)):
+ 1786 |             return NotImplemented
+      |                    ^^^^^^^^^^^^^^ Object of type `_NotImplementedType` is not assignable to return type `bool`
+ 1787 |
+ 1788 |         return self._datetime <= self._get_datetime(other)
+     ::: /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1784:37
+ 1782 |         return self._datetime < self._get_datetime(other)
+ 1783 |
+ 1784 |     def __le__(self, other: Any) -> bool:
+      |                                     ---- info: Return type is declared here as `bool`
+ 1785 |         if not isinstance(other, (Arrow, dt_datetime)):
+ 1786 |             return NotImplemented
- Found 73 diagnostics
+ Found 80 diagnostics

```
</details>


---

_Comment by @sharkdp on 2025-03-11 14:26_

> `mypy_primer` results

FYI, the results look mostly as expected to me. There are three distinct issues (none related to your PR), as far as I can tell:
- Missing assignability of intersection types (see #16611)
- Missing support for enums
- Missing support for `types.NotImplementedType`

---

_Review requested from @carljm by @mtshiba on 2025-03-11 18:24_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:396 on 2025-03-12 00:23_

Can we just use `VisibilityConstraints::evaluate` here? (Sorry, I directed you wrongly in my earlier comment suggesting we could simply check for `ALWAYS_FALSE`.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:325 on 2025-03-12 00:23_

```suggestion
    /// This is used to check if a function scope can implicitly return `None`.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:334 on 2025-03-12 00:24_

```suggestion
    /// In this case, the function may implicitly return `None`.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/visibility_constraints.rs`:235 on 2025-03-12 00:26_

It seems to me that we should be able to use `VisibilityConstraints::evaluate` in `UseDefMap::can_implicit_return`, and avoid the need for any of the changes in this module.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1139 on 2025-03-12 01:03_

We should also allow (and test for) the case where we have just a docstring (without `pass` or ellipsis). This is valid syntax and should be accepted as a stub suite.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1182 on 2025-03-12 01:04_

nit: what this is really checking is whether this function is an overload, not whether it is overloaded
```suggestion
            let is_overload = function.decorator_list.iter().any(|decorator| {
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1212 on 2025-03-12 01:26_

We shouldn't need this function at all; this is what `can_implicit_return` should do for us. I investigated why it doesn't, and realized that it's our temporary hack here: https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/src/semantic_index/builder.rs#L880-L899

Really sorry I missed this and it caused a bunch of extra unnecessary work for you! I found a 2-3 line temporary fix that will solve this until we get rid of this hack, will push that up to this PR since I already did it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2683 on 2025-03-12 01:29_

I don't think we need this? By definition, `Never` is assignable to all types, so it is impossible for a `Never` return to ever emit a diagnostic, no matter what the annotated return type is.

Removing this change doesn't cause any tests to fail; unless we can write a counter-example test, we should remove this.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/visibility_constraints.rs`:235 on 2025-03-12 01:30_

I went ahead and also pushed this change, since I'd already tested it locally to make sure I wasn't misleading you again!

---

_@carljm reviewed on 2025-03-12 01:30_

Looks good!

---

_@carljm reviewed on 2025-03-12 01:32_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:396 on 2025-03-12 01:32_

Pushed this change.

---

_@carljm reviewed on 2025-03-12 01:32_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2683 on 2025-03-12 01:32_

Pushed this change.

---

_Comment by @carljm on 2025-03-12 01:56_

Since the remainder of my comments were minor and I already had this loaded up locally, I just made the remaining changes and pushed it, will merge! Thank you for the PR! Please let me know if any of my changes missed an important point.

---

_Merged by @carljm on 2025-03-12 01:59_

---

_Closed by @carljm on 2025-03-12 01:59_

---
