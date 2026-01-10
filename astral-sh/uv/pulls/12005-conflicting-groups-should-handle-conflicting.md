```yaml
number: 12005
title: Conflicting groups should handle conflicting inclusions automatically
type: pull_request
state: merged
author: jtfmumm
labels: []
assignees: []
merged: true
base: main
head: jtfm/conflicting-groups
created_at: 2025-03-06T14:00:44Z
updated_at: 2025-03-08T18:21:27Z
url: https://github.com/astral-sh/uv/pull/12005
synced_at: 2026-01-10T11:10:39Z
```

# Conflicting groups should handle conflicting inclusions automatically

---

_Pull request opened by @jtfmumm on 2025-03-06 14:00_

This adds support for inferring dependency group conflict sets from the directly defined conflicts in configuration. For example, if you declare a conflict between groups `alpha` and `beta` and `dev` includes `beta`, then we will infer a conflict between `dev` and `alpha`. We will also handle a conflict between two groups if they transitively include groups that conflict with each other. See #11232 for more details.  

Closes #11232 


---

_Review requested from @BurntSushi by @jtfmumm on 2025-03-06 14:00_

---

_@jtfmumm reviewed on 2025-03-06 14:06_

---

_Review comment by @jtfmumm on `crates/uv-pypi-types/src/dependency_groups.rs`:1 on 2025-03-06 14:06_

I've moved this file so that I can import `DependencyGroups` in `conflicts.rs`. It would have caused a circular dependency in `uv-workspace`. 

---

_@jtfmumm reviewed on 2025-03-06 14:07_

---

_Review comment by @jtfmumm on `crates/uv-pypi-types/src/conflicts.rs`:154 on 2025-03-06 14:07_

Can we assume cycle detection before this point? Otherwise, we'll have to return an error

---

_@jtfmumm reviewed on 2025-03-06 14:36_

---

_Review comment by @jtfmumm on `crates/uv-pypi-types/src/conflicts.rs`:154 on 2025-03-06 14:36_

There's a failing test that wasn't running locally for some reason that shows we can't assume it at this point. I'll update.

---

_@jtfmumm reviewed on 2025-03-06 14:51_

---

_Review comment by @jtfmumm on `crates/uv-pypi-types/src/conflicts.rs`:154 on 2025-03-06 14:51_

What I've done now is to just return if there's a `Cycle` error on `toposort`. I've left a `FIXME` comment indicating  that we're currently bailing and allowing more detailed include cycle detection downstream. Is this the behavior we want? If so, I'll remove the `FIXME` tag.

---

_Review comment by @BurntSushi on `crates/uv-pypi-types/Cargo.toml`:37 on 2025-03-06 15:25_

How come this was made optional? I'm kinda wondering how this works at all, since I don't see any features enabling `serde` in this crate? I guess feature unification or something?

---

_Review comment by @BurntSushi on `crates/uv-pypi-types/src/conflicts.rs`:261 on 2025-03-06 15:34_

It looks like this was done?

---

_Review comment by @BurntSushi on `crates/uv/tests/it/sync.rs`:8309 on 2025-03-06 15:39_

Nice tests!

---

_Review comment by @BurntSushi on `crates/uv/tests/it/sync.rs`:8309 on 2025-03-06 15:39_

Should we add a test that covers cycle detection?

---

_Review comment by @BurntSushi on `crates/uv-pypi-types/src/conflicts.rs`:111 on 2025-03-06 15:47_

I think you can if `if seen.insert(group) {` here. And then remove the `seen.insert(group)` below. Or even better, use an early-continue:

```rust
if !seen.insert(group) {
    continue;
}
```

---

_Review comment by @BurntSushi on `crates/uv-pypi-types/src/conflicts.rs`:126 on 2025-03-06 15:48_

I would do (same as above):

```rust
if !seen.insert(group) {
    continue;
}
```

---

_Review comment by @BurntSushi on `crates/uv-pypi-types/src/conflicts.rs`:154 on 2025-03-06 15:52_

Hmmm cc @zanieb 

