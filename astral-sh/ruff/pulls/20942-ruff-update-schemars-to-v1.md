```yaml
number: 20942
title: "[`ruff`] Update schemars to v1"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - internal
assignees: []
merged: true
base: main
head: schemars-v1-upgrade
created_at: 2025-10-17T15:41:56Z
updated_at: 2025-10-20T07:05:38Z
url: https://github.com/astral-sh/ruff/pull/20942
synced_at: 2026-01-12T15:57:12Z
```

# [`ruff`] Update schemars to v1

---

_@TaKO8Ki_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

FIxes #19173

Update schemars to v1

**I’ve verified that the fundamentals of ruff.schema.json and ty.schema.json remain unchanged following the Schemars upgrade.**

The migration is based on https://graham.cool/schemars/migrating.

### Shared Changes

* ~~Upgraded both schemas to **JSON Schema draft 2020-12** andmigrated shared types into **`$defs`**, with all `$ref` paths now using `#/$defs/...`.~~
* Added reusable helpers such as `$defs/string` (and array helpers like `Array_of_string`) to avoid duplicating scalar/collection shapes.
* Reformatted descriptions with consistent newline breaks. **[Schemars 1.0]** — since **v1.0.0-alpha.3**, Schemars stops collapsing doc-comment whitespace; multi-line Rust docs flow through to `description` verbatim. ref: https://github.com/GREsau/schemars/issues/120
* Kept existing **deprecated** flags but moved them earlier within each property object to match the new formatting. 

### ty.schema.json

* Introduced `$defs` entries for path-related types (`RelativePathBuf`, `SystemPathBuf`) and updated path properties to reference them.
* Redirected overrides to a new `OverridesOptions` array definition with expanded multi-line documentation and examples, while maintaining the original rule descriptions under the updated formatting.

## Test Plan

<!-- How was it tested? -->

Check if existing tests pass.


---

_Comment by @github-actions[bot] on 2025-10-17 15:47_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-17 15:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-17 16:11_

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

_@TaKO8Ki reviewed on 2025-10-17 18:10_

---

_Review comment by @TaKO8Ki on `ruff.schema.json`:1408 on 2025-10-17 18:10_

These changes looks weird, but they refer to `Name` struct, which means String in schemars. That is why they use `defs/string` instead of `string`.

https://github.com/astral-sh/ruff/blob/c4240076459bc9128fee6f83cbf0f51d134b663b/crates/ruff_workspace/src/options.rs#L1970-L1977

https://github.com/astral-sh/ruff/blob/c4240076459bc9128fee6f83cbf0f51d134b663b/crates/ruff_workspace/src/options.rs#L1960-L1968

https://github.com/astral-sh/ruff/blob/c4240076459bc9128fee6f83cbf0f51d134b663b/crates/ruff_python_ast/src/name.rs#L204-L227

---

_@TaKO8Ki reviewed on 2025-10-17 18:11_

---

_Review comment by @TaKO8Ki on `ruff.schema.json`:1178 on 2025-10-17 18:11_

Same as below.

https://github.com/astral-sh/ruff/pull/20942#discussion_r2440788641

---

_@TaKO8Ki reviewed on 2025-10-17 18:12_

---

_Review comment by @TaKO8Ki on `ty.schema.json`:1137 on 2025-10-17 18:12_

This is related to https://github.com/astral-sh/ruff/pull/20942#discussion_r2440788641

---

_@TaKO8Ki reviewed on 2025-10-17 18:13_

---

_Review comment by @TaKO8Ki on `ruff.schema.json`:4353 on 2025-10-17 18:13_

This is related to https://github.com/astral-sh/ruff/pull/20942#discussion_r2440788641.

---

_@TaKO8Ki reviewed on 2025-10-17 18:25_

---

_Review comment by @TaKO8Ki on `ty.schema.json`:1093 on 2025-10-17 18:25_

With **Schemars 1.0** (defaulting to JSON Schema **2020-12**), named newtypes are **emitted as reusable `$ref`s in `#/$defs` by default**, even when `#[serde(transparent)]` makes their schema equal to the inner field. This preserves **type identity** and **deduplicates** shared shapes.

https://github.com/astral-sh/ruff/blob/c4240076459bc9128fee6f83cbf0f51d134b663b/crates/ty_project/src/metadata/value.rs#L330-L332

---

_@TaKO8Ki reviewed on 2025-10-17 18:26_

---

_Review comment by @TaKO8Ki on `ty.schema.json`:1111 on 2025-10-17 18:26_

