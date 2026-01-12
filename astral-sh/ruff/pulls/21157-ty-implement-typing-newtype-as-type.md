```yaml
number: 21157
title: "[ty] implement `typing.NewType` as `Type::NewTypeInstance`"
type: pull_request
state: merged
author: oconnor663
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: jack/newtype4
created_at: 2025-10-31T02:26:19Z
updated_at: 2025-11-11T15:49:41Z
url: https://github.com/astral-sh/ruff/pull/21157
synced_at: 2026-01-12T15:57:17Z
```

# [ty] implement `typing.NewType` as `Type::NewTypeInstance`

---

_@oconnor663_

Fixes https://github.com/astral-sh/ty/issues/742. This PR rewrites and supersedes my previous `NewType` PR, https://github.com/astral-sh/ruff/pull/20126. The high-level changes:

- I've taken @AlexWaygood's advice and added `Type::NewTypeInstance` instead of expanding `NominalInstanceInner`.
- Rebasing on top of https://github.com/astral-sh/ruff/pull/20598 required a rewrite of how the bindings are wired up. Now it's a new special case alongside `infer_legacy_typevar` in `TypeInferenceBuilder::infer_assignment_definition_impl`.
- Related to that, inference of the base type is now deferred. This makes it possible to support cyclic newtypes, and there's a section of test cases for this in the new mdtests.

~~There are several `// REVIEWERS:` questions inline, and I expect I've gone about some things the wrong way (particularly cycle detection?), so I'm leaving this in Draft status for now.~~ I've moved this out of Draft.

---

_Comment by @github-actions[bot] on 2025-10-31 02:28_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-10 22:18:44.267868777 +0000
+++ new-output.txt	2025-11-10 22:18:47.672897783 +0000
@@ -1,4 +1,4 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/05a9af7/src/function/execute.rs:451:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(18f71)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/05a9af7/src/function/execute.rs:451:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(19371)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
@@ -38,11 +38,19 @@
 aliases_implicit.py:118:10: error[invalid-type-form] Variable of type `Literal["int"]` is not allowed in a type expression
 aliases_implicit.py:119:10: error[invalid-type-form] Variable of type `Literal["int | str"]` is not allowed in a type expression
 aliases_implicit.py:133:6: error[call-non-callable] Object of type `UnionType` is not callable
-aliases_newtype.py:15:1: error[type-assertion-failure] Argument does not have asserted type `int`
-aliases_newtype.py:18:1: error[invalid-assignment] Object of type `NewType` is not assignable to `type`
-aliases_newtype.py:23:16: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `NewType`
-aliases_newtype.py:26:21: error[invalid-base] Invalid class base with type `NewType`
-aliases_newtype.py:63:43: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 3, got 4
+aliases_newtype.py:11:8: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["user"]`
+aliases_newtype.py:12:1: error[invalid-assignment] Object of type `Literal[42]` is not assignable to `UserId`
+aliases_newtype.py:18:1: error[invalid-assignment] Object of type `<NewType pseudo-class 'UserId'>` is not assignable to `type`
+aliases_newtype.py:23:16: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `<NewType pseudo-class 'UserId'>`
+aliases_newtype.py:26:21: error[invalid-base] Cannot subclass an instance of NewType
+aliases_newtype.py:35:1: error[invalid-newtype] The name of a `NewType` (`BadName`) must match the name of the variable it is assigned to (`GoodName`)
+aliases_newtype.py:41:6: error[invalid-type-form] `GoodNewType1` is a `NewType` and cannot be specialized
+aliases_newtype.py:47:38: error[invalid-newtype] invalid base for `typing.NewType`: type `int | str`
+aliases_newtype.py:52:38: error[invalid-newtype] invalid base for `typing.NewType`: type `Hashable`
+aliases_newtype.py:54:38: error[invalid-newtype] invalid base for `typing.NewType`: type `Literal[7]`
+aliases_newtype.py:61:38: error[invalid-newtype] invalid base for `typing.NewType`: type `TD1`
+aliases_newtype.py:63:15: error[invalid-newtype] Wrong number of arguments in `NewType` creation, expected 2, found 3
+aliases_newtype.py:65:38: error[invalid-newtype] invalid base for `typing.NewType`: type `Any`
 aliases_type_statement.py:10:19: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 3
 aliases_type_statement.py:17:1: error[unresolved-attribute] Object of type `typing.TypeAliasType` has no attribute `bit_count`
 aliases_type_statement.py:19:1: error[call-non-callable] Object of type `TypeAliasType` is not callable
@@ -579,7 +587,7 @@
 generics_typevartuple_basic.py:12:14: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_basic.py:16:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_basic.py:23:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
-generics_typevartuple_basic.py:42:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[@Todo(PEP 646), ...]`, found `Literal[1]`
+generics_typevartuple_basic.py:42:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[@Todo(PEP 646), ...]`, found `Height`
 generics_typevartuple_basic.py:65:27: error[unknown-argument] Argument `covariant` does not match any known parameter of function `__new__`
 generics_typevartuple_basic.py:66:27: error[too-many-positional-arguments] Too many positional arguments to function `__new__`: expected 2, got 4
 generics_typevartuple_basic.py:67:27: error[unknown-argument] Argument `bound` does not match any known parameter of function `__new__`
@@ -999,5 +1007,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 1001 diagnostics
+Found 1009 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_Label `ty` added by @AlexWaygood on 2025-10-31 02:31_

---

_Renamed from "implement `typing.NewType` as `Type::NewTypeInstance`" to "[ty] implement `typing.NewType` as `Type::NewTypeInstance`" by @AlexWaygood on 2025-10-31 02:32_

---

_@oconnor663 reviewed on 2025-10-31 02:38_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/newtype.rs`:234 on 2025-10-31 02:38_

@AlexWaygood One of the downsides of using the restricted `NewTypeBase` enum instead of just making the base a `Type`, is that calling `visitor.visit()` here isn't "natural". We need to synthesize a `Type` just to be able to make this call for its side effects, and then we discard the return value. But if we omit this (and of course I omitted it at first), the last line of the mdtests crashes with a stack overflow. I wonder if that's an argument for making our visitor machinery more flexible somehow?

(Incidentally `normalized_impl` is in the exact same situation, but I don't think I have a test case to provoke a stack overflow for it. Any suggestions?)

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-10-31 02:41_

---

_Comment by @github-actions[bot] on 2025-10-31 02:47_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 498 | 0 | 8 |
| `unused-ignore-comment` | 0 | 48 | 0 |
| `invalid-type-form` | 2 | 22 | 0 |
| `unsupported-operator` | 4 | 0 | 5 |
| `invalid-newtype` | 5 | 0 | 0 |
| `invalid-assignment` | 3 | 0 | 1 |
| `invalid-return-type` | 1 | 1 | 2 |
| `type-assertion-failure` | 2 | 0 | 2 |
| `no-matching-overload` | 3 | 0 | 0 |
| `call-non-callable` | 0 | 0 | 1 |
| `non-subscriptable` | 1 | 0 | 0 |
| `unresolved-attribute` | 0 | 0 | 1 |
| **Total** | **519** | **71** | **20** |

**[Full report with detailed diff](https://jack-newtype4.ecosystem-663.pages.dev/diff)** ([timing results](https://jack-newtype4.ecosystem-663.pages.dev/timing))




---

_@AlexWaygood reviewed on 2025-10-31 19:48_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/newtype.rs`:234 on 2025-10-31 19:48_

Ah I think you just want to do the `visitor.visit()` thing in the `match` in `types.rs`? All tests pass if I make this change to your branch, anyway :-)

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 2c65727ee2..0cfc92c5dd 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -6893,7 +6893,7 @@ impl<'db> Type<'db> {
             },
 
             Type::NewTypeInstance(newtype) => {
-                Type::NewTypeInstance(newtype.apply_type_mapping_impl(db, type_mapping, tcx, visitor))
+                visitor.visit(self, || Type::NewTypeInstance(newtype.apply_type_mapping_impl(db, type_mapping, tcx, visitor)))
             },
 
             Type::ProtocolInstance(instance) => {
diff --git a/crates/ty_python_semantic/src/types/newtype.rs b/crates/ty_python_semantic/src/types/newtype.rs
index 57c879343e..f9dda5ec78 100644
--- a/crates/ty_python_semantic/src/types/newtype.rs
+++ b/crates/ty_python_semantic/src/types/newtype.rs
@@ -214,30 +214,7 @@ impl<'db> NewType<'db> {
         visitor: &ApplyTypeMappingVisitor<'db>,
     ) -> Self {
         self.map_base_class_type(db, |base_class_type| {
-            // We don't really want to work with `Type` directly (`NewTypeBase` is much more
-            // restricted), but we need to let the `ApplyTypeMappingVisitor` see the `Type` of the
-            // base class, because this is its only chance to detect an indirect cycle like:
-            //
-            // ```py
-            // Foo = NewType("Foo", list["Foo"])
-            // ```
-            //
-            // Note that NewTypeBaseIter does *not* catch this kind of cycle, because it stops
-            // after returning `NewTypeBase::ClassType` for `list`.
-            let mut is_first_encounter = false;
-            let base_type = Type::instance(db, base_class_type);
-            _ = visitor.visit(base_type, || {
-                // If we hit in a cycle, this closure won't run.
-                is_first_encounter = true;
-                base_type
-            });
-            if is_first_encounter {
-                base_class_type.apply_type_mapping_impl(db, type_mapping, tcx, visitor)
-            } else {
-                // Cycle detected. Fall back to `object`.
-                // REVIEWERS: Is this correct?
-                ClassType::object(db)
-            }
+            base_class_type.apply_type_mapping_impl(db, type_mapping, tcx, visitor)
         })
     }
 }
```

---

_@oconnor663 reviewed on 2025-10-31 19:54_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/newtype.rs`:234 on 2025-10-31 19:54_

Ohhhh that does make sense.

---

_Comment by @AlexWaygood on 2025-10-31 19:59_

This change fixes the panic when type-checking py-fuzzer in CI (and I assume also the panics mypy_primer is surfacing) without breaking any of your tests. I _think_ it's the correct solution... in general, as soon as you do anything to a newtype, it's meant to lose its "newtypeness", so it makes sense to me that obtaining the meta-type of a newtype instance would just return the underlying nominal class for that newtype instance

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 2c65727ee2..13918d56ca 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -6744,9 +6744,7 @@ impl<'db> Type<'db> {
             // understand a more specific meta type in order to correctly handle `__getitem__`.
             Type::TypedDict(typed_dict) => SubclassOfType::from(db, typed_dict.defining_class()),
             Type::TypeAlias(alias) => alias.value_type(db).to_meta_type(db),
-            Type::NewTypeInstance(newtype) => {
-                Type::KnownInstance(KnownInstanceType::NewType(newtype))
-            }
+            Type::NewTypeInstance(newtype) => Type::from(newtype.base_class_type(db)),
         }
     }
 ```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:4672 on 2025-10-31 20:26_

I think this is probably fine for now? We could always try to factor out the common bits between this and TypeVar parsing as a followup. But it's not _that_ much duplication

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/newtype.rs`:124 on 2025-10-31 20:28_

For consistency with our other modules, I think it would be better to write `is_subtype_of` as a wrapper around `has_relation_to(_impl)`, rather than vice versa. But I think this is correct, yes -- subtyping/assignability/redundancy should all give the same result when the left-hand side and the right-hand side are both newtypes.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/newtype.rs`:203 on 2025-10-31 20:29_

(Same response as https://github.com/astral-sh/ruff/pull/21157/files#r2482511640)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/visitor.rs`:161 on 2025-10-31 20:35_

ummm you could probably argue it both ways. I think this is fine for now; if we find a reason in the future why it isn't fine we can always change it

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7546 on 2025-10-31 20:38_

I _think_ so? You can never have a "generic NewType" -- if it has a generic type as its underlying nominal class, that generic type _must_ have always been specialised. Therefore, it stands to reason that the typevar can never be relevant to this newtype, so must be bivariant with respect to this newtype.

---

_@AlexWaygood reviewed on 2025-10-31 20:38_

(I just looked at the `// REVIEWER` comments for now, since it's still in draft, but happy to look at the other bits if you'd like me to!)

---

