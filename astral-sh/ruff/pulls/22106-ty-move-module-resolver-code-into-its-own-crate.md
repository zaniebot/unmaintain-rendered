```yaml
number: 22106
title: "[ty] Move module resolver code into its own crate"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/split-out-resolver
created_at: 2025-12-20T09:11:20Z
updated_at: 2025-12-21T11:00:35Z
url: https://github.com/astral-sh/ruff/pull/22106
synced_at: 2026-01-12T15:57:41Z
```

# [ty] Move module resolver code into its own crate

---

_@MichaReiser_

## Summary

Extracts the module resolver into its own `ty_module_resolver` crate to reduce the size 
of `ty_python_semantic`. 

Closes https://github.com/astral-sh/ty/issues/2120

The only downsides of this are:

* A few more APIs are now `pub` instead of `pub(crate)` 
* Requires some boilerplate for `Db` and `TestDb`

## Test plan

CI

---

_Label `internal` added by @MichaReiser on 2025-12-20 09:11_

---

_Label `ty` added by @MichaReiser on 2025-12-20 09:11_

---

_Comment by @astral-sh-bot[bot] on 2025-12-20 09:13_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_@MichaReiser reviewed on 2025-12-20 09:13_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto_definition.rs`:1128 on 2025-12-20 09:13_

Hmm, what did Claude do that this snapshot changed

---

_Comment by @astral-sh-bot[bot] on 2025-12-20 09:14_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5744:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataarray.py:5744:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
- xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14423 diagnostics
+ Found 14424 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@MichaReiser reviewed on 2025-12-20 09:16_

---

_Review comment by @MichaReiser on `crates/ty_module_resolver/src/list.rs`:1465 on 2025-12-20 09:16_

Why did claude delete these tests?

---

_@MichaReiser reviewed on 2025-12-20 09:16_

---

_Review comment by @MichaReiser on `crates/ty_module_resolver/src/module.rs`:332 on 2025-12-20 09:16_

Can we just define those unconditionally

---

_@MichaReiser reviewed on 2025-12-20 09:17_

---

_Review comment by @MichaReiser on `crates/ty_module_resolver/src/module.rs`:375 on 2025-12-20 09:17_

Is it necessary to make all of those methods pub?

---

_@MichaReiser reviewed on 2025-12-20 09:17_

---

_Review comment by @MichaReiser on `crates/ty_module_resolver/src/path.rs`:445 on 2025-12-20 09:17_

Ugh, I hate this

---

_@MichaReiser reviewed on 2025-12-20 09:17_

---

_Review comment by @MichaReiser on `crates/ty_module_resolver/src/path.rs`:539 on 2025-12-20 09:17_

Is it necessary to make all these methods pub?

---

_@MichaReiser reviewed on 2025-12-20 09:18_

---

_Review comment by @MichaReiser on `crates/ty_module_resolver/src/resolve.rs`:557 on 2025-12-20 09:18_

This moved to `SearchPathSettings::to_search_paths`

---

_@MichaReiser reviewed on 2025-12-20 09:19_

---

_Review comment by @MichaReiser on `crates/ty_module_resolver/src/resolve.rs`:549 on 2025-12-20 09:19_

Feels a bit unnecessary to have a `build` method. Why not just have it on `builder`?

---

_@MichaReiser reviewed on 2025-12-20 09:21_

---

_Review comment by @MichaReiser on `crates/ty_module_resolver/src/testing.rs`:252 on 2025-12-20 09:21_

I don't like how verbose claude made all those tests

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/lib.rs`:38 on 2025-12-20 09:21_

This feels unnecessary

---

_@MichaReiser reviewed on 2025-12-20 09:21_

---

_@MichaReiser reviewed on 2025-12-20 09:22_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:19 on 2025-12-20 09:22_

This shouldn't re-export the symbols

---

_@MichaReiser reviewed on 2025-12-20 09:22_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:24 on 2025-12-20 09:22_

Why pub?

---

_Comment by @astral-sh-bot[bot] on 2025-12-20 09:28_


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

_@MichaReiser reviewed on 2025-12-20 16:35_

---

_Review comment by @MichaReiser on `crates/ty_module_resolver/src/path.rs`:400 on 2025-12-20 16:35_

I split this into two error codes. One used by the `SearchPath` factory methods and one by `SearchPaths::from_settings`. Mainly because I first didn't know if I can move `SearchPathSettings` too because of the dependency on `SitePackagesDiscoveryError`. However, I later realized (due to using different error enums) that the `SitePackagesDiscoveryError` variant is unused. 

So, while this change is no longer technically necessary, I do think that splitting it simplifies understanding the error root causes.

---

_Marked ready for review by @MichaReiser on 2025-12-20 17:03_

---

_Review requested from @carljm by @MichaReiser on 2025-12-20 17:03_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-20 17:03_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-20 17:03_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-20 17:03_

---

_Review comment by @AlexWaygood on `crates/ty_module_resolver/src/path.rs`:492 on 2025-12-20 17:38_

```suggestion
                SearchPathError::NoStdlibSubdirectory(_) => err,
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/db.rs`:8 on 2025-12-20 17:42_

you can simplify slightly -- everything compiles if I apply this to your branch:

```diff
diff --git a/crates/ty_python_semantic/src/db.rs b/crates/ty_python_semantic/src/db.rs
index f8e324f1e7..2feaf6befb 100644
--- a/crates/ty_python_semantic/src/db.rs
+++ b/crates/ty_python_semantic/src/db.rs
@@ -1,11 +1,10 @@
 use crate::lint::{LintRegistry, RuleSelection};
-use ruff_db::Db as SourceDb;
 use ruff_db::files::File;
 use ty_module_resolver::Db as ModuleResolverDb;
 
 /// Database giving access to semantic information about a Python program.
 #[salsa::db]
-pub trait Db: SourceDb + ModuleResolverDb {
+pub trait Db: ModuleResolverDb {
     /// Returns `true` if the file should be checked.
     fn should_check_file(&self, file: File) -> bool;
```

---

_@AlexWaygood approved on 2025-12-20 17:42_

LGTM

---

_@charliermarsh approved on 2025-12-20 18:58_

---

_Merged by @MichaReiser on 2025-12-21 11:00_

---

_Closed by @MichaReiser on 2025-12-21 11:00_

---

_Branch deleted on 2025-12-21 11:00_

---
