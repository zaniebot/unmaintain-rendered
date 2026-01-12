```yaml
number: 22511
title: "[ty] narrow right comparison op"
type: pull_request
state: open
author: drbh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: narrow-right-comparison-op
created_at: 2026-01-11T23:56:30Z
updated_at: 2026-01-12T11:46:39Z
url: https://github.com/astral-sh/ruff/pull/22511
synced_at: 2026-01-12T11:55:15Z
```

# [ty] narrow right comparison op

---

_Pull request opened by @drbh on 2026-01-11 23:56_

## Summary

This PR narrows the rhs operand similar to the existing functionality on lhs operands seen [here](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/narrow/conditionals/eq.md#bool)

## Test Plan

`repro.py`

```python
from typing import reveal_type

def _(flag: bool):
    x = None if flag else 1

    if None != x:
        reveal_type(x)  # revealed: Literal[1]
    else:
        reveal_type(x)  # revealed: None

def _(flag: bool):
    x = None if flag else 1

    if None == x:
        reveal_type(x)  # revealed: None
    else:
        reveal_type(x)  # revealed: Literal[1]
```

run with

```bash
cargo run --bin ty -- check --python-version 3.12 repro.py
```

output
```bash
info[revealed-type]: Revealed type
 --> repro.py:7:21
  |
6 |     if None != x:
7 |         reveal_type(x)  # revealed: Literal[1]
  |                     ^ `Literal[1]`
8 |     else:
9 |         reveal_type(x)  # revealed: None
  |

info[revealed-type]: Revealed type
  --> repro.py:9:21
   |
 7 |         reveal_type(x)  # revealed: Literal[1]
 8 |     else:
 9 |         reveal_type(x)  # revealed: None
   |                     ^ `None`
10 |
11 | def _(flag: bool):
   |

info[revealed-type]: Revealed type
  --> repro.py:15:21
   |
14 |     if None == x:
15 |         reveal_type(x)  # revealed: None
   |                     ^ `None`
16 |     else:
17 |         reveal_type(x)  # revealed: Literal[1]
   |

info[revealed-type]: Revealed type
  --> repro.py:17:21
   |
15 |         reveal_type(x)  # revealed: None
16 |     else:
17 |         reveal_type(x)  # revealed: Literal[1]
   |                     ^ `Literal[1]`
   |

Found 4 diagnostics
```

## Compared with

this output is an improvement over both `mypy` and `pyright` which incorrectly narrow the type

```bash
uvx mypy repro.py
repro.py:7: note: Revealed type is "builtins.int"
repro.py:9: note: Revealed type is "None | builtins.int"
repro.py:15: note: Revealed type is "None | builtins.int"
repro.py:17: note: Revealed type is "builtins.int"
Success: no issues found in 1 source file
```

```
uvx pyright repro.py
/Users/drbh/Projects/ruff/repro.py
  /Users/drbh/Projects/ruff/repro.py:7:21 - information: Type of "x" is "Literal[1] | None"
  /Users/drbh/Projects/ruff/repro.py:9:21 - information: Type of "x" is "Literal[1] | None"
  /Users/drbh/Projects/ruff/repro.py:15:21 - information: Type of "x" is "Literal[1] | None"
  /Users/drbh/Projects/ruff/repro.py:17:21 - information: Type of "x" is "Literal[1] | None"
0 errors, 0 warnings, 4 informations
```


---

_Review requested from @carljm by @drbh on 2026-01-11 23:56_

---

_Review requested from @AlexWaygood by @drbh on 2026-01-11 23:56_

---

_Review requested from @sharkdp by @drbh on 2026-01-11 23:56_

---

_Review requested from @dcreager by @drbh on 2026-01-11 23:56_

---

_Label `ty` added by @AlexWaygood on 2026-01-11 23:58_

---

_Comment by @astral-sh-bot[bot] on 2026-01-12 00:00_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-12 00:03_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5363 diagnostics
+ Found 5368 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1826 diagnostics
+ Found 1827 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/tests/scalar/test_na_scalar.py:158:12: error[unsupported-operator] Unary operator `-` is not supported for object of type `NAType`
- pandas/tests/scalar/test_na_scalar.py:159:16: error[invalid-argument-type] Argument to function `abs` is incorrect: Expected `SupportsAbs[Unknown]`, found `NAType`
- pandas/tests/scalar/test_na_scalar.py:160:12: error[unsupported-operator] Unary operator `~` is not supported for object of type `NAType`
- pandas/tests/scalar/test_na_scalar.py:165:12: error[unsupported-operator] Operator `&` is not supported between objects of type `Literal[True]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:166:12: error[unsupported-operator] Operator `&` is not supported between objects of type `NAType` and `Literal[False]`
- pandas/tests/scalar/test_na_scalar.py:167:12: error[unsupported-operator] Operator `&` is not supported between objects of type `Literal[False]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:168:12: error[unsupported-operator] Operator `&` is not supported between two objects of type `NAType`
- pandas/tests/scalar/test_na_scalar.py:171:12: error[unsupported-operator] Operator `&` is not supported between objects of type `NAType` and `numpy.bool[builtins.bool]`
- pandas/tests/scalar/test_na_scalar.py:172:12: error[unsupported-operator] Operator `&` is not supported between objects of type `numpy.bool[builtins.bool]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:173:12: error[unsupported-operator] Operator `&` is not supported between objects of type `NAType` and `numpy.bool[builtins.bool]`
- pandas/tests/scalar/test_na_scalar.py:174:12: error[unsupported-operator] Operator `&` is not supported between objects of type `numpy.bool[builtins.bool]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:178:9: error[unsupported-operator] Operator `&` is not supported between objects of type `NAType` and `Literal[5]`
- pandas/tests/scalar/test_na_scalar.py:185:12: error[unsupported-operator] Operator `|` is not supported between objects of type `Literal[False]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:186:12: error[unsupported-operator] Operator `|` is not supported between two objects of type `NAType`
- pandas/tests/scalar/test_na_scalar.py:189:12: error[unsupported-operator] Operator `|` is not supported between objects of type `NAType` and `numpy.bool[builtins.bool]`
- pandas/tests/scalar/test_na_scalar.py:190:12: error[unsupported-operator] Operator `|` is not supported between objects of type `numpy.bool[builtins.bool]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:191:12: error[unsupported-operator] Operator `|` is not supported between objects of type `NAType` and `numpy.bool[builtins.bool]`
- pandas/tests/scalar/test_na_scalar.py:192:12: error[unsupported-operator] Operator `|` is not supported between objects of type `numpy.bool[builtins.bool]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:196:9: error[unsupported-operator] Operator `|` is not supported between objects of type `NAType` and `Literal[5]`
- pandas/tests/scalar/test_na_scalar.py:201:12: error[unsupported-operator] Operator `^` is not supported between objects of type `Literal[True]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:202:12: error[unsupported-operator] Operator `^` is not supported between objects of type `NAType` and `Literal[False]`
- pandas/tests/scalar/test_na_scalar.py:203:12: error[unsupported-operator] Operator `^` is not supported between objects of type `Literal[False]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:204:12: error[unsupported-operator] Operator `^` is not supported between two objects of type `NAType`
- pandas/tests/scalar/test_na_scalar.py:207:12: error[unsupported-operator] Operator `^` is not supported between objects of type `NAType` and `numpy.bool[builtins.bool]`
- pandas/tests/scalar/test_na_scalar.py:208:12: error[unsupported-operator] Operator `^` is not supported between objects of type `numpy.bool[builtins.bool]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:209:12: error[unsupported-operator] Operator `^` is not supported between objects of type `NAType` and `numpy.bool[builtins.bool]`
- pandas/tests/scalar/test_na_scalar.py:210:12: error[unsupported-operator] Operator `^` is not supported between objects of type `numpy.bool[builtins.bool]` and `NAType`
- pandas/tests/scalar/test_na_scalar.py:214:9: error[unsupported-operator] Operator `^` is not supported between objects of type `NAType` and `Literal[5]`
- pandas/tests/scalar/timedelta/test_arithmetic.py:627:25: error[unsupported-operator] Operator `//` is not supported between objects of type `Timedelta` and `NaTType`
- Found 3792 diagnostics
+ Found 3763 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5169 diagnostics
+ Found 5170 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @MichaReiser on 2026-01-12 08:18_

---

_Comment by @astral-sh-bot[bot] on 2026-01-12 08:28_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unsupported-operator` | 0 | 28 | 0 |
| `invalid-return-type` | 0 | 1 | 5 |
| `invalid-argument-type` | 0 | 1 | 0 |
| **Total** | **0** | **30** | **5** |


**[Full report with detailed diff](https://80a6afa2.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://80a6afa2.ty-ecosystem-ext.pages.dev/timing))



---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:1204 on 2026-01-12 11:45_

Why do we exclude call expressions specifically? Is it just so that it doesn't "clobber" the `ast::Expr::Call()` branch immediately below this one? If so, maybe this branch could just go below that one?

No tests fail if I make this change to your PR branch locally:

```diff
diff --git a/crates/ty_python_semantic/src/types/narrow.rs b/crates/ty_python_semantic/src/types/narrow.rs
index 927f0f6943..c1ff380a84 100644
--- a/crates/ty_python_semantic/src/types/narrow.rs
+++ b/crates/ty_python_semantic/src/types/narrow.rs
@@ -1196,33 +1196,6 @@ impl<'db, 'ast> NarrowingConstraintsBuilder<'db, 'ast> {
                         constraints.insert(place, NarrowingConstraint::regular(ty));
                     }
                 }