_@oconnor663 reviewed on 2025-10-31 23:08_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/newtype.rs`:234 on 2025-10-31 23:08_

f3b6c6794d43ed97ce09707ce457b6c62c646335

---

_Comment by @github-actions[bot] on 2025-10-31 23:50_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
paroxython (https://github.com/laowantong/paroxython)
+ paroxython/__init__.py:55:31: error[invalid-argument-type] Argument to function `main` is incorrect: Expected `Source`, found `str`
- Found 5 diagnostics
+ Found 6 diagnostics

websockets (https://github.com/aaugustin/websockets)
- src/websockets/server.py:110:17: error[unresolved-attribute] Object of type `(ServerProtocol, Sequence[@Todo], /) -> @Todo | None` has no attribute `__get__`
+ src/websockets/server.py:110:17: error[unresolved-attribute] Object of type `(ServerProtocol, Sequence[Subprotocol], /) -> Subprotocol | None` has no attribute `__get__`

asynq (https://github.com/quora/asynq)
+ asynq/tools.py:467:28: error[unsupported-operator] Operator `-` is unsupported between objects of type `Utime` and `Unknown | None | Utime`
- Found 171 diagnostics
+ Found 172 diagnostics

kopf (https://github.com/nolar/kopf)
- kopf/_cogs/clients/creating.py:13:20: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_cogs/clients/creating.py:27:22: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_cogs/clients/fetching.py:13:20: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_cogs/clients/patching.py:11:20: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_cogs/clients/watching.py:55:20: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_cogs/clients/watching.py:109:20: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_cogs/clients/watching.py:168:20: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_cogs/clients/watching.py:235:20: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_cogs/structs/bodies.py:159:28: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_cogs/structs/bodies.py:160:21: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_cogs/structs/references.py:199:24: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_cogs/structs/references.py:492:21: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_core/engines/indexing.py:12:13: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_core/engines/peering.py:93:20: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_core/engines/peering.py:166:20: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_core/engines/peering.py:209:20: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_core/engines/peering.py:241:20: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_core/reactor/orchestration.py:44:16: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_core/reactor/orchestration.py:173:42: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_core/reactor/orchestration.py:193:32: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_core/reactor/orchestration.py:234:38: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- kopf/_core/reactor/queueing.py:123:20: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- Found 86 diagnostics
+ Found 64 diagnostics

rich (https://github.com/Textualize/rich)
+ tests/test_progress.py:57:17: error[invalid-argument-type] Argument is incorrect: Expected `TaskID`, found `Literal[1]`
+ tests/test_progress.py:66:17: error[invalid-argument-type] Argument is incorrect: Expected `TaskID`, found `Literal[1]`
+ tests/test_progress.py:71:17: error[invalid-argument-type] Argument is incorrect: Expected `TaskID`, found `Literal[1]`
+ tests/test_progress.py:78:17: error[invalid-argument-type] Argument is incorrect: Expected `TaskID`, found `Literal[1]`
+ tests/test_progress.py:88:17: error[invalid-argument-type] Argument is incorrect: Expected `TaskID`, found `Literal[1]`
+ tests/test_progress.py:92:28: error[invalid-argument-type] Argument is incorrect: Expected `TaskID`, found `Literal[1]`
+ tests/test_progress.py:125:17: error[invalid-argument-type] Argument is incorrect: Expected `TaskID`, found `Literal[1]`
+ tests/test_progress.py:138:17: error[invalid-argument-type] Argument is incorrect: Expected `TaskID`, found `Literal[1]`
+ tests/test_progress.py:154:22: error[invalid-argument-type] Argument is incorrect: Expected `TaskID`, found `Literal[1]`
+ tests/test_progress.py:162:22: error[invalid-argument-type] Argument is incorrect: Expected `TaskID`, found `Literal[1]`
- Found 323 diagnostics
+ Found 333 diagnostics

mypy-protobuf (https://github.com/dropbox/mypy-protobuf)
+ test/test_generated_mypy.py:484:5: error[type-assertion-failure] Argument does not have asserted type `UserId`
- test/test_generated_mypy.py:486:5: error[type-assertion-failure] Argument does not have asserted type `ScalarMap[@Todo, Email]`
+ test/test_generated_mypy.py:486:5: error[type-assertion-failure] Argument does not have asserted type `ScalarMap[UserId, Email]`
+ test/test_generated_mypy.py:495:5: error[type-assertion-failure] Argument does not have asserted type `UserId`
- test/test_generated_mypy.py:497:5: error[type-assertion-failure] Argument does not have asserted type `ScalarMap[@Todo, Email]`
+ test/test_generated_mypy.py:497:5: error[type-assertion-failure] Argument does not have asserted type `ScalarMap[UserId, Email]`
- Found 56 diagnostics
+ Found 58 diagnostics

trio (https://github.com/python-trio/trio)
+ src/trio/_core/_generated_io_windows.py:112:68: error[invalid-argument-type] Argument to bound method `notify_closing` is incorrect: Expected `int | _HasFileNo`, found `Handle | int | _HasFileNo`
+ src/trio/_tests/test_wait_for_object.py:149:43: error[invalid-argument-type] Argument to bound method `cast` is incorrect: Expected `_CDataBase | int`, found `Handle`
+ src/trio/_tests/test_wait_for_object.py:200:43: error[invalid-argument-type] Argument to bound method `cast` is incorrect: Expected `_CDataBase | int`, found `Handle`
+ src/trio/_wait_for_object.py:63:9: error[invalid-assignment] Cannot assign to object of type `HandleArray` with no `__setitem__` method
- Found 594 diagnostics
+ Found 598 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/federation/schema.py:41:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ strawberry/federation/schema.py:152:49: error[invalid-type-form] Variable of type `NewType` is not allowed in a type expression
+ strawberry/federation/schema.py:153:15: error[invalid-type-form] Variable of type `NewType` is not allowed in a type expression
+ strawberry/file_uploads/scalars.py:5:17: error[invalid-newtype] A `NewType` definition must be a simple variable assignment
+ strawberry/scalars.py:18:5: error[invalid-newtype] A `NewType` definition must be a simple variable assignment
+ strawberry/scalars.py:32:5: error[invalid-newtype] A `NewType` definition must be a simple variable assignment
+ strawberry/scalars.py:40:5: error[invalid-newtype] A `NewType` definition must be a simple variable assignment
+ strawberry/scalars.py:50:5: error[invalid-newtype] A `NewType` definition must be a simple variable assignment
- strawberry/schema/schema_converter.py:807:37: error[invalid-argument-type] Argument to bound method `from_scalar` is incorrect: Expected `type`, found `NewType`
+ strawberry/schema/schema_converter.py:807:37: error[invalid-argument-type] Argument to bound method `from_scalar` is incorrect: Expected `type`, found `<NewType pseudo-class 'ID'>`
- strawberry/types/arguments.py:237:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 376 diagnostics
+ Found 381 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/build.py:2813:89: error[invalid-argument-type] Argument to bound method `single_use` is incorrect: Expected `SubProject`, found `str`
+ mesonbuild/build.py:2885:40: error[invalid-argument-type] Argument is incorrect: Expected `SubProject`, found `str`
+ mesonbuild/build.py:3084:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SubProject`, found `str`
+ mesonbuild/build.py:3128:40: error[invalid-argument-type] Argument is incorrect: Expected `SubProject`, found `str`
+ mesonbuild/build.py:3188:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SubProject`, found `str`
+ mesonbuild/interpreter/interpreter.py:167:98: error[invalid-argument-type] Argument to bound method `single_use` is incorrect: Expected `SubProject`, found `str`
+ mesonbuild/interpreter/interpreter.py:170:76: error[invalid-argument-type] Argument to bound method `single_use` is incorrect: Expected `SubProject`, found `str`
+ mesonbuild/interpreter/interpreter.py:173:82: error[invalid-argument-type] Argument to bound method `single_use` is incorrect: Expected `SubProject`, found `str`
+ mesonbuild/interpreter/interpreter.py:259:71: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SubProject`, found `str`
+ mesonbuild/interpreter/interpreter.py:1442:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SubProject`, found `str`
- mesonbuild/interpreterbase/decorators.py:45:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[BaseNode, list[@Todo], Any, @Todo]`, found `tuple[Any, None | Any, None | Any, Any]`
+ mesonbuild/interpreterbase/decorators.py:45:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[BaseNode, list[@Todo], Any, SubProject]`, found `tuple[Any, None | Any, None | Any, Any]`
+ unittests/datatests.py:249:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SubProject`, found `Literal[""]`
+ unittests/platformagnostictests.py:41:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SubProject`, found `Literal[""]`
+ unittests/platformagnostictests.py:75:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SubProject`, found `Literal[""]`
- Found 1660 diagnostics
+ Found 1673 diagnostics

