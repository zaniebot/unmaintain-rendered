```yaml
number: 22496
title: "[ty] Preserve argument signature in `@total_ordering`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: charlie/total-ord
created_at: 2026-01-10T16:12:54Z
updated_at: 2026-01-10T19:35:59Z
url: https://github.com/astral-sh/ruff/pull/22496
synced_at: 2026-01-12T02:32:44Z
```

# [ty] Preserve argument signature in `@total_ordering`

---

_Pull request opened by @charliermarsh on 2026-01-10 16:12_

## Summary

Closes https://github.com/astral-sh/ty/issues/2435.


---

_Label `bug` added by @charliermarsh on 2026-01-10 16:12_

---

_Label `ty` added by @charliermarsh on 2026-01-10 16:12_

---

_Marked ready for review by @charliermarsh on 2026-01-10 16:13_

---

_Review requested from @carljm by @charliermarsh on 2026-01-10 16:13_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-10 16:13_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-10 16:13_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-10 16:13_

---

_Comment by @astral-sh-bot[bot] on 2026-01-10 16:14_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-10 16:15_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mkosi (https://github.com/systemd/mkosi)
- mkosi/__init__.py:1769:13: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal["258"]`
- mkosi/__init__.py:1829:13: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal["256"]`
- mkosi/__init__.py:1836:17: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion & ~AlwaysFalsy` and `Literal["256"]`
- mkosi/__init__.py:3451:17: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal[256]`
- mkosi/__init__.py:3456:17: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal[258]`
- mkosi/bootloader.py:738:83: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal["257"]`
- mkosi/config.py:1565:8: error[unsupported-operator] Operator `>` is not supported between objects of type `GenericVersion` and `Literal["27~devel"]`
- mkosi/config.py:1571:21: error[unsupported-operator] Operator `>` is not supported between objects of type `GenericVersion` and `str & ~AlwaysFalsy`
- mkosi/distribution/centos.py:61:13: error[unsupported-operator] Operator `>` is not supported between objects of type `GenericVersion` and `Literal[9]`
- mkosi/distribution/centos.py:70:12: error[unsupported-operator] Operator `<=` is not supported between objects of type `GenericVersion` and `Literal[8]`
- mkosi/distribution/centos.py:255:12: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal[10]`
- mkosi/distribution/opensuse.py:71:60: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal[16]`
- mkosi/distribution/opensuse.py:219:20: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal[16]`
- mkosi/qemu.py:276:24: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal["0.10.0"]`
- mkosi/tree.py:149:69: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal["9.5"]`
- Found 92 diagnostics
+ Found 77 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
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

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/aglib/nearpc.py:304:12: error[unsupported-operator] Operator `>` is not supported between objects of type `Parameter` and `Literal[0]`
- pwndbg/aglib/nearpc.py:310:16: error[unsupported-operator] Operator `>` is not supported between objects of type `Parameter` and `Literal[0]`
- pwndbg/commands/context.py:388:8: error[unsupported-operator] Operator `<=` is not supported between objects of type `Parameter` and `Literal[0]`
- pwndbg/commands/context.py:403:12: error[unsupported-operator] Operator `<=` is not supported between objects of type `Parameter` and `Literal[0]`
- pwndbg/commands/context.py:739:43: error[unsupported-operator] Operator `<=` is not supported between objects of type `Parameter` and `Literal[0]`
- pwndbg/commands/telescope.py:217:32: error[unsupported-operator] Operator `>=` is not supported between objects of type `int` and `Parameter`
- pwndbg/lib/pretty_print.py:327:12: error[unsupported-operator] Operator `>` is not supported between objects of type `Parameter` and `Literal[0]`
- Found 2062 diagnostics
+ Found 2055 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5170 diagnostics
+ Found 5169 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14503 diagnostics
+ Found 14502 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@MichaReiser reviewed on 2026-01-10 16:24_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:2490 on 2026-01-10 16:24_

It shows that you aren't a fan of let chains ;)

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/class.rs`:2490 on 2026-01-10 17:24_

Lmao... https://github.com/astral-sh/ruff/pull/22497

---

_@charliermarsh reviewed on 2026-01-10 17:24_

---

_@AlexWaygood reviewed on 2026-01-10 17:30_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2491 on 2026-01-10 17:30_

this let-chain is decent but I want moar let chain:

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index e8b1506477..0eb84fbe97 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -2484,28 +2484,27 @@ impl<'db> ClassLiteral<'db> {
         // ordering method. The decorator requires at least one of __lt__,
         // __le__, __gt__, or __ge__ to be defined (either in this class or
         // inherited from a superclass, excluding `object`).
-        if self.total_ordering(db) && matches!(name, "__lt__" | "__le__" | "__gt__" | "__ge__") {
-            if self.has_ordering_method_in_mro(db, specialization) {
-                if let Some(root_method_ty) = self.total_ordering_root_method(db, specialization)
-                    && let Some(callables) = root_method_ty.try_upcast_to_callable(db)
-                {
-                    let bool_ty = KnownClass::Bool.to_instance(db);
-                    let synthesized_callables = callables.map(|callable| {
-                        let signatures = CallableSignature::from_overloads(
-                            callable.signatures(db).iter().map(|signature| {
-                                Signature::new_generic(
-                                    signature.generic_context,
-                                    signature.parameters().clone(),
-                                    bool_ty,
-                                )
-                            }),
-                        );
-                        CallableType::new(db, signatures, CallableTypeKind::FunctionLike)
-                    });
+        if self.total_ordering(db)
+            && matches!(name, "__lt__" | "__le__" | "__gt__" | "__ge__")
+            && self.has_ordering_method_in_mro(db, specialization)
+            && let Some(root_method_ty) = self.total_ordering_root_method(db, specialization)
+            && let Some(callables) = root_method_ty.try_upcast_to_callable(db)
+        {
+            let bool_ty = KnownClass::Bool.to_instance(db);
+            let synthesized_callables = callables.map(|callable| {
+                let signatures = CallableSignature::from_overloads(
+                    callable.signatures(db).iter().map(|signature| {
+                        Signature::new_generic(
+                            signature.generic_context,
+                            signature.parameters().clone(),
+                            bool_ty,
+                        )
+                    }),
+                );
+                CallableType::new(db, signatures, CallableTypeKind::FunctionLike)
+            });
 
-                    return Some(synthesized_callables.into_type(db));
-                }
-            }
+            return Some(synthesized_callables.into_type(db));
         }
```

---

_@charliermarsh reviewed on 2026-01-10 17:31_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/class.rs`:2491 on 2026-01-10 17:31_

Oh my gosh

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1602 on 2026-01-10 17:39_

_Why_ do we "prefer methods defined on the class itself"? That doesn't seem to be how `total_ordering` behaves at runtime -- at runtime, it _always_ prefers a `__lt__` method over a `__gt__` method, even if the `__lt__` method was defined on a superclass and the `__gt__` method was defined on the class itself:

```pycon
>>> class Base:
...     def __lt__(self, other):
...         return True
...         
>>> class Child(Base):
...     def __gt__(self, other):
...         1/0
...         
>>> from functools import total_ordering
>>> Transformed = total_ordering(Child)
>>> Transformed() <= Transformed()
True
>>> Transformed() >= Transformed()
False
```

---

_@AlexWaygood reviewed on 2026-01-10 17:42_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/class.rs`:1602 on 2026-01-10 18:01_

You're absolutely right!

---

_@charliermarsh reviewed on 2026-01-10 18:01_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-10 18:03_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2511 on 2026-01-10 18:38_

I think we want to union together the return type of the root method and `bool` -- the generated methods won't necessarily return `bool` if the root method doesn't:

```pycon
>>> from functools import total_ordering
>>> @total_ordering
... class F:
...     def __init__(self, x: int):
...         self.x: int = x
...         
...     def __lt__(self, other) -> int:
...         return self.x
...         
...     def __eq__(self, other) -> bool:
...         return self.x == other.x
...         
>>> F(42) < F(42)
42
>>> F(42) > F(42)
False
>>> F(42) >= F(42)
False
>>> F(42) <= F(42)
42
```

Interestingly, three different type checkers give three different answers for what those comparisons evaluate to (and I'm proposing to give a fourth answer!). Pyright says they all evaluate to `bool`, pyrefly says they all evaluate to `int`, and mypy says they all evaluate to `Any`. Mypy seems the most accurate there (`Any` is at least not _wrong_ if the root method returns a non-`bool`), but I think we can be more precise. (Mypy only infers `Any` if the root method returns a non-`bool`; if it returns a `bool`, it infers a `bool`.)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1631 on 2026-01-10 18:55_

I would be tempted to inline this method, since I don't think it should be used outside of the `total_ordering` logic (and I think it can be simplified slightly):

```diff
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index 2102467619..6aaa22a60b 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -1626,24 +1626,6 @@ impl<'db> ClassLiteral<'db> {
         None
     }
 
-    /// Returns `true` if a method with the given name is defined in this class's MRO
-    /// (excluding `object`).
-    fn has_method_in_mro(
-        self,
-        db: &'db dyn Db,
-        specialization: Option<Specialization<'db>>,
-        name: &str,
-    ) -> bool {
-        self.iter_mro(db, specialization)
-            .filter_map(ClassBase::into_class)
-            .filter(|class| !class.class_literal(db).0.is_known(db, KnownClass::Object))
-            .any(|class| {
-                class_member(db, class.class_literal(db).0.body_scope(db), name)
-                    .ignore_possibly_undefined()
-                    .is_some()
-            })
-    }
-
     pub(crate) fn generic_context(self, db: &'db dyn Db) -> Option<GenericContext<'db>> {
         // Several typeshed definitions examine `sys.version_info`. To break cycles, we hard-code
         // the knowledge that this class is not generic.
@@ -2497,7 +2479,15 @@ impl<'db> ClassLiteral<'db> {
         // Only synthesize methods that are not already defined in the MRO.
         if self.total_ordering(db)
             && matches!(name, "__lt__" | "__le__" | "__gt__" | "__ge__")
-            && !self.has_method_in_mro(db, specialization, name)
+            && class_member(db, self.body_scope(db), name).is_undefined()
+            && self
+                .class_member_from_mro(
+                    db,
+                    name,
+                    MemberLookupPolicy::MRO_NO_OBJECT_FALLBACK,
+                    self.iter_mro(db, None).skip(1),
+                )
+                .is_undefined()
             && self.has_ordering_method_in_mro(db, specialization)
             && let Some(root_method_ty) = self.total_ordering_root_method(db, specialization)
             && let Some(callables) = root_method_ty.try_upcast_to_callable(db)
```

---

_@AlexWaygood approved on 2026-01-10 18:58_

---

_@AlexWaygood approved on 2026-01-10 19:32_

---

_Merged by @charliermarsh on 2026-01-10 19:35_

---

_Closed by @charliermarsh on 2026-01-10 19:35_

---

_Branch deleted on 2026-01-10 19:35_

---
