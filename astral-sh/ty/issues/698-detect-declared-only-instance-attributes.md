```yaml
number: 698
title: Detect declared-only instance attributes
type: issue
state: closed
author: graipher
labels:
  - good first issue
  - attribute access
assignees: []
created_at: 2025-06-25T05:59:44Z
updated_at: 2025-07-07T11:07:17Z
url: https://github.com/astral-sh/ty/issues/698
synced_at: 2026-01-10T02:07:36Z
```

# Detect declared-only instance attributes

---

_Issue opened by @graipher on 2025-06-25 05:59_

### Summary

When typing unit tests written with `pytest`, when parametrizing a fixture in the following way:

```python
# /// script
# dependencies = [
#   "pytest",
# ]
# ///

import pytest


@pytest.fixture(params=["foo", "bar"])
def foo_fixture(request: pytest.FixtureRequest) -> str:
    return request.param
```

`ty` does not recognize that the `pytest.FixtureRequest` object does indeed have a `param` attribute:

```shell
uvx ty check .
error[unresolved-attribute]: Type `FixtureRequest` has no attribute `param`
  --> main.py:12:12
   |
10 | @pytest.fixture(params=["foo", "bar"])
11 | def foo_fixture(request: pytest.FixtureRequest) -> str:
12 |     return request.param
   |            ^^^^^^^^^^^^^
   |
info: rule `unresolved-attribute` is enabled by default
```

despite the fact that `pytest.FixtureRequest` declares this attribute in it's `__init__` method:

```python
class FixtureRequest(abc.ABC):
    """The type of the ``request`` fixture.

    A request object gives access to the requesting test context and has a
    ``param`` attribute in case the fixture is parametrized.
    """

    def __init__(
        self,
        pyfuncitem: Function,
        fixturename: str | None,
        arg2fixturedefs: dict[str, Sequence[FixtureDef[Any]]],
        fixture_defs: dict[str, FixtureDef[Any]],
        *,
        _ispytest: bool = False,
    ) -> None:
        check_ispytest(_ispytest)
        #: Fixture for which this request is being performed.
        self.fixturename: Final = fixturename
        self._pyfuncitem: Final = pyfuncitem
        # The FixtureDefs for each fixture name requested by this item.
        # Starts from the statically-known fixturedefs resolved during
        # collection. Dynamically requested fixtures (using
        # `request.getfixturevalue("foo")`) are added dynamically.
        self._arg2fixturedefs: Final = arg2fixturedefs
        # The evaluated argnames so far, mapping to the FixtureDef they resolved
        # to.
        self._fixture_defs: Final = fixture_defs
        # Notes on the type of `param`:
        # -`request.param` is only defined in parametrized fixtures, and will raise
        #   AttributeError otherwise. Python typing has no notion of "undefined", so
        #   this cannot be reflected in the type.
        # - Technically `param` is only (possibly) defined on SubRequest, not
        #   FixtureRequest, but the typing of that is still in flux so this cheats.
        # - In the future we might consider using a generic for the param type, but
        #   for now just using Any.
        self.param: Any

    ...
```

Both `mypy` and `pyright` do not emit an error for this (`mypy` does complain about the return annotation being a `str` and `request.param` returning `Any` in strict mode, though).

I guess that attributes assigned in `__init__` methods are currently not considered or that there is at least no proper resolution to which `__init__` gets called (since this is an ABC)? The comment above `self.param` may also be relevant here.

### Version

ty 0.0.1-alpha.11

---

_Renamed from "`pytest.FixtureRequest` attributes not resolved correctly" to "`pytest.FixtureRequest` `param` attribute not resolved correctly" by @graipher on 2025-06-25 05:59_

---

_Label `bug` added by @sharkdp on 2025-06-25 06:13_

---

_Label `attribute access` added by @sharkdp on 2025-06-25 06:13_

---

_Comment by @sharkdp on 2025-06-25 06:14_

Thank you very much for reporting this — and for the initial analysis.

We're currently not recognizing a declaration-only annotated assignment (without a right-hand side) such as `self.param: Type` as evidence for an instance attribute:
```py
class C:
    def __init__(self):
        self.a: int


C().a  # unresolved-attribute
```
https://play.ty.dev/812a2e90-7b5f-4865-a3d6-daad7fa6a4bf