bokeh (https://github.com/bokeh/bokeh)
+ src/bokeh/models/widgets/buttons.py:225:49: error[invalid-argument-type] Argument is incorrect: Expected `ID`, found `Literal["help"]`
+ src/bokeh/server/views/autoload_js_handler.py:88:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ID | None`, found `str & ~AlwaysFalsy`
+ src/bokeh/server/views/session_handler.py:87:9: error[invalid-assignment] Object of type `str | None` is not assignable to `ID | None`
+ src/bokeh/server/views/session_handler.py:92:13: error[invalid-assignment] Object of type `Unknown | str | None` is not assignable to `ID | None`
- Found 651 diagnostics
+ Found 655 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/accounting/pot.py:456:61: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/accounting/structures/processed_event.py:151:42: error[no-matching-overload] No overload of bound method `sub` matches arguments
- rotkehlchen/api/rest.py:2907:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/api/rest.py:2915:89: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/api/rest.py:5905:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/assets/asset.py:59:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/assets/asset.py:597:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/assets/asset.py:603:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/assets/asset.py:773:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/assets/utils.py:118:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/balances/historical.py:73:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[Asset, HistoricalBalance].__setitem__(key: Asset, value: HistoricalBalance, /) -> None` cannot be called with a key of type `Unknown` and a value of type `dict[Unknown | str, Unknown | FVal]` on object of type `dict[Asset, HistoricalBalance]`
+ rotkehlchen/balances/historical.py:73:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[Asset, HistoricalBalance].__setitem__(key: Asset, value: HistoricalBalance, /) -> None` cannot be called with a key of type `Unknown` and a value of type `dict[Unknown | str, Unknown | Price]` on object of type `dict[Asset, HistoricalBalance]`
- rotkehlchen/chain/aggregator.py:586:98: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/aggregator.py:721:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/aggregator.py:1174:13: error[unsupported-operator] Operator `+=` is unsupported between objects of type `list[tuple[Literal[SupportedBlockchain.ETHEREUM, SupportedBlockchain.OPTIMISM, SupportedBlockchain.AVALANCHE, SupportedBlockchain.POLYGON_POS, SupportedBlockchain.ARBITRUM_ONE, ... omitted 5 literals], @Todo]]` and `list[tuple[SupportedBlockchain, @Todo] | Unknown]`
+ rotkehlchen/chain/aggregator.py:1174:13: error[unsupported-operator] Operator `+=` is unsupported between objects of type `list[tuple[Literal[SupportedBlockchain.ETHEREUM, SupportedBlockchain.OPTIMISM, SupportedBlockchain.AVALANCHE, SupportedBlockchain.POLYGON_POS, SupportedBlockchain.ARBITRUM_ONE, ... omitted 5 literals], ChecksumAddress]]` and `list[tuple[SupportedBlockchain, ChecksumAddress] | Unknown]`
- rotkehlchen/chain/aggregator.py:1182:51: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[Literal[SupportedBlockchain.ETHEREUM, SupportedBlockchain.OPTIMISM, SupportedBlockchain.AVALANCHE, SupportedBlockchain.POLYGON_POS, SupportedBlockchain.ARBITRUM_ONE, ... omitted 5 literals], @Todo]`, found `tuple[SupportedBlockchain | Unknown, @Todo]`
+ rotkehlchen/chain/aggregator.py:1182:51: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[Literal[SupportedBlockchain.ETHEREUM, SupportedBlockchain.OPTIMISM, SupportedBlockchain.AVALANCHE, SupportedBlockchain.POLYGON_POS, SupportedBlockchain.ARBITRUM_ONE, ... omitted 5 literals], ChecksumAddress]`, found `tuple[SupportedBlockchain | Unknown, ChecksumAddress]`
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Argument type `A@BaseDecoderTools` does not satisfy constraints (`BTCAddress`, `ChecksumAddress`, `SubstrateAddress`, `SolanaAddress`) of type variable `AnyBlockchainAddress`
- rotkehlchen/chain/ethereum/airdrops.py:666:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/ethereum/airdrops.py:667:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/ethereum/airdrops.py:669:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/ethereum/interfaces/ammswap/ammswap.py:96:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/ethereum/modules/curve/crvusd/decoder.py:70:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/ethereum/modules/eth2/eth2.py:609:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/ethereum/modules/liquity/decoder.py:81:117: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/ethereum/modules/liquity/decoder.py:148:117: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/ethereum/modules/liquity/decoder.py:235:117: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/ethereum/modules/liquity/decoder.py:274:117: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/ethereum/modules/thegraph/accountant.py:30:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/ethereum/modules/yearn/decoder.py:171:62: error[invalid-argument-type] Argument to function `_get_vault_token_name` is incorrect: Expected `ChecksumAddress`, found `Unknown | ChecksumAddress | None`
- rotkehlchen/chain/ethereum/modules/yearn/decoder.py:404:13: error[invalid-return-type] Return type does not match returned value: expected `dict[@Todo, str]`, found `dict[@Todo, Literal["yearn-v1", "yearn-v2"]]`
+ rotkehlchen/chain/ethereum/modules/yearn/decoder.py:404:13: error[invalid-return-type] Return type does not match returned value: expected `dict[ChecksumAddress, str]`, found `dict[ChecksumAddress, Literal["yearn-v1", "yearn-v2"]]`
- rotkehlchen/chain/evm/decoding/aave/v2/decoder.py:136:16: error[invalid-return-type] Return type does not match returned value: expected `dict[@Todo, str]`, found `dict[@Todo, Literal["aave-v2"]]`
- rotkehlchen/chain/evm/decoding/curve/lend/utils.py:107:13: error[invalid-argument-type] Argument to function `globaldb_set_unique_cache_value` is incorrect: Expected `Iterable[str | Literal[CacheType.CURVE_POOL_ADDRESS, CacheType.MAKERDAO_VAULT_ILK, CacheType.CURVE_GAUGE_ADDRESS, CacheType.YEARN_VAULTS, CacheType.ENS_NAMEHASH, ... omitted 24 literals]]`, found `list[Unknown | CacheType]`
+ rotkehlchen/chain/evm/decoding/curve/lend/utils.py:107:13: error[invalid-argument-type] Argument to function `globaldb_set_unique_cache_value` is incorrect: Expected `Iterable[str | Literal[CacheType.CURVE_POOL_ADDRESS, CacheType.MAKERDAO_VAULT_ILK, CacheType.CURVE_GAUGE_ADDRESS, CacheType.YEARN_VAULTS, CacheType.ENS_NAMEHASH, ... omitted 24 literals]]`, found `list[Unknown | CacheType | ChecksumAddress]`
- rotkehlchen/chain/evm/decoding/curve/lend/utils.py:112:13: error[invalid-argument-type] Argument to function `globaldb_set_unique_cache_value` is incorrect: Expected `Iterable[str | Literal[CacheType.CURVE_POOL_ADDRESS, CacheType.MAKERDAO_VAULT_ILK, CacheType.CURVE_GAUGE_ADDRESS, CacheType.YEARN_VAULTS, CacheType.ENS_NAMEHASH, ... omitted 24 literals]]`, found `list[Unknown | CacheType]`
+ rotkehlchen/chain/evm/decoding/curve/lend/utils.py:112:13: error[invalid-argument-type] Argument to function `globaldb_set_unique_cache_value` is incorrect: Expected `Iterable[str | Literal[CacheType.CURVE_POOL_ADDRESS, CacheType.MAKERDAO_VAULT_ILK, CacheType.CURVE_GAUGE_ADDRESS, CacheType.YEARN_VAULTS, CacheType.ENS_NAMEHASH, ... omitted 24 literals]]`, found `list[Unknown | CacheType | ChecksumAddress]`
- rotkehlchen/chain/evm/decoding/curve/lend/utils.py:118:17: error[invalid-argument-type] Argument to function `globaldb_set_unique_cache_value` is incorrect: Expected `Iterable[str | Literal[CacheType.CURVE_POOL_ADDRESS, CacheType.MAKERDAO_VAULT_ILK, CacheType.CURVE_GAUGE_ADDRESS, CacheType.YEARN_VAULTS, CacheType.ENS_NAMEHASH, ... omitted 24 literals]]`, found `list[Unknown | CacheType]`
+ rotkehlchen/chain/evm/decoding/curve/lend/utils.py:118:17: error[invalid-argument-type] Argument to function `globaldb_set_unique_cache_value` is incorrect: Expected `Iterable[str | Literal[CacheType.CURVE_POOL_ADDRESS, CacheType.MAKERDAO_VAULT_ILK, CacheType.CURVE_GAUGE_ADDRESS, CacheType.YEARN_VAULTS, CacheType.ENS_NAMEHASH, ... omitted 24 literals]]`, found `list[Unknown | CacheType | ChecksumAddress]`
- rotkehlchen/chain/evm/decoding/gearbox/decoder.py:299:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/evm/decoding/pendle/decoder.py:512:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/evm/decoding/pendle/decoder.py:516:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/evm/decoding/stakedao/decoder.py:91:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/evm/decoding/spark/savings/decoder.py:225:13: error[unsupported-operator] Operator `|=` is unsupported between objects of type `dict[Unknown, tuple[bound method Self@addresses_to_decoders._decode_spark_tokens_deposit_withdrawal(context: DecoderContext) -> EvmDecodingOutput]]` and `dict[Unknown | ChecksumAddress, Unknown | tuple[bound method Self@addresses_to_decoders._decode_psm_deposit_withdrawal(context: DecoderContext) -> EvmDecodingOutput]]`
- rotkehlchen/chain/evm/decoding/thegraph/balances.py:139:43: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[@Todo, @Todo, @Todo, int]`, found `tuple[@Todo, Unknown]`
+ rotkehlchen/chain/evm/decoding/thegraph/balances.py:139:43: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[ChecksumAddress, ChecksumAddress, ChecksumAddress, int]`, found `tuple[@Todo, Unknown]`
- rotkehlchen/chain/evm/decoding/zerox/decoder.py:146:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/evm/decoding/zerox/decoder.py:176:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/evm/decoding/zerox/decoder.py:250:97: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/evm/names.py:63:94: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/evm/names.py:67:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/evm/transactions.py:217:100: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/evm/transactions.py:339:91: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/zksync_lite/manager.py:646:39: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `tuple[int, HistoryEventType, HistoryEventSubType, Asset, FVal, ChecksumAddress, ChecksumAddress | None, str]`, found `tuple[Literal[0], Literal[HistoryEventType.WITHDRAWAL], Literal[HistoryEventSubType.BRIDGE], Asset, FVal, ChecksumAddress | None, None, str]`
- rotkehlchen/chain/zksync_lite/manager.py:700:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/data_migrations/migrations/migrations_18.py:56:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/db/dbhandler.py:1413:86: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/db/dbhandler.py:1417:90: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/db/dbhandler.py:1421:90: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/db/dbhandler.py:1425:78: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/db/dbhandler.py:2576:9: error[no-matching-overload] No overload of bound method `update` matches arguments
- rotkehlchen/db/history_events.py:347:61: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/externalapis/cryptocompare.py:414:41: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `list[dict[str, Any]] | Price`
+ rotkehlchen/externalapis/cryptocompare.py:415:26: error[non-subscriptable] Cannot subscript object of type `Price` with no `__getitem__` method
- rotkehlchen/externalapis/cryptocompare.py:425:22: error[unsupported-operator] Operator `*` is unsupported between objects of type `list[dict[str, Any]] | @Todo` and `list[dict[str, Any]] | @Todo`
+ rotkehlchen/externalapis/cryptocompare.py:425:22: error[unsupported-operator] Operator `*` is unsupported between objects of type `list[dict[str, Any]] | Price` and `list[dict[str, Any]] | Price`
- rotkehlchen/externalapis/etherscan.py:332:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/externalapis/etherscan.py:337:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/externalapis/etherscan.py:441:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/externalapis/etherscan.py:523:84: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/inquirer.py:1269:20: error[invalid-return-type] Return type does not match returned value: expected `Price | None`, found `FVal`
+ rotkehlchen/tasks/manager.py:347:67: error[invalid-argument-type] Argument to bound method `get_queried_range` is incorrect: Expected `ChecksumAddress`, found `Unknown | BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`
+ rotkehlchen/tasks/manager.py:348:34: error[invalid-argument-type] Method `__getitem__` of type `bound method defaultdict[tuple[ChecksumAddress, SupportedBlockchain], int].__getitem__(key: tuple[ChecksumAddress, SupportedBlockchain], /) -> int` cannot be called with key of type `tuple[Unknown | BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress, SupportedBlockchain]` on object of type `defaultdict[tuple[ChecksumAddress, SupportedBlockchain], int]`
+ rotkehlchen/tasks/manager.py:349:51: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `ChecksumAddress`, found `Unknown | BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`
- rotkehlchen/tests/api/test_ens.py:62:94: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/tests/api/test_ens.py:66:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/tests/db/test_db.py:361:9: error[invalid-argument-type] Argument to bound method `add_queried_address_for_module` is incorrect: Expected `ChecksumAddress`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/db/test_db.py:366:13: error[invalid-argument-type] Argument to bound method `save_tokens_for_address` is incorrect: Expected `ChecksumAddress`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/db/test_db.py:372:13: error[invalid-argument-type] Argument to bound method `save_tokens_for_address` is incorrect: Expected `ChecksumAddress`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/db/test_db.py:384:13: error[no-matching-overload] No overload of bound method `set_dynamic_cache` matches arguments
+ rotkehlchen/tests/db/test_db.py:411:17: error[invalid-argument-type] Argument to bound method `remove_single_blockchain_accounts` is incorrect: Expected `list[BTCAddress] | list[ChecksumAddress] | list[SubstrateAddress] | list[SolanaAddress]`, found `list[Unknown | str]`
+ rotkehlchen/tests/db/test_db.py:431:13: error[invalid-argument-type] Argument to bound method `remove_single_blockchain_accounts` is incorrect: Expected `list[BTCAddress] | list[ChecksumAddress] | list[SubstrateAddress] | list[SolanaAddress]`, found `list[Unknown | str]`
+ rotkehlchen/tests/db/test_db.py:443:13: error[invalid-argument-type] Argument to bound method `get_tokens_for_address` is incorrect: Expected `ChecksumAddress`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/db/test_db.py:449:13: error[invalid-argument-type] Argument to bound method `get_tokens_for_address` is incorrect: Expected `ChecksumAddress`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/db/test_db.py:614:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `int`
+ rotkehlchen/tests/db/test_db.py:731:13: error[invalid-argument-type] Argument to bound method `query_timed_balances` is incorrect: Expected `Timestamp | None`, found `Literal[1451606401]`
+ rotkehlchen/tests/db/test_db.py:732:13: error[invalid-argument-type] Argument to bound method `query_timed_balances` is incorrect: Expected `Timestamp | None`, found `Literal[1485907100]`
+ rotkehlchen/tests/db/test_db.py:744:13: error[invalid-argument-type] Argument to bound method `query_timed_balances` is incorrect: Expected `Timestamp | None`, found `Literal[1451606300]`
+ rotkehlchen/tests/db/test_db.py:745:13: error[invalid-argument-type] Argument to bound method `query_timed_balances` is incorrect: Expected `Timestamp | None`, found `Literal[1485907000]`
+ rotkehlchen/tests/db/test_db.py:1152:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp | None`, found `Literal[1451606400]`
+ rotkehlchen/tests/db/test_db.py:1153:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1451616500]`
+ rotkehlchen/tests/db/test_db.py:1163:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp | None`, found `Literal[1451626500]`
+ rotkehlchen/tests/db/test_db.py:1164:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1451636500]`
+ rotkehlchen/tests/db/test_db.py:1174:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp | None`, found `Literal[1452636501]`
+ rotkehlchen/tests/db/test_db.py:1175:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1459836501]`
+ rotkehlchen/tests/db/test_db.py:1282:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1288:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1306:17: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1312:17: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1332:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1338:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1344:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676729]`
+ rotkehlchen/tests/db/test_db.py:1350:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676829]`
+ rotkehlchen/tests/db/test_db.py:1356:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590677829]`
+ rotkehlchen/tests/db/test_db.py:1362:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590677829]`
+ rotkehlchen/tests/db/test_db.py:1368:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590777829]`
+ rotkehlchen/tests/db/test_db.py:1374:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590877829]`
+ rotkehlchen/tests/db/test_db.py:1380:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590877829]`
+ rotkehlchen/tests/db/test_db.py:1386:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590877829]`
+ rotkehlchen/tests/db/test_db.py:1400:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590877829]`
+ rotkehlchen/tests/db/test_db.py:1413:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1418:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676729]`
+ rotkehlchen/tests/db/test_db.py:1423:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590677829]`
+ rotkehlchen/tests/db/test_db.py:1427:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590777829]`
+ rotkehlchen/tests/db/test_db.py:1432:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590877829]`
+ rotkehlchen/tests/db/test_db.py:1461:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1467:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1480:59: error[invalid-argument-type] Argument to bound method `query_timed_balances` is incorrect: Expected `Timestamp | None`, found `Literal[0]`
+ rotkehlchen/tests/db/test_db.py:1480:70: error[invalid-argument-type] Argument to bound method `query_timed_balances` is incorrect: Expected `Timestamp | None`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1485:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1489:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1574:59: error[invalid-argument-type] Argument is incorrect: Expected `ApiKey`, found `str`
+ rotkehlchen/tests/db/test_db.py:1599:13: error[invalid-argument-type] Argument to bound method `add_queried_address_for_module` is incorrect: Expected `ChecksumAddress`, found `Literal["0xd36029d76af6fE4A356528e4Dc66B2C18123597D"]`
- rotkehlchen/tests/db/test_db.py:1602:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["0xd36029d76af6fE4A356528e4Dc66B2C18123597D"]` with `tuple[@Todo, ...] | None`
+ rotkehlchen/tests/db/test_db.py:1602:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["0xd36029d76af6fE4A356528e4Dc66B2C18123597D"]` with `tuple[ChecksumAddress, ...] | None`
+ rotkehlchen/tests/db/test_db.py:1608:13: error[invalid-argument-type] Argument to bound method `remove_single_blockchain_accounts` is incorrect: Expected `list[BTCAddress] | list[ChecksumAddress] | list[SubstrateAddress] | list[SolanaAddress]`, found `list[Unknown | str]`
+ rotkehlchen/tests/db/test_ens.py:77:17: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `int`
+ rotkehlchen/tests/db/test_ens.py:95:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `int`
+ rotkehlchen/tests/db/test_ens.py:100:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `int`
+ rotkehlchen/tests/db/test_ens.py:176:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `int`
+ rotkehlchen/tests/db/test_evmtx.py:116:83: error[invalid-argument-type] Argument to bound method `make` is incorrect: Expected `EVMTxHash | None`, found `Literal[b"dsadsad"]`
+ rotkehlchen/tests/db/test_history_events.py:43:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `EVMTxHash`
+ rotkehlchen/tests/db/test_history_events.py:58:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `EVMTxHash`
+ rotkehlchen/tests/db/test_history_events.py:67:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `EVMTxHash`
+ rotkehlchen/tests/db/test_history_events.py:81:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `EVMTxHash`
+ rotkehlchen/tests/db/test_history_events.py:214:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Unknown | str`
+ rotkehlchen/tests/db/test_ranges.py:13:72: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/db/test_ranges.py:13:75: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[2]`
+ rotkehlchen/tests/db/test_ranges.py:15:72: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[8]`
+ rotkehlchen/tests/db/test_ranges.py:15:75: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[17]`
+ rotkehlchen/tests/db/test_ranges.py:17:72: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[19]`
+ rotkehlchen/tests/db/test_ranges.py:17:76: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[23]`
+ rotkehlchen/tests/db/test_ranges.py:19:72: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[22]`
+ rotkehlchen/tests/db/test_ranges.py:19:76: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[57]`
+ rotkehlchen/tests/db/test_ranges.py:21:72: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[26]`
+ rotkehlchen/tests/db/test_ranges.py:21:76: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[125]`
+ rotkehlchen/tests/db/test_ranges.py:24:72: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[3]`
+ rotkehlchen/tests/db/test_ranges.py:24:75: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[9]`
+ rotkehlchen/tests/db/test_ranges.py:26:72: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[9]`
+ rotkehlchen/tests/db/test_ranges.py:26:75: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[17]`
+ rotkehlchen/tests/db/test_ranges.py:28:72: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[19]`
+ rotkehlchen/tests/db/test_ranges.py:28:76: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[23]`
+ rotkehlchen/tests/db/test_ranges.py:30:72: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[120]`
+ rotkehlchen/tests/db/test_ranges.py:30:77: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[250]`
+ rotkehlchen/tests/db/test_ranges.py:32:72: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[126]`
+ rotkehlchen/tests/db/test_ranges.py:32:77: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[170]`
+ rotkehlchen/tests/db/test_ranges.py:47:77: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[12]`
+ rotkehlchen/tests/db/test_ranges.py:47:87: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[90]`
+ rotkehlchen/tests/db/test_ranges.py:51:13: error[invalid-argument-type] Argument to bound method `update_used_query_range` is incorrect: Expected `list[tuple[Timestamp, Timestamp]]`, found `list[tuple[int, int] | Unknown]`
+ rotkehlchen/tests/db/test_ranges.py:57:77: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[250]`
+ rotkehlchen/tests/db/test_ranges.py:57:87: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[500]`
+ rotkehlchen/tests/db/test_ranges.py:61:13: error[invalid-argument-type] Argument to bound method `update_used_query_range` is incorrect: Expected `list[tuple[Timestamp, Timestamp]]`, found `list[tuple[int, int] | Unknown]`
- rotkehlchen/tests/db/test_reports.py:199:31: error[call-non-callable] Object of type `list[Unknown | int]` is not callable
+ rotkehlchen/tests/db/test_reports.py:199:31: error[call-non-callable] Object of type `list[Unknown | Timestamp]` is not callable
+ rotkehlchen/tests/exchanges/test_binance.py:57:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_binance.py:57:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_binance_us.py:27:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_binance_us.py:27:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_bitcoinde.py:46:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_bitcoinde.py:46:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_bitfinex.py:131:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_bitfinex.py:131:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_bitmex.py:33:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_bitmex.py:33:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_bitstamp.py:37:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_bitstamp.py:37:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_cryptocom.py:30:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_cryptocom.py:30:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_cryptocom.py:37:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_cryptocom.py:37:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_iconomi.py:31:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_iconomi.py:31:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_independentreserve.py:24:58: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_independentreserve.py:24:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_independentreserve.py:31:58: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_independentreserve.py:31:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_kraken.py:85:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_kraken.py:85:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"YQ=="]`
+ rotkehlchen/tests/exchanges/test_kraken.py:815:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_kraken.py:815:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"YW55IGNhcm5hbCBwbGVhc3VyZS4="]`
+ rotkehlchen/tests/exchanges/test_kraken.py:1079:9: error[invalid-argument-type] Argument to function `query_api_create_and_get_report` is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/exchanges/test_kraken.py:1080:9: error[invalid-argument-type] Argument to function `query_api_create_and_get_report` is incorrect: Expected `Timestamp`, found `Literal[1640493377]`
+ rotkehlchen/tests/exchanges/test_kraken.py:1094:9: error[invalid-argument-type] Argument to function `query_api_create_and_get_report` is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/exchanges/test_kraken.py:1095:9: error[invalid-argument-type] Argument to function `query_api_create_and_get_report` is incorrect: Expected `Timestamp`, found `Literal[1640493377]`
+ rotkehlchen/tests/exchanges/test_kucoin.py:38:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_kucoin.py:39:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_okx.py:23:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_okx.py:23:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_poloniex.py:53:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_poloniex.py:53:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_woo.py:25:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_woo.py:25:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/external_apis/test_blockscout.py:107:36: error[invalid-argument-type] Argument to bound method `has_activity` is incorrect: Expected `ChecksumAddress`, found `Literal["0x3C69Bc9B9681683890ad82953Fe67d13Cd91D5EE"]`
+ rotkehlchen/tests/external_apis/test_blockscout.py:108:36: error[invalid-argument-type] Argument to bound method `has_activity` is incorrect: Expected `ChecksumAddress`, found `Literal["0x014cd0535b2Ea668150a681524392B7633c8681c"]`
+ rotkehlchen/tests/external_apis/test_blockscout.py:109:36: error[invalid-argument-type] Argument to bound method `has_activity` is incorrect: Expected `ChecksumAddress`, found `Literal["0x6c66149E65c517605e0a2e4F707550ca342f9c1B"]`
+ rotkehlchen/tests/external_apis/test_cryptocompare.py:101:9: error[invalid-argument-type] Argument to bound method `query_and_store_historical_data` is incorrect: Expected `Timestamp`, found `Literal[1287957545]`
+ rotkehlchen/tests/external_apis/test_cryptocompare.py:125:9: error[invalid-argument-type] Argument to bound method `query_and_store_historical_data` is incorrect: Expected `Timestamp`, found `Literal[1289159945]`
+ rotkehlchen/tests/external_apis/test_cryptocompare.py:174:9: error[invalid-argument-type] Argument to bound method `query_and_store_historical_data` is incorrect: Expected `Timestamp`, found `Literal[1287957545]`
+ rotkehlchen/tests/external_apis/test_defillama.py:96:9: error[invalid-argument-type] Argument to bound method `query_historical_price` is incorrect: Expected `Timestamp`, found `Literal[1597024800]`
+ rotkehlchen/tests/external_apis/test_etherscan.py:52:82: error[invalid-argument-type] Argument is incorrect: Expected `ApiKey`, found `str & ~AlwaysFalsy`
+ rotkehlchen/tests/external_apis/test_etherscan.py:123:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1439048640]`
+ rotkehlchen/tests/external_apis/test_etherscan.py:125:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x5153493bB1E1642A63A098A65dD3913daBB6AE24"]`
+ rotkehlchen/tests/external_apis/test_etherscan.py:208:13: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0xC951900c341aBbb3BAfbf7ee2029377071Dbc36A"]`
+ rotkehlchen/tests/external_apis/test_etherscan.py:209:13: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x2910543Af39abA0Cd09dBb2D50200b3E800A63D2"]`
+ rotkehlchen/tests/external_apis/test_etherscan.py:225:13: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xC951900c341aBbb3BAfbf7ee2029377071Dbc36A"]`
+ rotkehlchen/tests/external_apis/test_gnosispay.py:78:57: error[invalid-argument-type] Argument to bound method `query_remote_for_tx_and_update_events` is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/external_apis/test_gnosispay.py:78:69: error[invalid-argument-type] Argument to bound method `query_remote_for_tx_and_update_events` is incorrect: Expected `Timestamp`, found `Literal[1]`
+ rotkehlchen/tests/external_apis/test_google_calendar.py:325:13: error[invalid-argument-type] Argument is incorrect: Expected `HexColorCode | None`, found `Literal["FF0000"]`
+ rotkehlchen/tests/external_apis/test_google_calendar.py:338:13: error[invalid-argument-type] Argument is incorrect: Expected `HexColorCode | None`, found `Literal["00FF00"]`
+ rotkehlchen/tests/external_apis/test_google_calendar.py:376:13: error[invalid-argument-type] Argument is incorrect: Expected `HexColorCode | None`, found `Literal["0000FF"]`
+ rotkehlchen/tests/external_apis/test_xratescom.py:29:9: error[invalid-argument-type] Argument to function `get_historical_xratescom_exchange_rates` is incorrect: Expected `Timestamp`, found `Literal[1459585352]`
+ rotkehlchen/tests/external_apis/test_xratescom.py:39:9: error[invalid-argument-type] Argument to function `get_historical_xratescom_exchange_rates` is incorrect: Expected `Timestamp`, found `Literal[1459585352]`
+ rotkehlchen/tests/external_apis/test_xratescom.py:52:13: error[invalid-argument-type] Argument to function `get_historical_xratescom_exchange_rates` is incorrect: Expected `Timestamp`, found `Literal[512814152]`
+ rotkehlchen/tests/fixtures/exchanges/bitmex.py:28:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["XY98JYVL15Zn-iU9f7OsJeVf"]`
+ rotkehlchen/tests/fixtures/exchanges/bitmex.py:29:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"671tM6f64bt6KhteDakj2uCCNBt7HhZVEE7H5x16Oy4zb1ag"]`
+ rotkehlchen/tests/fixtures/exchanges/cryptocom.py:15:9: error[invalid-argument-type] Argument to function `create_test_cryptocom` is incorrect: Expected `ApiKey | None`, found `Literal["ddddddd"]`
+ rotkehlchen/tests/fixtures/exchanges/cryptocom.py:16:9: error[invalid-argument-type] Argument to function `create_test_cryptocom` is incorrect: Expected `ApiSecret | None`, found `Literal[b"secret"]`
- rotkehlchen/tests/integration/test_blockchain.py:40:9: error[invalid-argument-type] Argument to function `mock_etherscan_query` is incorrect: Expected `dict[@Todo, dict[str | EvmToken, Any]]`, found `dict[Unknown, Unknown | dict[Unknown | Asset, Unknown | int]]`
+ rotkehlchen/tests/integration/test_blockchain.py:40:9: error[invalid-argument-type] Argument to function `mock_etherscan_query` is incorrect: Expected `dict[ChecksumAddress, dict[str | EvmToken, Any]]`, found `dict[Unknown | ChecksumAddress, Unknown | dict[Unknown | Asset, Unknown | int]]`
+ rotkehlchen/tests/integration/test_zksynclite.py:55:9: error[invalid-argument-type] Argument is incorrect: Expected `EVMTxHash`, found `Literal[b"\xb8\rF;\xbc\xf8J\x87m\xb6\xcf\x80_\x1d\x88k`\xe7\xab\r9!4\xb3t\xe2\xea\xb3\xa1\x93/\xe1"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:59:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:60:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x957A50DA7B25Ce98B7556AfEF1d4e5F5C60fC818"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:66:9: error[invalid-argument-type] Argument is incorrect: Expected `EVMTxHash`, found `Literal[b"1*\xb01\xb3PL\xde\xc45G\x95\x17l\xcc\xadL\xaf8\xa1P\xd5\xd3\x10\xc9^\x93I\x9bY\xee\xd1"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:70:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x9531C059098e3d194fF87FebB587aB07B30B1306"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:71:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:77:9: error[invalid-argument-type] Argument is incorrect: Expected `EVMTxHash`, found `Literal[b"\xe8wB<\xc4\xf2F\x13H\x96\xbfZf\xcc\x922\xa8\xbeM\xc7\x1au\xd7\xeap>\x10]\xfd\x8e{\x82"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:81:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x9531C059098e3d194fF87FebB587aB07B30B1306"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:82:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:94:9: error[invalid-argument-type] Argument is incorrect: Expected `EVMTxHash`, found `Literal[b'\xe8"\x81\xa8\\"\xc7r\xeb5\x17p5\xd9<\xdb\x7fU\x9b\xafp\xe0,\t\x00\xf5\x08\xe7#=\x1d\x0f']`
+ rotkehlchen/tests/integration/test_zksynclite.py:98:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:99:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x4D9339dd97db55e3B9bCBE65dE39fF9c04d1C2cd"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:111:9: error[invalid-argument-type] Argument is incorrect: Expected `EVMTxHash`, found `Literal[b"\x83\x00\x1f\x1cU\x80\xd9\r4Wy\xcd\x10v/\xc7\x1cL\x90  %Q\xbcH\x031\xd7\rT|\xc7"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:115:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:122:9: error[invalid-argument-type] Argument is incorrect: Expected `EVMTxHash`, found `Literal[b"3\x1f\xccI\xdc<\nw.\x0b^E\x185\x0f=\x9a\\Uv\xb4\xe8\xdb\xc7\xc5k,Y\xca\xa29\xbb"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:126:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x9531C059098e3d194fF87FebB587aB07B30B1306"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:127:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:133:9: error[invalid-argument-type] Argument is incorrect: Expected `EVMTxHash`, found `Literal[b"\xbdr;Z_\x87\xe4\x85\xa4x\xbc}\x1f6]\xb7\x94@\xb6\xe90[\xff;\x16\xa0\xe0\xab\x83\xe5\x19p"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:137:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:138:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/unit/accounting/evm/test_transactions.py:32:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Timestamp`
+ rotkehlchen/tests/unit/accounting/test_basic.py:63:44: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1436979735]`
+ rotkehlchen/tests/unit/accounting/test_basic.py:63:56: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1495751688]`
+ rotkehlchen/tests/unit/accounting/test_exchanges.py:49:77: error[invalid-argument-type] Argument to function `accounting_create_and_process_history` is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/accounting/test_exchanges.py:49:89: error[invalid-argument-type] Argument to function `accounting_create_and_process_history` is incorrect: Expected `Timestamp`, found `Literal[1611426233]`
+ rotkehlchen/tests/unit/accounting/test_prefork_assets.py:52:44: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1436979735]`
+ rotkehlchen/tests/unit/accounting/test_prefork_assets.py:52:56: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1495751688]`
+ rotkehlchen/tests/unit/accounting/test_prefork_assets.py:105:44: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1436979735]`
+ rotkehlchen/tests/unit/accounting/test_prefork_assets.py:105:56: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1519693374]`
+ rotkehlchen/tests/unit/accounting/test_prefork_assets.py:183:44: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1436979735]`
+ rotkehlchen/tests/unit/accounting/test_prefork_assets.py:183:56: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1569693374]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:56:44: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1436979735]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:56:56: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1519693374]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:71:44: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1436979735]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:71:56: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1519693374]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:86:44: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1436979735]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:86:56: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1519693374]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:123:44: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1436979735]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:123:65: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1619693374]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:153:44: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1436979735]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:153:56: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1519693374]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:182:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp | None`, found `Literal[1484438400]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:183:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1484629704]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:192:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp | None`, found `Literal[1487116800]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:193:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1487289600]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:204:9: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1436979735]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:205:9: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1519693374]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:263:9: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1466979735]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:264:9: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1519693374]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:207:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1605789951000]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:219:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1605789951000]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:232:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1605789951000]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:245:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1605789951000]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:271:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:314:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:328:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:356:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:399:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:413:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:441:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:504:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:518:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:533:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:690:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:752:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:766:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:781:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aerodrome.py:248:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x2223F9FE624F69Da4D8256A7bCc9104FBA7F8f75"]`
+ rotkehlchen/tests/unit/decoders/test_aerodrome.py:261:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x2223F9FE624F69Da4D8256A7bCc9104FBA7F8f75"]`
+ rotkehlchen/tests/unit/decoders/test_aerodrome.py:274:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x2223F9FE624F69Da4D8256A7bCc9104FBA7F8f75"]`
+ rotkehlchen/tests/unit/decoders/test_arbitrum_one_bridge.py:271:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x6D2457a4ad276000A615295f7A80F79E48CcD318"]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:166:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1655675488]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:412:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:474:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:488:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:503:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:526:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:528:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x95c5582D781d507A084c9E5f885C77BabACf8EeA"]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:615:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:629:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:653:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:706:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:720:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:744:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:789:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:803:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:827:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:881:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:895:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_cowswap.py:112:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xC92E8bdf79f0507f65a392b0ab4667716BFE0110"]`
+ rotkehlchen/tests/unit/decoders/test_cowswap.py:359:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xC92E8bdf79f0507f65a392b0ab4667716BFE0110"]`
+ rotkehlchen/tests/unit/decoders/test_cowswap.py:1094:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xC92E8bdf79f0507f65a392b0ab4667716BFE0110"]`
+ rotkehlchen/tests/unit/decoders/test_curve.py:107:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1650276061]`
+ rotkehlchen/tests/unit/decoders/test_curve.py:243:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1650276061]`
+ rotkehlchen/tests/unit/decoders/test_curve.py:245:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x767B35b9F06F6e28e5ed05eE7C27bDf992eba5d2"]`
+ rotkehlchen/tests/unit/decoders/test_curve.py:517:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1650276061]`
+ rotkehlchen/tests/unit/decoders/test_curve.py:519:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0xa8005630caE7b7d2AFADD38FD3B3040d13cbE2BC"]`
+ rotkehlchen/tests/unit/decoders/test_curve.py:568:70: error[invalid-argument-type] Argument to bound method `add_evm_internal_transactions` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xa8005630caE7b7d2AFADD38FD3B3040d13cbE2BC"]`
+ rotkehlchen/tests/unit/decoders/test_curve.py:631:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1650276061]`
+ rotkehlchen/tests/unit/decoders/test_curve.py:633:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x2fac74A3a04B031F240923621a578724C40678af"]`
- rotkehlchen/tests/unit/decoders/test_curve_lend.py:84:13: error[invalid-argument-type] Argument to function `globaldb_set_unique_cache_value` is incorrect: Expected `Iterable[str | Literal[CacheType.CURVE_POOL_ADDRESS, CacheType.MAKERDAO_VAULT_ILK, CacheType.CURVE_GAUGE_ADDRESS, CacheType.YEARN_VAULTS, CacheType.ENS_NAMEHASH, ... omitted 24 literals]]`, found `list[Unknown | CacheType]`
+ rotkehlchen/tests/unit/decoders/test_curve_lend.py:84:13: error[invalid-argument-type] Argument to function `globaldb_set_unique_cache_value` is incorrect: Expected `Iterable[str | Literal[CacheType.CURVE_POOL_ADDRESS, CacheType.MAKERDAO_VAULT_ILK, CacheType.CURVE_GAUGE_ADDRESS, CacheType.YEARN_VAULTS, CacheType.ENS_NAMEHASH, ... omitted 24 literals]]`, found `list[Unknown | CacheType | ChecksumAddress]`
+ rotkehlchen/tests/unit/decoders/test_eigenlayer.py:240:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xaCB55C530Acdb2849e6d4f36992Cd8c9D50ED8F7"]`
+ rotkehlchen/tests/unit/decoders/test_enriched.py:35:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1646375440]`
+ rotkehlchen/tests/unit/decoders/test_enriched.py:130:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1646375440]`
+ rotkehlchen/tests/unit/decoders/test_ens.py:255:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x084b1c3C81545d370f3634392De611CaaBFf8148"]`
+ rotkehlchen/tests/unit/decoders/test_ens.py:705:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Unknown | Literal["0x34207C538E39F2600FE672bB84A90efF190ae4C7", "0x4bBa290826C253BD854121346c370a9886d1bC26"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin.py:86:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xC8CA7F1C1a391CAfE43cf7348a2E54930648a0D4"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin.py:122:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x3A5bd1E37b099aE3386D13947b6a90d97675e5e3"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin.py:135:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xde21F729137C5Af1b01d73aF1dC21eFfa2B8a0d6"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin.py:162:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x6e08E6e2D0deeb294fd53e9708f53b0fBedc06d5"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:38:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xf0C2007aD05a8d66e98be932C698c232292eC8eA"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:61:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xc191a29203a83eec8e846c26340f828C68835715"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:115:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Unknown | str`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:155:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x8e1bD5Da87C14dd8e08F7ecc2aBf9D1d558ea174"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:168:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x8e1bD5Da87C14dd8e08F7ecc2aBf9D1d558ea174"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:203:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xe575282b376E3c9886779A841A2510F1Dd8C2CE4"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:238:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x03506eD3f57892C85DB20C36846e9c808aFe9ef4"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:273:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x15fa08599EB017F89c1712d0Fe76138899FdB9db"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:306:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xEb33BB3705135e99F7975cDC931648942cB2A96f"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:329:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xebaF311F318b5426815727101fB82f0Af3525d7b"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:355:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x6017B1d17f4D7547dC4aac88fbD0AA1826e7e6CE"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:381:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x3d1f546F05834423Acc7e4CA1169ae320cee9AF0"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:419:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xa1D52F9b5339792651861329A046dD912761E9A9"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:446:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xcD9a4e7C2ad6AAae7Ac25c2139d71739d9Fa2284"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:472:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x830862F98399520f351273B12FD3C622a226bDfE"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:509:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x8e1bD5Da87C14dd8e08F7ecc2aBf9D1d558ea174"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:521:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x8e1bD5Da87C14dd8e08F7ecc2aBf9D1d558ea174"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:534:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xb9ecee9a0e273d8A1857F3B8EeA30e5dD3cb6335"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:547:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xE6D7b9Fb31B93E542f57c7B6bfa0a5a48EfC9D0f"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:585:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x698386C93513d6D0C58f296633A7A3e529bd4026"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:598:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xfcBf17200C64E860F6639aa12B525015d115F863"]`
+ rotkehlchen/tests/unit/decoders/test_kyber.py:34:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1591043988000]`
+ rotkehlchen/tests/unit/decoders/test_kyber.py:46:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1591043988000]`
+ rotkehlchen/tests/unit/decoders/test_kyber.py:58:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1591043988000]`
+ rotkehlchen/tests/unit/decoders/test_kyber.py:85:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1644182638000]`
+ rotkehlchen/tests/unit/decoders/test_kyber.py:97:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1644182638000]`
+ rotkehlchen/tests/unit/decoders/test_kyber.py:109:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1644182638000]`
+ rotkehlchen/tests/unit/decoders/test_liquity_v2.py:45:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x3Dd5BbB839f8AE9B64c73780e89Fdd1181Bf5205"]`
+ rotkehlchen/tests/unit/decoders/test_liquity_v2.py:58:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x3Dd5BbB839f8AE9B64c73780e89Fdd1181Bf5205"]`
+ rotkehlchen/tests/unit/decoders/test_liquity_v2.py:77:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x807DEf5E7d057DF05C796F4bc75C3Fe82Bd6EeE1"]`
+ rotkehlchen/tests/unit/decoders/test_liquity_v2.py:90:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x807DEf5E7d057DF05C796F4bc75C3Fe82Bd6EeE1"]`
+ rotkehlchen/tests/unit/decoders/test_liquity_v2.py:129:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xBb4A9306f99ea6813187140fd0f26C7725e83c60"]`
+ rotkehlchen/tests/unit/decoders/test_liquity_v2.py:142:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x807DEf5E7d057DF05C796F4bc75C3Fe82Bd6EeE1"]`
+ rotkehlchen/tests/unit/decoders/test_main.py:275:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_main.py:277:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0xF99973C9F33793cb83a4590daF15b36F0ab62228"]`
+ rotkehlchen/tests/unit/decoders/test_main.py:305:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_main.py:313:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xF99973C9F33793cb83a4590daF15b36F0ab62228"]`
+ rotkehlchen/tests/unit/decoders/test_main.py:344:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_main.py:387:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Timestamp`
+ rotkehlchen/tests/unit/decoders/test_main.py:401:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Timestamp`
+ rotkehlchen/tests/unit/decoders/test_main.py:439:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_main.py:471:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Timestamp`
+ rotkehlchen/tests/unit/decoders/test_main.py:486:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_main.py:523:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_main.py:555:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Timestamp`
+ rotkehlchen/tests/unit/decoders/test_main.py:570:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_main.py:601:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_main.py:604:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xAe2D4617c862309A3d75A0fFB358c7a5009c673F"]`
+ rotkehlchen/tests/unit/decoders/test_main.py:631:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Timestamp`
+ rotkehlchen/tests/unit/decoders/test_main.py:646:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:54:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1513958719]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:56:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x01349510117dC9081937794939552463F5616dfb"]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:106:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1513958719000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:118:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1513958719000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:143:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1513957014]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:145:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0xD5684Ae2a4a722B8B31168AcF6fF3477617073ea"]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:258:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1513957014000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:270:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1513957014000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:295:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1513954042]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:297:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x277E4b7F5DaB01C8E4389B930d3Bd1c9690CE1E8"]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:441:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1513958625]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:443:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0xB4be361f092D9d5edFE8606fD53260eCED3b776E"]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:611:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1513955555]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:613:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x8d44EAAe757884f4F8fb4664D07ACECee71CFd89"]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:722:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1513955635]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:724:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x8d44EAAe757884f4F8fb4664D07ACECee71CFd89"]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:812:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1513952436]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:814:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x005e157Ae9708c55dB34e3e936CD3ebEE7265Fbc"]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:913:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1513952436000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:925:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1513952436000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:950:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1514047441]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:952:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0xb0e83C2D71A991017e0116d58c5765Abc57384af"]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:1061:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1663338359]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:1063:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x153685A03c2025b6825AE164e2ff5681EE487667"]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:1160:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1565146195]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:1162:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x6D1723Af1727d857964d12f19ed92E63736c8dA2"]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:1481:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1588030530]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:1483:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x720972Dc53741a72fEE22400828122836640a74b"]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:1660:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1588030530000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:1672:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1588030530000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:1692:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1588035170]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:1694:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x720972Dc53741a72fEE22400828122836640a74b"]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:1903:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1588035170000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:1915:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1588035170000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:1928:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1588035170000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:1941:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1588035170000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:1954:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1588035170000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:1975:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1588030595]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:1977:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x720972Dc53741a72fEE22400828122836640a74b"]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:1991:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x720972Dc53741a72fEE22400828122836640a74b"]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:2132:70: error[invalid-argument-type] Argument to bound method `add_evm_internal_transactions` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x720972Dc53741a72fEE22400828122836640a74b"]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:2142:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1588030595000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:2154:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1588030595000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:2167:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1588030595000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:2196:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1579044372000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:2210:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1579044372000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:2222:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1579044372000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:2235:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1579044372000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:2247:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1579044372000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:2259:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1579044372000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:2271:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1579044372000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:2284:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1579044372000]`
+ rotkehlchen/tests/unit/decoders/test_makerdao_sai.py:2296:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1579044372000]`
+ rotkehlchen/tests/unit/decoders/test_paladin.py:48:110: error[invalid-argument-type] Argument to function `timestamp_to_date` is incorrect: Expected `Timestamp`, found `Literal[1671062400]`
+ rotkehlchen/tests/unit/decoders/test_paraswap.py:1215:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x216B4B4Ba9F3e719726886d34a177484278Bfcae"]`
+ rotkehlchen/tests/unit/decoders/test_paraswap.py:1227:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xDEF171Fe48CF0115B1d80b88dc8eAB59176FEe57"]`
+ rotkehlchen/tests/unit/decoders/test_paraswap.py:1239:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xDEF171Fe48CF0115B1d80b88dc8eAB59176FEe57"]`
+ rotkehlchen/tests/unit/decoders/test_paraswap_v6.py:1017:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x0BeBD2FcA9854F657329324aA7dc90F656395189"]`
+ rotkehlchen/tests/unit/decoders/test_safe.py:202:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xFEB4acf3df3cDEA7399794D0869ef76A6EfAff52"]`
+ rotkehlchen/tests/unit/decoders/test_safe.py:245:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xFEB4acf3df3cDEA7399794D0869ef76A6EfAff52"]`
+ rotkehlchen/tests/unit/decoders/test_safe.py:316:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xC63c477465a792537D291ADb32Ed15c0095E106B"]`
+ rotkehlchen/tests/unit/decoders/test_stakedao.py:84:117: error[invalid-argument-type] Argument to function `timestamp_to_date` is incorrect: Expected `Timestamp`, found `Literal[1684368000]`
+ rotkehlchen/tests/unit/decoders/test_stakedao.py:123:117: error[invalid-argument-type] Argument to function `timestamp_to_date` is incorrect: Expected `Timestamp`, found `Literal[1673481600]`
+ rotkehlchen/tests/unit/decoders/test_stakedao.py:165:118: error[invalid-argument-type] Argument to function `timestamp_to_date` is incorrect: Expected `Timestamp`, found `Literal[1678924800]`
+ rotkehlchen/tests/unit/decoders/test_stakedao.py:178:118: error[invalid-argument-type] Argument to function `timestamp_to_date` is incorrect: Expected `Timestamp`, found `Literal[1678924800]`
+ rotkehlchen/tests/unit/decoders/test_stakedao.py:509:115: error[invalid-argument-type] Argument to function `timestamp_to_date` is incorrect: Expected `Timestamp`, found `Literal[1678924800]`
+ rotkehlchen/tests/unit/decoders/test_uniswapv2.py:389:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D"]`
+ rotkehlchen/tests/unit/decoders/test_uniswapv2.py:505:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1672784687000]`
+ rotkehlchen/tests/unit/decoders/test_uniswapv2.py:517:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1672784687000]`
+ rotkehlchen/tests/unit/decoders/test_uniswapv2.py:530:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1672784687000]`
+ rotkehlchen/tests/unit/decoders/test_uniswapv2.py:542:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1672784687000]`
+ rotkehlchen/tests/unit/decoders/test_uniswapv3.py:169:53: error[invalid-argument-type] Argument to function `ethaddress_to_identifier` is incorrect: Expected `ChecksumAddress`, found `Literal["0xd9Fcd98c322942075A5C3860693e9f4f03AAE07b"]`
+ rotkehlchen/tests/unit/decoders/test_uniswapv3.py:689:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x3fC91A3afd70395Cd496C647d5a6CC9D4B2b7FAD"]`
+ rotkehlchen/tests/unit/decoders/test_uniswapv3.py:701:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x3fC91A3afd70395Cd496C647d5a6CC9D4B2b7FAD"]`
+ rotkehlchen/tests/unit/decoders/test_zerox.py:1282:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x0d0E364aa7852291883C162B22D6D81f6355428F"]`
+ rotkehlchen/tests/unit/decoders/test_zerox.py:1294:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x0d0E364aa7852291883C162B22D6D81f6355428F"]`
+ rotkehlchen/tests/unit/decoders/test_zerox.py:1330:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x000000000022D473030F116dDEE9F6B43aC78BA3"]`
+ rotkehlchen/tests/unit/decoders/test_zerox.py:1342:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x0d0E364aa7852291883C162B22D6D81f6355428F"]`
+ rotkehlchen/tests/unit/decoders/test_zerox.py:1354:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x0d0E364aa7852291883C162B22D6D81f6355428F"]`
+ rotkehlchen/tests/unit/decoders/test_zerox.py:1390:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x0d0E364aa7852291883C162B22D6D81f6355428F"]`
+ rotkehlchen/tests/unit/decoders/test_zerox.py:1402:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x0d0E364aa7852291883C162B22D6D81f6355428F"]`
+ rotkehlchen/tests/unit/decoders/test_zerox.py:1435:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xB254ee265261675528bdDb0796741c0C65a4C158"]`
+ rotkehlchen/tests/unit/decoders/test_zerox.py:1447:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xB254ee265261675528bdDb0796741c0C65a4C158"]`
+ rotkehlchen/tests/unit/decoders/test_zerox.py:1480:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x5C9bdC801a600c006c388FC032dCb27355154cC9"]`
+ rotkehlchen/tests/unit/decoders/test_zerox.py:1492:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x5C9bdC801a600c006c388FC032dCb27355154cC9"]`
+ rotkehlchen/tests/unit/decoders/test_zerox.py:1525:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x4C6F446dD88fD1be8B80D2940806002777dc12a2"]`
+ rotkehlchen/tests/unit/decoders/test_zerox.py:1537:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x4C6F446dD88fD1be8B80D2940806002777dc12a2"]`
+ rotkehlchen/tests/unit/decoders/test_zerox.py:1570:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x402867B638339ad8Bec6e5373cfa95Da0b462c85"]`
+ rotkehlchen/tests/unit/decoders/test_zerox.py:1582:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x402867B638339ad8Bec6e5373cfa95Da0b462c85"]`
+ rotkehlchen/tests/unit/decoders/test_zerox.py:1615:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x7f20a7A526D1BAB092e3Be0733D96287E93cEf59"]`
+ rotkehlchen/tests/unit/decoders/test_zerox.py:1627:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x7f20a7A526D1BAB092e3Be0733D96287E93cEf59"]`
+ rotkehlchen/tests/unit/globaldb/test_globaldb.py:246:9: error[invalid-argument-type] Argument to bound method `initialize` is incorrect: Expected `Timestamp | None`, found `Literal[0]`
+ rotkehlchen/tests/unit/globaldb/test_globaldb.py:258:9: error[invalid-argument-type] Argument to bound method `initialize` is incorrect: Expected `Timestamp | None`, found `Literal[0]`
+ rotkehlchen/tests/unit/globaldb/test_globaldb.py:329:9: error[invalid-argument-type] Argument to bound method `initialize` is incorrect: Expected `Timestamp | None`, found `Literal[1585090944]`
+ rotkehlchen/tests/unit/globaldb/test_globaldb.py:345:9: error[invalid-argument-type] Argument to bound method `initialize` is incorrect: Expected `ChecksumAddress`, found `Literal["0xDBf31dF14B66535aF65AaC99C32e9eA844e14501"]`
+ rotkehlchen/tests/unit/globaldb/test_globaldb.py:355:9: error[invalid-argument-type] Argument to bound method `initialize` is incorrect: Expected `Timestamp | None`, found `Literal[1605069649]`
+ rotkehlchen/tests/unit/globaldb/test_globaldb.py:358:9: error[invalid-argument-type] Argument to bound method `initialize` is incorrect: Expected `ChecksumAddress`, found `Literal["0xfCe146bF3146100cfe5dB4129cf6C82b0eF4Ad8c"]`
+ rotkehlchen/tests/unit/globaldb/test_globaldb.py:566:9: error[invalid-argument-type] Argument to bound method `initialize` is incorrect: Expected `Timestamp | None`, found `Literal[0]`
+ rotkehlchen/tests/unit/globaldb/test_globaldb.py:580:9: error[invalid-argument-type] Argument to bound method `initialize` is incorrect: Expected `Timestamp | None`, found `Literal[0]`
+ rotkehlchen/tests/unit/globaldb/test_globaldb.py:588:9: error[invalid-argument-type] Argument to bound method `initialize` is incorrect: Expected `Timestamp | None`, found `Literal[0]`
+ rotkehlchen/tests/unit/globaldb/test_globaldb.py:688:9: error[invalid-argument-type] Argument to bound method `initialize` is incorrect: Expected `Timestamp | None`, found `Literal[0]`
+ rotkehlchen/tests/unit/globaldb/test_globaldb.py:702:9: error[invalid-argument-type] Argument to bound method `initialize` is incorrect: Expected `Timestamp | None`, found `Literal[0]`
+ rotkehlchen/tests/unit/globaldb/test_globaldb.py:706:9: error[invalid-argument-type] Argument to bound method `initialize` is incorrect: Expected `ChecksumAddress`, found `Literal["0x111111111117dC0aa78b770fA6A738034120C302"]`
+ rotkehlchen/tests/unit/globaldb/test_globaldb.py:721:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1337]`
+ rotkehlchen/tests/unit/globaldb/test_globaldb.py:722:9: error[invalid-argument-type] Argument is incorrect: Expected `Price`, found `FVal`
+ rotkehlchen/tests/unit/globaldb/test_globaldb.py:1002:16: error[unsupported-operator] Operator `>=` is not supported for types `Timestamp` and `None`, in comparing `Timestamp` with `Timestamp | None`
+ rotkehlchen/tests/unit/globaldb/test_globaldb.py:1002:31: error[unsupported-operator] Operator `>=` is not supported for types `None` and `Timestamp`, in comparing `Timestamp | None` with `Timestamp`
+ rotkehlchen/tests/unit/globaldb/test_globaldb_cache.py:392:9: error[invalid-argument-type] Argument to function `get_evm_token` is incorrect: Expected `ChecksumAddress`, found `Literal["0x3Ed3B47Dd13EC9a98b44e6204A523E766B225811"]`
+ rotkehlchen/tests/unit/globaldb/test_globaldb_cache.py:453:26: error[invalid-argument-type] Argument to bound method `get` is incorrect: Expected `ChecksumAddress`, found `Unknown | str`
+ rotkehlchen/tests/unit/globaldb/test_globaldb_cache.py:458:9: error[invalid-argument-type] Argument to function `get_evm_token` is incorrect: Expected `ChecksumAddress`, found `Literal["0x3Ed3B47Dd13EC9a98b44e6204A523E766B225811"]`
+ rotkehlchen/tests/unit/test_assets.py:1009:9: error[invalid-argument-type] Argument to function `get_or_create_evm_token` is incorrect: Expected `ChecksumAddress`, found `Literal["0x6B175474E89094C44Da98b954EedeAC495271d0F"]`
+ rotkehlchen/tests/unit/test_assets.py:1016:9: error[invalid-argument-type] Argument to function `get_or_create_evm_token` is incorrect: Expected `ChecksumAddress`, found `Literal["0xA379B8204A49A72FF9703e18eE61402FAfCCdD60"]`
+ rotkehlchen/tests/unit/test_assets.py:1026:9: error[invalid-argument-type] Argument to function `get_or_create_evm_token` is incorrect: Expected `ChecksumAddress`, found `Literal["0xB179B8204A49672FF9703e18eE61402FAfCCdD60"]`
+ rotkehlchen/tests/unit/test_assets.py:1036:9: error[invalid-argument-type] Argument to function `get_or_create_evm_token` is incorrect: Expected `ChecksumAddress`, found `Literal["0xdAC17F958D2ee523a2206206994597C13D831ec7"]`
+ rotkehlchen/tests/unit/test_bitcoin_cash.py:58:48: error[invalid-argument-type] Argument to function `force_addresses_to_legacy_addresses` is incorrect: Expected `set[ChecksumAddress]`, found `set[Unknown | str]`
+ rotkehlchen/tests/unit/test_bitcoin_cash.py:82:13: error[invalid-argument-type] Argument to function `validate_bch_address_input` is incorrect: Expected `set[ChecksumAddress]`, found `set[Unknown | str]`
+ rotkehlchen/tests/unit/test_ethereum_inquirer.py:91:17: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1]`
+ rotkehlchen/tests/unit/test_ethereum_inquirer.py:120:17: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x5bEaBAEBB3146685Dd74176f68a0721F91297D37"]`
+ rotkehlchen/tests/unit/test_ethereum_inquirer.py:128:21: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x73282A63F0e3D7e9604575420F777361ecA3C86A"]`
+ rotkehlchen/tests/unit/test_ethereum_inquirer.py:148:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1633128954]`
+ rotkehlchen/tests/unit/test_ethereum_inquirer.py:150:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x2F6789A208A05C762cA8d142A3df95d29C18b065"]`
+ rotkehlchen/tests/unit/test_ethereum_inquirer.py:151:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x7Be8076f4EA4A4AD08075C2508e481d6C946D12b"]`
+ rotkehlchen/tests/unit/test_etherscan.py:6:34: error[invalid-argument-type] Argument to function `_hashes_tuple_to_list` is incorrect: Expected `set[tuple[EVMTxHash, Timestamp]]`, found `set[Unknown | tuple[str, int]]`
+ rotkehlchen/tests/unit/test_evm_tx_decoding.py:134:38: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/unit/test_evm_tx_decoding.py:152:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1569924574000]`
+ rotkehlchen/tests/unit/test_evm_tx_decoding.py:167:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1569924574000]`
+ rotkehlchen/tests/unit/test_pairs.py:8:34: error[invalid-argument-type] Argument to function `get_pair_position_str` is incorrect: Expected `TradePair`, found `Literal["ETH_BTC"]`
+ rotkehlchen/tests/unit/test_pairs.py:9:34: error[invalid-argument-type] Argument to function `get_pair_position_str` is incorrect: Expected `TradePair`, found `Literal["ETH_BTC"]`
+ rotkehlchen/tests/unit/test_pairs.py:12:31: error[invalid-argument-type] Argument to function `get_pair_position_str` is incorrect: Expected `TradePair`, found `Literal["ETH_BTC"]`
+ rotkehlchen/tests/unit/test_pairs.py:14:31: error[invalid-argument-type] Argument to function `get_pair_position_str` is incorrect: Expected `TradePair`, found `Literal["ETH_BTC"]`
+ rotkehlchen/tests/unit/test_pairs.py:17:31: error[invalid-argument-type] Argument to function `get_pair_position_str` is incorrect: Expected `TradePair`, found `Literal["_"]`
+ rotkehlchen/tests/unit/test_pairs.py:19:31: error[invalid-argument-type] Argument to function `get_pair_position_str` is incorrect: Expected `TradePair`, found `Literal["ETH_"]`
+ rotkehlchen/tests/unit/test_pairs.py:21:31: error[invalid-argument-type] Argument to function `get_pair_position_str` is incorrect: Expected `TradePair`, found `Literal["_BTC"]`
+ rotkehlchen/tests/unit/test_pairs.py:23:31: error[invalid-argument-type] Argument to function `get_pair_position_str` is incorrect: Expected `TradePair`, found `Literal["ETH_BTC_USD"]`
+ rotkehlchen/tests/unit/test_pairs.py:26:34: error[invalid-argument-type] Argument to function `get_pair_position_str` is incorrect: Expected `TradePair`, found `Literal["ETH_FDFSFDSFDSF"]`
+ rotkehlchen/tests/unit/test_pairs.py:27:34: error[invalid-argument-type] Argument to function `get_pair_position_str` is incorrect: Expected `TradePair`, found `Literal["FDFSFDSFDSF_BTC"]`
+ rotkehlchen/tests/unit/test_pairs.py:29:34: error[invalid-argument-type] Argument to function `get_pair_position_str` is incorrect: Expected `TradePair`, found `Literal["ETH_RDN"]`
+ rotkehlchen/tests/unit/test_pairs.py:30:34: error[invalid-argument-type] Argument to function `get_pair_position_str` is incorrect: Expected `TradePair`, found `Literal["ETH_RDN"]`
+ rotkehlchen/tests/unit/test_price_historian.py:182:121: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Timestamp`, found `Literal[1614556800]`
+ rotkehlchen/tests/unit/test_structures.py:206:38: error[invalid-argument-type] Argument to function `ethaddress_to_identifier` is incorrect: Expected `ChecksumAddress`, found `Literal["0x6B175474E89094C44Da98b954EedeAC495271d0F"]`
+ rotkehlchen/tests/unit/test_structures.py:229:38: error[invalid-argument-type] Argument to function `ethaddress_to_identifier` is incorrect: Expected `ChecksumAddress`, found `Literal["0x6B175474E89094C44Da98b954EedeAC495271d0F"]`
+ rotkehlchen/tests/unit/test_substrate_manager.py:115:13: error[invalid-argument-type] Argument is incorrect: Expected `BlockNumber`, found `Literal[1000]`
+ rotkehlchen/tests/unit/test_substrate_manager.py:122:13: error[invalid-argument-type] Argument is incorrect: Expected `BlockNumber`, found `Literal[750]`
+ rotkehlchen/tests/unit/test_substrate_manager.py:129:13: error[invalid-argument-type] Argument is incorrect: Expected `BlockNumber`, found `Literal[1000]`
+ rotkehlchen/tests/unit/test_tokens.py:202:9: error[invalid-argument-type] Argument to function `generate_multicall_chunks` is incorrect: Expected `Mapping[ChecksumAddress, Sequence[Unknown]]`, found `dict[Unknown | str, Unknown | list[Unknown | str]]`
+ rotkehlchen/tests/unit/test_utils.py:356:30: error[invalid-argument-type] Argument to function `timestamp_to_date` is incorrect: Expected `Timestamp`, found `Literal[1611395717]`
- rotkehlchen/tests/utils/blockchain.py:636:12: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `list[@Todo] | None`
+ rotkehlchen/tests/utils/blockchain.py:636:12: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `list[ChecksumAddress] | None`
- rotkehlchen/tests/utils/blockchain.py:641:12: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `list[@Todo] | None`
+ rotkehlchen/tests/utils/blockchain.py:641:12: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `list[ChecksumAddress] | None`
+ rotkehlchen/tests/utils/makerdao.py:123:52: error[invalid-argument-type] Argument to bound method `contract` is incorrect: Expected `ChecksumAddress`, found `Literal["DS_PROXY_REGISTRY"]`
+ rotkehlchen/tests/utils/xpubs.py:18:52: error[invalid-argument-type] Argument to bound method `add_tag` is incorrect: Expected `HexColorCode`, found `Literal["ffffff"]`
+ rotkehlchen/tests/utils/xpubs.py:18:62: error[invalid-argument-type] Argument to bound method `add_tag` is incorrect: Expected `HexColorCode`, found `Literal["000000"]`
+ rotkehlchen/tests/utils/xpubs.py:19:53: error[invalid-argument-type] Argument to bound method `add_tag` is incorrect: Expected `HexColorCode`, found `Literal["ffffff"]`
+ rotkehlchen/tests/utils/xpubs.py:19:63: error[invalid-argument-type] Argument to bound method `add_tag` is incorrect: Expected `HexColorCode`, found `Literal["000000"]`
+ rotkehlchen/tests/utils/xpubs.py:58:46: error[invalid-argument-type] Argument is incorrect: Expected `BTCAddress`, found `Literal["1LZypJUwJJRdfdndwvDmtAjrVYaHko136r"]`
+ rotkehlchen/tests/utils/xpubs.py:59:46: error[invalid-argument-type] Argument is incorrect: Expected `BTCAddress`, found `Literal["1MKSdDCtBSXiE49vik8xUG2pTgTGGh5pqe"]`
+ rotkehlchen/tests/utils/xpubs.py:60:46: error[invalid-argument-type] Argument is incorrect: Expected `BTCAddress`, found `Literal["16zNpyv8KxChtjXnE5oYcPqcXcrSQXX2JJ"]`
+ rotkehlchen/tests/utils/xpubs.py:88:46: error[invalid-argument-type] Argument is incorrect: Expected `BTCAddress`, found `Literal["bc1qc3qcxs025ka9l6qn0q5cyvmnpwrqw2z49qwrx5"]`
+ rotkehlchen/tests/utils/xpubs.py:89:46: error[invalid-argument-type] Argument is incorrect: Expected `BTCAddress`, found `Literal["bc1qnus7355ecckmeyrmvv56mlm42lxvwa4wuq5aev"]`
+ rotkehlchen/tests/utils/xpubs.py:90:46: error[invalid-argument-type] Argument is incorrect: Expected `BTCAddress`, found `Literal["bc1qup7f8g5k3h5uqzfjed03ztgn8hhe542w69wc0g"]`
+ rotkehlchen/tests/utils/xpubs.py:91:46: error[invalid-argument-type] Argument is incorrect: Expected `BTCAddress`, found `Literal["bc1qr4r8vryfzexvhjrx5fh5uj0s2ead8awpqspqra"]`
+ rotkehlchen/tests/utils/xpubs.py:92:46: error[invalid-argument-type] Argument is incorrect: Expected `BTCAddress`, found `Literal["bc1qr5r8vryfzexvhjrx5fh5uj0s2ead8awpqspalz"]`
- Found 1672 diagnostics
+ Found 2102 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-01 00:01_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-01 00:02_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-01 01:49_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-01 01:49_

---

_Comment by @oconnor663 on 2025-11-01 02:10_

Relevant to the error formatting tweak I just pushed in daeb800c4c5f3972cc6899dda7f0d210c647eecb, do we like the example in that commit message:

```py
from typing import NewType
from nonexistent_module import Foo
Bar = NewType("Bar", Foo)
```

```
error[invalid-newtype]: invalid base for `typing.NewType`
 --> test.py:3:22
  |