This is related to https://github.com/astral-sh/ruff/pull/20942#discussion_r2440836378.

---

_Marked ready for review by @TaKO8Ki on 2025-10-17 18:27_

---

_Review requested from @carljm by @TaKO8Ki on 2025-10-17 18:27_

---

_Review requested from @AlexWaygood by @TaKO8Ki on 2025-10-17 18:27_

---

_Review requested from @sharkdp by @TaKO8Ki on 2025-10-17 18:27_

---

_Review requested from @dcreager by @TaKO8Ki on 2025-10-17 18:27_

---

_Review requested from @MichaReiser by @TaKO8Ki on 2025-10-17 18:27_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-10-17 18:36_

---

_Review comment by @MichaReiser on `ty.schema.json`:259 on 2025-10-19 10:25_

The escaping here looks incorrect to me

---

_@MichaReiser reviewed on 2025-10-19 10:25_

---

_@MichaReiser reviewed on 2025-10-19 10:34_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/name.rs`:206 on 2025-10-19 10:34_

I think we could replace this with

```rust
#[cfg_attr(
    feature = "serde",
    derive(serde::Serialize, serde::Deserialize),
    serde(transparent)
)]
#[cfg_attr(feature = "cache", derive(ruff_macros::CacheKey))]
#[cfg_attr(feature = "salsa", derive(salsa::Update))]
#[cfg_attr(feature = "get-size", derive(get_size2::GetSize))]
#[cfg_attr(
    feature = "schemars",
    derive(schemars::JsonSchema),
    schemars(with = "String")
)]
```

(serde(transparent) and `schemars(with="String")`)

---

_@MichaReiser reviewed on 2025-10-19 10:35_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/python_version.rs`:206 on 2025-10-19 10:35_

Nit
```suggestion
            let mut any_of: Vec<Value> = vec![
            	schemars::json_schema!({
	                "type": "string",
	                "pattern": r"^\\d+\\.\\d+$",
	            })
            ];
```

---

_@MichaReiser reviewed on 2025-10-19 10:37_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:3942 on 2025-10-19 10:37_

What's the reason for changing those fields to a `BTreeMap`?

---

_@MichaReiser reviewed on 2025-10-19 10:38_

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:802 on 2025-10-19 10:38_

Nit: I'd prefer to keep this in its own `mod` (and in the same position, it makes attributing changes with git easier)

---

_@MichaReiser reviewed on 2025-10-19 10:39_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/python_platform.rs`:81 on 2025-10-19 10:39_

Can we preserve this comment

---

_@MichaReiser reviewed on 2025-10-19 10:41_

---

_Review comment by @MichaReiser on `ruff.schema.json`:2 on 2025-10-19 10:41_

SchemaStore's recommendation is to still use draft-07 because many editors lack support for newer drafts, see 

> We recommend using the draft-07 JSON schema version. Later versions of JSON Schema are not yet recommended for use in SchemaStore until IDE and language support improves for those versions.

https://github.com/SchemaStore/schemastore/blob/master/CONTRIBUTING.md#best-practices

That's why I think we should not change the schema version as part of this PR


---

_@MichaReiser reviewed on 2025-10-19 10:47_

Thank you. This looks great overall. My only concern is the bump of the schema version. SchemaStore recommends draft-07 for best editor compatibility. I suggest we defer the schema version bump as it requires extensive testing across editors.

---

_Label `internal` added by @MichaReiser on 2025-10-19 10:47_

---

_@TaKO8Ki reviewed on 2025-10-19 11:42_

---

_Review comment by @TaKO8Ki on `ruff.schema.json`:2 on 2025-10-19 11:42_

Ok. It's also possible to continue to use draft-07. I will replace the schema version.

---

_@TaKO8Ki reviewed on 2025-10-19 11:53_

---

_Review comment by @TaKO8Ki on `crates/ty_project/src/metadata/options.rs`:802 on 2025-10-19 11:53_

What I want to do is implementing JsonSchema for Rules instead of `schemars(with = "schema::Rules")`, so I will keep the mod and implement it inside the module.

---

_Comment by @TaKO8Ki on 2025-10-20 06:54_

@MichaReiser Thank you for the review. I have addressed your comments!

---

_Comment by @MichaReiser on 2025-10-20 06:58_

This is great. Thank you so much!

---

_Merged by @MichaReiser on 2025-10-20 06:59_

---

_Closed by @MichaReiser on 2025-10-20 06:59_

---

_Branch deleted on 2025-10-20 07:00_

---