One thing that strikes me here is that the cycle detection error would only kick in when a relevant conflicting group is declared? What happens if there aren't any conflicts declared? Is there cycle detection happening somewhere else?

---

_Review comment by @BurntSushi on `crates/uv-pypi-types/src/conflicts.rs`:282 on 2025-03-06 15:54_

Does this need to be public?

---

_@BurntSushi reviewed on 2025-03-06 15:58_

Nice work!

The failing `duplicate_torch_and_sympy_because_of_wrong_inferences` is somewhat worrisome, because that doesn't use dependency groups at all. So in theory, it shouldn't be impacted by the work here?

In your code, I wonder if something wonky is happening when no `DependencyGroupSpecifier::IncludeGroup` are found and thus no edges are added?

---

_@jtfmumm reviewed on 2025-03-06 16:25_

---

_Review comment by @jtfmumm on `crates/uv-pypi-types/Cargo.toml`:37 on 2025-03-06 16:25_

It's because of moving `DependencyGroups` into this crate in order to depend on it in `conflicts.rs`. It looks like my comment on that file disappeared from this PR.

---

_@jtfmumm reviewed on 2025-03-06 16:27_

---

_Review comment by @jtfmumm on `crates/uv-pypi-types/src/conflicts.rs`:282 on 2025-03-06 16:27_

Nope, it's being called from a newtype. Updated

---

_@jtfmumm reviewed on 2025-03-06 16:27_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/sync.rs`:8309 on 2025-03-06 16:27_

Good idea. Added one!

---

_@jtfmumm reviewed on 2025-03-06 16:31_

---

_Review comment by @jtfmumm on `crates/uv-pypi-types/src/conflicts.rs`:154 on 2025-03-06 16:31_

Cycle detection is happening somewhere, maybe in the resolver? But that happens after this code is called. With this new version where I just return (no error) when `toposort()` returns an error, then only the original conflict sets are sent along. 

But it does seem weird to silently swallow the error. The reason I don't want it propagated is because better cycle detection happens elsewhere ("there was a cycle" vs. "there was a cycle: dev -> test -> dev"). So I could either warn the user in this method or match at the caller and warn there. Do either of those options make sense?

---

_Comment by @jtfmumm on 2025-03-06 16:36_

> The failing duplicate_torch_and_sympy_because_of_wrong_inferences is somewhat worrisome, because that doesn't use dependency groups at all. So in theory, it shouldn't be impacted by the work here?

Good point. I'll investigate it further tomorrow.

---

_@BurntSushi reviewed on 2025-03-06 16:37_

---

_Review comment by @BurntSushi on `crates/uv-pypi-types/Cargo.toml`:37 on 2025-03-06 16:37_

Does it have to be optional? (I'm overall kinda skeptical of even having optional dependencies in our crates unless there is a very specific need for them. In practice, in most cases, all of the features end up enabled through feature unification.)

---

_@BurntSushi reviewed on 2025-03-06 16:39_

---

_Review comment by @BurntSushi on `crates/uv-pypi-types/src/conflicts.rs`:154 on 2025-03-06 16:39_

I think what you're doing now is probably reasonable with a comment explaining what's going on here (basically, what you just said). @zanieb might have a different opinion though!

---

_@charliermarsh reviewed on 2025-03-06 16:42_

---

_Review comment by @charliermarsh on `crates/uv-pypi-types/Cargo.toml`:37 on 2025-03-06 16:42_

(In general I think it's fine -- it's a pattern we use for Serde and Schemars, so I have no objection to propagating it. We should revisit elsewhere if we want to move away from it.)

---

_Review comment by @charliermarsh on `crates/uv-pypi-types/Cargo.toml`:37 on 2025-03-06 16:42_

I am slightly confused about why it was not optional before, but is optional now. Maybe that's Andrew's question.

---

_@charliermarsh reviewed on 2025-03-06 16:42_

---

_@charliermarsh reviewed on 2025-03-06 16:43_

---

_Review comment by @charliermarsh on `crates/uv-pypi-types/src/dependency_groups.rs`:8 on 2025-03-06 16:43_

Did you consider putting this file-move into a separate PR? It looks separable from the rest of the changes, and would be easy to merge on its own.

---

_@charliermarsh reviewed on 2025-03-06 16:45_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock_conflict.rs`:2144 on 2025-03-06 16:45_

