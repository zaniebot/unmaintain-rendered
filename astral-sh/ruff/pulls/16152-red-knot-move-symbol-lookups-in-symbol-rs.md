```yaml
number: 16152
title: "red-knot: move symbol lookups in `symbol.rs`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/symbol-api
created_at: 2025-02-14T03:40:51Z
updated_at: 2025-02-17T12:15:42Z
url: https://github.com/astral-sh/ruff/pull/16152
synced_at: 2026-01-12T15:55:53Z
```

# red-knot: move symbol lookups in `symbol.rs`

---

_@dhruvmanila_

## Summary

This PR does the following:
* Moves the following from `types.rs` in `symbol.rs`:
	* `symbol`
	* `global_symbol`
	* `imported_symbol`
	* `symbol_from_bindings`
	* `symbol_from_declarations`
	* `SymbolAndQualifiers`
	* `SymbolFromDeclarationsResult`
* Moves the following from `stdlib.rs` in `symbol.rs` and removes `stdlib.rs`:
	* `known_module_symbol`
	* `builtins_symbol`
	* `typing_symbol` (only for tests)
	* `typing_extensions_symbol`
	* `builtins_module_scope`
	* `core_module_scope`
* Add `symbol_from_bindings_impl` and `symbol_from_declarations_impl` to keep `RequiresExplicitReExport` an implementation detail
* Make `declaration_type` a `pub(crate)` as it's required in `symbol_from_declarations` (`binding_type` is already `pub(crate)`

The main motivation is to keep the implementation details private and only expose an ergonomic API which uses sane defaults for various scenario to avoid any mistakes from the caller. Refer to https://github.com/astral-sh/ruff/pull/16133#discussion_r1955262772, https://github.com/astral-sh/ruff/pull/16133#issue-2850146612 for details.



---

_Label `red-knot` added by @dhruvmanila on 2025-02-14 03:40_

---

_Marked ready for review by @dhruvmanila on 2025-02-14 04:35_

---

_Review requested from @carljm by @dhruvmanila on 2025-02-14 04:35_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-02-14 04:35_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-02-14 04:35_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-02-14 04:35_

---

_@AlexWaygood approved on 2025-02-14 13:48_

Nice! It's great to move these out of `types.rs`.

Bikeshedding: I'm wondering if we should put these in a new submodule (`lookup.rs`?) rather than `symbol.rs`. The APIs in `symbol.rs` are currently quite abstract and don't have much to do with the business of implementing Python's lookup semantics correctly; these APIs we're moving here feel a little different from that. I don't have a strong opinion, however; I'm happy to go with this

We could also add some more doc-comments to the `module_type_symbol` function, as I mentioned in https://github.com/astral-sh/ruff/pull/16133#discussion_r1956160477

---

_@carljm approved on 2025-02-14 21:51_

Looks good to me! I agree with all of Alex's comments.

---

_Comment by @dhruvmanila on 2025-02-17 10:46_

> Bikeshedding: I'm wondering if we should put these in a new submodule (`lookup.rs`?) rather than `symbol.rs`. The APIs in `symbol.rs` are currently quite abstract and don't have much to do with the business of implementing Python's lookup semantics correctly; these APIs we're moving here feel a little different from that. I don't have a strong opinion, however; I'm happy to go with this

We added a `LookupResult` in `symbol.rs` in https://github.com/astral-sh/ruff/pull/16160 which makes me wonder if we should rename `symbol.rs` to `lookup.rs` instead.

But, I also see this comment on `LookupResult`:

https://github.com/astral-sh/ruff/blob/89cefbec07e39df143e2cbdc22cf5a5a7266cca3/crates/red_knot_python_semantic/src/symbol.rs#L176-L177

which makes me hesitant to do it now and can be done easily in the future as well.

---

_Merged by @dhruvmanila on 2025-02-17 12:15_

---

_Closed by @dhruvmanila on 2025-02-17 12:15_

---

_Branch deleted on 2025-02-17 12:15_

---
