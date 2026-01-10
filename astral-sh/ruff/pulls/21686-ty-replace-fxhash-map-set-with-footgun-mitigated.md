```yaml
number: 21686
title: "[ty] replace `FxHash{Map, Set}` with footgun-mitigated APIs"
type: pull_request
state: closed
author: mtshiba
labels:
  - ty
assignees: []
base: main
head: stable-iteration
created_at: 2025-11-29T06:35:34Z
updated_at: 2025-12-10T16:48:37Z
url: https://github.com/astral-sh/ruff/pull/21686
synced_at: 2026-01-10T16:42:11Z
```

# [ty] replace `FxHash{Map, Set}` with footgun-mitigated APIs

---

_Pull request opened by @mtshiba on 2025-11-29 06:35_

## Summary

from https://github.com/astral-sh/ty/issues/1670

~~If a data structure that depends on salsa IDs is used as the key for an `FxHashMap` or the value of an `FxHashSet`, the output order of the iterator will be unstable. If a query depends on this order, the results of fixed-point iteration will be unstable.
In this case, `Fx{Index, Order}{Map, Set}` should be used, but it seems that this is not being done thoroughly.~~

~~Simply replacing all `FxHash{Map, Set}` with `Fx{Index, Order}{Map, Set}` would solve the problem, but it is generally believed that the former has slightly better performance if we are only using insertion and retrieval without using a set/map as an iterator.~~

~~Therefore, this PR proposes a compromise.~~
That is, replace all `FxHash{Map, Set}` used within ty with wrapper structs that does not implement `(Into)Iterator`, and instead define methods like `unstable_iter` to make users of these structs aware of whether iteration operations are safe.

After performing this refactoring, I discovered some suspicious parts. I hope this fix will help to resolve the issue.

## Test Plan

<!-- How was it tested? -->


---

_Comment by @astral-sh-bot[bot] on 2025-11-29 06:37_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-29 06:39_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 41 diagnostics
+ Found 42 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1209:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5511 diagnostics
+ Found 5512 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-29 06:45_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/ide_support.rs`:106 on 2025-11-29 08:28_

It's probably better to use `FxIndexSet` here.

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/overrides.rs`:34 on 2025-11-29 08:29_

We should probably use `FxIndexSet` here.

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/constraints.rs`:1040 on 2025-11-29 08:50_

We should probably use `FxIndexSet` here.
The rest of this module seems okay to use unstable iterators, unless I'm overlooking something.

---

_@mtshiba reviewed on 2025-11-29 08:50_

---

_Closed by @mtshiba on 2025-11-29 09:25_

---

_Reopened by @mtshiba on 2025-11-29 09:25_

---

_Review comment by @mtshiba on `crates/ty_project/src/files.rs`:155 on 2025-11-29 10:41_

We should probably use `FxIndexSet` here.

---

_@mtshiba reviewed on 2025-11-29 10:41_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/generics.rs`:123 on 2025-11-29 10:52_

We should probably use `FxIndexSet` here.

---

_@mtshiba reviewed on 2025-11-29 10:52_

---

_Closed by @mtshiba on 2025-11-29 11:00_

---

_Reopened by @mtshiba on 2025-11-29 11:00_

---

_Label `ty` added by @AlexWaygood on 2025-11-29 11:22_

---

_Comment by @mtshiba on 2025-11-29 11:38_

Unfortunately, it appears that non-determinism remains due to factors entirely different from those considered in this PR.
However, that doesn't mean I think these changes are useless (they appear to improve performance slightly in benchmarks of large projects).

Anyway, I'm marking this PR as ready for review.

---

_Marked ready for review by @mtshiba on 2025-11-29 11:38_

---

_Review requested from @carljm by @mtshiba on 2025-11-29 11:38_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-11-29 11:38_

---

_Review requested from @sharkdp by @mtshiba on 2025-11-29 11:38_

---

_Review requested from @dcreager by @mtshiba on 2025-11-29 11:38_

---

_Review requested from @MichaReiser by @mtshiba on 2025-11-29 11:38_

---

_Comment by @MichaReiser on 2025-11-29 18:11_

I like the approach, but I don't think using an IndexMap is the solution. 

Iterating over two `HashMap`s, each created by inserting the same elements in the same order, will yield the same iteration order when run on the same platform. 

The issue we see with fix point is that it's no longer guaranteed that the elements are inserted in the same order. However, we'll have the exact same issue when using `IndexMap` because `IndexMap`'s iteration order is defined by insertion order, which isn't guaranteed to be deterministic within a cyclic query. 

