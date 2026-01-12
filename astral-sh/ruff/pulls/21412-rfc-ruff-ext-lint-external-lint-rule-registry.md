```yaml
number: 21412
title: "RFC [ruff][ext-lint] external lint rule registry, type definitions and serde config"
type: pull_request
state: open
author: pieterh-oai
labels: []
assignees: []
base: main
head: pieterh/ext-lint-types
created_at: 2025-11-12T20:38:36Z
updated_at: 2026-01-07T01:47:17Z
url: https://github.com/astral-sh/ruff/pull/21412
synced_at: 2026-01-12T15:57:23Z
```

# RFC [ruff][ext-lint] external lint rule registry, type definitions and serde config

---

_@pieterh-oai_

## Summary

> [!Note]
> This is PR 1/3 in an initial 3-PR stack. The two follow-on PRs are #21413 and #21415 .

The goal of this stack is, more or less, to explore what it looks like if we hook up PyO3 and use it to run out-of-tree lint checks written in Python (focusing only on AST linters and excluding fixers). This is based on some of the discussion (on Discord and Notion) regarding #283. 

This PR adds a registry, the config datatypes, TOML file parsing, and some initial plumbing to support external linters, with the following constraints:

- We want to minimize changes to the existing Ruff plumbing for how ‘internal’ Rust-based linters and rules are defined, selected, and run.
    - This PR adds a single rule,  `RUF300`, that is used to represent all external linters. This is used mostly internally (though it can be `--select`ed). This makes it easy to disable all external linters in one place; folks who don’t use this functionality can be reasonably sure that it isn’t costing them performance.
    - The downside is that we will need our own machinery for selecting/ignoring/extend-*-ing external linters, supporting noqa, and so on. This is fairly complex, so I made it a separate PR.

```bash
ruff.toml / pyproject.toml 1--->n my_external_linter.toml 1--->n my_rule.py
```

- The ‘schema’ ^ expects each external linter to live in its own TOML file, defining one or more rules. Each rule has a few TOML properties and is defined in a separate Python file. Rules ‘subscribe’ to the node types they want to examine. We store that mapping in a registry on the Rust side so that the AST visitor can locate and run the correct rules reasonably quickly/efficiently.

## Test Plan

```bash
cargo clippy --workspace --all-targets --all-features -- -D warnings
cargo test --workspace --exclude ty
uvx pre-commit run --all-files --show-diff-on-failure

# add RUF300 to ruff.schema.json
cargo dev generate-all
```

---

_Comment by @astral-sh-bot[bot] on 2025-11-12 20:49_


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

_Review requested from @amyreese by @amyreese on 2025-11-12 22:43_

---

_Review requested from @MichaReiser by @amyreese on 2025-11-12 22:43_

---

_Review requested from @ntBre by @amyreese on 2025-11-12 22:43_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/external/ast/registry.rs`:137 on 2025-11-13 08:40_

This might be more tricky. Just hashing the linter metadata isn't enough. We'd also need to hash the linter implementation, e.g. by hasing the linter file content and all its dependencies. 


I suspect that we want the linter to provide its own hash during initialization. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/external/ast/registry.rs`:25 on 2025-11-13 08:42_

Nit: You could use an `IndexVec` here to get a type safe `LinterIndex` type (over just an `usize` where it's unclear into what the value is indexing into)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/external/ast/registry.rs`:32 on 2025-11-13 08:43_

Nit: These could probably become `Vec`'s if we add a method to convert from `StmtKind` to `usize`.

---

_@MichaReiser reviewed on 2025-11-13 08:45_

I started skimming the code. I'll leave comments as I go.

---

_@pieterh-oai reviewed on 2025-11-13 17:14_

---

_Review comment by @pieterh-oai on `crates/ruff_linter/src/external/ast/registry.rs`:137 on 2025-11-13 17:14_

Ack. For transparency, I haven't worried or tested caching yet in trying to get this to just work. This is currently used to select interpreter+environments for the runtime. It's not really ready for actual caching yet, but I think it can be made to work with a few more fields included.

---

_Comment by @pieterh-oai on 2025-11-19 19:44_

(rebase)

---

_Comment by @pieterh-oai on 2025-11-21 04:09_

(push: Hopefully satify the mkdoc CI job).

---

_Comment by @pieterh-oai on 2025-11-21 05:04_

(check_docs_formatted.py)

---

_Review requested from @carljm by @pieterh-oai on 2025-12-19 20:51_

---

_Review requested from @sharkdp by @pieterh-oai on 2025-12-19 20:51_

---

_Review requested from @dcreager by @pieterh-oai on 2025-12-19 20:51_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 20:53_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-19 20:54_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Top[Index[Any]] | Top[Series[Any, Any]] | ... omitted 7 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any], generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`

jax (https://github.com/google/jax)
+ jax/_src/tree_util.py:295:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:298:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:301:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2802 diagnostics
+ Found 2805 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14425 diagnostics
+ Found 14426 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_@pieterh-oai reviewed on 2025-12-19 20:55_

---

_Review comment by @pieterh-oai on `crates/ruff_linter/src/external/ast/registry.rs`:25 on 2025-12-19 20:55_

Changed this to IndexVec with a newtype equivalent.

---

_Comment by @pieterh-oai on 2025-12-19 20:56_

(Rebase; IndexVec per @MichaReiser )

---

_Review request for @carljm removed by @carljm on 2025-12-24 01:54_

---