Do we know where these changes are coming from? Is it an Insta version mismatch on the team? Would be good to understand prior to merging so that they don't oscillate.

---

_@charliermarsh reviewed on 2025-03-06 16:46_

---

_Review comment by @charliermarsh on `crates/uv-pypi-types/src/conflicts.rs`:223 on 2025-03-06 16:46_

I think this field would be written out in the `Serialize` representation, is that intentional? (Do you know where this is serialized?)

---

_@charliermarsh reviewed on 2025-03-06 16:48_

---

_Review comment by @charliermarsh on `crates/uv-pypi-types/src/conflicts.rs`:103 on 2025-03-06 16:48_

Did you consider `let mut seen = FxHashSet::<&GroupName>::default();`?

---

_@charliermarsh reviewed on 2025-03-06 16:49_

---

_Review comment by @charliermarsh on `crates/uv-pypi-types/src/conflicts.rs`:103 on 2025-03-06 16:49_

(I think the type can be omitted entirely, but if you want to declare it, this seems simpler. Or at least `: FxHashSet<&GroupName>` to match the syntax you used above.)

---

_Review comment by @jtfmumm on `crates/uv-pypi-types/Cargo.toml`:37 on 2025-03-06 16:50_

The superficial reason is because the compiler complained when I moved over `dependency-groups.rs`. But I think it might be because of the fact that `DependencyGroups` is used as a field on a struct with a feature flag in `pyproject.rs`.

---

_@jtfmumm reviewed on 2025-03-06 16:50_

---

