```yaml
number: 2260
title: "Variable is resolved to `Never` after `match` against `Enum | None`"
type: issue
state: closed
author: condemil
labels:
  - set-theoretic types
assignees: []
created_at: 2025-12-29T16:01:36Z
updated_at: 2025-12-30T03:19:30Z
url: https://github.com/astral-sh/ty/issues/2260
synced_at: 2026-01-10T01:56:41Z
```

# Variable is resolved to `Never` after `match` against `Enum | None`

---

_Issue opened by @condemil on 2025-12-29 16:01_

### Summary

The expected outcome is that `ty` will resolve `c` variable as `TestClass`, but it's resolved to `Never`. Probably `ty` thinks that `none_value` will always match to one of `case`'s, but it's not.

```python
from enum import Enum


class TestEnum(Enum):
    FOO = "FOO"
    BAR = "BAR"

class TestClass: ...

async def test():
    none_value = get_none()

    match none_value:
        case TestEnum.FOO:
            return
        case TestEnum.BAR:
            return

    c = TestClass()  # Never



def get_none() -> TestEnum | None:
    return None
```

### Version

_No response_

---

_Renamed from "Return variables resolved to `Never` after `match` against `Enum | None`" to "Variable is resolved to `Never` after `match` against `Enum | None`" by @condemil on 2025-12-29 16:01_

---

_Label `control flow` added by @mtshiba on 2025-12-29 16:15_

---

_Comment by @carljm on 2025-12-29 16:23_

Thanks for the report -- this definitely looks like a bug!

---

_Added to milestone `Stable` by @carljm on 2025-12-29 16:23_

---

_Comment by @mtshiba on 2025-12-29 16:28_

It looks like the reachability of the code after the match statement is determined to be `AlwaysFalse`.

---

_Comment by @AlexWaygood on 2025-12-29 18:35_

Slightly smaller repro:

```py
from enum import Enum

class TestEnum(Enum):
    FOO = "FOO"
    BAR = "BAR"

def test(none_value: TestEnum | None):
    if none_value != TestEnum.FOO:
        reveal_type(none_value)  # revealed: Literal[TestEnum.BAR] ??
```

https://play.ty.dev/23764653-906c-47c0-86b5-070649c3172e

---

_Removed from milestone `Stable` by @carljm on 2025-12-29 19:19_

---

_Added to milestone `Pre-stable 1` by @carljm on 2025-12-29 19:19_

---

_Comment by @charliermarsh on 2025-12-29 19:30_

This diff appears to fix it (though haven't dug-in to understand if it's correct):
```diff
diff --git a/crates/ty_python_semantic/src/types/builder.rs b/crates/ty_python_semantic/src/types/builder.rs
index 132c5b077d..36a258f20d 100644
--- a/crates/ty_python_semantic/src/types/builder.rs
+++ b/crates/ty_python_semantic/src/types/builder.rs
@@ -810,13 +810,6 @@ impl<'db> IntersectionBuilder<'db> {
         ty: Type<'db>,
         seen_aliases: &mut Vec<Type<'db>>,
     ) -> Self {
-        let contains_enum = |enum_instance| {
-            self.intersections
-                .iter()
-                .flat_map(|intersection| &intersection.positive)
-                .any(|ty| *ty == enum_instance)
-        };
-
         // See comments above in `add_positive`; this is just the negated version.
         match ty {
             Type::TypeAlias(alias) => {
@@ -871,12 +864,27 @@ impl<'db> IntersectionBuilder<'db> {
                     },
                 )
             }
-            Type::EnumLiteral(enum_literal)
-                if contains_enum(enum_literal.enum_class_instance(self.db)) =>
-            {
-                let db = self.db;
-                self.add_positive_impl(
-                    UnionType::from_elements(
+            Type::EnumLiteral(enum_literal) => {
+                let enum_instance = enum_literal.enum_class_instance(self.db);
+
+                // Partition intersections into those that contain the enum instance and those that don't.
+                // For intersections containing the enum, we need to expand to remaining members.
+                // For others, we just add the negative normally.
+                let (enum_intersections, other_intersections): (Vec<_>, Vec<_>) = self
+                    .intersections
+                    .into_iter()
+                    .partition(|inner| inner.positive.contains(&enum_instance));
+
+                if enum_intersections.is_empty() {
+                    // No inner intersection contains the enum, just add negative normally
+                    self.intersections = other_intersections;
+                    for inner in &mut self.intersections {
+                        inner.add_negative(self.db, ty);
+                    }
+                    self
+                } else {
+                    let db = self.db;
+                    let remaining_members = UnionType::from_elements(
                         db,
                         enum_member_literals(
                             db,
@@ -884,9 +892,32 @@ impl<'db> IntersectionBuilder<'db> {
                             Some(enum_literal.name(db)),
                         )
                         .expect("Calling `enum_member_literals` on an enum class"),
-                    ),
-                    seen_aliases,
-                )
+                    );
+
+                    // For enum-containing intersections, add the remaining members as positive
+                    let mut enum_builder = IntersectionBuilder {
+                        db,
+                        order_elements: self.order_elements,
+                        intersections: enum_intersections,
+                    }
+                    .add_positive_impl(remaining_members, seen_aliases);
+
+                    // For non-enum intersections, just add the negative normally
+                    let mut other_builder = IntersectionBuilder {
+                        db,
+                        order_elements: self.order_elements,
+                        intersections: other_intersections,
+                    };
+                    for inner in &mut other_builder.intersections {
+                        inner.add_negative(db, ty);
+                    }
+
+                    // Combine the results
+                    enum_builder
+                        .intersections
+                        .extend(other_builder.intersections);
+                    enum_builder
+                }
             }
             _ => {
                 for inner in &mut self.intersections {
```

---

_Label `control flow` removed by @AlexWaygood on 2025-12-29 19:45_

---

_Label `set-theoretic types` added by @AlexWaygood on 2025-12-29 19:45_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-29 20:35_

---

_Closed by @charliermarsh on 2025-12-30 03:19_

---