I'm not sure what the solution here is, other than applying some sort of sorting (somewhere?). 

---

_Comment by @MichaReiser on 2025-11-30 16:29_

There's also one flaky diagnostic. What I suspect is that our convergence functions are now sensitive to which query is the outer-most cycle or some query that bails early as soon as it sees the first `Divergent` type (any type), and the any type is only visible depending on the cycle nesting

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/ty.rs`:91 on 2025-12-01 09:29_

I don't think we should make this change. The check result should never depend on iteration order because reading a directory doesn't guarantee that the files are listed in a deterministic ordering. 

If we rely on any ordering, then we'd have to sort the files manually 

---

_Review comment by @MichaReiser on `crates/ty_project/src/db/changes.rs`:321 on 2025-12-01 09:30_

Same here, file watching behavior should not depend on iteration order.

---

_Review comment by @MichaReiser on `crates/ty_project/src/files.rs`:155 on 2025-12-01 09:30_

I don't think we should because there's nothing guaranteeing that we insert files in a deterministic order. See my other comments.

---

_Review comment by @MichaReiser on `crates/ty_project/src/lib.rs`:58 on 2025-12-01 09:31_

Same here, I don't think preserving insertion order gives us anything here because we don't control insertion order. Therefore, it's important that our check result doesn't depend on in which iteration individual files are checked.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/typeshed.rs`:225 on 2025-12-01 09:34_

Given that we sort, using `stable_iter` here doesn't seem important

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/place.rs`:142 on 2025-12-01 09:41_

Implementing the type on the key seems problematic to me, because it isn't the key that ensures iteration order is stable. In fact, the key doesn't matter at all. 

* Iterating over a regular `HashMap` should always be considered non-consistent (it's stable but not platform agnostic). Ideally, ty doesn't depend on any platform specific ordering
* For the iteration order to be stable, it is required that all items are inserted in the exact same ordering. This isn't something the key controls. Instead, it's something that depends on how the map is built and why all maps in `SemanticIndex` are stable (because we build them in a non-cyclic query)


I like "removing" `into_iter` and `iter` from `HashSet` and `HashMap` and only exposing a `iter_unstable` and `into_iter_unstable` methods to "warn" about the possiblity that the iteration order isn't guaranteed. But I don't think we should add a `StableKey` trait as this gives a false sense of "safety". 

But I do get your idea. What you want to express is: We know that this map is built in a deterministic way, it's safe to iterate it and assume stable ordering. Unfortunately, we can't do this without using a stable hash function, which `FxHash` isn't. 

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/lib.rs`:67 on 2025-12-01 09:43_

Let's move this to its own module (`hash.rs`)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/lib.rs`:174 on 2025-12-01 09:44_

If this is commonly required, then using a `BTreeSet` is probably better

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/lib.rs`:326 on 2025-12-01 09:45_

What's the motivation for offering these methods if they always panic at runtime. It seems preferred to not have those methods in the first place (or name then `keys_unstable` etc.)

---

_@MichaReiser reviewed on 2025-12-01 09:47_

I haven't reviewed all the changes but I don't think the `StableKey` assumption is safe. 

I do think it makes sense to have a hash map wrapper and this is also what rustc does https://github.com/rust-lang/rust/blob/ae90dcf0207c57c3034f00b07048d63f8b2363c8/compiler/rustc_data_structures/src/stable_map.rs#L45


Another solution (maybe less invasive?) could be to initialize the hashser function with a random state (instead of 0) in debug builds, so that iteration order is guaranteed to be different across runs (or have a feature flag that we can turn on)

---

_Comment by @Gankra on 2025-12-01 14:39_

re: randomizing layouts: if you want to go down that route I suggest cribbing from [rustc's `-Zrandomize-layout`](https://doc.rust-lang.org/beta/unstable-book/compiler-flags/randomize-layout.html) which lets you pass the seed in as a CLI argument (and print the seed any time one is selected so people can try to reproduce an issue).



---

_@mtshiba reviewed on 2025-12-01 16:44_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/semantic_index/place.rs`:142 on 2025-12-01 16:44_

You're right. I considered `FxHashMap` for the same keys as "stable" in a narrow sense, but it certainly doesn't guarantee that it will give a consistent order in all environments and versions. I'll remove `StableKey` APIs.

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/lib.rs`:326 on 2025-12-01 17:07_

`crate::FxHashMap` implements `Deref<Target=rustc_hash::FxHashMap>`. I wanted to avoid the hassle of wrapping all the APIs.
But then `iter`s would also be available via `Deref`, so I decided to override them.

---

_@mtshiba reviewed on 2025-12-01 17:07_

---

_@MichaReiser reviewed on 2025-12-01 18:06_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/lib.rs`:326 on 2025-12-01 18:06_

