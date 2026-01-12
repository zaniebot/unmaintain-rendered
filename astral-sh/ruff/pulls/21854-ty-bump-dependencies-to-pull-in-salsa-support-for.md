```yaml
number: 21854
title: "[ty] bump dependencies to pull in Salsa support for `ordermap`"
type: pull_request
state: merged
author: oconnor663
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: order_map
created_at: 2025-12-08T22:46:35Z
updated_at: 2025-12-10T03:08:05Z
url: https://github.com/astral-sh/ruff/pull/21854
synced_at: 2026-01-12T15:57:35Z
```

# [ty] bump dependencies to pull in Salsa support for `ordermap`

---

_@oconnor663_

As part of an earlier version of https://github.com/astral-sh/ruff/pull/21784, I added upstream `salsa` and `get_size2` support for `ordermap`, but that ended up not being needed in the current version of that PR. However, @ibraheemdev has mentioned that we might like to switch to `ordermap` across the board once the `salsa` support was there. So I'm separating out these changes into this separate PR, and I'm expanding them to entirely remove `indexmap` as a dependency of `ty_python_semantic`. Do we like these changes?

---

_Review requested from @carljm by @oconnor663 on 2025-12-08 22:46_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-12-08 22:46_

---

_Review requested from @sharkdp by @oconnor663 on 2025-12-08 22:46_

---

_Review requested from @dcreager by @oconnor663 on 2025-12-08 22:46_

---

_Label `ty` added by @oconnor663 on 2025-12-08 22:46_

---

_Comment by @astral-sh-bot[bot] on 2025-12-08 22:48_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Label `internal` added by @AlexWaygood on 2025-12-08 22:50_

---

_Comment by @MichaReiser on 2025-12-08 22:51_

I don't think we should use OrderedMap or Set everywhere

* we often want to preserve source order, e.g the order in which a user specified their settings 
* salsa IDs are non deterministic. It's not strictly worse than insertion order but also not strictly better, but ordering isn't free

But I might be missing something here. What's the reason why we should  use OrderedMap?

---

_Comment by @AlexWaygood on 2025-12-08 22:53_

> * we often want to preserve source order

`OrderMap` does preserve insertion order. `OrderMap` is a thin wrapper around an IndexMap that adds order-sensitive `Hash` and `Eq` implementations.

---

_Comment by @astral-sh-bot[bot] on 2025-12-08 22:53_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 492 diagnostics
+ Found 494 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5544 diagnostics
+ Found 5545 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @Gankra on 2025-12-08 22:55_

@MichaReiser I think you might be quiet reasonably assuming this is a bigger change than it is

OrderMap and IndexMap are nearly identical types, both being "HashMap but iteration order is insertion order" (i.e. *neither* is BTreeMap-like)

I think the only difference that exists between them is:

> Since its reintroduction in 0.5, ordermap has also used its entry order for PartialEq and Eq, whereas indexmap considers the same entries in any order to be equal for drop-in compatibility with HashMap semantics. Using the order is faster, and also allows ordermap to implement PartialOrd, Ord, and Hash.

---

_Comment by @MichaReiser on 2025-12-08 23:01_

Ah right. Order not ordered.