-                // If left is not a narrowable target and not a Call expression try to narrow the
-                // right operand instead. For symmetric operators (==, !=, is, is not), we can swap
-                // the operands.
-                //
-                // E.g., `None != x` should narrow `x` the same way as `x != None`.
-                _ if !matches!(left, ast::Expr::Call(_))
-                    && matches!(
-                        op,
-                        ast::CmpOp::Eq | ast::CmpOp::NotEq | ast::CmpOp::Is | ast::CmpOp::IsNot
-                    )
-                    && matches!(
-                        right,
-                        ast::Expr::Name(_)
-                            | ast::Expr::Attribute(_)
-                            | ast::Expr::Subscript(_)
-                            | ast::Expr::Named(_)
-                    ) =>
-                {
-                    if let Some(right_place) = place_expr(right)
-                        // Swap lhs_ty and rhs_ty since we're narrowing the right operand
-                        && let Some(ty) =
-                            self.evaluate_expr_compare_op(rhs_ty, lhs_ty, *op, is_positive)
-                    {
-                        let place = self.expect_place(&right_place);
-                        constraints.insert(place, NarrowingConstraint::regular(ty));
-                    }
-                }
                 ast::Expr::Call(ast::ExprCall {
                     range: _,
                     node_index: _,
@@ -1275,6 +1248,31 @@ impl<'db, 'ast> NarrowingConstraintsBuilder<'db, 'ast> {
                         );
                     }
                 }
