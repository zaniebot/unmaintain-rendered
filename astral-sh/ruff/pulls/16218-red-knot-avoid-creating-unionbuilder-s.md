```yaml
number: 16218
title: "[red-knot] Avoid creating `UnionBuilder`s unnecessarily"
type: pull_request
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
draft: true
base: main
head: micha/union-builder-from-elements
created_at: 2025-02-17T17:38:26Z
updated_at: 2025-05-03T17:47:45Z
url: https://github.com/astral-sh/ruff/pull/16218
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Avoid creating `UnionBuilder`s unnecessarily

---

_@MichaReiser_

## Summary
`UnionBuilder::from_elements` is called very frequently. Let's see if short-circuiting in the case where `elements` is zero or one elements
long helps improve perf.

## Test Plan

This should not change behavior.


---

_Review requested from @carljm by @MichaReiser on 2025-02-17 17:38_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-17 17:38_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-17 17:38_

---

_Label `red-knot` added by @MichaReiser on 2025-02-17 17:38_

---

_Converted to draft by @MichaReiser on 2025-02-17 17:38_

---

_Comment by @AlexWaygood on 2025-02-18 11:46_

This looks great to me, and codspeed's reporting a 1% speedup. Another optimisation idea could be to pre-size the `UnionBuilder`, e.g.

```diff
diff --git a/crates/red_knot_python_semantic/src/types.rs b/crates/red_knot_python_semantic/src/types.rs
index 4601543d4..6df55b467 100644
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -4695,7 +4695,13 @@ impl<'db> UnionType<'db> {
             return first.into();
         };
 
-        let mut builder = UnionBuilder::new(db).add(first.into()).add(second.into());
+        let iterator_length = elements.size_hint().1.and_then(|size| size.checked_add(2));
+
+        let mut builder = iterator_length
+            .map(|num_elements| UnionBuilder::with_capacity(db, num_elements))
+            .unwrap_or_else(|| UnionBuilder::new(db))
+            .add(first.into())
+            .add(second.into());
 
         for element in elements {
             builder = builder.add(element.into());
diff --git a/crates/red_knot_python_semantic/src/types/builder.rs b/crates/red_knot_python_semantic/src/types/builder.rs
index 45be6ac48..c5cdf294d 100644
--- a/crates/red_knot_python_semantic/src/types/builder.rs
+++ b/crates/red_knot_python_semantic/src/types/builder.rs
@@ -43,6 +43,13 @@ impl<'db> UnionBuilder<'db> {
         }
     }
 
+    pub(crate) fn with_capacity(db: &'db dyn Db, capacity: usize) -> Self {
+        Self {
+            db,
+            elements: Vec::with_capacity(capacity),
+        }
+    }
+
     /// Collapse the union to a single type: `object`.
     fn collapse_to_object(mut self) -> Self {
         self.elements.clear();
```

---

_Renamed from "[red-knot] Avoid creating `UnionBuilder`s unnecessary" to "[red-knot] Avoid creating `UnionBuilder`s unnecessarily" by @AlexWaygood on 2025-02-18 11:49_

---

_Closed by @MichaReiser on 2025-02-28 09:21_

---

_Branch deleted on 2025-05-03 17:47_

---
