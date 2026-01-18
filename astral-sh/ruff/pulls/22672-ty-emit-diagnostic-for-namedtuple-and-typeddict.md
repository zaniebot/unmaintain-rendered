```yaml
number: 22672
title: "[ty] Emit diagnostic for `NamedTuple` and `TypedDict` decorated with dataclass"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
base: main
head: charlie/data-decorator
created_at: 2026-01-18T01:17:41Z
updated_at: 2026-01-18T15:07:22Z
url: https://github.com/astral-sh/ruff/pull/22672
synced_at: 2026-01-18T15:23:26Z
```

# [ty] Emit diagnostic for `NamedTuple` and `TypedDict` decorated with dataclass

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/2515.

Closes https://github.com/astral-sh/ty/issues/2527.


---

_Label `ty` added by @AlexWaygood on 2026-01-18 01:19_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 01:22_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-18 01:24_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
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
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
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
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
- Found 5406 diagnostics
+ Found 5411 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 46 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Yarn[Any] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- Found 1825 diagnostics
+ Found 1824 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14497 diagnostics
+ Found 14496 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-18 01:45_

---

_Review requested from @carljm by @charliermarsh on 2026-01-18 01:45_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-18 01:45_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-18 01:45_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-18 01:45_

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-18 01:45_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2710 on 2026-01-18 11:58_

This method is called `is_named_tuple()`, but it doesn't just return `true` if `self` is a namedtuple class -- it also returns `true` if `self` inherits from a namedtuple class. I think this method should be called `has_named_tuple_class_in_mro()` or similar. 

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:595 on 2026-01-18 11:59_

(same comment here)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:1471 on 2026-01-18 12:01_

I find the parenthetical here slightly confusing, because inheriting from `NamedTuple` just creates a special `tuple` subclass at runtime, and so neither a `NamedTuple` class nor a subclass of a `NamedTuple` class actually has `NamedTuple` in its MRO

```suggestion
Classes that inherit from a `NamedTuple` class also cannot be decorated with `@dataclass`:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:1480 on 2026-01-18 12:02_

maybe the error code should actually be `invalid-dataclass`? `Child` here inherits from a `NamedTuple` class, but it isn't a `NamedTuple` class itself.

We could possibly use `invalid-dataclass` for both the `NamedTuple` case and the `TypedDict` case? They both involve invalid applications of the `@dataclass` decorator

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:2382 on 2026-01-18 12:07_

I'm not sure it'll lead to an `AttributeError`, but it can definitely lead to other kinds of errors. Whether it does depends on exactly how you try to instantiate it:

```pycon
>>> from typing import TypedDict
>>> from dataclasses import dataclass
>>> @dataclass
... class Foo(TypedDict):
...     x: str
...     
>>> Foo(x='x')
{'x': 'x'}
>>> Foo('x')
Traceback (most recent call last):
  File "<python-input-6>", line 1, in <module>
    Foo('x')
    ~~~^^^^^
ValueError: dictionary update sequence element #0 has length 1; 2 is required
```

I think the better rationale when it comes to `TypedDict`s is possibly just that it's conceptually incoherent to apply `@dataclass` to a `TypedDict` class -- `TypedDict`s are abstract structural types that have no effect on the runtime class ("instantiating" a `TypedDict` always gives you a `dict` at runtime), whereas `@dataclass` is a tool for easily customising the creation of new nominal types/classes at runtime

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:701 on 2026-01-18 12:14_

you want to use the `header_range` of the class rather than the full range of the class here -- your diagnostics are currently very verbose if the `TypedDict` or `NamedTuple` class definition spans many lines of code 😄

<img width="2390" height="1426" alt="image" src="https://github.com/user-attachments/assets/0276d72e-6036-4ec3-81d7-4c29b8616f59" />


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:706 on 2026-01-18 12:15_

this is quite a long diagnostic message; I'd split the second half into a subdiagnostic

```suggestion
                    let mut diagnostic = builder.into_diagnostic(format_args!(
                        "Class `{}` inherits from `NamedTuple` and is decorated with `@dataclass`",
                        class.name(self.db()),
                    ));
                    diagnostic.info("An exception will be raised when instantiating the class at runtime");
