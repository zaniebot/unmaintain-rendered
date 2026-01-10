```yaml
number: 5898
title: Implement marker trees using algebraic decision diagrams
type: pull_request
state: merged
author: ibraheemdev
labels:
  - lock
assignees: []
merged: true
base: main
head: ibraheem/canonical-markers
created_at: 2024-08-07T23:55:54Z
updated_at: 2024-08-10T04:02:52Z
url: https://github.com/astral-sh/uv/pull/5898
synced_at: 2026-01-10T13:31:54Z
```

# Implement marker trees using algebraic decision diagrams

---

_Pull request opened by @ibraheemdev on 2024-08-07 23:55_

## Summary

This PR rewrites the `MarkerTree` type to use algebraic decision diagrams (ADD). This has many benefits:
- The diagram is canonical for a given marker function. It is impossible to create two functionally equivalent marker trees that don't refer to the same underlying ADD. This also means that any trivially true or unsatisfiable markers are represented by the same constants.
- The diagram can handle complex operations (conjunction/disjunction) in polynomial time, as well as constant-time negation.
- The diagram can be converted to a simplified DNF form for user-facing output.

The new representation gives us a lot more confidence in our marker operations and simplification, which is proving to be very important (see https://github.com/astral-sh/uv/pull/5733 and https://github.com/astral-sh/uv/pull/5163).

Unfortunately, it is not easy to split this PR into multiple commits because it is a large rewrite of the `marker` module. I'd suggest reading through the `marker/algebra.rs`, `marker/simplify.rs`, and `marker/tree.rs` files for the new implementation, as well as the updated snapshots to verify how the new simplification rules work in practice. However, a few other things were changed:
- [We now use release-only comparisons for `python_full_version`, where we previously only did for `python_version`](https://github.com/astral-sh/uv/blob/ibraheem/canonical-markers/crates/pep508-rs/src/marker/algebra.rs#L522). I'm unsure how marker operations should work in the presence of pre-release versions if we decide that this is incorrect.
- [Meaningless marker expressions are now ignored](https://github.com/astral-sh/uv/blob/ibraheem/canonical-markers/crates/pep508-rs/src/marker/parse.rs#L502). This means that a marker such as `'x' == 'x'` will always evaluate to `true` (as if the expression did not exist), whereas we previously treated this as always `false`. It's negation however, remains `false`. 
- [Unsatisfiable markers are written as `python_version < '0'`](https://github.com/astral-sh/uv/blob/ibraheem/canonical-markers/crates/pep508-rs/src/marker/tree.rs#L1329).
- The `PubGrubSpecifier` type has been moved to the new `uv-pubgrub` crate, shared by `pep508-rs` and `uv-resolver`. `pep508-rs` also depends on the `pubgrub` crate for the `Range` type, we probably want to move `pubgrub::Range` into a separate crate to break this, but I don't think that should block this PR (cc @konstin).

There is still some remaining work here that I decided to leave for now  for the sake of unblocking some of the related work on the resolver. 
- We still use `Option<MarkerTree>` throughout uv, which is unnecessary now that `MarkerTree::TRUE` is canonical.
- The `MarkerTree` type is now interned globally and can potentially implement `Copy`. However, it's unclear if we want to add more information to marker trees that would make it `!Copy`. For example, we may wish to attach extra and requires-python environment information to avoid simplifying after construction.
- We don't currently combine `python_full_version` and `python_version` markers.
- I also have not spent too much time investigating performance and there is probably some low-hanging fruit. Many of the test cases I did run actually saw large performance improvements due to the markers being simplified internally, reducing the stress on the old `normalize` routine, especially for the extremely large markers seen in `transformers` and other projects.

Resolves https://github.com/astral-sh/uv/issues/5660, https://github.com/astral-sh/uv/issues/5179.

---

_Label `lock` added by @ibraheemdev on 2024-08-07 23:55_

---

_Review requested from @BurntSushi by @ibraheemdev on 2024-08-07 23:55_

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-08-07 23:55_

---

_Review requested from @konstin by @ibraheemdev on 2024-08-07 23:55_

---

_Review comment by @BurntSushi on `crates/pep508-rs/Cargo.toml`:43 on 2024-08-08 16:30_

Should these go into the root `Cargo.toml` and then we can use `{ workspace = true }` here?

---

_Review comment by @BurntSushi on `crates/pep508-rs/Cargo.toml`:43 on 2024-08-08 16:34_

I also think we try to sort our dependencies lexicographically.

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/marker/algebra.rs`:17 on 2024-08-08 16:50_

How come this isn't `(!= 'Linux') -> FALSE` and `(== 'Linux') -> TRUE`? Why the inequalities?

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/marker/algebra.rs`:17 on 2024-08-08 16:51_

If I had to guess, I'd say it's because the inequalities are more general and easier to merge with other expressions?

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/marker/algebra.rs`:67 on 2024-08-08 16:55_

Should this be `pub(crate)`?

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/marker/algebra.rs`:121 on 2024-08-08 17:11_

I haven't read through the full API yet, but my initial reaction here would be to note that this panics if `children` has no nodes.

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/marker/algebra.rs`:85 on 2024-08-08 17:18_

What kind of operations? At first I was thinking the operation itself should be part of the key. But I think this is only used for `and`?

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/marker/algebra.rs`:408 on 2024-08-08 17:24_

Should this be a fixed width integer like `u64`?

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/marker/algebra.rs`:493 on 2024-08-08 17:25_

What is the significance of `high` and `low` here?

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/marker/algebra.rs`:513 on 2024-08-08 17:25_

I would note that this panics for `In` and `Contains` variants.

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/marker/algebra.rs`:586 on 2024-08-08 17:35_

Nit: add a comment saying why this is unreachable.

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/marker/algebra.rs`:559 on 2024-08-08 17:37_

I had some trouble grok'ing this routine. The `map2` name I think also confused me a bit. Perhaps expand the comment here some?

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/marker/simplify.rs`:36 on 2024-08-08 17:46_

What is `path` here?

---

_Review comment by @BurntSushi on `crates/pep508-rs/src/marker/tree.rs`:678 on 2024-08-08 18:00_

I think this was a nice API decision. Prevents one from emitting an "invalid" marker, but still makes it easy to print a debug repr.

---

_@BurntSushi approved on 2024-08-08 18:06_

This PR is next-level awesome. Solves a _huge_ problem we've been having with marker expressions and solves it well.

What do you think about making `MarkerTree::and` and `MarkerTree::or` smart constructors instead of routines that mutate trees in place? Or perhaps that's for another PR, in order to keep the amount of code changed outside of `pep508_rs` smaller.

---

_@ibraheemdev reviewed on 2024-08-08 19:23_

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/algebra.rs`:17 on 2024-08-08 19:23_

Yes, exactly. Even if we did merge the two `FALSE` children, we would have to represent the `!= 'Linux'` as `(< 'Linux') or (> 'Linux')` with pubgrub ranges. We could merge them to reduce the size of the tree, but for now this is simpler.

---

_@ibraheemdev reviewed on 2024-08-08 19:24_

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/algebra.rs`:85 on 2024-08-08 19:24_

Yes, only for `and` (`or` is implemented in terms of `and`). Added a note of this to the docs.

---

_@ibraheemdev reviewed on 2024-08-08 19:24_

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/algebra.rs`:408 on 2024-08-08 19:24_

The IDs are indexes into a vector, so I think `usize` makes sense here.

---

_@ibraheemdev reviewed on 2024-08-08 19:25_

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/algebra.rs`:493 on 2024-08-08 19:25_

`high` and `low` is the terminology used by the original BDD paper for the `true` and `false` edges of a decision node. I clarified the meaning in the docs.

---

_@ibraheemdev reviewed on 2024-08-08 19:26_

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/algebra.rs`:559 on 2024-08-08 19:26_

I added some more general documentation. Is it clearer now?

---

_@ibraheemdev reviewed on 2024-08-08 19:26_

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/simplify.rs`:36 on 2024-08-08 19:26_

It is the list of expressions encountered on the current path in the depth-first traversal. I added a note of this to the docs.

---

_Comment by @ibraheemdev on 2024-08-08 19:45_

> What do you think about making `MarkerTree::and` and `MarkerTree::or` smart constructors instead of routines that mutate trees in place? Or perhaps that's for another PR, in order to keep the amount of code changed outside of pep508_rs smaller.

Yes, I also want to remove uses of `Option<MarkerTree>` now that `MarkerTree::TRUE` exists. Will do in a follow-up PR.

---

_Review comment by @sid-kap on `crates/pep508-rs/src/marker/algebra.rs`:24 on 2024-08-09 02:29_

```suggestion
//! - Isomorphic nodes are merged.
```

---

_@sid-kap reviewed on 2024-08-09 02:29_

---

_Review comment by @konstin on `crates/pep508-rs/src/marker/algebra.rs`:86 on 2024-08-09 10:25_

```suggestion
    /// Note that that `OR` is implemented in terms of `AND`.
```

---

_Review comment by @konstin on `crates/pep508-rs/src/marker/algebra.rs`:40 on 2024-08-09 10:30_

Could you add something about the interning and basic structs here?

---

_Review comment by @konstin on `crates/pep508-rs/src/marker/algebra.rs`:434 on 2024-08-09 10:35_

Can you add comments to the bit operations?

---

_Review comment by @konstin on `crates/pep508-rs/src/marker/algebra.rs`:593 on 2024-08-09 11:07_

Is this ever used with something other than AND?

---

_Review comment by @konstin on `crates/pep508-rs/src/marker/algebra.rs`:286 on 2024-08-09 11:14_

What does the return value of `f` mean?

---

_Review comment by @konstin on `crates/pep508-rs/src/marker/algebra.rs`:281 on 2024-08-09 11:17_

Took me a while to realize that we clip it to boolean values, so this is for cases with two children(?)

---

_Review comment by @konstin on `crates/pep508-rs/src/marker/algebra.rs`:522 on 2024-08-09 11:21_

For `python_version` doesn't get prerelease versions (we copied that from pip) `python_full_version` is a todo though

---

_Review comment by @konstin on `crates/pep508-rs/src/marker/algebra.rs`:476 on 2024-08-09 11:22_

Should we use `(Option<Bound>, Option<Bound>)` instead to encode this invariant?

---

_Review comment by @konstin on `crates/pep508-rs/src/marker/algebra.rs`:541 on 2024-08-09 11:27_

nit: inline variable

---

_Review comment by @konstin on `crates/pep508-rs/src/marker/simplify.rs`:36 on 2024-08-09 11:41_

How does push+truncate behave vs. `path` being owned and eagerly cloned?

---

_Review comment by @konstin on `crates/pep508-rs/src/marker/tree.rs`:1025 on 2024-08-09 11:50_

What about we make `extra` the highest level in the ADD (because we know it must be on the top level, otherwise our resolver assumptions fall apart) and read it from the top level node here without having to go through the DNF?

---

_Review comment by @konstin on `crates/pep508-rs/src/marker/simplify.rs`:20 on 2024-08-09 11:56_

Can you talk about why DNF and not serializing the ADD?

---

_Review comment by @konstin on `crates/pep508-rs/src/marker/simplify.rs`:177 on 2024-08-09 11:57_

```suggestion
/// Note: This function has quadratic time complexity. However, it is not applied on every marker
```

---

_Review comment by @konstin on `crates/pep508-rs/src/marker/simplify.rs`:170 on 2024-08-09 11:59_

Can you add an example of a redundancy to the doc comment?

---

_Review comment by @konstin on `crates/uv/tests/lock.rs`:3589 on 2024-08-09 12:22_

nice

---

_Review comment by @konstin on `crates/uv/tests/pip_compile.rs`:6474 on 2024-08-09 12:24_

How is the ordering defined now, is it the ordering of the marker variable flowing down? It looks like in conjunctions and disjunctions the ordering is now different. Not high priority as long as the  ordering is deterministic and robust, but might be easy enough to justify changing

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:87 on 2024-08-09 12:31_

What is the difference between fresh and from lock? We always know the current requires-python in the resolver (we determine it on fork construction), so marker construction should already have this information.

Also note that requires-python is the bound for the entire lockfile, but individual forks can have stricter bounds, e.g. say overall is `>=3.7`, but there's one fork `>=3.7,<3.8` and one fork `>=3.8`

---

_Review comment by @konstin on `crates/uv-requirements/src/source_tree.rs`:110 on 2024-08-09 12:35_

nit: `is_false`

---

_@konstin approved on 2024-08-09 12:41_

The ADDs are much better than my naive DNF idea. All the maths looks well-implemented.

---

_@konstin reviewed on 2024-08-09 12:43_

---

_Review comment by @konstin on `crates/pep508-rs/src/marker/algebra.rs`:493 on 2024-08-09 12:43_

I'd use more pragmatic terms than the literature, like `true_` and `false_`. Related, could you link the literature you used in a top level doc comment?

---

_Comment by @konstin on 2024-08-09 12:44_

Could you also a top level doc comment sentence that we implement all `or` as `and` by demorgan and cheap negation?

---

_@BurntSushi reviewed on 2024-08-09 12:53_

---

_Review comment by @BurntSushi on `crates/uv-requirements/src/source_tree.rs`:110 on 2024-08-09 12:53_

I think `!marker.is_true()` does not imply `marker.is_false()`. I do like the succinctness of the method names, but they do suggest a mutual exclusivity that isn't there. Perhaps `is_always_true` and `is_always_false` are better names? Not sure.

---

_@konstin reviewed on 2024-08-09 13:03_

---

_Review comment by @konstin on `crates/uv-requirements/src/source_tree.rs`:110 on 2024-08-09 13:03_

Good call, totally missed that


---

_@ibraheemdev reviewed on 2024-08-09 15:12_

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/algebra.rs`:593 on 2024-08-09 15:12_

No, but it's possible that we may want to reuse this for other operations in the future (such as `XOR` or `IFF`).

---

_@ibraheemdev reviewed on 2024-08-09 15:12_

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/algebra.rs`:286 on 2024-08-09 15:12_

Updated the documentation.

---

_@ibraheemdev reviewed on 2024-08-09 15:14_

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/algebra.rs`:281 on 2024-08-09 15:14_

This is used for extras which are always represented as boolean variables.  For example, `extra == 'foo'` and `extra == 'bar'` creates two separate variables in the tree, as they are unrelated.

---

_@ibraheemdev reviewed on 2024-08-09 15:15_

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/algebra.rs`:476 on 2024-08-09 15:15_

I tried this but it means it was easier to keep them as `Range` to access `complement`/`union` operations without cloning.

---

_@ibraheemdev reviewed on 2024-08-09 15:16_

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/simplify.rs`:36 on 2024-08-09 15:16_

I believe this is better as we can reuse the single `Vec` for the entire tree only cloning when we reach `true`, whereas eagerly cloning would be wasteful when we hit a `false` terminal (which is generally around half of the tree).

---

_@ibraheemdev reviewed on 2024-08-09 15:18_

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/tree.rs`:1025 on 2024-08-09 15:18_

I kept extras at the lowest level because it makes `simplify_extras` cheaper. Having extras at the top would mean `simplify_extras` has to create a new node for every single node in the tree (because nodes are immutable).

Also, extras are not represented as a single variable, each extra expression creates a distinct variable.

---

_@ibraheemdev reviewed on 2024-08-09 15:20_

---

_Review comment by @ibraheemdev on `crates/uv/tests/pip_compile.rs`:6474 on 2024-08-09 15:20_

It is the ordering of the markers flowing down yeah. In this case the original expression would have been `(python_version <= '3.11' and sys_platform == 'win32') or python_version > '3.11` before simplifying, which is why the order appears reversed despite `python_version` being the highest order variable.

I guess we could sort these the same way we used to?

---

_@ibraheemdev reviewed on 2024-08-09 15:23_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/lock.rs`:87 on 2024-08-09 15:23_

This is emulating [the call to `simplify_python_versions` when constructing the `ResolutionGraph`](https://github.com/astral-sh/uv/blob/ibraheem/canonical-markers/crates/uv-resolver/src/resolution/graph.rs#L165).

I agree that marker construction should always have this information, I would like to avoid simplifying requires-python and extras in a separate step.

---

_@notatallshaw reviewed on 2024-08-09 15:53_

---

_Review comment by @notatallshaw on `crates/uv/tests/pip_compile.rs`:6474 on 2024-08-09 15:53_

FYI, doing a final sort on simplified markers is preferential for users not seeing a change in equivelent output if something about the resolution itself changes.

---

_@ibraheemdev reviewed on 2024-08-09 16:16_

---

_Review comment by @ibraheemdev on `crates/uv/tests/pip_compile.rs`:6474 on 2024-08-09 16:16_

The resolution is still deterministic, but sorting may result in a more syntactically consistent order.

---

_@notatallshaw reviewed on 2024-08-09 16:25_

---

_Review comment by @notatallshaw on `crates/uv/tests/pip_compile.rs`:6474 on 2024-08-09 16:25_

I meant in a scenario where the input changed, (e.g. the user changed the order, or added requirements that were previously transative dependencies, or new versions of packages changed the requirements in some similiar sense), but the output was equivelent (e.g. same packages, same versions, same markers), but perhaps the marker order could be affected.

---

_@ibraheemdev reviewed on 2024-08-09 16:30_

---

_Review comment by @ibraheemdev on `crates/uv/tests/pip_compile.rs`:6474 on 2024-08-09 16:30_

That should never happen. The decision diagram is canonical for any functionally equivalent marker and the simplification routine is deterministic. All sorting could do is give us a *different* canonical output for a given marker, but it shouldn't affect whether or not it is canonical.

---

_Merged by @ibraheemdev on 2024-08-09 17:40_

---

_Closed by @ibraheemdev on 2024-08-09 17:40_

---

_Branch deleted on 2024-08-09 17:40_

---

_Comment by @charliermarsh on 2024-08-10 04:01_

Amazing work.

> We don't currently combine python_full_version and python_version markers.

Does this mean we no longer compute unions, etc., to combine `or` markers of versions? (Or we don't combine `python_version` with `python_full_version`?)

---

_Comment by @ibraheemdev on 2024-08-10 04:02_

We compute unions of `python_version` and `python_full_version` independently, but we don't combine them.

---

_Comment by @charliermarsh on 2024-08-10 04:02_

Makes sense thank you.

---