I'd be fine copying over the signatures from `FxHashMap` that we need, similar to what rustc does. I suspect that it won't be that many (mainly `get`, `entry` etc)

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/ty.rs`:5 on 2025-12-02 07:56_

Let's not use `BTreeSet` for indexed files. It won't give us any guarantees because the deterministic ordering goes out of the window in `project.check`, where we process the files concurrently.

---

_Review comment by @MichaReiser on `crates/ty_project/src/db/changes.rs`:55 on 2025-12-02 07:57_

Same here. I don't see why a stable ordering would be necessary here. We sort the returned diagnostics

---

_Review comment by @MichaReiser on `crates/ty_project/src/files.rs`:130 on 2025-12-02 07:58_

Let's revert this change for the same reason as stated above

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/hash.rs`:44 on 2025-12-02 07:59_

Why is it okay to `Deref` `FxHashSet` but not `FxHashMap`?




---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/lint.rs`:349 on 2025-12-02 08:00_

What's the reason for this change?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:1040 on 2025-12-02 08:03_

Can you say more what `query-dependent` means? 

Looking at the loop below, it doesn't seem to depend on ordering as it returns `true` only if all typevars satisfy the constraints and there's no state between the type var checking, as far as I can tell

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:518 on 2025-12-02 08:05_

Are we using this anywhere outside the LSP?

---

_@MichaReiser requested changes on 2025-12-02 08:06_

I think I'm leaning towards changing the hash maps in separate PRs and more explicitly talk about why the changes are necessary. I'm not convinced that it's necessary to use `FxIndexMap` in many cases.

---

_Review request for @sharkdp removed by @sharkdp on 2025-12-02 08:20_

---

_Review request for @carljm removed by @carljm on 2025-12-03 06:46_

---

_@mtshiba reviewed on 2025-12-03 16:55_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/hash.rs`:44 on 2025-12-03 16:55_

I simply forgot to revert. There is no longer any need to implement `Deref` for both `FxHash{Set, Map}`.

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/ide_support.rs`:518 on 2025-12-03 17:18_

It seems to be used in ty_python_semantic as well (https://github.com/mtshiba/ruff/blob/stable-iteration/crates/ty_python_semantic/src/types/function.rs#L1464-L1467). In that case, however, an unstable iterator is not a problem.


---

_@mtshiba reviewed on 2025-12-03 17:18_

---

_@mtshiba reviewed on 2025-12-03 17:23_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/lint.rs`:349 on 2025-12-03 17:23_