```

and similar for the `TypedDict` case below

---

_@AlexWaygood reviewed on 2026-01-18 12:18_

Nice. I wonder if we should also flag enums and protocols decorated with `@dataclass`. Those also indicate that the user is probably very confused. And the enum docs explicitly lay out that adding `@dataclass` to an enum class is [not supported](https://docs.python.org/3/howto/enum.html#dataclass-support)

---

_@charliermarsh reviewed on 2026-01-18 13:27_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:1511 on 2026-01-18 13:27_

This is turning out to be fairly annoying to fix.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:1511 on 2026-01-18 13:42_

Looks like it's a pre-existing issue, though? I think fine to defer it for this PR?

---

_@AlexWaygood reviewed on 2026-01-18 13:42_

---

_@charliermarsh reviewed on 2026-01-18 13:43_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:1511 on 2026-01-18 13:43_

👍 Yeah that was my plan.

---

_Converted to draft by @charliermarsh on 2026-01-18 14:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:596 on 2026-01-18 14:11_

I think you can simplify this:

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index 68a3899829..018afa69ef 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -585,13 +585,11 @@ impl<'db> ClassLiteral<'db> {
         }
     }
 
-    /// Returns whether this class is, or inherits from, a `NamedTuple`.
-    pub fn has_named_tuple_class_in_mro(self, db: &'db dyn Db) -> bool {
-        match self {
-            Self::DynamicNamedTuple(_) => true,
-            Self::Static(class) => class.has_named_tuple_class_in_mro(db),
-            Self::Dynamic(_) => false,
-        }
+    /// Returns whether this class is, or inherits from, a `NamedTuple` class.
+    pub(crate) fn has_named_tuple_class_in_mro(self, db: &'db dyn Db) -> bool {
+        self.iter_mro(db)
+            .filter_map(ClassBase::into_class)
+            .any(|class| CodeGeneratorKind::NamedTuple.matches(db, class.class_literal(db), None))
     }
 
     /// Returns whether this class is `builtins.tuple` exactly
@@ -2697,21 +2695,6 @@ impl<'db> StaticClassLiteral<'db> {
             .any(|base| matches!(base, ClassBase::TypedDict))
     }
 
-    /// Return `true` if this class is, or inherits from, a `NamedTuple` (inherits from
-    /// `typing.NamedTuple`, either directly or indirectly, including functional forms like
-    /// `NamedTuple("X", ...)`).
-    pub fn has_named_tuple_class_in_mro(self, db: &'db dyn Db) -> bool {
-        self.explicit_bases(db).iter().any(|base| match base {
-            Type::SpecialForm(SpecialFormType::NamedTuple) => true,
-            Type::ClassLiteral(ClassLiteral::DynamicNamedTuple(_)) => true,
-            Type::ClassLiteral(ClassLiteral::Static(class)) => {
-                class.has_named_tuple_class_in_mro(db)
-            }
-            Type::GenericAlias(alias) => alias.origin(db).has_named_tuple_class_in_mro(db),
-            _ => false,
-        })
-    }
-
     /// Compute `TypedDict` parameters dynamically based on MRO detection and AST parsing.
     fn typed_dict_params(self, db: &'db dyn Db) -> Option<TypedDictParams> {
         if !self.is_typed_dict(db) {
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index 9b57950b18..3b97b9f454 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -697,7 +697,7 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
             }
 
             // (4) Check if a `NamedTuple` class is decorated with `@dataclass`.
-            if class.has_named_tuple_class_in_mro(self.db())
+            if ClassLiteral::Static(class).has_named_tuple_class_in_mro(self.db())
                 && class.dataclass_params(self.db()).is_some()
             {
                 if let Some(builder) = self
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:4717 on 2026-01-18 14:13_

these two should also use the `invalid-dataclass` error code, I think

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:2390 on 2026-01-18 14:13_

not sure we need this new error code anymore

---

_@AlexWaygood reviewed on 2026-01-18 14:13_

---

_@AlexWaygood reviewed on 2026-01-18 14:16_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:596 on 2026-01-18 14:16_

Oh, that doesn't work, because `CodeGeneratorKind::NamedTuple.matches()` doesn't actually return `true` if the `NamedTuple` class has `@dataclass` on it (the dataclassness takes "priority").

I still think you can probably simplify it a little though ;)

---

_@AlexWaygood reviewed on 2026-01-18 14:21_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:596 on 2026-01-18 14:21_

Okay _this_ patch passes all your tests 😆

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index 68a3899829..cb97e9f5e9 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -585,13 +585,17 @@ impl<'db> ClassLiteral<'db> {
         }
     }
 
-    /// Returns whether this class is, or inherits from, a `NamedTuple`.
-    pub fn has_named_tuple_class_in_mro(self, db: &'db dyn Db) -> bool {
-        match self {
-            Self::DynamicNamedTuple(_) => true,
-            Self::Static(class) => class.has_named_tuple_class_in_mro(db),
-            Self::Dynamic(_) => false,
-        }
+    /// Returns whether this class is, or inherits from, a `NamedTuple` class.
+    pub(crate) fn has_named_tuple_class_in_mro(self, db: &'db dyn Db) -> bool {
+        self.iter_mro(db)
+            .filter_map(ClassBase::into_class)
+            .any(|class| match class.class_literal(db) {
+                ClassLiteral::DynamicNamedTuple(_) => true,
+                ClassLiteral::Dynamic(_) => false,
+                ClassLiteral::Static(class) => class
+                    .explicit_bases(db)
+                    .contains(&Type::SpecialForm(SpecialFormType::NamedTuple)),
+            })
     }
 
     /// Returns whether this class is `builtins.tuple` exactly
@@ -2697,21 +2701,6 @@ impl<'db> StaticClassLiteral<'db> {
             .any(|base| matches!(base, ClassBase::TypedDict))
     }
 
-    /// Return `true` if this class is, or inherits from, a `NamedTuple` (inherits from
-    /// `typing.NamedTuple`, either directly or indirectly, including functional forms like
-    /// `NamedTuple("X", ...)`).
-    pub fn has_named_tuple_class_in_mro(self, db: &'db dyn Db) -> bool {
-        self.explicit_bases(db).iter().any(|base| match base {
-            Type::SpecialForm(SpecialFormType::NamedTuple) => true,
-            Type::ClassLiteral(ClassLiteral::DynamicNamedTuple(_)) => true,
-            Type::ClassLiteral(ClassLiteral::Static(class)) => {
-                class.has_named_tuple_class_in_mro(db)
-            }
-            Type::GenericAlias(alias) => alias.origin(db).has_named_tuple_class_in_mro(db),
-            _ => false,
-        })
-    }
-
     /// Compute `TypedDict` parameters dynamically based on MRO detection and AST parsing.
     fn typed_dict_params(self, db: &'db dyn Db) -> Option<TypedDictParams> {
         if !self.is_typed_dict(db) {
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index 9b57950b18..3b97b9f454 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -697,7 +697,7 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
             }
 
             // (4) Check if a `NamedTuple` class is decorated with `@dataclass`.
-            if class.has_named_tuple_class_in_mro(self.db())
+            if ClassLiteral::Static(class).has_named_tuple_class_in_mro(self.db())
                 && class.dataclass_params(self.db()).is_some()
             {
                 if let Some(builder) = self
```

