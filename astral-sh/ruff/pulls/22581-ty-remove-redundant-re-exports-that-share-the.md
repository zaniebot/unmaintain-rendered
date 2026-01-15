```yaml
number: 22581
title: "[ty] Remove redundant re-exports that share the same top-most module"
type: pull_request
state: open
author: BurntSushi
labels:
  - server
  - ty
assignees: []
base: main
head: ag/remove-redundant-auto-import-symbols
created_at: 2026-01-14T19:18:15Z
updated_at: 2026-01-15T16:03:10Z
url: https://github.com/astral-sh/ruff/pull/22581
synced_at: 2026-01-15T16:50:22Z
```

# [ty] Remove redundant re-exports that share the same top-most module

---

_@BurntSushi_

The idea here is to avoid flooding completions with a bunch of
redundant symbols when those symbols are just re-exports of one
another. And in particular, to avoid suggesting symbols "deeper"
in the module graph.

The implementation here is (to me) surprisingly complicated. The main
constraints leading to some of that complexity are:

1. Trying to limit the redundant detection to as few of the symbols
we extract as possible. In particular, while I haven't done benchmarking
on this, I perceive the redundancy detection to be somewhat expensive
and auto-import can return lots of symbols. So we're careful to only do
this extra checking on (typically) small groups of symbols that could
possibly be merged.