_@jtfmumm reviewed on 2025-03-06 16:51_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/lock_conflict.rs`:2144 on 2025-03-06 16:51_

These are from `cargo insta review`

---

_@jtfmumm reviewed on 2025-03-06 16:52_

---

_Review comment by @jtfmumm on `crates/uv-pypi-types/src/dependency_groups.rs`:8 on 2025-03-06 16:52_

That's true. Would you rather I create that PR now, or just for the future?

---

_Review comment by @charliermarsh on `crates/uv-pypi-types/src/conflicts.rs`:105 on 2025-03-06 16:54_

You can remove a couple clones from this method:
```diff
diff --git a/crates/uv-pypi-types/src/conflicts.rs b/crates/uv-pypi-types/src/conflicts.rs
index 61233cee4..05d23eaff 100644
--- a/crates/uv-pypi-types/src/conflicts.rs
+++ b/crates/uv-pypi-types/src/conflicts.rs
@@ -85,7 +85,7 @@ impl Conflicts {
         groups: &DependencyGroups,
     ) {
         let mut graph = DiGraph::new();
-        let mut group_node_idxs: FxHashMap<GroupName, NodeIndex> = FxHashMap::default();
+        let mut group_node_idxs: FxHashMap<&GroupName, NodeIndex> = FxHashMap::default();
         let mut node_conflict_items: FxHashMap<NodeIndex, Rc<ConflictItem>> = FxHashMap::default();
         // Used for transitively deriving new conflict sets with substitutions.
         // The keys are canonical items (mentioned directly in configured conflicts).
@@ -94,15 +94,14 @@ impl Conflicts {
             FxHashMap::default();
 
         // Conflict sets that were directly defined in configuration.
-        let mut direct_conflict_sets: FxHashSet<ConflictSet> = FxHashSet::default();
+        let mut direct_conflict_sets: FxHashSet<&ConflictSet> = FxHashSet::default();
         // Conflict sets that we will transitively infer in this method.
         let mut transitive_conflict_sets: FxHashSet<ConflictSet> = FxHashSet::default();
 
         // Add groups in directly defined conflict sets to the graph.
-        let mut seen: std::collections::HashSet<&GroupName, rustc_hash::FxBuildHasher> =
-            FxHashSet::default();
+        let mut seen = FxHashSet::default();
         for set in &self.0 {
-            direct_conflict_sets.insert(set.clone());
+            direct_conflict_sets.insert(set);
             for item in set.iter() {
                 let ConflictPackage::Group(group) = &item.conflict else {
                     // TODO: Do we also want to handle extras here?
@@ -115,8 +114,8 @@ impl Conflicts {
                 let mut canonical_items = FxHashSet::default();
                 canonical_items.insert(item.clone());
                 let node_id = graph.add_node(canonical_items);
-                group_node_idxs.insert(group.clone(), node_id);
-                node_conflict_items.insert(node_id, item.clone());
+                group_node_idxs.insert(group, node_id);
+                node_conflict_items.insert(node_id, item);
             }
         }
 
@@ -130,7 +129,7 @@ impl Conflicts {
                 conflict: ConflictPackage::Group(group.clone()),
             };
             let node_id = graph.add_node(FxHashSet::default());
-            group_node_idxs.insert(group.clone(), node_id);
+            group_node_idxs.insert(group, node_id);
             node_conflict_items.insert(node_id, Rc::new(group_conflict_item));
         }
 
@@ -185,6 +184,7 @@ impl Conflicts {
             let mut new_conflict_sets = FxHashSet::default();
             for conflict_set in direct_conflict_sets
                 .iter()
+                .copied()
                 .chain(transitive_conflict_sets.iter())
                 .filter(|set| set.contains_item(&canonical_item))
             {
```

---

_@charliermarsh reviewed on 2025-03-06 16:54_

---

_@charliermarsh reviewed on 2025-03-06 16:54_

---

_Review comment by @charliermarsh on `crates/uv-pypi-types/src/conflicts.rs`:108 on 2025-03-06 16:54_

Nit: `TODO(john)` please just to match style elsewhere.

---

_Review comment by @jtfmumm on `crates/uv-pypi-types/src/conflicts.rs`:103 on 2025-03-06 16:58_

Yeah that's weird. I must have copied that in from a compiler error while quickly fixing something early on and missed it during my own review.

---

_@jtfmumm reviewed on 2025-03-06 16:59_

---

_Comment by @charliermarsh on 2025-03-06 17:00_

I think the `duplicate_torch_and_sympy_because_of_wrong_inferences` changes look okay... The main thing I was concerned by was:

```diff
 2955       │-    { name = "pyparsing" },
       2915 │+    { name = "pyparsing", version = "2.4.7", source = { registry = "https://pypi.org/simple" }, marker = "extra == 'extra-4-test-alignn' or (extra == 'extra-4-test-all' and extra == 'extra-4-test-m3gnet') or (extra == 'extra-4-test-chgnet' and extra == 'extra-4-test-m3gnet')" },
       2916 │+    { name = "pyparsing", version = "3.2.1", source = { registry = "https://pypi.org/simple" }, marker = "extra == 'extra-4-test-all' or extra == 'extra-4-test-chgnet' or extra != 'extra-4-test-alignn'" },
```

We want to make sure that those markers cover the entire domain, and that they're not overlapping. The `extra != 'extra-4-test-alignn'` and `extra == 'extra-4-test-alignn'` ensure they cover the entire domain, and then the rest of the clauses are covered by conflicts, I think.

---

_@jtfmumm reviewed on 2025-03-06 17:00_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/lock_conflict.rs`:2144 on 2025-03-06 17:00_

My version is `1.41.1`, which isn't the latest.

---

_Comment by @BurntSushi on 2025-03-06 17:24_

> I think the `duplicate_torch_and_sympy_because_of_wrong_inferences` changes look okay... The main thing I was concerned by was:
> 
> ```diff
>  2955       │-    { name = "pyparsing" },
>        2915 │+    { name = "pyparsing", version = "2.4.7", source = { registry = "https://pypi.org/simple" }, marker = "extra == 'extra-4-test-alignn' or (extra == 'extra-4-test-all' and extra == 'extra-4-test-m3gnet') or (extra == 'extra-4-test-chgnet' and extra == 'extra-4-test-m3gnet')" },
>        2916 │+    { name = "pyparsing", version = "3.2.1", source = { registry = "https://pypi.org/simple" }, marker = "extra == 'extra-4-test-all' or extra == 'extra-4-test-chgnet' or extra != 'extra-4-test-alignn'" },
> ```
> 
> We want to make sure that those markers cover the entire domain, and that they're not overlapping. The `extra != 'extra-4-test-alignn'` and `extra == 'extra-4-test-alignn'` ensure they cover the entire domain, and then the rest of the clauses are covered by conflicts, I think.

Shouldn't there be no changes at all since that test doesn't use dependency groups (or the include mechanism)?

---

_Comment by @charliermarsh on 2025-03-06 17:37_

Hmm yeah, probably not. Looks like the order of the conflicts changed:

```diff
@@ -10225,14 +10225,14 @@ fn duplicate_torch_and_sympy_because_of_wrong_inferences() -> Result<()> {
             "python_full_version < '3.11' and extra != 'extra-4-test-alignn' and extra != 'extra-4-test-all' and extra != 'extra-4-test-chgnet' and extra != 'extra-4-test-m3gnet'",
         ]
         conflicts = [[
-            { package = "test", extra = "chgnet" },
             { package = "test", extra = "alignn" },
+            { package = "test", extra = "chgnet" },
         ], [
             { package = "test", extra = "chgnet" },
             { package = "test", extra = "m3gnet" },
         ], [
-            { package = "test", extra = "all" },
             { package = "test", extra = "alignn" },
+            { package = "test", extra = "all" },
         ], [
             { package = "test", extra = "all" },
             { package = "test", extra = "m3gnet" },
```

---

_@zanieb reviewed on 2025-03-06 17:54_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock_conflict.rs`:2144 on 2025-03-06 17:54_

Yes it's an insta version thing. Old versions used `###` / new versions use `#` when they can, I think.

---

_Comment by @jtfmumm on 2025-03-06 17:57_

For the transitive conflict expansion algorithm, I updated `ConflictSet` to use a `BTreeSet` so that they could be inserted into sets. That's why the order is now alphabetical. I didn't expect the order within a `ConflictSet` to matter during resolution though (and I didn't think users would expect that when defining conflicts).

---

_@zanieb reviewed on 2025-03-06 18:03_

---

_Review comment by @zanieb on `crates/uv-pypi-types/src/conflicts.rs`:154 on 2025-03-06 18:03_

I don't know what our current behavior for cycles is. Are we talking about cycles in inclusions or cycles in the conflicts?

1. If `dev` includes `test` and `test` includes `dev` don't we have a consistent set of dependencies? (just the union of `test` and `dev`?) 

2. Similarly, if `test` is marked as conflicting with some other group `foo` then `dev` conflicts with `foo` too, the cycle doesn't matter here.

3. If `test` is marked as conflicting with `dev`, now the cycle is a problem because... they include each other and can't conflict. This applies to longer chains too, e.g., if we have `test ->  foo -> dev -> test` a conflict between `test` and `dev` is "invalid".

If we don't have test coverage for each of these cases, we should add it. In reply to

> One thing that strikes me here is that the cycle detection error would only kick in when a relevant conflicting group is declared?

This seems fine, because this form of cycle (3) is the only one that creates an unresolvable situation?

---

_@charliermarsh reviewed on 2025-03-07 01:57_

---

_Review comment by @charliermarsh on `crates/uv-pypi-types/src/conflicts.rs`:154 on 2025-03-07 01:57_

Unless I'm misunderstanding, cycles in dependency groups are an error. Per the PEP:

> Dependency Group Includes MUST NOT include cycles, and tools SHOULD report an error if they detect a cycle.

I think we have an `DependencyGroupCycle` error.

---

_@charliermarsh reviewed on 2025-03-07 02:01_

---

_Review comment by @charliermarsh on `crates/uv-pypi-types/src/dependency_groups.rs`:8 on 2025-03-07 02:01_

It's up to you. I think it's easier to do it even in this case -- we can merge that PR immediately, it makes this one easier to review, etc.

---

_@jtfmumm reviewed on 2025-03-07 09:42_

---

_Review comment by @jtfmumm on `crates/uv-pypi-types/src/conflicts.rs`:154 on 2025-03-07 09:42_

Yeah, on `main` we definitely error on cycles. That's the behavior I'm relying on when just returning from the conflict expansion method when detecting a cycle during `toposort`.

---

_@jtfmumm reviewed on 2025-03-07 11:34_

---

_Review comment by @jtfmumm on `crates/uv-pypi-types/src/dependency_groups.rs`:8 on 2025-03-07 11:34_

Done. The other PR has been merged and this one simplified

---

_Review comment by @zanieb on `crates/uv-pypi-types/src/conflicts.rs`:154 on 2025-03-07 14:09_

Oh, thanks for the correction. Maybe I was just thinking about feedback I wrote on the PEP.

---

_@zanieb reviewed on 2025-03-07 14:09_

---

_Review comment by @jtfmumm on `crates/uv-pypi-types/src/conflicts.rs`:223 on 2025-03-07 14:38_

This is there because it was already on both `ConflictSet` and `Conflicts` on `main`. I couldn't find anywhere that used it, so I tried removing it from both and that didn't seem to break anything. 

---

_@jtfmumm reviewed on 2025-03-07 14:38_

---

_Review comment by @BurntSushi on `Cargo.lock`:6051 on 2025-03-07 16:11_

Do we know why the versions for `windows-sys` changed here?

---

_@BurntSushi reviewed on 2025-03-07 16:12_

---

_@jtfmumm reviewed on 2025-03-07 16:24_

---

_Review comment by @jtfmumm on `Cargo.lock`:6051 on 2025-03-07 16:24_

That's a good question. I'm not sure

---

_Review comment by @charliermarsh on `crates/uv-pypi-types/src/conflicts.rs`:286 on 2025-03-07 16:26_

Did you consider something like `with_inferred(self) -> Self` to avoid mutability?

---

_Review comment by @charliermarsh on `Cargo.lock`:6051 on 2025-03-07 16:27_

Yeah this is pretty strange. I'd recommend checking out `Cargo.lock` from main, running `cargo check`, and seeing if these get reverted.

---

_@charliermarsh approved on 2025-03-07 16:27_

Looks good from my perspective modulo the `Cargo.lock` changes which we should verify before merging.

---

_@jtfmumm reviewed on 2025-03-07 16:57_

---

_Review comment by @jtfmumm on `crates/uv-pypi-types/src/conflicts.rs`:286 on 2025-03-07 16:57_

Updated

---

_@zanieb reviewed on 2025-03-07 17:08_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:8308 on 2025-03-07 17:08_

I don't quite understand this error, are we just repeating the groups here at the end? What's the value of that?

---

_@jtfmumm reviewed on 2025-03-08 11:49_

---

_Review comment by @jtfmumm on `Cargo.lock`:6051 on 2025-03-08 11:49_

Resolved

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:8308 on 2025-03-08 14:40_

I believe this is identical to the existing error we show in these cases, except "transitively" has been inserted. Perhaps this should be hashed out separately from this PR, since it doesn't seem to be a change? But for reference, the set at the end can be larger than "just" these two items. The set at the end is of arbitrary size, and the groups we list at the start are the activated, conflicting members.


---

_@charliermarsh reviewed on 2025-03-08 14:40_

---

_@zanieb reviewed on 2025-03-08 14:43_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:8308 on 2025-03-08 14:43_

Oh, if it's not changing here that's fine and we can revisit it separately — I just looked at the test case not the implementation.

---

_@zanieb approved on 2025-03-08 14:48_

---

_Merged by @jtfmumm on 2025-03-08 18:21_

---

_Closed by @jtfmumm on 2025-03-08 18:21_

---

_Branch deleted on 2025-03-08 18:21_

---