Do we need the more strict definition of ordermap (that they're only equal if they have the same elements in the exact same order)?

Like I could see this hurt fixpoint convergence. 

---

_Comment by @oconnor663 on 2025-12-08 23:07_

> Do we need the more strict definition of ordermap (that they're only equal if they have the same elements in the exact same order)?

The most important practical difference is this part above: "Using the order...allows ordermap to implement PartialOrd, Ord, and Hash". `IndexMap` doesn't support `Hash`, which means it can't be a field in a `salsa::interned` struct. That already forces us to use `OrderMap` in some places. Until the upstream changes I mentioned above, though, only `IndexMap` impl'd `salsa::Update`, so we were also forced to use `IndexMap` in some places. But with the dependency bumps in `Cargo.toml` here, `OrderMap` also impls `salsa::Update`, and we never have to use `IndexMap`.

> Like I could see this hurt fixpoint convergence.

Ah that's a good point. Apart from seeing that tests pass, I'm not sure.

---

_Comment by @AlexWaygood on 2025-12-08 23:08_

> Do we need the more strict definition of ordermap (that they're only equal if they have the same elements in the exact same order)?

We need OrderMaps/OrderSets whenever we need to use an insertion-order-preserving map/set as a field of a salsa-interned struct. Because fields of salsa-interned structs must be hashable, and IndexMaps/IndexSets are not hashable. There are therefore many places in ty where we already use OrderMap/OrderSet.

This PR switches all the other bits of the ty codebase, that _don't_ strictly need to be OrderMaps/OrderSets, over to using those structs instead of IndexMaps/IndexSets. I believe the rationale is that we have OrderMap as a dependency anyway, and it lowers the cognitive burden on us as developers if we no longer have to worry every time we write a salsa-interned struct whether a field should be an OrderMap/OrderSet or an IndexMap/IndexSet.

---

_Comment by @astral-sh-bot[bot] on 2025-12-08 23:09_


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

_Comment by @oconnor663 on 2025-12-08 23:09_

(Since there's only a single usage of `IndexMap` outside of `ty_python_semantic`, I've gone ahead and rolled that in here: d16073781db1054433551d1ec81f9a2cd11f8a66)

---

_Comment by @MichaReiser on 2025-12-08 23:10_

Thanks for the additional context. I'm still not sure this justifies the stricter equality everywhere but I'm probably missing some context.

Only having to know one map type (as I'm a perfect example of), certainly makes live easier

> longer have to worry every time we write a salsa-interned struct whether a field should be an OrderMap/OrderSet or an IndexMap/IndexSet.

I don't think you have to worry: it always must be an orderedset



---

_Comment by @AlexWaygood on 2025-12-08 23:15_

> I don't think you have to worry: it always must be an orderedset

Yes, bad wording on my part :-) but you do have to think twice in contexts _outside_ of salsa-interned struct fields currently.

I'm very neutral on this FWIW — I do agree that it would make life a bit easier, but I don't really have a strong opinion any way at all!

---

_Comment by @MichaReiser on 2025-12-08 23:20_

My main worry is fixpoint convergence because a query might never converge if the order keeps changing between iterations (remember exported_names). 

Because of that, changing the type for cyclic queries is only safe if we know the query converges on consistent ordering. I don't have a good sense for when this is safe and simply changing all types seems too high risk to me for what we gain (I don't mind migrating one by one or defaulting to orderedmap in the future)

---

_Comment by @oconnor663 on 2025-12-08 23:44_

It might make sense to pare this down to just the dependency version bumps, and make a collective mental note that we no longer _have_ to use `indexmap` in `salsa::Update` contexts?

---

_Comment by @MichaReiser on 2025-12-09 08:29_

> It might make sense to pare this down to just the dependency version bumps, and make a collective mental note that we no longer have to use indexmap in salsa::Update contexts?

Yes, that sounds good to me. It should also be safe to change `IndexMap` to `OrderMap` in places other than salsa query return types.

---

_@MichaReiser approved on 2025-12-09 21:44_

---

_@carljm approved on 2025-12-09 21:56_

---

_Comment by @carljm on 2025-12-09 21:56_

Please retitle the PR before landing.

---

_Renamed from "[ty] switch from IndexMap to OrderMap everywhere" to "[ty] bump dependencies to pull in Salsa support for `ordermap`" by @oconnor663 on 2025-12-09 23:17_

---

_Merged by @oconnor663 on 2025-12-10 03:08_

---

_Closed by @oconnor663 on 2025-12-10 03:08_

---

_Branch deleted on 2025-12-10 03:08_

---
