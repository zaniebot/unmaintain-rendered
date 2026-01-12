```yaml
number: 18663
title: "[ty] fix binary expression inference between boolean literals and bool instances"
type: pull_request
state: merged
author: alpaylan
labels:
  - ty
assignees: []
merged: true
base: main
head: main
created_at: 2025-06-13T16:12:14Z
updated_at: 2025-06-17T17:02:59Z
url: https://github.com/astral-sh/ruff/pull/18663
synced_at: 2026-01-12T15:56:23Z
```

# [ty] fix binary expression inference between boolean literals and bool instances

---

_@alpaylan_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This PR fixes a false type inference issue specified in https://github.com/astral-sh/ty/issues/649 

If there is a better solution, I'm open to implementing that too. The current version uses the right-hand-side type when it finds a `bool & NominalInstance` pair, which is an improvement over the previous version, but I'm guessing there are bugs introduces by this too. I will keep inspecting those as I wait for the reviews.

Closes https://github.com/astral-sh/ty/issues/649 

## Test Plan

<!-- How was it tested? -->

This PR was tested through a regression test, added as an inline test to the module.


---

_Review requested from @carljm by @alpaylan on 2025-06-13 16:12_

---

_Review requested from @AlexWaygood by @alpaylan on 2025-06-13 16:12_

---

_Review requested from @sharkdp by @alpaylan on 2025-06-13 16:12_

---

_Review requested from @dcreager by @alpaylan on 2025-06-13 16:12_

---

_Renamed from "fix 'BitwiseAnd' bug at https://github.com/astral-sh/ty/issues/649 " to "[ty] fix 'BitwiseAnd' bug" by @MichaReiser on 2025-06-13 16:41_

---

_Label `ty` added by @MichaReiser on 2025-06-13 16:41_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:10595 on 2025-06-13 18:13_

We write all such tests as mdtests (test snippets embedded as Markdown) rather than as regular Rust tests. See https://github.com/astral-sh/ruff/blob/main/crates/ty/CONTRIBUTING.md#writing-tests for details. This test should be easy to transform to an mdtest.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:6721 on 2025-06-13 18:23_

I realize that our prior logic here used a fallback to an int literal, but I think a fallback to `KnownClass::Bool.to_instance()` is more correct. (Falling back to an int literal may give more precise results in some cases, but is just wrong in other cases; fallback to `bool` should never be wrong.) So I would probably try just replacing the fallback to `Type::IntLiteral(...)` with a fallback to `KnownClass::Bool.to_instance()`, and see what (if any) existing test cases fail. It may be that we can simply decide we don't need that level of precision in those tests, and we can just fall back to bool instead. Or if we do want to keep that precision, we could add a special case for fallback to int literal only for specific cases where we know it's correct to do so.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:6712 on 2025-06-13 18:24_

I don't think we can just assume the output is a bool without even knowing what operation is being performed -- the better approach here would be to recursively call `self.infer_binary_expression_type` but with the left operand replaced by `KnownClass::Bool.to_instance()`. Similar to how we currently fall back to `Type::IntLiteral(...)`

---

_@carljm had review dismissed on 2025-06-13 18:25_

Thanks for identifying the bug and proposing a fix! I think we want to adjust a few things in this PR, but it's a great start.

---

