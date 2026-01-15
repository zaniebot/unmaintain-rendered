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
updated_at: 2026-01-15T14:09:33Z
url: https://github.com/astral-sh/ruff/pull/22581
synced_at: 2026-01-15T14:51:20Z
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



## Typing Conformance

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 0 | 0 | +0 | ⏬ (❌) |
| False Positives | 969 | 969 | +0 | ⏬ (✅) |
| False Negatives | 1155 | 1155 | +0 | ⏬ (✅) |
| Total Diagnostics | 969 | 969 | 0 | ⏬ |
| Precision | 0.00% | 0.00% | +0.00% | ⏬ (❌) |
| Recall | 0.00% | 0.00% | +0.00% | ⏬ (❌) |


The percentage of diagnostics emitted that were expected errors held steady at 0.00%, and the percentage of expected errors that received a diagnostic held steady at 0.00%.



[Typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)



---

_Comment by @astral-sh-bot[bot] on 2026-01-14 19:22_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1824 diagnostics
+ Found 1826 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 4359 diagnostics
+ Found 4361 diagnostics


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

_Review comment by @MichaReiser on `crates/ty_ide/src/all_symbols.rs`:80 on 2026-01-15 13:14_

Nit, it could be assigning the result of `AllSymbolInfo::` to a variable before acquiring the lock, if whatever we do in `::from_module `is expensive. The same applies for line 85

---

_Review comment by @MichaReiser on `crates/ty_ide/src/symbols.rs`:295 on 2026-01-15 13:18_

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

_Review comment by @MichaReiser on `crates/ty_ide/src/all_symbols.rs`:257 on 2026-01-15 13:39_

This `impl` block is a bit larger than what I normally would feel comfortable having within a function body. Should we make `merge` a module? 

---

_Review comment by @MichaReiser on `crates/ty_ide/src/all_symbols.rs`:254 on 2026-01-15 13:40_

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

_Review comment by @MichaReiser on `crates/ty_ide/src/all_symbols.rs`:308 on 2026-01-15 14:01_

What are cases where the kind between two symbols with the same fully qualified name is different? 

Would two different kinds suggest that these are different symbols?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/all_symbols.rs`:250 on 2026-01-15 14:05_

Nit: This might be more annoying, so I leave it up to you. As I understand it, the `origin_keep` act as a mechanism to remove elements from `origin` and `reexport` without having to actually remove them (because `swap_remove` would change the ordering). 

An alternative (but maybe too annoying to deal with) approach would be to "tombstone" elements that have been removed, e.g. by making `orign` a `Vec` over `Option<AllSymbolInfo>` where `None` is the tombstone.

---

_@MichaReiser approved on 2026-01-15 14:09_

This looks good. There's certainly a fair amount of complexity in `merge` (functional but also to avoid allocations) that requires a fair amount of concentration to understand what's going on. 

I have one design question on whether filtering out re-export must be sensitive to the current module.

---