+                // If left is not a narrowable target and not a Call expression try to narrow the
+                // right operand instead. For symmetric operators (==, !=, is, is not), we can swap
+                // the operands.
+                //
+                // E.g., `None != x` should narrow `x` the same way as `x != None`.
+                _ if matches!(
+                    op,
+                    ast::CmpOp::Eq | ast::CmpOp::NotEq | ast::CmpOp::Is | ast::CmpOp::IsNot
+                ) && matches!(
+                    right,
+                    ast::Expr::Name(_)
+                        | ast::Expr::Attribute(_)
+                        | ast::Expr::Subscript(_)
+                        | ast::Expr::Named(_)
+                ) =>
+                {
+                    if let Some(right_place) = place_expr(right)
+                        // Swap lhs_ty and rhs_ty since we're narrowing the right operand
+                        && let Some(ty) =
+                            self.evaluate_expr_compare_op(rhs_ty, lhs_ty, *op, is_positive)
+                    {
+                        let place = self.expect_place(&right_place);
+                        constraints.insert(place, NarrowingConstraint::regular(ty));
+                    }
+                }
                 _ => {}
             }
         }
```

I was also a bit confused about why you had the `matches!(op, ast::CmpOp::Eq | ast::CmpOp::NotEq | ast::CmpOp::Is | ast::CmpOp::IsNot)` check in there before calling `self.evaluate_expr_compare_op()`, since `self.evaluate_expr_compare_op()` does its own check to see whether the `op` is valid. After staring at it a bit, I think it's because (unlike narrowing for the left-hand side) narrowing the right-hand side is only valid if `op` is a _symmetric_ operator, and `in`/`not in` are not symmetric operators? But it would be great to add a test that would fail without that check -- no tests currently fail if I get rid of it.

---

_@AlexWaygood reviewed on 2026-01-12 11:46_

Thanks, this is great!!

---
