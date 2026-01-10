```yaml
number: 16306
title: "[red-knot] Use arena-allocated association lists for narrowing constraints"
type: pull_request
state: merged
author: dcreager
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: dcreager/rework-bindings
created_at: 2025-02-21T15:30:21Z
updated_at: 2025-02-28T18:37:01Z
url: https://github.com/astral-sh/ruff/pull/16306
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Use arena-allocated association lists for narrowing constraints

---

_Pull request opened by @dcreager on 2025-02-21 15:30_

This PR adds an implementation of [association lists](https://en.wikipedia.org/wiki/Association_list), and uses them to replace the previous `BitSet`/`SmallVec` representation for narrowing constraints.

An association list is a linked list of key/value pairs. We additionally guarantee that the elements of an association list are sorted (by their keys), and that they do not contain any entries with duplicate keys.

Association lists have fallen out of favor in recent decades, since you often need operations that are inefficient on them. In particular, looking up a random element by index is O(n), just like a linked list; and looking up an element by key is also O(n), since you must do a linear scan of the list to find the matching element. Luckily we don't need either of those operations for narrowing constraints!

The typical implementation also suffers from poor cache locality and high memory allocation overhead, since individual list cells are typically allocated separately from the heap. We solve that last problem by storing the cells of an association list in an `IndexVec` arena.

---

_Comment by @codspeed-hq[bot] on 2025-02-21 15:35_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Frework-bindings)

### Merging #16306 will **improve performances by 5.6%**

<sub>Comparing <code>dcreager/rework-bindings</code> (c6d0c5a) with <code>main</code> (5c007db)</sub>



### Summary

`⚡ 1` improvements  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` red_knot_check_file[cold] `` | 88.9 ms | 84.2 ms | +5.6% |


---

_Comment by @github-actions[bot] on 2025-02-21 15:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `performance` added by @AlexWaygood on 2025-02-21 16:25_

---

_Label `red-knot` added by @AlexWaygood on 2025-02-21 16:25_

---

_Review comment by @MichaReiser on `crates/ruff_index/src/list.rs`:43 on 2025-02-21 20:29_

I wonder if it would make sense to use an arena crate here that avoids moving the vec when resizing and instead simply allocates a new vec, considering that we aren't interested in accessing all items as a slice

This is clever 

---

_@MichaReiser reviewed on 2025-02-21 20:29_

---

_@dcreager reviewed on 2025-02-21 20:58_

---

_Review comment by @dcreager on `crates/ruff_index/src/list.rs`:43 on 2025-02-21 20:58_

Oh that's an interesting idea!  I'd prefer to experiment on that with a separate future PR, since this is already giving a nice performance gain without bringing on an extra dependency

---

_Review comment by @dcreager on `crates/ruff_index/src/list.rs`:7 on 2025-02-21 21:06_

> key/value pairs

Technically we don't need full key/value pairs for narrowing constraints (note that the value type is `()`).  But I plan on using this in a follow-on PR to also replace the `SmallVec`s of live bindings and declarations in `SymbolState`, which will have both keys and values to store.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/narrowing_constraints.rs`:56 on 2025-02-21 21:09_

