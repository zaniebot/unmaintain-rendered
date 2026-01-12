```yaml
number: 21717
title: "[ty] Add a diagnostic for prohibited `NamedTuple` attribute overrides"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/overrides
created_at: 2025-12-01T02:51:42Z
updated_at: 2025-12-02T02:47:00Z
url: https://github.com/astral-sh/ruff/pull/21717
synced_at: 2026-01-12T15:57:31Z
```

# [ty] Add a diagnostic for prohibited `NamedTuple` attribute overrides

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/1684.


---

_Review requested from @carljm by @charliermarsh on 2025-12-01 02:51_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-01 02:51_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-01 02:51_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-01 02:51_

---

_Review requested from @MichaReiser by @charliermarsh on 2025-12-01 02:51_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 02:53_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-01 02:55_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 45 diagnostics
+ Found 41 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1207:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5815 diagnostics
+ Found 5814 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_Renamed from "Add a diagnostic for prohibited `NamedTuple` attribute overrides" to "[ty] Add a diagnostic for prohibited `NamedTuple` attribute overrides" by @AlexWaygood on 2025-12-01 09:58_

---

_Label `ty` added by @AlexWaygood on 2025-12-01 09:58_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1817 on 2025-12-01 18:42_

is it worth adding a new error code just for this, or would it be better to reuse the `invalid-named-tuple` error code? The name of the new error code seems _very_ long... and I'm not sure I can think of a situation where a user would want to e.g. enable one error code spotting ways namedtuple class creation might fail, but disable another error code that also spots ways namedtuple class creation might fail.

The examples are nice though; we could add those for sure!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/overrides.rs`:372 on 2025-12-01 18:49_

I think you can simplify this (and optimise slightly), e.g.

```diff
diff --git a/crates/ty_python_semantic/src/types/overrides.rs b/crates/ty_python_semantic/src/types/overrides.rs
index f4bd5fc6cd..46016541ac 100644
--- a/crates/ty_python_semantic/src/types/overrides.rs
+++ b/crates/ty_python_semantic/src/types/overrides.rs
@@ -56,12 +56,6 @@ pub(super) fn check_class<'db>(context: &InferContext<'db, '_>, class: ClassLite
     let own_class_members: FxHashSet<_> =
         all_declarations_and_bindings(db, class.body_scope(db)).collect();
 
-    if configuration.check_prohibited_named_tuple_attrs()
-        && CodeGeneratorKind::NamedTuple.matches(db, class, None)
-    {
-        check_prohibited_namedtuple_attrs(context, &own_class_members);
-    }
-
     for member in own_class_members {
         check_class_declaration(context, configuration, class_specialized, &member);
     }
@@ -152,6 +146,27 @@ fn check_class_declaration<'db>(
     let (literal, specialization) = class.class_literal(db);
     let class_kind = CodeGeneratorKind::from_class(db, literal, specialization);
 
+    // Check for prohibited `NamedTuple` attribute overrides.
+    //
+    // `NamedTuple` classes have certain synthesized attributes (like `_asdict`, `_make`, etc.)
+    // that cannot be overwritten. Attempting to assign to these attributes (without type
+    // annotations) or define methods with these names will raise an `AttributeError` at runtime.
+    if class_kind == Some(CodeGeneratorKind::NamedTuple)
+        && configuration.check_prohibited_named_tuple_attrs()
+        && PROHIBITED_NAMEDTUPLE_ATTRS.contains(&member.name.as_str())
+        && !matches!(definition.kind(db), DefinitionKind::AnnotatedAssignment(_))
+        && let Some(builder) = context.report_lint(
+            &OVERRIDE_OF_PROHIBITED_NAMED_TUPLE_ATTRIBUTE,
+            definition.focus_range(db, context.module()),
+        )
+    {
+        let mut diagnostic = builder.into_diagnostic(format_args!(
+            "Cannot overwrite NamedTuple attribute `{}`",
+            &member.name
+        ));
+        diagnostic.info("This will cause the class creation to fail at runtime");
+    }
+
     let mut subclass_overrides_superclass_declaration = false;
     let mut has_dynamic_superclass = false;
     let mut has_typeddict_in_mro = false;
@@ -361,43 +376,6 @@ fn check_class_declaration<'db>(
     }
 }
 
-/// Check for prohibited `NamedTuple` attribute overrides.
-///
-/// `NamedTuple` classes have certain synthesized attributes (like `_asdict`, `_make`, etc.)
-/// that cannot be overwritten. Attempting to assign to these attributes (without type
-/// annotations) or define methods with these names will raise an `AttributeError` at runtime.
-fn check_prohibited_namedtuple_attrs<'db>(
-    context: &InferContext<'db, '_>,
-    own_class_members: &FxHashSet<MemberWithDefinition<'db>>,
-) {
-    let db = context.db();
-    let module = context.module();
-
-    for member in own_class_members {
-        if !PROHIBITED_NAMEDTUPLE_ATTRS.contains(&member.member.name.as_str()) {
-            continue;
-        }
-
-        let Some(definition) = member.definition else {
-            continue;
-        };
-
-        // Only annotated assignments are allowed (they become fields).
-        // Everything else (assignments, method definitions, etc.) is an error.
-        if !matches!(definition.kind(db), DefinitionKind::AnnotatedAssignment(_)) {
-            if let Some(builder) = context.report_lint(
-                &OVERRIDE_OF_PROHIBITED_NAMED_TUPLE_ATTRIBUTE,
-                definition.full_range(db, module),
-            ) {
-                builder.into_diagnostic(format_args!(
-                    "Cannot overwrite NamedTuple attribute `{}`",
-                    member.member.name
-                ));
-            }
-        }
-    }
-}
-
 #[derive(Debug, Clone, Copy, PartialEq, Eq, Default)]
 pub(super) enum MethodKind<'db> {
     Synthesized(CodeGeneratorKind<'db>),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/overrides.rs`:390 on 2025-12-01 18:49_

we should use `.focus_range()` here rather than `.full_range()` -- the full range might be huge!

---

_@AlexWaygood approved on 2025-12-01 18:52_

---

_@charliermarsh reviewed on 2025-12-02 01:23_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/diagnostic.rs`:1817 on 2025-12-02 01:23_

Makes sense, will consolidate.

---

_Merged by @charliermarsh on 2025-12-02 02:46_

---

_Closed by @charliermarsh on 2025-12-02 02:46_

---

_Branch deleted on 2025-12-02 02:47_

---