1 | from typing import NewType
2 | from nonexistent_module import Foo
3 | Bar = NewType("Bar", Foo)
  |                      ^^^ type `Unknown`
info: The base of a `NewType` must be a class type or another `NewType`.
info: rule `invalid-newtype` is enabled by default
```

That's a result of the check that says a `NewType` base should be either a class type or another newtype. `Unknown` is neither of those things. Is this error what we expect? Will this sort of thing by noisy around unresolved imports? (I haven't played much with how those work yet.)

---

_Comment by @AlexWaygood on 2025-11-02 01:47_

> Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)

Nice, these changes all look like true positives -- places where we should have been emitting diagnostics, but we previously weren't!

---

_Comment by @AlexWaygood on 2025-11-02 01:49_

> That's a result of the check that says a `NewType` base should be either a class type or another newtype. `Unknown` is neither of those things. Is this error what we expect? Will this sort of thing by noisy around unresolved imports? (I haven't played much with how those work yet.)

I think it would be good to suppress the diagnostic here if the second argument has a dynamic type. The user will get a diagnostic from the unresolved import in your example; there's not much use in having cascading diagnostics later on in the module due to the same issue.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1495 on 2025-11-02 02:04_

this isn't quite right. The question this branch is asking is "What is the `Callable` supertype for an instance of a given newtype?". I.e., given this snippet:

```py
N = NewType("N", int)
i = N(42)
```

The type of `i` is `N`, but what is the `Callable` supertype of `i`? The correct answer should be that it has no `Callable` supertype (the object `i` is not callable), but on your branch currently we say it is in some situations:

```py
from typing import NewType, Callable, Any
from ty_extensions import CallableTypeOf

