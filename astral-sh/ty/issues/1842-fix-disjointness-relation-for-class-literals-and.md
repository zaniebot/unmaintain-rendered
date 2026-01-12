```yaml
number: 1842
title: Fix disjointness relation for class literals and generic aliases
type: issue
state: closed
author: sharkdp
labels:
  - type properties
assignees: []
created_at: 2025-12-10T16:19:27Z
updated_at: 2025-12-10T20:28:20Z
url: https://github.com/astral-sh/ty/issues/1842
synced_at: 2026-01-12T15:54:25Z
```

# Fix disjointness relation for class literals and generic aliases

---

_@sharkdp_

There are some inconsistencies in our disjointness relation modeling for class literals and generic aliases. They are documented in the failing tests here:

https://github.com/astral-sh/ruff/blob/951766d1fbce5bfa6758f81876ae26124892f33e/crates/ty_python_semantic/resources/mdtest/type_properties/is_disjoint_from.md?plain=1#L669-L709

We should fix those tests and probably also increase the test coverage.


If it's helpful, I started to work on this, and this is how far I got:

<details><summary>Diff</summary>
<p>

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 12a38a8e34..49021d4b08 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -3347,7 +3347,6 @@ impl<'db> Type<'db> {
                 | Type::WrapperDescriptor(..)
                 | Type::ModuleLiteral(..)
                 | Type::ClassLiteral(..)
-                | Type::GenericAlias(..)
                 | Type::SpecialForm(..)
                 | Type::KnownInstance(..)),
                 right @ (Type::BooleanLiteral(..)
@@ -3361,11 +3360,90 @@ impl<'db> Type<'db> {
                 | Type::WrapperDescriptor(..)
                 | Type::ModuleLiteral(..)
                 | Type::ClassLiteral(..)
-                | Type::GenericAlias(..)
                 | Type::SpecialForm(..)
                 | Type::KnownInstance(..)),
             ) => ConstraintSet::from(left != right),
 
+            (
+                Type::GenericAlias(..),
+                Type::BooleanLiteral(..)
+                | Type::IntLiteral(..)
+                | Type::StringLiteral(..)
+                | Type::BytesLiteral(..)
+                | Type::EnumLiteral(..)
+                | Type::FunctionLiteral(..)
+                | Type::BoundMethod(..)
+                | Type::KnownBoundMethod(..)
+                | Type::WrapperDescriptor(..)
+                | Type::ModuleLiteral(..)
+                | Type::SpecialForm(..)
+                | Type::KnownInstance(..),
+            )
+            | (
+                Type::BooleanLiteral(..)
+                | Type::IntLiteral(..)
+                | Type::StringLiteral(..)
+                | Type::BytesLiteral(..)
+                | Type::EnumLiteral(..)
+                | Type::FunctionLiteral(..)
+                | Type::BoundMethod(..)
+                | Type::KnownBoundMethod(..)
+                | Type::WrapperDescriptor(..)
+                | Type::ModuleLiteral(..)
+                | Type::SpecialForm(..)
+                | Type::KnownInstance(..),
+                Type::GenericAlias(..),
+            ) => ConstraintSet::from(false),
+
+            (Type::GenericAlias(left_alias), Type::GenericAlias(right_alias)) => {
+                disjointness_visitor.visit((self, other), || {
+                    ConstraintSet::from(left_alias != right_alias).or(db, || {
+                        (left_alias
+                            .specialization(db)
+                            .has_relation_to_impl(
+                                db,
+                                right_alias.specialization(db),
+                                InferableTypeVars::None,
+                                TypeRelation::Assignability,
+                                relation_visitor,
+                                disjointness_visitor,
+                            )
+                            .negate(db))
+                        .and(db, || {
+                            right_alias
+                                .specialization(db)
+                                .has_relation_to_impl(
+                                    db,
+                                    left_alias.specialization(db),
+                                    InferableTypeVars::None,
+                                    TypeRelation::Assignability,
+                                    relation_visitor,
+                                    disjointness_visitor,
+                                )
+                                .negate(db)
+                        })
+                    })
+                })
+            }
+
+            (Type::ClassLiteral(class_literal), Type::GenericAlias(alias))
+            | (Type::GenericAlias(alias), Type::ClassLiteral(class_literal)) => {
+                disjointness_visitor.visit((self, other), || {
+                    class_literal
+                        .default_specialization(db)
+                        .into_generic_alias()
+                        .when_none_or(|other_alias| {
+                            Type::GenericAlias(other_alias).is_disjoint_from_impl(
+                                db,
+                                Type::GenericAlias(alias),
+                                inferable,
+                                disjointness_visitor,
+                                relation_visitor,
+                            )
+                        })
+                })
+            }
+
             (
                 Type::SubclassOf(_),
                 Type::BooleanLiteral(..)

```

</p>
</details> 

---

_Label `type properties` added by @sharkdp on 2025-12-10 16:19_

---

_Added to milestone `Stable` by @sharkdp on 2025-12-10 16:19_

---

_Comment by @sharkdp on 2025-12-10 20:28_

Closed by @ibraheemdev in https://github.com/astral-sh/ruff/pull/21770

---

_Closed by @sharkdp on 2025-12-10 20:28_

---