Using `Deref` prevented [two-phase borrows](https://rustc-dev-guide.rust-lang.org/borrow_check/two_phase_borrows.html) from working properly. Now that we're no longer using `Deref`.

---

_@mtshiba reviewed on 2025-12-03 17:32_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/constraints.rs`:1040 on 2025-12-03 17:32_

`{valid, required}_specialization` uses `lazy_{bound, constraints}` internally. I meant that the order in which these queries are called is non-deterministic.

---

_@AlexWaygood reviewed on 2025-12-03 17:37_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/ide_support.rs`:518 on 2025-12-03 17:37_

that is just for internal use so that we can test the function itself in our mdtests; it's not a public-facing part of ty_python_semantic

---

_Renamed from "[ty] disallow using unstable iterators of `FxHash{Map, Set}` as query input" to "[ty] replace `FxHash{Map, Set}` with footgun-mitigated APIs" by @mtshiba on 2025-12-03 17:55_

---

_Comment by @mtshiba on 2025-12-03 18:04_

> I think I'm leaning towards changing the hash maps in separate PRs and more explicitly talk about why the changes are necessary. I'm not convinced that it's necessary to use `FxIndexMap` in many cases.

I changed this PR to just replace `FxHash{Map, Set}` with new APIs. Therefore, this PR does not change any of the behavior of ty. Changes that affect behavior will be made after this PR (with reconsideration).

---

_Review requested from @MichaReiser by @mtshiba on 2025-12-03 18:08_

---

_@MichaReiser reviewed on 2025-12-04 08:04_

Thanks for updating.

I'd be curious to get some more opinions on this. To me, this PR goes from one extreme to the other. I don't think we need to enforce this new API in all crates. E.g. I think it's completely fine to use the normal `FxHashMap` and `IntoIterator` in the project file discovery. 

I'm leaning towards:

* Giving those types a different name instead of overriding `FxHashSet`. I'm not entirely sure what to name them
* Update our Contribution guidelines to state that these types should be used in `ty_python_semantic` with an explanation why it's important
* Add the types to `ruff_db`

But maybe it's just me who dislikes the extra verbosity and others actually prefer to override `FxHashSet` and `FxHashMap`

---

_Comment by @mtshiba on 2025-12-04 09:46_

The purpose of this PR is to make developers aware that using `FxHash{Map, Set}` as an iterator is inherently unstable, and in a sense, it intentionally introduces noisy APIs.
However, if you feel it's troublesome to continue using `unstable_iter()` in places where it is clearly OK to use it as an iterator, one option is to give `FxHash{Map, Set}` a tag type and change its behavior depending on the tag. In this case, there is no need to prepare two types of structs.

```rust
use std::collections::HashMap;
use std::collections::hash_map::IntoIter;

/// You are sure you can always use this hash map/set as an iterator.
#[derive(Default)]
pub struct ImplIntoIterator;
#[derive(Default)]
pub struct NoIntoIterator;

#[derive(Default)]
pub struct FxHashMap<K, V, Tag=NoIntoIterator>(HashMap<K, V>, std::marker::PhantomData<Tag>);

impl<K, V> IntoIterator for FxHashMap<K, V, ImplIntoIterator> {
    type Item = (K, V);
    type IntoIter = IntoIter<K, V>;

    fn into_iter(self) -> IntoIter<K, V> {
        self.0.into_iter()
    }
}

fn main() {
    let map1: FxHashMap<u32, u32> = FxHashMap::default();
    let map2: FxHashMap<u32, u32, ImplIntoIterator> = FxHashMap::default();
    
    for _i in map1 {} // ERR
    for _i in map2 {} // OK
}
```

Using `FxHashMap<K, V, ImplIntoIterator>` means that you are declaring that it can always be used as an iterator and there is no problem with that, and using `FxHashMap<K, V, NoIntoIterator>` means that you need to make sure it is OK to use before each iteration (if it is safe you can use `unstable_iter()`).

---

_Comment by @AlexWaygood on 2025-12-04 16:27_

I'm weakly in favour of this PR.

Points I see in favour:
- We've run into multiple bugs now due to iterating over `HashSet`s when we should be using `IndexSet`s.
- It's been nontrivial to track down those bugs when we've hit them. This PR should make it much less likely for us to introduce them in the first place, since it forces us to think explicitly about whether iterating over a `HashSet` is safe in that specific location. It also makes it easier to grep for places where we're iterating over sets, which makes it easier to figure out where the bug might be when we _do_ come across one of these bugs.
- IIUC, the new abstraction should be zero-cost when it comes to performance
- The new code is all pretty well isolated in its own submodule

Points I see against:
- It does seem like a somewhat elaborate solution to the problem. It's a fair amount of new code.
- Adding APIs to the wrapper structs as and when we need them feels like it could be tedious, and it might confuse third-party contributors when they find `HashSet` APIs aren't exposed on these wrappers
- I think iterating over `HashSet`s is probably fine outside of `ty_python_semantic`? It's when the hash depends on the hash of a `Type` that things get problematic, because that changes unpredictably depending on how many `Type`s Salsa interns in which places and in which order.
- I don't think our biggest source of nondeterminism right now is the fact that we sometimes iterate over `HashSet`s, it's issues resulting from our cycle normalization

---

_Comment by @carljm on 2025-12-05 18:57_

I don't have strong feelings about this change in any direction. It seems like a lot of added code/machinery to provide compile-time protection from a problem that hasn't really ever caused us that much difficulty. Unstable iterators have only caused non-determinism a couple times that I can recall, and it hasn't been that hard to track down. The real difficult non-determinism comes from reliance on Salsa IDs, which this won't help with.

I am also concerned with the ongoing overhead of contributors being confused that methods on upstream hash-sets don't exist on our hashsets, and have to be added if we haven't used them before.

---

_Comment by @mtshiba on 2025-12-10 16:48_

The original purpose of this PR was to comprehensively identify and fix uses of unstable iterators that negatively impact deterministic results.
The refactoring itself is a side effect, and if you all don't feel a strong need to accept it, applying the YAGNI principle is probably a better choice.

---

_Closed by @mtshiba on 2025-12-10 16:48_

---