N = NewType("N", int)
i = N(42)

# no diagnostic here, but there should be one!
y: Callable[..., Any] = i

def f(x: CallableTypeOf[i]):
    # revealed: Overload[(x: @Todo(Support for `typing.TypeAlias`) = Literal[0], /) -> int, (x: str | bytes | bytearray, /, base: SupportsIndex) -> int]
    reveal_type(x)
```

The fix for the bug here is simple:

```suggestion
            Type::NewTypeInstance(newtype) => {
                Type::instance(db, newtype.base_class_type(db)).try_upcast_to_callable(db)
            }
```

you were answering the question by delegating to the `Callable` supertype of the newtype's underlying class, but what you need to do is to delegate to the `Callable` supertype of an _instance_ of the newtype's underlying class

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2505 on 2025-11-02 02:09_

nit: can you adjust the comment a few lines above?

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 3ce82eebc9..0370e86e60 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -2433,8 +2433,8 @@ impl<'db> Type<'db> {
                 disjointness_visitor,
             ),
 
-            // Other than the special cases enumerated above, `Instance` types and typevars are
-            // never subtypes of any other variants
+            // Other than the special cases enumerated above, nominal-instance types,
+            // newtype-instance types and typevars are never subtypes of any other variants
             (Type::TypeVar(bound_typevar), _) => {
                 // All inferable cases should have been handled above
                 assert!(!bound_typevar.is_inferable(db, inferable));
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3501 on 2025-11-02 02:13_

nit: could you add a test for this? E.g. a newtype of `types.EllipsisType` should be recognized as a singleton type, since we recognize `types.EllipsisType` as being a singleton type. You can add a test using our `ty_extensions.is_singleton` function

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3554 on 2025-11-02 02:13_

same here -- this looks correct, but it would be great to have an explicit test for this

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5292 on 2025-11-02 02:18_

one issue here is that on Python 3.9, `NewType` is a function rather than a class. I don't know if this really matters this much TBH, since Python 3.9 is now end-of-life, and supporting both the pre-3.9 version of `NewType` (where it was a function) and the 3.10-version of `NewType` (where it's a class) is going to lead to complications for us.

At the very least, it might be good to add an mdtest with `python-version = "3.9"` so that we document that we don't support `NewType` on Python <=3.9 currently. But I would certainly be inclined to leave <=3.9 support out of the first PR adding support for `NewType`.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5804 on 2025-11-02 02:19_

it would be great to add a test for this -- e.g.

```py
from typing import NewType