2. Even by restricting our work, this merging process could still be
called quite a bit. (Thousands of times in my "standard data scienc-y
test environment.") So I went out of my way to amortize allocs.

3. Re-exports can form a chain and we want to find all of them.

4. We (probably) don't want to remove redundant re-exports unless
they share the same top-level module. Otherwise, e.g., a library
that re-exports another library's symbols could have all of its
re-exports dropped.

5. We want to only keep the top-most re-exports, and there may be
multiple such re-exports. So we keep all of them.

6. We can't assume anything about the relationship of re-exports
and the original definition. A re-export could be deeper in the
module hierarchy than the original definition or above it.

Reviewers are encouraged to read this PR commit by commit.

Fixes astral-sh/ty#1857


---

_Review requested from @carljm by @BurntSushi on 2026-01-14 19:18_

---

_Review requested from @MichaReiser by @BurntSushi on 2026-01-14 19:18_

---

_Review requested from @AlexWaygood by @BurntSushi on 2026-01-14 19:18_

---

_Review requested from @Gankra by @BurntSushi on 2026-01-14 19:18_

---

_Review requested from @sharkdp by @BurntSushi on 2026-01-14 19:18_

---

_Review requested from @dcreager by @BurntSushi on 2026-01-14 19:18_

---

_Comment by @BurntSushi on 2026-01-14 19:20_

Demo:

https://github.com/user-attachments/assets/3c141fb3-f48b-489a-b7d9-9b6ec54fa457

Here's what the status quo looks like:

https://github.com/user-attachments/assets/a5d8bc24-6537-4d1a-b192-a1b2eed2061b



---

_Comment by @astral-sh-bot[bot] on 2026-01-14 19:20_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-14 19:22_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | T@resolve_variables | float | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | T@resolve_variables | float | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1825 diagnostics
+ Found 1823 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14509 diagnostics
+ Found 14508 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review request for @dcreager removed by @BurntSushi on 2026-01-14 19:23_

---

_Review request for @carljm removed by @BurntSushi on 2026-01-14 19:23_

---

_Review request for @Gankra removed by @BurntSushi on 2026-01-14 19:23_

---

_Review request for @sharkdp removed by @BurntSushi on 2026-01-14 19:23_

---

_Label `server` added by @BurntSushi on 2026-01-14 19:24_

---

_Label `ty` added by @BurntSushi on 2026-01-14 19:24_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/all_symbols.rs`:79 on 2026-01-15 13:14_

Nit, it could be assigning the result of `AllSymbolInfo::` to a variable before acquiring the lock, if whatever we do in `::from_module `is expensive. The same applies for line 85

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:294 on 2026-01-15 13:18_

`ModuleName` is also a compact str, meaning we only clone a heap allocation if the string is more than 24 bytes

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:837 on 2026-01-15 13:19_

Nit: I have a slight preference for using our own enum here (which could also have methods like `symbol_kind`). Reading `is_left`, `either` in this body feels rather cryptic.

---

_Review comment by @MichaReiser on `crates/ty_module_resolver/src/module_name.rs`:96 on 2026-01-15 13:29_

Nit: I'm leaning towards using `root` here, which IMO goes better with `ancestors` and `parent` than `top`, but I can't specifically say why :)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/all_symbols.rs`:239 on 2026-01-15 13:35_

One concern that I have with removing it without considering the outer context is if you're working on said library. Let's say I work on pandas and are writing a module in `pandas.io.parsers.readers` or a sibling package of it. It's probably desired to then import the symbol from the closest module rather than always preferring the outer most module?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/all_symbols.rs`:350 on 2026-01-15 13:39_

This `impl` block is a bit larger than what I normally would feel comfortable having within a function body. Should we make `merge` a module? 

---

_Review comment by @MichaReiser on `crates/ty_ide/src/all_symbols.rs`:347 on 2026-01-15 13:40_

I'd find a a short comment on what the meaning of these fields helpful. The code here is complicated enough that I'm failing to easily infer the meaning (and all the invariants) by simply scanning the method bodies

---

_Review comment by @MichaReiser on `crates/ty_ide/src/all_symbols.rs`:245 on 2026-01-15 13:51_

All the lints emitted by clippy here seem reasonable to me (except maybe the match one but can we just disable this specific lint). Any specific reason why you disabled all warning lints?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/all_symbols.rs`:436 on 2026-01-15 13:55_

Is this semantically the same as "finishing" a group? If so, should we add a `finish_into(&mut self, target: &mut Vec)` method (or `finish(&mut self) -> impl Iterator<...>` ?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/all_symbols.rs`:447 on 2026-01-15 13:55_

haha, you're not one of the `is_some_and` fans :)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/all_symbols.rs`:401 on 2026-01-15 14:01_

What are cases where the kind between two symbols with the same fully qualified name is different? 

Would two different kinds suggest that these are different symbols?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/all_symbols.rs`:322 on 2026-01-15 14:05_

Nit: This might be more annoying, so I leave it up to you. As I understand it, the `origin_keep` act as a mechanism to remove elements from `origin` and `reexport` without having to actually remove them (because `swap_remove` would change the ordering). 

An alternative (but maybe too annoying to deal with) approach would be to "tombstone" elements that have been removed, e.g. by making `orign` a `Vec` over `Option<AllSymbolInfo>` where `None` is the tombstone.

---

_@MichaReiser approved on 2026-01-15 14:09_

This looks good. There's certainly a fair amount of complexity in `merge` (functional but also to avoid allocations) that requires a fair amount of concentration to understand what's going on. 

I have one design question on whether filtering out re-export must be sensitive to the current module.

---

_Review comment by @BurntSushi on `crates/ty_module_resolver/src/module_name.rs`:96 on 2026-01-15 14:27_

I was unsure whether "root" was really appropriate since I'm not sure, e.g., `a.b.c` has a "root" at `a` per se.

I could just make this more verbose and call it `first_component`. Which I think I kind of like better since it's less ambiguous and it isn't frequently used either, so the verbosity doesn't bother me.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/all_symbols.rs`:79 on 2026-01-15 15:09_

Not expensive, but not free. I've moved it outside of the critical section for reasons of good sense.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/all_symbols.rs`:239 on 2026-01-15 15:19_

Maaaaybe. I think that's very plausible, but I'm not sure.

The other similar thing in this space is that we might only want to do this merging in non-first party code, under the reasoning that in first party code, you probably want to see _all_ your re-exports. That would also I think address this concern without coming up with a notion of what "closest module" means.

(We talked about this in our 1-on-1. The reason I didn't go down this route is that it's pretty annoying to set up the test infrastructure for it.)

If you're okay with it, I'd like to move forward with this as-is and prioritize future work here based on user feedback. I filed https://github.com/astral-sh/ty/issues/2519 to track this.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/all_symbols.rs`:245 on 2026-01-15 15:20_

An uncommented `allow(warnings)` from me is almost certainly a mistake. :P I sometimes slap it in there when writing code because I find the warnings in my editor very distracting.

I try not to override warnings (Clippy or otherwise) without adding a comment saying something about it.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/all_symbols.rs`:322 on 2026-01-15 15:22_

Yeah I see that. I think I'm going to stick with the current approach though. Using indices avoids borrowck woes _and_ it avoids needing to do an additional `unwrap()` every time we access something in `origin` and `reexport`.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/all_symbols.rs`:347 on 2026-01-15 15:30_

Yes, thank you for the nudge. Done!

---

_Review comment by @BurntSushi on `crates/ty_ide/src/all_symbols.rs`:350 on 2026-01-15 15:30_

It was nice to not have to redo the imports. But sure I don't feel strongly. It has been modulized. :-)

---

_Review comment by @BurntSushi on `crates/ty_ide/src/all_symbols.rs`:401 on 2026-01-15 15:33_

Ah good question! I added this comment to the source clarifying why:

```rust
                    // The reason why we do this unconditionally is that
                    // reexport symbols usually do not have a very specific
                    // kind, and are typically just assigned a kind of
                    // "variable." This is because the symbol extraction
                    // reports these symbols as-is by inspecting import
                    // statements and doesn't follow them to their original
                    // definition to discover their "true" kind. But that's
                    // exactly what we're doing here! We have the original
                    // definition. So assign the kind from the original,
                    // even when it crosses package hierarchies.
```

Does that help?

---

_Review comment by @BurntSushi on `crates/ty_ide/src/all_symbols.rs`:436 on 2026-01-15 15:35_

Yeah that kind of API is what I started with. But the iterator returned borrows from the `Group` and that makes it difficult to clear the other state.

This could be fixed with a dedicated iterator type and a `Drop` impl that does the clearing, but it just seemed like too much ceremony for a one-time-use API. If this were a "real" API with broader scope, yeah absolutely, it'd be worth some strong encapsulation and coupling reduction.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/all_symbols.rs`:447 on 2026-01-15 15:35_

No I am! I just spent many years writing Rust without it, so I still sometimes forget it exists. :P I changed this to use `is_some_and`. :-)

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:294 on 2026-01-15 15:36_

Yeah I just figured it'd be pretty common for `ModuleName` to be longer than 24 bytes in this context.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/symbols.rs`:837 on 2026-01-15 15:41_

Fair enough. I don't feel strongly!

---

_@BurntSushi reviewed on 2026-01-15 15:44_

---