This is a known limitation (see the `TODO` for `declared_only` in [this test case](https://github.com/astral-sh/ruff/blob/cb152b47253182df9bbd19a93a700e90060774cb/crates/ty_python_semantic/resources/mdtest/attributes.md#variable-only-declaredbound-in-__init__)).

---

_Label `bug` removed by @sharkdp on 2025-06-25 06:20_

---

_Comment by @sharkdp on 2025-06-25 06:32_

This could probably be a good (but not too easy) task for interested contributors, with a relatively limited scope (?). Instead of only considering bindings for attribute assignments, we need to consider both bindings and declarations. Declarations are already correctly recorded in the semantic index, but in type inference, we currently only access the bindings. The patch below may be useful as a reference to see which places in the source code need to change. It switches everything from bindings-only to declarations-only (which would fix the issue here), but of course that needs to be extended to consider both.

Just like when resolving references to normal variables, declarations should take precedence over bindings. That is, even if we see conflicting definitions such as `self.x: int` and `self.x = "a string"`, the type of the `x` attribute should just be `int`.

<details>

```diff
diff --git a/crates/ty_python_semantic/src/semantic_index.rs b/crates/ty_python_semantic/src/semantic_index.rs
index d4f848e69d..9be2b157d7 100644
--- a/crates/ty_python_semantic/src/semantic_index.rs
+++ b/crates/ty_python_semantic/src/semantic_index.rs
@@ -108,7 +108,7 @@ pub(crate) fn attribute_assignments<'db, 's>(
     db: &'db dyn Db,
     class_body_scope: ScopeId<'db>,
     name: &'s str,
-) -> impl Iterator<Item = (BindingWithConstraintsIterator<'db, 'db>, FileScopeId)> + use<'s, 'db> {
+) -> impl Iterator<Item = (DeclarationsIterator<'db, 'db>, FileScopeId)> + use<'s, 'db> {
     let file = class_body_scope.file(db);
     let index = semantic_index(db, file);
 
@@ -116,7 +116,7 @@ pub(crate) fn attribute_assignments<'db, 's>(
         let place_table = index.place_table(function_scope_id);
         let place = place_table.place_id_by_instance_attribute_name(name)?;
         let use_def = &index.use_def_maps[function_scope_id];
-        Some((use_def.public_bindings(place), function_scope_id))
+        Some((use_def.public_declarations(place), function_scope_id))
     })
 }
 
diff --git a/crates/ty_python_semantic/src/semantic_index/use_def.rs b/crates/ty_python_semantic/src/semantic_index/use_def.rs
index 8ac7f91811..978ce2de7a 100644
--- a/crates/ty_python_semantic/src/semantic_index/use_def.rs
+++ b/crates/ty_python_semantic/src/semantic_index/use_def.rs
@@ -463,10 +463,10 @@ impl<'db> UseDefMap<'db> {
             .is_always_false()
     }
 
-    pub(crate) fn is_binding_reachable(
+    pub(crate) fn is_declaration_reachable(
         &self,
         db: &dyn crate::Db,
-        binding: &BindingWithConstraints<'_, 'db>,
+        binding: &DeclarationWithConstraint<'db>,
     ) -> Truthiness {
         self.reachability_constraints.evaluate(
             db,
diff --git a/crates/ty_python_semantic/src/types/class.rs b/crates/ty_python_semantic/src/types/class.rs
index 3cd7bffa5b..371291a94a 100644
--- a/crates/ty_python_semantic/src/types/class.rs
+++ b/crates/ty_python_semantic/src/types/class.rs
@@ -1744,10 +1744,10 @@ impl<'db> ClassLiteral<'db> {
                     let method = index.expect_single_definition(method_def);
                     let method_place = class_table.place_id_by_name(&method_def.name).unwrap();
                     class_map
-                        .public_bindings(method_place)
-                        .find_map(|bind| {
-                            (bind.binding.is_defined_and(|def| def == method))
-                                .then(|| class_map.is_binding_reachable(db, &bind))
+                        .public_declarations(method_place)
+                        .find_map(|decl| {
+                            (decl.declaration.is_defined_and(|def| def == method))
+                                .then(|| class_map.is_declaration_reachable(db, &decl))
                         })
                         .unwrap_or(Truthiness::AlwaysFalse)
                 } else {
@@ -1762,7 +1762,7 @@ impl<'db> ClassLiteral<'db> {
             let mut unbound_binding = None;
 
             for attribute_assignment in attribute_assignments {
-                if let DefinitionState::Undefined = attribute_assignment.binding {
+                if let DefinitionState::Undefined = attribute_assignment.declaration {
                     // Store the implicit unbound binding here so that we can delay the
                     // computation of `unbound_reachability` to the point when we actually
                     // need it. This is an optimization for the common case where the
@@ -1772,11 +1772,11 @@ impl<'db> ClassLiteral<'db> {
                     continue;
                 }
 
-                let DefinitionState::Defined(binding) = attribute_assignment.binding else {
+                let DefinitionState::Defined(binding) = attribute_assignment.declaration else {
                     continue;
                 };
                 match method_map
-                    .is_binding_reachable(db, &attribute_assignment)
+                    .is_declaration_reachable(db, &attribute_assignment)
                     .and(is_method_reachable)
                 {
                     Truthiness::AlwaysTrue => {
@@ -1797,7 +1797,7 @@ impl<'db> ClassLiteral<'db> {
                 // TODO: this is incomplete logic since the attributes bound after termination are considered reachable.
                 let unbound_reachability = unbound_binding
                     .as_ref()
-                    .map(|binding| method_map.is_binding_reachable(db, binding))
+                    .map(|binding| method_map.is_declaration_reachable(db, binding))
                     .unwrap_or(Truthiness::AlwaysFalse);
 
                 if unbound_reachability
```

</details>

---

_Label `good first issue` added by @sharkdp on 2025-06-25 06:32_

---

_Comment by @iyakushev on 2025-06-25 08:38_

Looks interesting. I will take a look this/next week. @sharkdp If I have any questions regarding the PR should I post them here?

---

_Comment by @sharkdp on 2025-06-25 09:01_

@iyakushev Sounds great. Feel free to post questions here or on our [Discord channel](https://discord.com/invite/astral-sh) in `#typing`.

---

_Renamed from "`pytest.FixtureRequest` `param` attribute not resolved correctly" to "Detect declared-only instance attributes" by @sharkdp on 2025-07-02 15:30_

---

_Comment by @AlexWaygood on 2025-07-07 11:07_

fixed in https://github.com/astral-sh/ruff/pull/19048

---

_Closed by @AlexWaygood on 2025-07-07 11:07_

---