N = NewType("N", tuple[int, str])

a, b = N((1, "foo"))

reveal_type(a)  # revealed: int
reveal_type(b)  # revealed: str
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7173 on 2025-11-02 02:23_

if we do the proper validation elsewhere so that you're unable to construct a `NewType` from an unspecialized generic class, I think we can optimize this a bit and just skip it?

```suggestion
            Type::NewTypeInstance(newtype) => {
                // A newtype can never be constructed from an unspecialized generic class,
                // so it is impossible that we could ever find any legacy typevars
                // in a newtype instance or its underlying class
            }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7744 on 2025-11-02 02:24_

nit: when you call it, it has an identity-function signature, but it is not actually an instance of `types.FunctionType` at runtime (it's an instance of `typing.NewType`)

```suggestion
    /// An identity callable created with `typing.NewType(name, base)`, which behaves like a
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/bound_super.rs`:409 on 2025-11-02 02:26_

nit: could you add a test for this to https://github.com/astral-sh/ruff/blob/bff32a41dc440b30764e9414767794e01d25c265/crates/ty_python_semantic/resources/mdtest/class/super.md?plain=1#L63-L121 ?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class_base.rs`:165 on 2025-11-02 02:32_

I'd feel slightly safer here if we delegated:

```suggestion
            | Type::TypedDict(_) => None,
            
            Type::NewTypeInstance(newtype) => {
                ClassBase::try_from_type(db, Type::instance(db, newtype.base_class_type(db)), subclass)
            }
```