_Comment by @github-actions[bot] on 2025-06-13 22:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
optuna (https://github.com/optuna/optuna)
- error[invalid-argument-type] optuna/visualization/_pareto_front.py:327:9: Argument is incorrect: Expected `bool`, found `int`
- Found 1005 diagnostics
+ Found 1004 diagnostics

yarl (https://github.com/aio-libs/yarl)
- error[invalid-assignment] yarl/_url.py:1019:13: Object of type `int` is not assignable to `bool`
- Found 159 diagnostics
+ Found 158 diagnostics

mypy (https://github.com/python/mypy)
- error[invalid-return-type] mypy/inspections.py:427:16: Return type does not match returned value: expected `tuple[dict[FuncBase | SymbolNode, State], bool]`, found `tuple[dict[Unknown, Unknown], int | @Todo(map_with_boundness: intersections with negative contributions)]`
- Found 2023 diagnostics
+ Found 2022 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[invalid-return-type] ddtrace/appsec/_ddwaf/waf.py:143:16: Return type does not match returned value: expected `bool`, found `int`
- Found 6897 diagnostics
+ Found 6896 diagnostics

scipy (https://github.com/scipy/scipy)
- error[unresolved-attribute] scipy/stats/_mstats_basic.py:1919:7: Type `int` has no attribute `filled`
+ error[unresolved-attribute] scipy/stats/_mstats_basic.py:1919:7: Type `bool` has no attribute `filled`

```
</details>


---

_@alpaylan reviewed on 2025-06-13 22:54_

---

_Review comment by @alpaylan on `crates/ty_python_semantic/src/types/infer.rs`:6721 on 2025-06-13 22:54_

As far as I can tell from `mdtest__binary_booleans` doing this kills the precision for the arithmetic ops, because right now there's no separate case for them. I've moved them to a separate case with 
```rust
   (
                Type::BooleanLiteral(b1),
                right,
                ast::Operator::Add
                | ast::Operator::Sub
                | ast::Operator::Mult
                | ast::Operator::Mod
                | ast::Operator::FloorDiv
                | ast::Operator::Pow
                | ast::Operator::Div,
            ) => self.infer_binary_expression_type(
                node,
                emitted_division_by_zero_diagnostic,
                Type::IntLiteral(i64::from(b1)),
                right,
                op)
```
and with that, we can use the `KnownClass::Bool.to_instance` fallback without breaking existing test cases.

---

_@alpaylan reviewed on 2025-06-13 23:01_

---

_Review comment by @alpaylan on `crates/ty_python_semantic/src/types/infer.rs`:10595 on 2025-06-13 23:01_

Moved 👍🏻 

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/binary/booleans.md`:1 on 2025-06-16 17:51_

These tests all pass on our `main` branch (i.e., if I revert all your changes to `crates/ty_python_semantic/src/types/infer.rs`, these tests all still pass). Can you make sure that all the tests you're adding fail on the `main` branch?

The reason why these tests pass is that the bug you're fixing only manifests if you use a _literal_ `bool` value. I.e. on `main` we have this behaviour:

```py
from random import random

def f() -> bool:
    return random() > 0.5

reveal_type(True & f())  # int
reveal_type(False & f())  # int
reveal_type(f() & True)  # int
reveal_type(f() & False)  # int

reveal_type(f() & f())  # bool
```

On the first four operations, we get the wrong answer on `main`, because the first four operations all use values that we infer as having literal `bool` types (`True` has type `Literal[True]`). But we get the last answer right. All the tests you've added look like the last `reveal_type` call.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:6737 on 2025-06-16 17:57_

Rather than checking what `op` is, I think it would be simpler to check what the other value is -- something like this (the diff is relative to your branch):

```diff
diff --git a/crates/ty_python_semantic/src/types/infer.rs b/crates/ty_python_semantic/src/types/infer.rs
index 64fd0069d2..1fb868c007 100644
--- a/crates/ty_python_semantic/src/types/infer.rs
+++ b/crates/ty_python_semantic/src/types/infer.rs
@@ -6701,40 +6701,24 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
             (Type::BooleanLiteral(b1), Type::BooleanLiteral(b2), ast::Operator::BitXor) => {
                 Some(Type::BooleanLiteral(b1 ^ b2))
             }
-            (
-                Type::BooleanLiteral(b1),
-                right,
-                ast::Operator::Add
-                | ast::Operator::Sub
-                | ast::Operator::Mult
-                | ast::Operator::Mod
-                | ast::Operator::FloorDiv
-                | ast::Operator::Pow
-                | ast::Operator::Div,
-            ) => self.infer_binary_expression_type(
-                node,
-                emitted_division_by_zero_diagnostic,
-                Type::IntLiteral(i64::from(b1)),
-                right,
-                op,
-            ),
-            (
-                left,
-                Type::BooleanLiteral(b2),
-                ast::Operator::Add
-                | ast::Operator::Sub
-                | ast::Operator::Mult
-                | ast::Operator::Mod
-                | ast::Operator::FloorDiv
-                | ast::Operator::Pow
-                | ast::Operator::Div,
-            ) => self.infer_binary_expression_type(
-                node,
-                emitted_division_by_zero_diagnostic,
-                left,
-                Type::IntLiteral(i64::from(b2)),
-                op,
-            ),
+
+            (Type::BooleanLiteral(b1), Type::IntLiteral(_), op) => self
+                .infer_binary_expression_type(
+                    node,
+                    emitted_division_by_zero_diagnostic,
+                    Type::IntLiteral(i64::from(b1)),
+                    right_ty,
+                    op,
+                ),
+            (Type::IntLiteral(_), Type::BooleanLiteral(b2), op) => self
+                .infer_binary_expression_type(
+                    node,
+                    emitted_division_by_zero_diagnostic,
+                    left_ty,
+                    Type::IntLiteral(i64::from(b2)),
+                    op,
+                ),
+
             (Type::BooleanLiteral(_), right, op) => self.infer_binary_expression_type(
                 node,
                 emitted_division_by_zero_diagnostic,
```

The logic I'm suggesting here would only treat boolean literals like their value-equivalent integer literals if the boolean literal in question is involved in an operation with an integer literal. We can say with confidence that this will be safe: binary operations between integer literals and boolean literals will always fall back to the relevant operations on `int`, so it's okay to treat boolean literals as integer literals for these specific operations. And there's no real other use case I can think of for treating boolean literals like their value-equivalent integer literals

---

_@AlexWaygood requested changes on 2025-06-16 17:58_

Thanks!

---

_@AlexWaygood reviewed on 2025-06-16 18:03_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:6751 on 2025-06-16 18:03_

the third and fourth new branches here shouldn't be necessary; they should be handled by the fallback branch at the bottom of this method

---

_@alpaylan reviewed on 2025-06-17 14:19_

---

_Review comment by @alpaylan on `crates/ty_python_semantic/src/types/infer.rs`:6737 on 2025-06-17 14:19_

Wouldn't this fail when the other expression is a nominal instance of int such as `f: () -> int` instead of an `IntLiteral` like `5`?

---

_@AlexWaygood reviewed on 2025-06-17 14:30_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:6737 on 2025-06-17 14:30_

if the other expression is a nominal `int` then we'll just fall through to the final fallback branch in this `match`, which will just look up the methods in typeshed and get the correct result, I think?

---

_@alpaylan reviewed on 2025-06-17 14:45_

---

_Review comment by @alpaylan on `crates/ty_python_semantic/src/types/infer.rs`:6737 on 2025-06-17 14:45_

Updates after tests: the proposed change breaks the old arithmetic narrowing in the basic arithmetic case

```

  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:11 unmatched assertion: revealed: Literal[2]
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:11 unexpected error: 13 [revealed-type] "Revealed type: `int`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:12 unmatched assertion: revealed: Literal[1]
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:12 unexpected error: 13 [revealed-type] "Revealed type: `int`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:13 unmatched assertion: revealed: Literal[1]
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:13 unexpected error: 13 [revealed-type] "Revealed type: `int`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:14 unmatched assertion: revealed: Literal[0]
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:14 unexpected error: 13 [revealed-type] "Revealed type: `int`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:16 unmatched assertion: revealed: Literal[0]
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:16 unexpected error: 13 [revealed-type] "Revealed type: `int`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:17 unmatched assertion: revealed: Literal[1]
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:17 unexpected error: 13 [revealed-type] "Revealed type: `int`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:18 unmatched assertion: revealed: Literal[-1]
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:18 unexpected error: 13 [revealed-type] "Revealed type: `int`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:19 unmatched assertion: revealed: Literal[0]
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:19 unexpected error: 13 [revealed-type] "Revealed type: `int`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:21 unmatched assertion: revealed: Literal[1]
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:21 unexpected error: 13 [revealed-type] "Revealed type: `int`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:22 unmatched assertion: revealed: Literal[0]
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:22 unexpected error: 13 [revealed-type] "Revealed type: `int`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:23 unmatched assertion: revealed: Literal[0]
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:23 unexpected error: 13 [revealed-type] "Revealed type: `int`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:24 unmatched assertion: revealed: Literal[0]
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:24 unexpected error: 13 [revealed-type] "Revealed type: `int`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:26 unmatched assertion: revealed: Literal[0]
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:26 unexpected error: 13 [revealed-type] "Revealed type: `int`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:27 unmatched assertion: revealed: Literal[0]
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:27 unexpected error: 13 [revealed-type] "Revealed type: `int`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:29 unmatched assertion: revealed: Literal[1]
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:29 unexpected error: 13 [revealed-type] "Revealed type: `int`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:30 unmatched assertion: revealed: Literal[0]
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:30 unexpected error: 13 [revealed-type] "Revealed type: `int`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:32 unmatched assertion: revealed: Literal[1]
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:32 unexpected error: 13 [revealed-type] "Revealed type: `int`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:33 unmatched assertion: revealed: Literal[1]
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:33 unexpected error: 13 [revealed-type] "Revealed type: `int`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:34 unmatched assertion: revealed: Literal[0]
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:34 unexpected error: 13 [revealed-type] "Revealed type: `int`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:35 unmatched assertion: revealed: Literal[1]
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:35 unexpected error: 13 [revealed-type] "Revealed type: `int`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:38 unmatched assertion: revealed: float
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:38 unexpected error: 13 [revealed-type] "Revealed type: `int | float`"
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:39 unmatched assertion: revealed: float
  crates/ty_python_semantic/resources/mdtest/binary/booleans.md:39 unexpected error: 13 [revealed-type] "Revealed type: `int | float`"
```

---

_@AlexWaygood reviewed on 2025-06-17 14:55_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:6737 on 2025-06-17 14:55_

Ah, sorry -- good point! What about something like this (again, the diff is relative to your PR branch)? It doesn't seem to cause any regressions on our existing tests, and I think it fixes the issue

```diff
diff --git a/crates/ty_python_semantic/src/types/infer.rs b/crates/ty_python_semantic/src/types/infer.rs
index 64fd0069d2..49275c11de 100644
--- a/crates/ty_python_semantic/src/types/infer.rs
+++ b/crates/ty_python_semantic/src/types/infer.rs
@@ -6701,54 +6701,22 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
             (Type::BooleanLiteral(b1), Type::BooleanLiteral(b2), ast::Operator::BitXor) => {
                 Some(Type::BooleanLiteral(b1 ^ b2))
             }
-            (
-                Type::BooleanLiteral(b1),
-                right,
-                ast::Operator::Add
-                | ast::Operator::Sub
-                | ast::Operator::Mult
-                | ast::Operator::Mod
-                | ast::Operator::FloorDiv
-                | ast::Operator::Pow
-                | ast::Operator::Div,
-            ) => self.infer_binary_expression_type(
-                node,
-                emitted_division_by_zero_diagnostic,
-                Type::IntLiteral(i64::from(b1)),
-                right,
-                op,
-            ),
-            (
-                left,
-                Type::BooleanLiteral(b2),
-                ast::Operator::Add
-                | ast::Operator::Sub
-                | ast::Operator::Mult
-                | ast::Operator::Mod
-                | ast::Operator::FloorDiv
-                | ast::Operator::Pow
-                | ast::Operator::Div,
-            ) => self.infer_binary_expression_type(
-                node,
-                emitted_division_by_zero_diagnostic,
-                left,
-                Type::IntLiteral(i64::from(b2)),
-                op,
-            ),
-            (Type::BooleanLiteral(_), right, op) => self.infer_binary_expression_type(
-                node,
-                emitted_division_by_zero_diagnostic,
-                KnownClass::Bool.to_instance(self.db()),
-                right,
-                op,
-            ),
-            (left, Type::BooleanLiteral(_), op) => self.infer_binary_expression_type(
-                node,
-                emitted_division_by_zero_diagnostic,
-                left,
-                KnownClass::Bool.to_instance(self.db()),
-                op,
-            ),
+            (Type::BooleanLiteral(b1), Type::BooleanLiteral(_) | Type::IntLiteral(_), op) => self
+                .infer_binary_expression_type(
+                    node,
+                    emitted_division_by_zero_diagnostic,
+                    Type::IntLiteral(i64::from(b1)),
+                    right_ty,
+                    op,
+                ),
+            (Type::IntLiteral(_), Type::BooleanLiteral(b2), op) => self
+                .infer_binary_expression_type(
+                    node,
+                    emitted_division_by_zero_diagnostic,
+                    left_ty,
+                    Type::IntLiteral(i64::from(b2)),
+                    op,
+                ),
             (Type::Tuple(lhs), Type::Tuple(rhs), ast::Operator::Add) => {
                 // Note: this only works on heterogeneous tuples.
                 let lhs_elements = lhs.elements(self.db());
@@ -6767,6 +6735,7 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
             // fall back on looking for dunder methods on one of the operand types.
             (
                 Type::FunctionLiteral(_)
+                | Type::BooleanLiteral(_)
                 | Type::Callable(..)
                 | Type::BoundMethod(_)
                 | Type::WrapperDescriptor(_)
@@ -6793,6 +6762,7 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
                 | Type::BoundSuper(_)
                 | Type::TypeVar(_),
                 Type::FunctionLiteral(_)
+                | Type::BooleanLiteral(_)
                 | Type::Callable(..)
                 | Type::BoundMethod(_)
                 | Type::WrapperDescriptor(_)
```

---

_@alpaylan reviewed on 2025-06-17 16:30_

---

_Review comment by @alpaylan on `crates/ty_python_semantic/src/types/infer.rs`:6737 on 2025-06-17 16:30_

This works! Committing the local updates now.

---

_@AlexWaygood approved on 2025-06-17 16:51_

Looks great, thank you!

---

_Merged by @AlexWaygood on 2025-06-17 17:02_

---

_Closed by @AlexWaygood on 2025-06-17 17:02_

---

_Renamed from "[ty] fix 'BitwiseAnd' bug" to "[ty] fix binary expression inference between boolean literals and bool instances" by @AlexWaygood on 2025-06-17 17:02_

---
