```yaml
number: 14207
title: "[red-knot] Consolidate detection of cyclically defined classes"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/cycle-detection
created_at: 2024-11-08T17:49:55Z
updated_at: 2024-11-08T22:17:57Z
url: https://github.com/astral-sh/ruff/pull/14207
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] Consolidate detection of cyclically defined classes

---

_@AlexWaygood_

## Summary

Fixes #14141.

Attempting to infer the MRO or metaclass of a cyclically defined class must be avoided, or we'll end up in an infinite loop. As such, our logic for determining metaclasses and our logic for determining MROs both have logic included to detect if a class is cyclically defined and abort if so.

This PR moves the cycle-detection logic from `mro.rs` into a single method on `ClassType` that is queried by both the MRO-inference and metaclass-inference methods. This has several advantages:
- It means that for cyclically defined classes, we don't even need to call the `try_mro` and `try_metaclass` Salsa queries in `check_class_definitions`. We know that we won't be able to calculate either, so there's no need to do so. It doesn't cost us anything to eagerly check whether the class is cyclically defined in the `check_class_definitions` method, because we would need to do this check anyway before even attempting to check that the class's MRO or metaclass was resolvable.
- It gets rid of this awkward patch of code here, where we have to just ignore the error and move on, because we know it'll have already been handled by the MRO-checking logic immediately above: https://github.com/astral-sh/ruff/blob/953e862aca1449651d96036fcd35f7fab4ccda51/crates/red_knot_python_semantic/src/types/infer.rs#L567-L570
- It's a reasonably significant reduction in code quantity. It's also a reasonably significant reduction in code complexity: there's now no need for a recursive inner function in `try_metaclass`, for example.

The diff in the metaclass-inference logic is a bit hard to read -- sorry! -- but there's really not much changed overall. It's just all been dedented because there's no longer any need for the large recursive inner function.

## Can we use the `specify` Salsa functionality?

@carljm suggested in https://github.com/astral-sh/ruff/issues/14141 that we could use Salsa's `specify` functionality to let Salsa know that it didn't even need to call `try_mro` or `try_metaclass` if there was a cyclic class definition, since if there's a cyclic class definition we know we'll just return an error variant from these queries. I tried this, and it doesn't seem to work: Salsa complains that `Class` doesn't implement the `salsa::plumbing::TrackedStructInDb` trait if I make this change:

```diff
diff --git a/crates/red_knot_python_semantic/src/types.rs b/crates/red_knot_python_semantic/src/types.rs
index 2af8f1d9b..107305860 100644
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -2322,7 +2322,7 @@ impl<'db> Class<'db> {
     }
 
     /// Return the metaclass of this class, or an error if the metaclass cannot be inferred.
-    #[salsa::tracked]
+    #[salsa::tracked(specify)]
     pub(crate) fn try_metaclass(self, db: &'db dyn Db) -> Result<Type<'db>, MetaclassError<'db>> {
```

I looked into this a little bit and I don't think it would be correct (and maybe not possible?) for `Class` to implement that trait, so I don't think we can use `specify` for Salsa-tracked methods on `Class`.

## Test Plan

Existing tests all pass.


---

_Label `red-knot` added by @AlexWaygood on 2024-11-08 17:49_

---

_Review requested from @carljm by @AlexWaygood on 2024-11-08 17:49_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-08 17:49_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-11-08 17:49_

---

_Comment by @github-actions[bot] on 2024-11-08 18:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2333 on 2024-11-08 21:33_

nit: obviously it's not impossible, we did it prior to this diff :) might be clearer to just say "we'd enter an infinite loop."

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2447 on 2024-11-08 21:35_

I guess the main cost of this diff (due to the fact that we can't use `specify`) is an additional tracked function for each class. But I think this is a reasonable tradeoff; there are relatively few classes (compared to the number of, say, definitions), so I don't think this will make a huge difference to our total tracked-functions count.

---

_@carljm approved on 2024-11-08 21:36_

Looks good to me, thanks for this cleanup!

Interesting that no mdtests had to change here. Does that mean that this didn't actually regress the MRO we infer for cases where only a sub-graph of our MRO is cyclic?

---

_Comment by @AlexWaygood on 2024-11-08 21:44_

> Interesting that no mdtests had to change here. Does that mean that this didn't actually regress the MRO we infer for cases where only a sub-graph of our MRO is cyclic?

That's correct. At one point in my MRO PR I had a very weak form of partial recovery for cyclic classes. But it was pretty ad-hoc/unprincipled, and I didn't think it was particularly useful (given how cyclic classes inherently don't make any sense, and always fail at runtime anyway). So I ripped it out before landing the MRO PR; even though it wasn't _much_ additional complexity, it didn't seem worth it considering how tiny the benefits were.

For any class with a cycle in its MRO, we infer the MRO as being `[<the class in question>, <Unknown>, <object>]`. That was true before this PR, and it's true with this PR as well.

---

_@AlexWaygood reviewed on 2024-11-08 21:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2333 on 2024-11-08 21:48_

I mean... kinda? We would *try* to infer the metaclass prior to this diff, but we would immediately throw up our hands and bail out of it as soon as we detected a cycle. That feels like kind of the same thing to me 😄 but, I'll rewrite the comment :-)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2447 on 2024-11-08 21:56_

Yes. I suppose we could also try not caching this, and just recomputing it everywhere. I don't really have any idea how much the caching helps or hurts us here.

Codspeed's entirely neutral on this PR, so it at least doesn't seem to be leading to a significant regression when checking tomllib. But then tomllib doesn't really have any deep inheritance hierarchies (I don't think!). So it might not be the best benchmark for this specific issue.

---

_@AlexWaygood reviewed on 2024-11-08 21:56_

---

_@carljm reviewed on 2024-11-08 22:16_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2333 on 2024-11-08 22:16_

Ok, fair enough!

---

_@carljm reviewed on 2024-11-08 22:17_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2447 on 2024-11-08 22:17_

I think it's fine as is until/unless we have evidence it's an issue. 

---

_Merged by @AlexWaygood on 2024-11-08 22:17_

---

_Closed by @AlexWaygood on 2024-11-08 22:17_

---

_Branch deleted on 2024-11-08 22:17_

---