It's unlikely that we'll ever accept any `NominalInstance` types as base classes, but there _is_ a TODO above the `NominalInstance` branch above which could _plausibly_ be fixed, and it seems important to keep the behaviour for a given `NewType` here consistent with the behaviour of that `NewType`'s nominal-instance supertype

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:1107 on 2025-11-02 02:37_

It would be great to add a test for this -- something like this?

```py
from typing import NewType

N = NewType("N", int)

def f(x: N):
    # TODO: we should emit an error here (requires PEP-613 support)
    reveal_type(isinstance(x, N))  # revealed: bool

    reveal_type(isinstance(x, int))  # revealed: Literal[True]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:4340 on 2025-11-02 02:39_

I think you could do this -- and a similar bit below -- slightly more efficiently, e.g.

```diff
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index b6a9df7d11..95f7369c67 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -4341,24 +4341,20 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
                         TypeContext::default(),
                     );
 
-                    let typevar_class = callable_type
+                    let known_class = callable_type
                         .as_class_literal()
-                        .and_then(|cls| cls.known(self.db()))
-                        .filter(|cls| {
-                            matches!(cls, KnownClass::TypeVar | KnownClass::ExtensionsTypeVar)
-                        });
-
-                    let is_newtype = callable_type
-                        .as_class_literal()
-                        .and_then(|cls| cls.known(self.db()))
-                        .is_some_and(|cls| matches!(cls, KnownClass::NewType));
+                        .and_then(|cls| cls.known(self.db()));
 