This might seem like an unnecessary extra wrapper, but I have hopes to move `is_positive` from `Constraint` to here, so I wanted to go ahead and have this type available as the place to move it to.  Also I think it adds clarity to the distinction between _constraints_ (the expressions in the source code), and _narrowing/visibility constraints_ (the two ways in which we're using those).

---

_@dcreager reviewed on 2025-02-21 21:09_

---

_Marked ready for review by @dcreager on 2025-02-21 21:09_

---

_Review requested from @carljm by @dcreager on 2025-02-21 21:09_

---

_Review requested from @AlexWaygood by @dcreager on 2025-02-21 21:09_

---

_Review requested from @sharkdp by @dcreager on 2025-02-21 21:09_

---

_Review comment by @carljm on `crates/ruff_index/src/list.rs`:30 on 2025-02-21 21:45_

This is great. One thing I noticed both in the PR summary and here is that we discuss limitations of this data structure that might mean it's not suitable for some tasks, and how this implementation mitigates some limitations, but we really don't discuss (apart from a brief mention of structural sharing) what the advantages of the data structure are (what kind of tasks _is_ it particularly well-suited for), or (and this might not belong in the this comment here, but does belong somewhere) why it is well-suited for narrowing constraints in the use-def map.

---

_Review comment by @carljm on `crates/ruff_index/src/list.rs`:50 on 2025-02-21 21:54_

This doesn't seem to be used in this PR, and I wonder if we should even provide it, given that if you need it this might not be the data structure for you?

Though IIRC maybe one of your future PRs will try using this for a case where we do sometimes need random access, but not too frequently and where cardinality shouldn't be too high, either? That's probably a good case for having it.

---

_Review comment by @carljm on `crates/ruff_index/src/list.rs`:149 on 2025-02-21 21:59_

Maybe add a comment here about the reason for the "unnecessary" Option wrapping? (It looks to me like the reason is because otherwise we'd have to wrap many/all call sites of this method in `Some(...)`, but I had to think and read code for a bit to reach that conclusion.)

---

_Review comment by @carljm on `crates/ruff_index/src/list.rs`:127 on 2025-02-21 22:07_

If we required values to implement `Eq`, then we could consider the case where the new key already exists in the list with the same value and no updates are needed, so we could avoid needlessly cloning all cells up to the matching key.

This would require an internal recursive `insert` that returns an extra bit of information ("did anything change") wrapped by the public `insert`.

No idea if this would be worth it in practice for our uses. I guess for the case where we know all values are equal (because they are all `()`) we can just as well use `insert_if_needed` instead.

---

_Review comment by @carljm on `crates/ruff_index/src/list.rs`:761 on 2025-02-21 22:14_

Might not be a terrible idea to include some unit tests of the data structure itself?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/narrowing_constraints.rs`:20 on 2025-02-21 22:39_

Similarly here I miss an explanation of not just why it's _ok_ to use an alist (because its disadvantages don't apply in this case), but what advantages it has that mean we would _want_ to.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/narrowing_constraints.rs`:11 on 2025-02-21 22:47_

I think there's a bit of a terminology problem here. In this one sentence (and throughout the comments in this file, and code changes throughout this PR) we now have two distinct uses of the term "constraint" to refer to either a) a single test expression which may constrain the type of some binding, or b) a list of test expressions ("clauses"), all of which constrain the type of a binding at a certain point in control flow.

I think we should make a clear decision about which one of these we will use the term "constraint" for, and stick to it. Prior to this PR, the intent was always that a "constraint" (represented by a Constraint) is a single test expression, and a binding at some point in control flow can be associated with multiple narrowing constraints, plural. I realize that's awkward because now you are introducing an arena which contains multiple sets of narrowing constraints, and you want to call _that_ "narrowing_constraints", which shifts all the plurals down one level :)

I'm not opposed to shifting our use of terms such that we say that "a narrowing constraint" (singular) is a list of constraining test expressions, or clauses, or predicates. But if we do that, I think we need to be consistent about it, and IMO that means renaming the `Constraint` ingredient and the `ScopedConstraintId` index to use some other term ("constraint clause" or "predicate"?)

(Open to doing this further rename pass as a separate PR, to minimize the need for re-review of this one.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/narrowing_constraints.rs`:44 on 2025-02-21 22:52_

```suggestion
/// Note that those [`Constraint`]s are stored in [their own per-scope
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/narrowing_constraints.rs`:68 on 2025-02-21 22:58_

This comment reads a bit like we intend to not store them in `UseDefMap` in future, or not have them be per-scope? But I'm not sure what the benefit would be; I don't think there would be any additional sharing permitted by not storing them per-scope, because a given constraint expression is only ever relevant in the scope in which it exists.

If you do think there would be advantages to consolidating these for an entire file, maybe we should spell that out here?

If that's not the intent, I suggest simply:

```suggestion
/// A collection of narrowing constraints for a given scope.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:38 on 2025-02-21 23:04_

👍 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/symbol.rs`:545 on 2025-02-21 23:06_

Coming back to the terminology problem: here we iterate over `narrowing_constraint`, and the iteration yields something we also call a `constraint`, which we then pass to a function named `infer_narrowing_constraint`.

---

_@carljm approved on 2025-02-21 23:24_

This is really fantastic!! I have literally no notes on the actual implementation strategy, just on comments and terminology :)

---

_@MichaReiser reviewed on 2025-02-22 08:03_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/narrowing_constraints.rs`:20 on 2025-02-22 08:03_

I agree with this. The part that makes reviewing this PR difficult without deep knowledge of narrowing constraints is why associated lists are a good fit for this particular use case. What are the access patterns, how is the list built? I do think that the arena does help to reduce heap allocations, I'm not necessarily sure if it actually does help improve cache locality. I think that very much depends on how likely it is that nodes from the same list are insert in close proximity and I find it difficult to know if that's the case. 

---

_@carljm reviewed on 2025-02-22 16:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/narrowing_constraints.rs`:20 on 2025-02-22 16:22_

I think part of it is that this data structure allows representing a list as a single u32 (without fragmenting allocation as you would have to to represent it as a single pointer). This makes cloning flow states a lot cheaper, because we replace what used to be (for each and every binding) a smallvec of four u32, with a single u32. 

---

_Comment by @Skylion007 on 2025-02-23 16:31_

As an aside, wow, an actual use case where linked lists are actually useful for once outside of memory management! And are actually more performant!

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/narrowing_constraints.rs`:54 on 2025-02-24 08:41_

The `#[inline]` attribute is mainly useful to inline function calls across crate boundaries (when LTO is disabled). 
I'd strongly suspect that LLVM inlines those calls even without the attribute being present. That's why I'd suggest removing them unless the benchmark show that they in fact do improve performance.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/narrowing_constraints.rs`:68 on 2025-02-24 08:43_

Just a comment why having IDs per scopes can be useful: It ensures that changes are local to the affected scopes and that the IDs from other scopes shouldn't change. This helps to reduce unnecessary salsa recomputations because the ids still compare equal. However, that assumes that the IDs are used in places where are aren't relying on the AST already.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/narrowing_constraints.rs`:71 on 2025-02-24 08:45_

Should we default the value (or is it the key?) to `()` in `ListStorage` and `ListBuilder` so that we can write
```suggestion
    lists: ListStorage<ScopedNarrowingConstraintId, ScopedNarrowingConstraintClause>,
```

---

_Review comment by @MichaReiser on `crates/ruff_index/src/list.rs`:18 on 2025-02-24 08:52_

I don't think that poor cache locality is fully solved by using an arena. A program that always inserts one element per list before inserting another element per list will lead to a similar level of fragementation but the arena now comes at the cost of an extra level of indirection. 

---

_Review comment by @MichaReiser on `crates/ruff_index/src/list.rs`:125 on 2025-02-24 08:55_

I'd suggest calling out in the documentation when and why a key and value gets cloned. I'd find this rather surprising by an `insert` operation. 

---

_Review comment by @MichaReiser on `crates/ruff_index/src/list.rs`:134 on 2025-02-24 08:57_

Should we instead have an `entry` method similar to rust's hash map entry? 

If not, then I suggest renaming the method to `insert_if_vaccant` to at least use the hash map terminology (*needed* is very unclear about what it means. I expected that the method does some extra merging other than just dropping the values if the keys are missing)

---

_Review comment by @MichaReiser on `crates/ruff_index/src/list.rs`:192 on 2025-02-24 08:58_

Using recursion here could be problematic for long lists. Could we rewrite the function to use an iterator/loop instead?

---

_Review comment by @MichaReiser on `crates/ruff_index/src/list.rs`:217 on 2025-02-24 08:59_

Same as for intersection. Recursion is problematic for long lists.

---

_@MichaReiser approved on 2025-02-24 08:59_

---

_@dcreager reviewed on 2025-02-24 15:59_

---

_Review comment by @dcreager on `crates/ruff_index/src/list.rs`:761 on 2025-02-24 15:59_

Details details... :joy: 

(j/k of course, yes that's a good idea)

---

_Review comment by @dcreager on `crates/ruff_index/src/list.rs`:127 on 2025-02-24 20:26_

Good idea, done!  @MichaReiser's suggestion to add an entry API gave us the internal helper function that you mention

---

_Review comment by @dcreager on `crates/ruff_index/src/list.rs`:134 on 2025-02-24 20:26_

Done!  That let me get rid of some of the copy/pasta in the insert methods

---

_Review comment by @dcreager on `crates/ruff_index/src/list.rs`:192 on 2025-02-24 20:28_

Done.  Note that this required adding a scratch-space accumulator to the builder.  (Implementing these operations iteratively reverses the list, so we do that into a scratch vec, and which we then pop new entries off of in reverse to maintain the correct ordering)

---

_Review comment by @dcreager on `crates/ruff_index/src/list.rs`:217 on 2025-02-24 20:28_

Done

---

_Review comment by @dcreager on `crates/ruff_index/src/list.rs`:761 on 2025-02-24 20:28_

Done

---

_Review comment by @dcreager on `crates/ruff_index/src/list.rs`:149 on 2025-02-24 20:33_

Done

---

_Review comment by @dcreager on `crates/ruff_index/src/list.rs`:125 on 2025-02-24 20:35_

Done

---

_Review comment by @dcreager on `crates/ruff_index/src/list.rs`:18 on 2025-02-24 20:39_

To make sure I understand: you're talking about a list whose cells don't appear consecutively in the arena, is that right?  I think you're right that that wouldn't give us _ideal_ cache locality, but it should still be better than heap-allocated cells, since all of the cells are still in a single contiguous region of memory.

If you're inserting elements into two different lists in an interleaving pattern, then the cells of those two lists will be interleaved, but I think iterating over either of them would still be faster — you'd just have a stride of 2 instead of 1.  (Actually -2 instead of -1, since the list cells will end up in the arena in reverse order)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/narrowing_constraints.rs`:11 on 2025-02-24 20:41_

> and you want to call _that_ "narrowing_constraints", which shifts all the plurals down one level

Yes I agree with this completely.  In the commit history I think you can see the point where I realized I needed to name something `narrowing_constraintses` :joy: 

> I'm not opposed to shifting our use of terms such that we say that "a narrowing constraint" (singular) is a list of constraining test expressions, or clauses, or predicates. But if we do that, I think we need to be consistent about it, and IMO that means renaming the `Constraint` ingredient and the `ScopedConstraintId` index to use some other term ("constraint clause" or "predicate"?)
> 
> (Open to doing this further rename pass as a separate PR, to minimize the need for re-review of this one.)

I like this suggestion of `Constraint` → `Predicate` — that would allow us to use them for things other than "constraining" in the future, should we need to.  (Or maybe put better, the name would suggest or remind us that we could do that)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/narrowing_constraints.rs`:68 on 2025-02-24 20:46_

> If that's not the intent, I suggest simply:

Nope, that was not the intent.  Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/narrowing_constraints.rs`:44 on 2025-02-24 20:46_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/narrowing_constraints.rs`:54 on 2025-02-24 20:46_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/narrowing_constraints.rs`:71 on 2025-02-24 21:07_

Done.  I also added several helper methods (that are only available when `V = ()`) that make it easier to work with set-like alists.

---

_Review comment by @dcreager on `crates/ruff_index/src/list.rs`:30 on 2025-02-24 21:52_

Added a bit more text about this, lmkwyt

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/narrowing_constraints.rs`:20 on 2025-02-24 21:55_

I added some more text here.  Carl is right about the single-u32 representation being a benefit.  Also, if you insert elements in order, then insertion (of each element) is constant time.  And merging is very fast, since zipping through two linked lists is fast.  (That is, the alist representing gives us a much faster implementation for `SymbolState::merge`)

---

_Review comment by @dcreager on `crates/ruff_index/src/list.rs`:50 on 2025-02-24 21:57_

Yes, that's a good point... it's also not currently being tested.  I went ahead and removed it, since I can add it back as part of that experiment to see if this will be fast enough for the per-symbol lists too.

---

_@dcreager reviewed on 2025-02-24 21:59_

---

_@dcreager reviewed on 2025-02-24 22:04_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/symbol.rs`:546 on 2025-02-24 22:04_

This was the easiest way to un-reverse the narrowing constraints.  I had tried adding a `reverse` to `IntersectionBuilder`, but that ran into problems where there were nested intersections — multiple narrowing constraint clauses some of which are themselves intersections.  It would end up reversing the combined result, when you really only want to (un)reverse the outer intersection.  In local testing, this didn't seem to affect performance even though we're collecting into a vec.  (Presumably because all of the type builders are themselves allocating a fair bit)

---

_Review comment by @carljm on `crates/ruff_index/src/list.rs`:56 on 2025-02-24 22:22_

Doctest doesn't seem to be listening to you here...

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/symbol.rs`:546 on 2025-02-24 22:26_

I think the previous version here was aiming to avoid the allocation for this collect? But benchmark suggests it's fine.

---

_Review comment by @carljm on `crates/ruff_index/src/list.rs`:37 on 2025-02-24 22:27_

```suggestion
/// - You should construct lists in key order; doing this lets you insert each value in constant time.
```

---

_@carljm approved on 2025-02-24 22:46_

---

_Review comment by @dcreager on `crates/ruff_index/src/list.rs`:56 on 2025-02-25 15:23_

Should be `ignore`, not `no_run`!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/symbol.rs`:546 on 2025-02-25 15:24_

Yes that's right, see https://github.com/astral-sh/ruff/pull/16306/files#r1968481090.  (Two ~ships~ comments passing in the night!)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/symbol.rs`:546 on 2025-02-25 15:27_

And to clarify, in a previous iteration of this PR, we were using cons lists, which meant that the iterator was returning elements in order, and so we didn't need to worry about reversing anything here.

But that meant that you needed to _construct_ lists in reverse order to get the best efficiency, and it seemed easier to figure out how to reverse the output here instead of how to process the constraints in reverse order during semantic index building.

(Also, these, er, constraints are all mentioned in the doc comments)

---

_@dcreager reviewed on 2025-02-25 15:28_

---

_@carljm reviewed on 2025-02-25 15:42_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/symbol.rs`:546 on 2025-02-25 15:42_

I guess one last question here. If I'm understanding all this right, it suggests that in the previous version of this diff, we were constructing our lists in the most inefficient order (because we were doing a forward traversal of the IDs and constructing in-order, but using cons lists internally), so our construction was quadratic? But now it's linear.

And yet this version of the PR has roughly the same performance impact as the previous version (around 5%). Does this mostly suggest that our constraint lists tend to be so small that quadratic vs linear doesn't even really show up?

---

_@dcreager reviewed on 2025-02-25 15:53_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/symbol.rs`:546 on 2025-02-25 15:53_

> And yet this version of the PR has roughly the same performance impact as the previous version (around 5%). Does this mostly suggest that our constraint lists tend to be so small that quadratic vs linear doesn't even really show up?

Yes, I think that's the right conclusion.  I was testing locally on black, which I thought would be large enough for that quadratic difference to show up.  I haven't rebased #16311 onto this PR's updates yet, but my (completely unsubstantiated) hope is _that's_ where we were seeing the performance difference.  Since it seems more possible that black and tomllib have larger lists of live bindings than lists of active narrowing constraints.

---

_@dcreager reviewed on 2025-02-25 15:53_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/symbol.rs`:546 on 2025-02-25 15:53_

(I had added some printfs to verify that we are indeed visiting constraints in numeric order, and that we do produce a much smaller number of total list cells with the snoc list change)

---

_Merged by @dcreager on 2025-02-25 15:58_

---

_Closed by @dcreager on 2025-02-25 15:58_

---

_Branch deleted on 2025-02-25 15:59_

---

_@MichaReiser reviewed on 2025-02-28 18:37_

---

_Review comment by @MichaReiser on `crates/ruff_index/src/list.rs`:43 on 2025-02-28 18:37_

I tried this and... it's very disappointing haha :D It shows no difference locally.


```patch
Subject: [PATCH] Change `iterate` to return a `Result`
---
Index: crates/ruff_index/src/slice.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_index/src/slice.rs b/crates/ruff_index/src/slice.rs
--- a/crates/ruff_index/src/slice.rs	(revision b04413079859fd265817c67f184ba7f86e1e92a3)
+++ b/crates/ruff_index/src/slice.rs	(date 1740767606877)
@@ -50,6 +50,11 @@
         self.raw.len()
     }
 
+    #[inline]
+    pub const fn capacity(&self) -> usize {
+        self.raw.len()
+    }
+
     #[inline]
     pub const fn is_empty(&self) -> bool {
         self.raw.is_empty()
Index: crates/red_knot_python_semantic/src/list.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/red_knot_python_semantic/src/list.rs b/crates/red_knot_python_semantic/src/list.rs
--- a/crates/red_knot_python_semantic/src/list.rs	(revision b04413079859fd265817c67f184ba7f86e1e92a3)
+++ b/crates/red_knot_python_semantic/src/list.rs	(date 1740767857167)
@@ -61,6 +61,7 @@
 //!
 //! [alist]: https://en.wikipedia.org/wiki/Association_list
 
+use std::cell::RefCell;
 use std::cmp::Ordering;
 use std::marker::PhantomData;
 use std::ops::Deref;
@@ -69,7 +70,7 @@
 
 /// A handle to an association list. Use [`ListStorage`] to access its elements, and
 /// [`ListBuilder`] to construct other lists based on this one.
-#[derive(Clone, Copy, Debug, Eq, Ord, PartialEq, PartialOrd)]
+#[derive(Clone, Copy, Debug, Eq, PartialEq)]
 pub(crate) struct List<K, V = ()> {
     last: Option<ListCellId>,
     _phantom: PhantomData<(K, V)>,
@@ -95,14 +96,82 @@
 }
 
 #[newtype_index]
-#[derive(PartialOrd, Ord)]
+// #[derive(PartialOrd, Ord)]
 struct ListCellId;
 
 /// Stores one or more association lists. This type provides read-only access to the lists.  Use a
 /// [`ListBuilder`] to create lists.
 #[derive(Debug, Eq, PartialEq)]
 pub(crate) struct ListStorage<K, V = ()> {
-    cells: IndexVec<ListCellId, ListCell<K, V>>,
+    cells: ListChunk<K, V>,
+}
+
+impl<K, V> ListStorage<K, V> {
+    fn new() -> Self {
+        Self {
+            cells: ListChunk {
+                current: Vec::with_capacity(16),
+                rest: Vec::new(),
+            },
+        }
+    }
+
+    fn push(&mut self, cell: ListCell<K, V>) -> ListCellId {
+        self.push_fast(cell)
+            .unwrap_or_else(|cell| self.push_slow(cell))
+    }
+
+    #[inline]
+    fn push_fast(&mut self, cell: ListCell<K, V>) -> Result<ListCellId, ListCell<K, V>> {
+        let len = self.cells.current.len();
+
+        if len < self.cells.current.capacity() {
+            let list_len = self.cells.current.len();
+            self.cells.current.push(cell);
+            let chunk = self.cells.rest.len() << 28;
+            Ok(ListCellId::from_usize(list_len | chunk))
+        } else {
+            Err(cell)
+        }
+    }
+
+    fn push_slow(&mut self, cell: ListCell<K, V>) -> ListCellId {
+        let mut next_vec = Vec::with_capacity(self.cells.current.capacity() * 2);
+        next_vec.push(cell);
+
+        let current = std::mem::replace(&mut self.cells.current, next_vec);
+        self.cells.rest.push(current);
+
+        let chunk = self.cells.rest.len() << 28;
+        ListCellId::from_usize(chunk)
+    }
+}
+
+impl<K, V> std::ops::Index<ListCellId> for ListStorage<K, V> {
+    type Output = ListCell<K, V>;
+
+    fn index(&self, index: ListCellId) -> &Self::Output {
+        let offset = index.as_u32();
+
+        let chunk = offset >> 28;
+        let list_id = offset & !(0b1111 << 28);
+        let list = self
+            .cells
+            .rest
+            .get(chunk as usize)
+            .unwrap_or(&self.cells.current);
+
+        &list[list_id as usize]
+    }
+}
+
+#[newtype_index]
+struct ChunkId;
+
+#[derive(Debug, Eq, PartialEq, Default)]
+struct ListChunk<K, V = ()> {
+    current: Vec<ListCell<K, V>>,
+    rest: Vec<Vec<ListCell<K, V>>>,
 }
 
 /// Each association list is represented by a sequence of snoc cells. A snoc cell is like the more
@@ -151,9 +220,7 @@
 impl<K, V> Default for ListBuilder<K, V> {
     fn default() -> Self {
         ListBuilder {
-            storage: ListStorage {
-                cells: IndexVec::default(),
-            },
+            storage: ListStorage::new(),
             scratch: Vec::default(),
         }
     }
@@ -170,7 +237,8 @@
     /// Finalizes a `ListBuilder`. After calling this, you cannot create any new lists managed by
     /// this storage.
     pub(crate) fn build(mut self) -> ListStorage<K, V> {
-        self.storage.cells.shrink_to_fit();
+        self.storage.cells.current.shrink_to_fit();
+        self.storage.cells.rest.shrink_to_fit();
         self.storage
     }
 
@@ -182,7 +250,7 @@
     /// list.
     #[allow(clippy::unnecessary_wraps)]
     fn add_cell(&mut self, rest: Option<ListCellId>, key: K, value: V) -> Option<ListCellId> {
-        Some(self.storage.cells.push(ListCell { rest, key, value }))
+        Some(self.storage.push(ListCell { rest, key, value }))
     }
 
     /// Returns an entry pointing at where `key` would be inserted into a list.
@@ -210,7 +278,7 @@
         // (and any succeeding keys) onto.
         let mut curr = list.last;
         while let Some(curr_id) = curr {
-            let cell = &self.storage.cells[curr_id];
+            let cell = &self.storage[curr_id];
             match key.cmp(&cell.key) {
                 // We found an existing entry in the input list with the desired key.
                 Ordering::Equal => {
@@ -339,8 +407,8 @@
         let mut a = a.last;
         let mut b = b.last;
         while let (Some(a_id), Some(b_id)) = (a, b) {
-            let a_cell = &self.storage.cells[a_id];
-            let b_cell = &self.storage.cells[b_id];
+            let a_cell = &self.storage[a_id];
+            let b_cell = &self.storage[b_id];
             match a_cell.key.cmp(&b_cell.key) {
                 // Both lists contain this key; combine their values
                 Ordering::Equal => {
@@ -390,7 +458,7 @@
     type Item = &'a K;
 
     fn next(&mut self) -> Option<Self::Item> {
-        let cell = &self.storage.cells[self.curr?];
+        let cell = &self.storage[self.curr?];
         self.curr = cell.rest;
         Some(&cell.key)
     }
@@ -531,7 +599,7 @@
         type Item = (&'a K, &'a V);
 
         fn next(&mut self) -> Option<Self::Item> {
-            let cell = &self.storage.cells[self.curr?];
+            let cell = &self.storage[self.curr?];
             self.curr = cell.rest;
             Some((&cell.key, &cell.value))
         }

```

I suspect that our lists just aren't big enough for it to matter

---
