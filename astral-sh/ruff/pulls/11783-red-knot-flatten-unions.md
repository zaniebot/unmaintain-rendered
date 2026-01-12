```yaml
number: 11783
title: "[red-knot] flatten unions"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/ifexpr
created_at: 2024-06-06T21:00:47Z
updated_at: 2024-06-08T13:51:19Z
url: https://github.com/astral-sh/ruff/pull/11783
synced_at: 2026-01-12T15:55:38Z
```

# [red-knot] flatten unions

---

_@carljm_

Flatten union types. Fixes #11781


---

_Review requested from @MichaReiser by @carljm on 2024-06-06 21:00_

---

_Label `red-knot` added by @carljm on 2024-06-06 21:00_

---

_Review requested from @AlexWaygood by @carljm on 2024-06-06 21:01_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/semantic/types/infer.rs`:627 on 2024-06-06 21:08_

When printing the revealed type to the user, it would be nice if we could prettify this to `Literal[C1, C2, C3]` (pyright does this: https://pyright-play.net/?pyrightVersion=1.1.361&strict=true&code=MYRgBAvGIFDATJM84GYmpgMwDYEMBzAGjF0MSgCMB7anACgEoSa6mYAPJUMASy1L4CYAKY4AziLAI%2BAsgURjJ0zACcRANxF4cAfQAuATwAOI%2Bh0YwgA). But doesn't need to be done now

---

_Comment by @github-actions[bot] on 2024-06-06 21:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot/src/semantic/types.rs`:245 on 2024-06-06 21:20_

It took me a little while to understand why `either` was being used here. I think here I'd find a more imperative approach more readable (and then we don't need the new `either` dependency for the crate):

```suggestion
        let mut flattened = Vec::with_capacity(elems.len());
        for elem in elems {
            match elem {
                Type::Union(union_id) => flattened.extend(union_id.elements(self)),
                _ => flattened.push(*elem),
            }
        }
```

---

_Review comment by @AlexWaygood on `crates/red_knot/src/semantic/types.rs`:546 on 2024-06-06 21:26_

It's a bit weird to me that we have to call `.collect()` here in order to create the iterator instance. Normally I wouldn't expect a method returning an iterator to allocate anything, but here there is an allocation under the hood.

If we're already collecting, why don't we just return `Vec<Type>` from `.elements()`? You can do more with a `Vec` than you can with an iterator. Or do you not want us to start relying on that API?

---

_@AlexWaygood reviewed on 2024-06-06 21:26_

---

_@AlexWaygood reviewed on 2024-06-06 21:27_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/semantic/types/infer.rs`:627 on 2024-06-06 21:27_

Oh, I see you already created #11782 :)

---

_Review comment by @carljm on `crates/red_knot/src/semantic/types/infer.rs`:627 on 2024-06-06 21:37_

Feel free to open an issue for this!

One thing that's not clear to me is whether we want to mix different kinds of literals or not. E.g. do we prefer `Literal[1, C, "foo", "bar"]` or `Literal[1] | Literal[C] | Literal["foo", "bar"]`?

---

_@carljm reviewed on 2024-06-06 21:37_

---

_@carljm reviewed on 2024-06-06 21:37_

---

_Review comment by @carljm on `crates/red_knot/src/semantic/types/infer.rs`:627 on 2024-06-06 21:37_

In #11784 I did this condensing, but only for int literals so far.

---

_@carljm reviewed on 2024-06-06 21:40_

---

_Review comment by @carljm on `crates/red_knot/src/semantic/types.rs`:245 on 2024-06-06 21:40_

Ugh, but it took me a while to figure out how to do this successfully with `flat_map`! Ok, ok, I'll switch to imperative style...

---

_@AlexWaygood reviewed on 2024-06-06 21:44_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/semantic/types/infer.rs`:627 on 2024-06-06 21:44_

> One thing that's not clear to me is whether we want to mix different kinds of literals or not. E.g. do we prefer `Literal[1, C, "foo", "bar"]` or `Literal[1] | Literal[C] | Literal["foo", "bar"]`?

yeah, not sure... especially with objects (such as classes) that users might not be expecting to see in a `Literal` slice... I'll possibly open an issue tomorrow, gonna call it for the night pretty soon!

---

_@carljm reviewed on 2024-06-06 21:48_

---

_Review comment by @carljm on `crates/red_knot/src/semantic/types.rs`:546 on 2024-06-06 21:48_

Yeah, you're right, this should just return an owned vec. I might not even need it at all if I do the imperative version above.

---

_@carljm reviewed on 2024-06-06 21:51_

---

_Review comment by @carljm on `crates/red_knot/src/semantic/types/infer.rs`:627 on 2024-06-06 21:51_

I went ahead and created #11785

---

_Merged by @carljm on 2024-06-06 22:13_

---

_Closed by @carljm on 2024-06-06 22:13_

---

_Branch deleted on 2024-06-06 22:13_

---

_Comment by @AlexWaygood on 2024-06-07 16:08_

I managed to get the `elements()` method working without any `.collect()` calls, if you're interested:

```diff
diff --git a/crates/red_knot/src/semantic/types.rs b/crates/red_knot/src/semantic/types.rs
index 360b157c5..ae866e00c 100644
--- a/crates/red_knot/src/semantic/types.rs
+++ b/crates/red_knot/src/semantic/types.rs
@@ -1,4 +1,6 @@
 #![allow(dead_code)]
+use std::iter::FusedIterator;
+
 use crate::ast_ids::NodeKey;
 use crate::db::{QueryResult, SemanticDb, SemanticJar};
 use crate::files::FileId;
@@ -531,12 +533,41 @@ pub struct UnionTypeId {
 }
 
 impl UnionTypeId {
-    pub fn elements(self, type_store: &TypeStore) -> Vec<Type> {
+    pub fn elements(self, type_store: &TypeStore) -> UnionElementsIterator {
         let union = type_store.get_union(self);
-        union.elements.iter().copied().collect()
+        UnionElementsIterator { union, index: 0 }
     }
 }
 
+pub struct UnionElementsIterator<'a> {
+    union: UnionTypeRef<'a>,
+    index: usize,
+}
+
+impl<'a> Iterator for UnionElementsIterator<'a> {
+    type Item = Type;
+
+    fn next(&mut self) -> Option<Self::Item> {
+        let item = self.union.elements.get_index(self.index).copied();
+        self.index += 1;
+        item
+    }
+
+    fn size_hint(&self) -> (usize, Option<usize>) {
+        let size = self.union.elements.len() - self.index;
+        (size, Some(size))
+    }
+
+    fn last(self) -> Option<Self::Item> {
+        self.union.elements.last().copied()
+    }
+}
+
+impl<'a> FusedIterator for UnionElementsIterator<'a> {}
+impl<'a> ExactSizeIterator for UnionElementsIterator<'a> {}
```

A bit more verbose, but maybe worth it if we'll be iterating over union elements a lot? WDYT?

---

_Comment by @carljm on 2024-06-07 16:29_

Yeah, I think that's probably worth it! I knew this type of borrowing iterator was an option, but at the time I was being stubborn about trying to get it working with `flat_map`, which requires that the returned iterator own its items, meaning this type of iterator wouldn't work for that. But now that we aren't using `flat_map` to flatten anyway, this would work fine.

We should do the same for `IntersectionPositiveIterator` and `IntersectionNegativeIterator` as well.

I think this is something that can wait until post-Salsa, though; no need to introduce more PRs right now for Micha to port.

---