-                    let ty = if let Some(typevar_class) = typevar_class {
-                        self.infer_legacy_typevar(target, call_expr, definition, typevar_class)
-                    } else if is_newtype {
-                        self.infer_newtype_expression(target, call_expr, definition)
-                    } else {
-                        self.infer_call_expression_impl(call_expr, callable_type, tcx)
+                    let ty = match known_class {
+                        Some(
+                            typevar_class @ (KnownClass::TypeVar | KnownClass::ExtensionsTypeVar),
+                        ) => {
+                            self.infer_legacy_typevar(target, call_expr, definition, typevar_class)
+                        }
+                        Some(KnownClass::NewType) => {
+                            self.infer_newtype_expression(target, call_expr, definition)
+                        }
+                        _ => self.infer_call_expression_impl(call_expr, callable_type, tcx),
                     };
 
                     self.store_expression_type(value, ty);
@@ -4743,21 +4739,16 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
         let callable_type =
             self.infer_maybe_standalone_expression(func.as_ref(), TypeContext::default());
 
-        let is_typevar = callable_type
-            .as_class_literal()
-            .and_then(|cls| cls.known(self.db()))
-            .filter(|cls| matches!(cls, KnownClass::TypeVar | KnownClass::ExtensionsTypeVar))
-            .is_some();
-
-        let is_newtype = callable_type
+        let known_class = callable_type
             .as_class_literal()
-            .and_then(|cls| cls.known(self.db()))
-            .is_some_and(|cls| matches!(cls, KnownClass::NewType));
+            .and_then(|cls| cls.known(self.db()));
 
-        if is_typevar {
-            self.infer_typevar_assignment_deferred(arguments);
-        } else if is_newtype {
-            self.infer_newtype_assignment_deferred(arguments);
+        match known_class {
+            Some(KnownClass::TypeVar | KnownClass::ExtensionsTypeVar) => {
+                self.infer_typevar_assignment_deferred(arguments);
+            }
+            Some(KnownClass::NewType) => self.infer_newtype_assignment_deferred(arguments),
+            _ => {}
         }
     }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/narrow.rs`:256 on 2025-11-02 02:48_

could we add an explicit test for this? e.g.

```py
from typing import NewType

N = NewType("N", int)

def f(x: N | str):
    # TODO: we should emit an error here (requires PEP-613 support)
    if isinstance(x, N):
        # we do not narrow the type here,
        # since the `isinstance()` call will actually fail at runtime!
        reveal_type(x)  # revealed: N | str
    else:
        reveal_type(x)  # revealed: N | str
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class_base.rs`:182 on 2025-11-02 02:50_

the runtime actually has a special-cased error message specifically for this, which we could consider emulating:

```pycon
>>> from typing import NewType
>>> X = NewType("X", int)
>>> class Foo(X): ...
... 
Traceback (most recent call last):
  File "<python-input-2>", line 1, in <module>
    class Foo(X): ...
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 3428, in __init_subclass__
    raise TypeError(
    ...<2 lines>...
    )
TypeError: Cannot subclass an instance of NewType. Perhaps you were looking for: `Foo = NewType('Foo', X)`
```

But that obviously doesn't need to be done in this PR!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/newtype.rs`:11 on 2025-11-02 02:50_

```suggestion
/// identity-callable-that-acts-like-a-subtype-in-type-expressions returned by the call to
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/newtype.rs`:12 on 2025-11-02 02:50_

```suggestion
/// `typing.NewType()`, or from the perspective of instances of that subtype returned by the
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/newtype.rs`:30 on 2025-11-02 02:52_

nit: could you add a note similar to this to the doc-comment for the struct?

https://github.com/astral-sh/ruff/blob/bff32a41dc440b30764e9414767794e01d25c265/crates/ty_python_semantic/src/types.rs#L481-L485

---

_@AlexWaygood reviewed on 2025-11-02 02:58_

Here's my full review. Overall this looks excellent!! This definitely looks like right overall approach IMO.

My main feedback is that it would be great to have some more tests for various parts of the behaviour that you're implementing here. I left some comments inline about things that look correct, but that it would be great to explicitly test. It might also be worth looking through the typing conformance-suite tests for newtype, and/or [mypy's tests for newtype](https://github.com/python/mypy/blob/master/test-data/unit/check-newtype.test), to see if there are any other things that would be worth testing for explicitly.

---

_@oconnor663 reviewed on 2025-11-05 20:06_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types.rs`:5292 on 2025-11-05 20:06_

What's your recommendation for this diagnostic? It doesn't look like I have an `InferContext` at this point to directly emit something. Is there a type that I can return that indirectly forces a diagnostic? (edit: ah you're not asking for a diagnostic)

---

_@oconnor663 reviewed on 2025-11-05 20:17_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types.rs`:7173 on 2025-11-05 20:17_

What's the story with unspecified type vars? Do they get replaced with `object` at some point, such that the call to `.base_class_type(db).find_legacy_typevars_impl(...)` is definitely not going to find anything?

---

_@AlexWaygood reviewed on 2025-11-05 20:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7173 on 2025-11-05 20:19_

yes, they _should_ always be substituted with `Unknown` if they're not specified: we see here that `list[Unknown]` appears in the MRO rather than `list[T]` or anything else:

https://play.ty.dev/89863b06-3878-484c-a393-bb9562c70efc

---

_@oconnor663 reviewed on 2025-11-06 19:33_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/function.rs`:1107 on 2025-11-06 19:33_

When I try this, I _do_ get an error there:

> Expected `type | UnionType | tuple[Unknown, ...]`, found `<NewType pseudo-class 'N'>`.

That seems like what we want?

---

_@AlexWaygood reviewed on 2025-11-06 20:13_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:1107 on 2025-11-06 20:13_

Aha, yes! The missing piece here was added in https://github.com/astral-sh/ruff/pull/21195. That's definitely the behaviour we want!

---

_@AlexWaygood reviewed on 2025-11-06 20:14_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:1107 on 2025-11-06 20:14_

(It would still be good to add a test!)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:792 on 2025-11-06 21:17_

```suggestion
    /// identity callables it returns (which behave like subtypes in type expressions) are of
    /// `Type::KnownInstance` with `KnownInstanceType::NewType`. This `Type` refers to the objects
    /// wrapped/returned by a specific one of those identity callables, or by another that inherits
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class_base.rs`:179 on 2025-11-06 21:17_

```suggestion
                // wrappers are just identity callables at runtime, so this sort of inheritance
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:138 on 2025-11-06 21:20_

we could also add a test here for a `NewType` of a type that _is_ callable, e.g.

````suggestion
```py
from typing import NewType, Callable, Any
from ty_extensions import CallableTypeOf

N = NewType("N", int)
i = N(42)

y: Callable[..., Any] = i  # error: [invalid-assignment] "Object of type `N` is not assignable to `(...) -> Any`"

# error: [invalid-type-form] "Expected the first argument to `ty_extensions.CallableTypeOf` to be a callable object, but got an object of type `N`"
def f(x: CallableTypeOf[i]):
    reveal_type(x)  # revealed: Unknown

class SomethingCallable:
    def __call__(self, a: str) -> bytes:
        raise NotImplementedError

N2 = NewType("N2", SomethingCallable)
j = N2(SomethingCallable())

z: Callable[[str], bytes] = j  # fine

def g(x: CallableTypeOf[j]):
    reveal_type(x)  # revealed: (str) -> bytes
```
````

---

_@AlexWaygood reviewed on 2025-11-06 21:20_

Looking really good -- a couple more small comments!

---

_@oconnor663 reviewed on 2025-11-06 22:27_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/infer/builder.rs`:5006 on 2025-11-06 22:27_

I just pushed a big rebase (addressing most of @AlexWaygood's comments, but now I see there are more :-D ), and this part ended up being tricky. It looks like if I don't short-circuit here, I run into a panic about inferring the same expression (the second arg) twice. Please let me know if this looks like the right way to avoid that, and whether it's maybe worthy of a block comment saying why this is sensitive?

---

_@oconnor663 reviewed on 2025-11-06 22:41_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:138 on 2025-11-06 22:41_

In retrospect clear that I should have done that :sweat_smile: 

---

_Marked ready for review by @oconnor663 on 2025-11-06 22:45_

---

_Review requested from @carljm by @oconnor663 on 2025-11-06 22:45_

---

_Review requested from @sharkdp by @oconnor663 on 2025-11-06 22:45_

---

_Review requested from @dcreager by @oconnor663 on 2025-11-06 22:45_

---

_Review requested from @MichaReiser by @oconnor663 on 2025-11-06 22:45_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:34 on 2025-11-07 16:14_

we could add some more assertions here that directly check the subtype relationship:

```suggestion
from typing_extensions import NewType
from ty_extensions import static_assert, is_subtype_of, is_equivalent_to

Foo = NewType("Foo", int)
Bar = NewType("Bar", Foo)

static_assert(is_subtype_of(Foo, int))
static_assert(not is_equivalent_to(Foo, int))

static_assert(is_subtype_of(Bar, Foo))
static_assert(is_subtype_of(Bar, int))
static_assert(not is_equivalent_to(Bar, Foo))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:1 on 2025-11-07 16:16_

One pathological thing I wondered while reviewing this PR was "What happens if you have a newtype of an enum?" I think our behaviour makes more sense than mypy/pyright here, so it might be worth adding a test for it. (I think it's correct _not_ to narrow the type of `x` in the first `match` case to `Literal[Foo.X]`, for example, because `Literal[Foo.X]` is actually disjoint from `N`:

```py
from enum import Enum
from typing import NewType, reveal_type

class Foo(Enum):
    X = 0
    Y = 1

N = NewType("N", Foo)

def f(x: N):
    match x:
        case Foo.X:
            reveal_type(x)  # revealed: N
        case Foo.Y:
            reveal_type(x)  # revealed: N
        case _:
            reveal_type(x)  # revealed: N
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:97 on 2025-11-07 16:20_

again here, it could be worth adding a direct assertion for the property we're trying to test (the callable supertype of the NewType pseudo-class) as well as the existing assertions you have for the impact this has in user code:

```suggestion
from collections.abc import Callable
from typing_extensions import NewType
from ty_extensions import CallableTypeOf

Foo = NewType("Foo", int)

def _(obj: CallableTypeOf[Foo]):
    reveal_type(obj)  # revealed: (int) -> Foo
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:134 on 2025-11-07 16:21_

maybe demonstrate here that we infer the name just fine?

```suggestion
name = "Foo"
Foo = NewType(name, int)
reveal_type(Foo)  # revealed: <NewType pseudo-class 'Foo'>
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:176 on 2025-11-07 16:30_

I had to read this twice because the prose describes how we _aren't_ going to emit a diagnostic but then the example shows us emitting a diagnostic 😆

````suggestion
```py
# Here we only emit one `invalid-type-form` diagnostic,
# rather than one `invalid-type-form` diagnostic and another `invalid-base` diagnostic:
#
# error: [invalid-type-form] "Int literals are not allowed in this context in a type expression"
Foo = NewType("Foo", 42)
```
````

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:237 on 2025-11-07 16:44_

We could add some more stress tests here to show that we really do understand the recursive nature of the newtype (which is really cool!)

````suggestion
Normal classes can't inherit from newtypes, but generic classes can be parametrized with them, so we
also need to detect "ordinary" type cycles that happen to involve a newtype.

```py
E = NewType("E", list["E"])
reveal_type(E)  # revealed: <NewType pseudo-class 'E'>
e: E = E([])
reveal_type(e)  # revealed: E
reveal_type(E(E(E(E(E([]))))))  # revealed: E
reveal_type(E([E([E([]), E([E([])])]), E([])]))  # revealed: E
E(["foo"])  # error: [invalid-argument-type]
E(E(E(["foo"])))  # error: [invalid-argument-type]
````

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:253 on 2025-11-07 16:46_

and these also reveal the correct result, which is pretty cool!

```suggestion
A = NewType("A", EllipsisType)
static_assert(is_singleton(A))
static_assert(is_single_valued(A))
reveal_type(type(A(...)) is EllipsisType)  # revealed: Literal[True]
reveal_type(A(...) is ...)  # revealed: Literal[True]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:226 on 2025-11-07 16:49_

```suggestion
## `NewType`s of tuples can be iterated/unpacked
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:239 on 2025-11-07 16:50_

```suggestion
## `isinstance` of a `NewType` instance and its base class is inferred as `Literal[True]`
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:1 on 2025-11-07 16:50_

I think we're also still missing a test that demonstrates our `NewType` behaviour on Python 3.9, too (https://github.com/astral-sh/ruff/pull/21157#discussion_r2484100928)

It's fine to leave support for 3.9 `NewType` out of this PR, I think (and possibly never add it, since Python 3.9 is end-of-life). But I'd love a test that just demonstrates what our behaviour is when you use `typing.NewType` on Python 3.9

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:1 on 2025-11-07 16:54_

[One thing the typing conformance suite points out](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/aliases_newtype.py#L37-L65) is that we need to emit an error if you try to create a generic `NewType` -- e.g. we should emit an error on this snippet, but that's not currently implemented in this PR:

```py
from typing import NewType, TypeVar

T = TypeVar("T")
BadNewType2 = NewType("BadNewType2", list[T])
```

For this as well, I think it's fine to leave it out of this PR, but it would be great to add a test with a TODO comment saying that we should try to emit a diagnostic on this in the future

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:1 on 2025-11-07 17:03_

it would also be nice to add a test somewhere that demonstrates that we infer member access on the pseudo-class _itself_ correctly, e.g.

```py
from typing import NewType

N = NewType("N", int)
reveal_type(N.__supertype__)  # revealed: type | NewType
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:5028 on 2025-11-07 17:10_

this causes us to emit an error if you try to use a `Protocol` class as a `NewType` base. [According to the spec](https://typing.python.org/en/latest/spec/protocol.html#newtype-and-type-aliases), this is correct! However, we currently emit quite a confusing diagnostic for it -- the diagnostic currently implies that we haven't passed a class object, but we have (the issue is that it's a `Protocol` class object, which means that the `self.infer_type_expression()` call has returned a `Type::ProtocolInstance()` variant rather than a `Type::NominalInstance()` variant).

```sh
~/dev/ruff (jack/newtype4)⚡ % cat foo.py                                        
from typing import NewType, Protocol

class Id(Protocol):
    code: int

UserId = NewType('UserId', Id)
~/dev/ruff (jack/newtype4)⚡ % cargo run -p ty check foo.py --python-version=3.13
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.11s
     Running `target/debug/ty check foo.py --python-version=3.13`
error[invalid-newtype]: invalid base for `typing.NewType`
 --> foo.py:6:28
  |
4 |     code: int
5 |
6 | UserId = NewType('UserId', Id)
  |                            ^^ type `Id`
  |
info: The base of a `NewType` must be a class type or another `NewType`.
info: rule `invalid-newtype` is enabled by default

Found 1 diagnostic
```


It would be great if we could say something a bit clearer here, that says directly that the issue is that it's a `Protocol` class, which isn't allowed.



---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:4905 on 2025-11-07 17:20_

for type variables we explicitly reject the assignment unless it's a simple `ast::Assign` node (`ast::AnnAssign` nodes are rejected). I think we should probably do the same for `NewType` definitions:

```py
from typing import TypeVar, NewType

T: TypeVar = TypeVar("T")  # error[invalid-legacy-type-variable] "A `TypeVar` definition must be a simple variable assignment"
N: NewType = NewType("N", int)  # no error here currently


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:5006 on 2025-11-07 17:24_

this looks correct to me. I think the issue is that type variables can have an arbitrary number of positional arguments when they're constructed, but a `NewType` can only have 2. I don't think it needs a comment!

---

_@AlexWaygood approved on 2025-11-07 17:27_

Let's go!

Lots of minor comments below, but nothing blocking; feel free to defer stuff to followup PRs. This looks great!

---

_@oconnor663 reviewed on 2025-11-10 18:19_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:253 on 2025-11-10 18:19_

Hmm, I'm getting a `Revealed type: bool` failure for the last line there:

```
  crates/ty_python_semantic/resources/mdtest/annotations/new_types.md:247 unmatched assertion: revealed: Literal[True]
  crates/ty_python_semantic/resources/mdtest/annotations/new_types.md:247 unexpected error: 13 [revealed-type] "Revealed type: `bool`"
```

Did you see something different? Is that unexpected?

---

_@oconnor663 reviewed on 2025-11-10 18:23_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:253 on 2025-11-10 18:23_

Digging into this, looks like `is_equivalent_to` is returning false in this case. Probably a bug worth fixing here...

Edit: Ooo the `match` at the bottom of that has a default arm, so my compiler-error-driven-development approach never saw it. Makes me worry that there might be a lot more cases like that.

---

_@AlexWaygood reviewed on 2025-11-10 18:27_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:253 on 2025-11-10 18:27_

The two types aren't equivalent (one's a subtype of the other), so I think that's correct

---

_@AlexWaygood reviewed on 2025-11-10 18:29_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:253 on 2025-11-10 18:29_

> Hmm, I'm getting a `Revealed type: bool` failure for the last line there:

I could've sworn I tried this out on your branch the other day and got `Literal[True]` 😆 but I don't think it matters too much either way; this is very much an edge case. The main thing I'd like is a test that records what our current behaviour is here, so we can see if it changes in the future.

---

_@oconnor663 reviewed on 2025-11-10 18:47_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:253 on 2025-11-10 18:47_

Err, hmm, I was kind of confusing `is_equivalent_to` ("these types are the exact same set of values") with `is_single_valued` ("all values compare equal"). A newtype isn't equivalent to any other newtype besides itself (which I should still fix probably?). Maybe `infer_binary_type_comparison` needs to be tweaked. Thinking about this...

---

_@AlexWaygood reviewed on 2025-11-10 18:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:253 on 2025-11-10 18:57_

> A newtype isn't equivalent to any other newtype besides itself (which I should still fix probably?).

Again, I think that's arguably correct as it currently is. I wouldn't spend too much time looking at this; this all really feels very much like an extreme edge case. I'm sort-of regretting bringing it up at this point 😄

---

_@oconnor663 reviewed on 2025-11-10 19:35_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:253 on 2025-11-10 19:35_

Ok I'll make the second one a TODO.

---

_@oconnor663 reviewed on 2025-11-10 20:27_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/infer/builder.rs`:4905 on 2025-11-10 20:27_

Ah neat, as part of copy/pasting `TypeVar`'s handling of this, I noticed that there was a dead special case for `NewType` in the call-binding machinery, added in an earlier version of this PR. That special case was preventing control from reaching the new error. Removing.

---

_@AlexWaygood reviewed on 2025-11-10 20:36_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:345 on 2025-11-10 20:36_

```suggestion
We implement `typing.NewType` as a `KnownClass`, but in Python 3.9 it's actually a function, so all
we get is the `Any` annotations from typeshed. However, `typing_extensions.NewType` is always a
class.

This could be improved in the future, but Python 3.9 is now end-of-life, so it's not high-priority.
```

---

_@AlexWaygood reviewed on 2025-11-10 20:42_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/new_types.md_-_NewType_-_The_base_of_a_`NewTy…_(8e2d1e1f951b7fc4).snap`:37 on 2025-11-10 20:42_

great diagnostic!

---

_@AlexWaygood approved on 2025-11-10 21:17_

https://github.com/astral-sh/ruff/pull/21157#discussion_r2504575209 is the only remaining significant thing I think that's missing from our `NewType` implementation now! And it's absolutely fine to defer that to a followup IMO

---

_Label `ecosystem-analyzer` removed by @oconnor663 on 2025-11-10 22:11_

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-11-10 22:11_

---

_Label `ecosystem-analyzer` removed by @oconnor663 on 2025-11-10 22:16_

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-11-10 22:18_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:1 on 2025-11-10 22:54_

This test suite is fantastic. So thorough, so clear, all the prose is on-point. Thank you!!

---

_Merged by @oconnor663 on 2025-11-10 22:55_

---

_Closed by @oconnor663 on 2025-11-10 22:55_

---

_Branch deleted on 2025-11-10 22:55_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:4979 on 2025-11-10 22:56_

Not clear what the name `tp` refers to here?
```suggestion
        // Inference of the base type must be deferred, to avoid cycles.
```

---

_@oconnor663 reviewed on 2025-11-10 22:57_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:397 on 2025-11-10 22:57_

@AlexWaygood do we like to open issues for these, or is the comment itself sufficient?

---

_@carljm reviewed on 2025-11-10 23:04_

Whoops I'm slightly slow on the draw here -- just one comment on a comment :) Probably doesn't even matter enough to address in a follow-up PR.

Awesome work here!

---

_@AlexWaygood reviewed on 2025-11-10 23:05_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:397 on 2025-11-10 23:05_

This is something mandated by the spec, so it would be great to open an issue for it. I don't see it as particularly high priority right now, though!

---

_@oconnor663 reviewed on 2025-11-11 00:23_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:1 on 2025-11-11 00:23_

A *lot* of those cases are copy/pasted from @AlexWaygood's suggestions, to be clear :sweat_smile: Obviously all the bits that are like

`from ty_extensions import is_bivariant_endofunctor`

---

_@AlexWaygood reviewed on 2025-11-11 00:26_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:1 on 2025-11-11 00:26_

No no, bivariant endofunctors are your assignment for the GA release 

---

_@oconnor663 reviewed on 2025-11-11 00:31_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/infer/builder.rs`:4979 on 2025-11-11 00:31_

Ah, the names of `NewType`'s arguments are `name` and `tp`. I guess I spent enough time swimming in this that I forgot how non-obvious that is. `NewType` technically supports keyword args, though I didn't include that in this PR, and given all the "only simple assignments are allowed" behavior here and in other typecheckers, I'm not sure whether it would be a good idea to use them...

---

_@oconnor663 reviewed on 2025-11-11 00:43_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/annotations/new_types.md`:397 on 2025-11-11 00:43_

https://github.com/astral-sh/ty/issues/1519

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:4979 on 2025-11-11 15:49_

I ended up supporting keyword args for `TypeVar`. I think it's totally fine to base our decision for `NewType` on whether we ever get a user report. (For reference, it looks like pyright does support keyword arguments with `NewType`, but neither mypy nor pyrefly does.)

---

_@carljm reviewed on 2025-11-11 15:49_

---