---

_Marked ready for review by @charliermarsh on 2026-01-18 14:34_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2704 on 2026-01-18 14:39_

this method is now unused (Clippy didn't complain about it because it's `pub`)

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:4227 on 2026-01-18 14:49_

I'd be inclined to consolidate these into one variant, since they all share the same error code:

```diff
diff --git a/crates/ty_python_semantic/src/types/call/bind.rs b/crates/ty_python_semantic/src/types/call/bind.rs
index a4998b973d..2ab1b3049e 100644
--- a/crates/ty_python_semantic/src/types/call/bind.rs
+++ b/crates/ty_python_semantic/src/types/call/bind.rs
@@ -614,13 +614,29 @@ impl<'db> Bindings<'db> {
                     Type::DataclassDecorator(params) => match overload.parameter_types() {
                         [Some(Type::ClassLiteral(class_literal))] => {
                             if class_literal.has_named_tuple_class_in_mro(db) {
-                                overload.errors.push(BindingError::DataclassOnNamedTuple);
+                                overload
+                                    .errors
+                                    .push(BindingError::InvalidDataclassApplication(
+                                        DataclassApplicationError::NamedTuple,
+                                    ));
                             } else if class_literal.is_typed_dict(db) {
-                                overload.errors.push(BindingError::DataclassOnTypedDict);
+                                overload
+                                    .errors
+                                    .push(BindingError::InvalidDataclassApplication(
+                                        DataclassApplicationError::TypedDict,
+                                    ));
                             } else if is_enum_class(db, Type::from(*class_literal)) {
-                                overload.errors.push(BindingError::DataclassOnEnum);
+                                overload
+                                    .errors
+                                    .push(BindingError::InvalidDataclassApplication(
+                                        DataclassApplicationError::Enum,
+                                    ));
                             } else if class_literal.is_protocol(db) {
-                                overload.errors.push(BindingError::DataclassOnProtocol);
+                                overload
+                                    .errors
+                                    .push(BindingError::InvalidDataclassApplication(
+                                        DataclassApplicationError::Protocol,
+                                    ));
                             } else {
                                 overload.set_return_type(Type::from(
                                     class_literal.with_dataclass_params(db, Some(params)),
@@ -1148,13 +1164,29 @@ impl<'db> Bindings<'db> {
                                 overload.parameter_types()
                             {
                                 if class_literal.has_named_tuple_class_in_mro(db) {
-                                    overload.errors.push(BindingError::DataclassOnNamedTuple);
+                                    overload.errors.push(
+                                        BindingError::InvalidDataclassApplication(
+                                            DataclassApplicationError::NamedTuple,
+                                        ),
+                                    );
                                 } else if class_literal.is_typed_dict(db) {
-                                    overload.errors.push(BindingError::DataclassOnTypedDict);
+                                    overload.errors.push(
+                                        BindingError::InvalidDataclassApplication(
+                                            DataclassApplicationError::TypedDict,
+                                        ),
+                                    );
                                 } else if is_enum_class(db, Type::from(*class_literal)) {
-                                    overload.errors.push(BindingError::DataclassOnEnum);
+                                    overload.errors.push(
+                                        BindingError::InvalidDataclassApplication(
+                                            DataclassApplicationError::Enum,
+                                        ),
+                                    );
                                 } else if class_literal.is_protocol(db) {
-                                    overload.errors.push(BindingError::DataclassOnProtocol);
+                                    overload.errors.push(
+                                        BindingError::InvalidDataclassApplication(
+                                            DataclassApplicationError::Protocol,
+                                        ),
+                                    );
                                 } else {
                                     let params = DataclassParams::default_params(db);
                                     overload.set_return_type(Type::from(
@@ -4156,6 +4188,14 @@ impl std::fmt::Display for ParameterContexts {
     }
 }
 
+#[derive(Clone, Copy, Debug, PartialEq, Eq)]
+pub(crate) enum DataclassApplicationError {
+    NamedTuple,
+    TypedDict,
+    Enum,
+    Protocol,
+}
+
 #[derive(Clone, Debug, PartialEq, Eq)]
 pub(crate) enum BindingError<'db> {
     /// The type of an argument is not assignable to the annotated type of its corresponding
@@ -4217,14 +4257,8 @@ pub(crate) enum BindingError<'db> {
     /// represents the infinite union of all callables. While such types *are* callable (they pass
     /// `callable()`), any specific call should fail because we don't know the actual signature.
     CalledTopCallable(Type<'db>),
-    /// The `@dataclass` decorator was applied to a `NamedTuple`.
-    DataclassOnNamedTuple,
-    /// The `@dataclass` decorator was applied to a `TypedDict`.
-    DataclassOnTypedDict,
-    /// The `@dataclass` decorator was applied to an `Enum`.
-    DataclassOnEnum,
-    /// The `@dataclass` decorator was applied to a `Protocol`.
-    DataclassOnProtocol,
+    /// The `@dataclass` decorator was applied to an invalid target.
+    InvalidDataclassApplication(DataclassApplicationError),
 }
 
 impl<'db> BindingError<'db> {
@@ -4673,46 +4707,44 @@ impl<'db> BindingError<'db> {
                 }
             }
 
-            Self::DataclassOnNamedTuple => {
+            Self::InvalidDataclassApplication(kind) => {
                 let node = Self::get_node(node, None);
-                if let Some(builder) = context.report_lint(&INVALID_DATACLASS, node) {
-                    let mut diag = builder.into_diagnostic(
-                        "Cannot use `dataclass()` on a class that inherits from `NamedTuple`",
-                    );
-                    diag.info(
-                        "An exception will be raised when instantiating the class at runtime",
-                    );
-                }
-            }
-
-            Self::DataclassOnTypedDict => {
-                let node = Self::get_node(node, None);
-                if let Some(builder) = context.report_lint(&INVALID_DATACLASS, node) {
-                    let mut diag = builder.into_diagnostic(
-                        "Cannot use `dataclass()` on a class that inherits from `TypedDict`",
-                    );
-                    diag.info(
-                        "An exception will be raised when instantiating the class at runtime",
-                    );
-                }
-            }
-
-            Self::DataclassOnEnum => {
-                let node = Self::get_node(node, None);
-                if let Some(builder) = context.report_lint(&INVALID_DATACLASS, node) {
-                    let mut diag = builder.into_diagnostic(
-                        "Cannot use `dataclass()` on a class that inherits from `Enum`",
-                    );
-                    diag.info("Dataclass support for enums is explicitly not supported");
-                }
-            }
 
-            Self::DataclassOnProtocol => {
-                let node = Self::get_node(node, None);
                 if let Some(builder) = context.report_lint(&INVALID_DATACLASS, node) {
-                    let mut diag =
-                        builder.into_diagnostic("Cannot use `dataclass()` on a protocol class");
-                    diag.info("Protocols define interfaces and cannot be instantiated");
+                    match kind {
+                        DataclassApplicationError::NamedTuple => {
+                            let mut diag = builder.into_diagnostic(
+                                "Cannot use `dataclass()` on a class that inherits \
+                                from `NamedTuple`",
+                            );
+                            diag.info(
+                                "An exception will be raised when instantiating \
+                                the class at runtime",
+                            );
+                        }
+                        DataclassApplicationError::TypedDict => {
+                            let mut diag = builder
+                                .into_diagnostic("Cannot use `dataclass()` on a `TypedDict` class");
+                            diag.info(
+                                "An exception may be raised when instantiating \
+                                the class at runtime",
+                            );
+                        }
+                        DataclassApplicationError::Enum => {
+                            let mut diag = builder
+                                .into_diagnostic("Cannot use `dataclass()` on an enum class");
+                            diag.info(
+                                "Applying `@dataclass` to an enum is explicitly not supported",
+                            );
+                        }
+                        DataclassApplicationError::Protocol => {
+                            let mut diag = builder
+                                .into_diagnostic("Cannot use `dataclass()` on a protocol class");
+                            diag.info(
+                                "Protocols define abstract interfaces and cannot be instantiated",
+                            );
+                        }
+                    }
                 }
             }
         }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:468 on 2026-01-18 14:50_

```suggestion
    /// - `NamedTuple` and `TypedDict` classes will often raise an exception at runtime when
```

since you can avoid the `TypedDict` exception if you're very careful and use keyword arguments to instantiate the class:

```pycon
>>> from typing import TypedDict
>>> from dataclasses import dataclass
>>> @dataclass
... class Foo(TypedDict):
...     x: int
...     
>>> Foo(x=42)
{'x': 42}
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:723 on 2026-01-18 14:52_

```suggestion
                            "TypedDict class `{}` cannot be decorated with `@dataclass`",
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:727 on 2026-01-18 14:52_

```suggestion
                            "An exception may be raised when instantiating the class at runtime",
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:737 on 2026-01-18 14:52_

```suggestion
                            "Enum class `{}` cannot be decorated with `@dataclass`",
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:740 on 2026-01-18 14:52_

```suggestion
                        diagnostic.info("Applying `@dataclass` to an enum is not supported at runtime");
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:749 on 2026-01-18 14:53_

```suggestion
                            "Protocol class `{}` cannot be decorated with `@dataclass`",
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:752 on 2026-01-18 14:53_

```suggestion
                        diagnostic.info("Protocols define abstract interfaces and cannot be instantiated");
```

---

_@AlexWaygood reviewed on 2026-01-18 14:53_

---
