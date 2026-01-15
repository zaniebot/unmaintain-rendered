```yaml
number: 22601
title: "[ty] Make `NamedTuple(...)` and `namedtuple(...)` calls stricter"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/diags
created_at: 2026-01-15T14:03:40Z
updated_at: 2026-01-15T18:24:29Z
url: https://github.com/astral-sh/ruff/pull/22601
synced_at: 2026-01-15T18:49:33Z
```

# [ty] Make `NamedTuple(...)` and `namedtuple(...)` calls stricter

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/2513.


---

_Label `ty` added by @charliermarsh on 2026-01-15 14:03_

---

_Marked ready for review by @charliermarsh on 2026-01-15 14:03_

---

_Review requested from @carljm by @charliermarsh on 2026-01-15 14:03_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-15 14:03_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-15 14:03_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-15 14:03_

---

_Converted to draft by @charliermarsh on 2026-01-15 14:03_

---

_Comment by @astral-sh-bot[bot] on 2026-01-15 14:05_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-15 14:06_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5410 diagnostics
+ Found 5405 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1824 diagnostics
+ Found 1825 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/tests/utils/mock.py:74:39: error[invalid-argument-type] Invalid `NamedTuple()` field definition: Expected a `(name, type)` tuple, found `Literal["version"]`
- Found 2053 diagnostics
+ Found 2054 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14508 diagnostics
+ Found 14509 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-15 15:10_

---

_Comment by @charliermarsh on 2026-01-15 15:12_

I believe the Rotki diagnostic is a true positive (`NamedTuple('Version', ['version'])`).

---

_Comment by @AlexWaygood on 2026-01-15 15:35_

> I believe the Rotki diagnostic is a true positive (`NamedTuple('Version', ['version'])`).

yup, it fails at runtime (with... not a great error message, hmm):

```pycon
>>> from typing import NamedTuple
>>> NamedTuple('Version', ['version'])(version=1)
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    NamedTuple('Version', ['version'])(version=1)
    ~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 3102, in NamedTuple
    nt = _make_nmtuple(typename, fields, module=_caller())
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 2976, in _make_nmtuple
    fields = [n for n, t in types]
                    ^^^^
ValueError: too many values to unpack (expected 2)
```

---

_Comment by @AlexWaygood on 2026-01-15 15:41_

You can reduce duplicated code between the branches a bit:

```diff
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index 42578d4889..bc86b65b33 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -6522,6 +6522,25 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
         let has_starred = args.iter().any(ast::Expr::is_starred_expr);
         let has_double_starred = keywords.iter().any(|kw| kw.arg.is_none());
 
+        // Emit diagnostic for missing required arguments or unsupported variadic arguments.
+        // For `typing.NamedTuple`, emit a diagnostic since variadic arguments are not supported.
+        // For `collections.namedtuple`, silently fall back since it's more permissive at runtime.
+        if (has_starred || has_double_starred)
+            && kind.is_typing()
+            && let Some(builder) = self.context.report_lint(&INVALID_ARGUMENT_TYPE, call_expr)
+        {
+            let arg_type = if has_starred && has_double_starred {
+                "Variadic positional and keyword arguments are"
+            } else if has_starred {
+                "Variadic positional arguments are"
+            } else {
+                "Variadic keyword arguments are"
+            };
+            builder.into_diagnostic(format_args!(
+                "{arg_type} not supported in `NamedTuple()` calls"
+            ));
+        }
+
         // Need at least typename and fields/field_names.
         let [name_arg, fields_arg, rest @ ..] = &**args else {
             for arg in args {
@@ -6530,30 +6549,8 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
             for kw in keywords {
                 self.infer_expression(&kw.value, TypeContext::default());
             }
-            // Emit diagnostic for missing required arguments or unsupported variadic arguments.
-            if has_starred || has_double_starred {
-                // For `typing.NamedTuple`, emit a diagnostic since variadic arguments are not supported.
-                // For `collections.namedtuple`, silently fall back since it's more permissive at runtime.
-                match kind {
-                    NamedTupleKind::Typing => {
-                        if let Some(builder) =
-                            self.context.report_lint(&INVALID_ARGUMENT_TYPE, call_expr)
-                        {
-                            let arg_type = if has_starred && has_double_starred {
-                                "Variadic positional and keyword arguments are"
-                            } else if has_starred {
-                                "Variadic positional arguments are"
-                            } else {
-                                "Variadic keyword arguments are"
-                            };
-                            builder.into_diagnostic(format_args!(
-                                "{arg_type} not supported in `NamedTuple()` calls"
-                            ));
-                        }
-                    }
-                    NamedTupleKind::Collections => {}
-                }
-            } else {
+
+            if !has_starred && !has_double_starred {
                 let fields_param = match kind {
                     NamedTupleKind::Typing => "fields",
                     NamedTupleKind::Collections => "field_names",
@@ -6586,27 +6583,6 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
             for kw in keywords {
                 self.infer_expression(&kw.value, TypeContext::default());
             }
-            // For `typing.NamedTuple`, emit a diagnostic since variadic arguments are not supported.
-            // For `collections.namedtuple`, silently fall back since it's more permissive at runtime.
-            match kind {
-                NamedTupleKind::Typing => {
-                    if let Some(builder) =
-                        self.context.report_lint(&INVALID_ARGUMENT_TYPE, call_expr)
-                    {
-                        let arg_type = if has_starred && has_double_starred {
-                            "Variadic positional and keyword arguments are"
-                        } else if has_starred {
-                            "Variadic positional arguments are"
-                        } else {
-                            "Variadic keyword arguments are"
-                        };
-                        builder.into_diagnostic(format_args!(
-                            "{arg_type} not supported in `NamedTuple()` calls"
-                        ));
-                    }
-                }
-                NamedTupleKind::Collections => {}
-            }
             return KnownClass::NamedTupleFallback.to_subclass_of(self.db());
         }
 
@@ -15614,6 +15590,10 @@ impl NamedTupleKind {
         matches!(self, Self::Collections)
     }
 
+    const fn is_typing(self) -> bool {
+        matches!(self, Self::Typing)
+    }
+
     fn from_type<'db>(db: &'db dyn Db, ty: Type<'db>) -> Option<Self> {
         match ty {
             Type::SpecialForm(SpecialFormType::NamedTuple) => Some(NamedTupleKind::Typing),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:7140 on 2026-01-15 15:44_

```suggestion
                let field_ty = field_value_ty
                    .in_type_expression(db, scope_id, typevar_binding_context)
                    .unwrap_or_else(|error| {
                        // Report diagnostic for invalid type expression.
                        if let Some(builder) = self
                            .context
                            .report_lint(&INVALID_TYPE_FORM, field_type_expr)
                        {
                            builder.into_diagnostic(format_args!(
                                "Object of type `{}` is not valid as a `NamedTuple` field type",
                                field_value_ty.display(db)
                            ));
                        }
                        error.fallback_type
                    });
```

---

_@AlexWaygood reviewed on 2026-01-15 15:45_

---

_@AlexWaygood approved on 2026-01-15 16:03_

LG other than my two suggested simplifications above!

---

_Merged by @charliermarsh on 2026-01-15 18:24_

---

_Closed by @charliermarsh on 2026-01-15 18:24_

---

_Branch deleted on 2026-01-15 18:24_

---
