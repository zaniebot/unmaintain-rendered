```yaml
number: 21443
title: "[ty] Propagate type context through conditional expressions"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/conditional-expr-tcx
created_at: 2025-11-14T02:12:29Z
updated_at: 2025-11-14T20:19:10Z
url: https://github.com/astral-sh/ruff/pull/21443
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Propagate type context through conditional expressions

---

_Pull request opened by @ibraheemdev on 2025-11-14 02:12_

## Summary

Resolves https://github.com/astral-sh/ty/issues/1543.

---

_Review requested from @carljm by @ibraheemdev on 2025-11-14 02:12_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-11-14 02:12_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-11-14 02:12_

---

_Review requested from @dcreager by @ibraheemdev on 2025-11-14 02:12_

---

_Label `ty` added by @ibraheemdev on 2025-11-14 02:12_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-11-14 02:13_

---

_Comment by @astral-sh-bot[bot] on 2025-11-14 02:14_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-14 02:15_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
kopf (https://github.com/nolar/kopf)
- kopf/_cogs/clients/creating.py:21:5: error[invalid-assignment] Object of type `(RawBody & ~None) | dict[Unknown, Unknown]` is not assignable to `RawBody | None`
- kopf/_cogs/clients/creating.py:23:9: warning[possibly-missing-attribute] Attribute `setdefault` may be missing on object of type `RawBody | None`
- kopf/_cogs/clients/creating.py:25:9: warning[possibly-missing-attribute] Attribute `setdefault` may be missing on object of type `RawBody | None`
- kopf/_cogs/clients/creating.py:27:44: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `RawBody | None`
- Found 64 diagnostics
+ Found 60 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/cargo/manifest.py:304:16: error[missing-typed-dict-key] Missing required key 'workspace' in TypedDict `FromWorkspace` constructor
+ mesonbuild/cargo/manifest.py:304:17: error[invalid-key] Invalid key for TypedDict `FromWorkspace`: Unknown key "version"
- Found 1726 diagnostics
+ Found 1728 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/plugins/worksearch/code.py:446:21: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 944 diagnostics
+ Found 945 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- src/hydra_zen/structured_configs/_implementations.py:3506:9: error[invalid-assignment] Object of type `@Todo | dict[Unknown, Unknown]` is not assignable to `DataclassOptions`
- Found 548 diagnostics
+ Found 547 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/cli/deploy/_core.py:246:63: error[invalid-argument-type] Argument to function `_generate_actions_for_remote_flow_storage` is incorrect: Expected `list[dict[str, Any]]`, found `(dict[str, Any] & ~AlwaysFalsy) | dict[Unknown, Unknown]`
+ src/prefect/cli/deploy/_core.py:246:63: error[invalid-argument-type] Argument to function `_generate_actions_for_remote_flow_storage` is incorrect: Expected `list[dict[str, Any]]`, found `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:266:13: error[invalid-argument-type] Argument to function `_generate_default_pull_action` is incorrect: Expected `list[dict[str, Any]]`, found `(dict[str, Any] & ~AlwaysFalsy) | dict[Unknown, Unknown] | dict[str, list[dict[str, Any]]]`
+ src/prefect/cli/deploy/_core.py:266:13: error[invalid-argument-type] Argument to function `_generate_default_pull_action` is incorrect: Expected `list[dict[str, Any]]`, found `dict[str, Any] | dict[str, list[dict[str, Any]]]`

altair (https://github.com/vega/altair)
- altair/datasets/_reader.py:177:9: error[invalid-assignment] Object of type `(Metadata & Top[Mapping[Unknown, object]]) | (Path & Top[Mapping[Unknown, object]]) | (str & Top[Mapping[Unknown, object]]) | dict[Unknown | str, Unknown]` is not assignable to `Metadata | Path | str`
- altair/datasets/_reader.py:178:28: error[invalid-argument-type] Argument to bound method `_solve` is incorrect: Expected `Metadata`, found `Metadata | Path | str`
+ altair/datasets/_reader.py:178:28: error[invalid-argument-type] Argument to bound method `_solve` is incorrect: Expected `Metadata`, found `Metadata | (Path & Top[Mapping[Unknown, object]]) | (str & Top[Mapping[Unknown, object]])`
- Found 1020 diagnostics
+ Found 1019 diagnostics

streamlit (https://github.com/streamlit/streamlit)
- lib/streamlit/elements/widgets/data_editor.py:144:9: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | dict[Unknown, Unknown] | list[Unknown]] | Any` is not assignable to `EditingState`
- Found 138 diagnostics
+ Found 137 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/resources.py:316:9: error[invalid-assignment] Object of type `list[Literal["bokeh", "bokeh-gl", "bokeh-widgets", "bokeh-tables", "bokeh-mathjax", "bokeh-api"]] | list[str]` is not assignable to attribute `components` of type `list[Literal["bokeh", "bokeh-gl", "bokeh-widgets", "bokeh-tables", "bokeh-mathjax", "bokeh-api"]]`
- Found 645 diagnostics
+ Found 644 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/db/upgrades/v45_v46.py:121:25: error[invalid-argument-type] Argument to function `maybe_set_transaction_extra_data` is incorrect: Expected `AssetMovementExtraData | None`, found `dict[Unknown | str, Unknown] | None`
- rotkehlchen/exchanges/coinbase.py:1063:21: error[invalid-argument-type] Argument to function `maybe_set_transaction_extra_data` is incorrect: Expected `AssetMovementExtraData | None`, found `dict[Unknown | str, Unknown] | None`
- Found 2137 diagnostics
+ Found 2135 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-14 02:20_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 2 | 3 |
| `invalid-assignment` | 0 | 5 | 0 |
| `possibly-missing-attribute` | 0 | 3 | 0 |
| `invalid-key` | 1 | 0 | 0 |
| `missing-typed-dict-key` | 1 | 0 | 0 |
| **Total** | **2** | **10** | **3** |

**[Full report with detailed diff](https://ibraheem-conditional-expr-tc.ecosystem-663.pages.dev/diff)** ([timing results](https://ibraheem-conditional-expr-tc.ecosystem-663.pages.dev/timing))




---

_@AlexWaygood approved on 2025-11-14 08:55_

Lovely 

---

_Merged by @ibraheemdev on 2025-11-14 20:19_

---

_Closed by @ibraheemdev on 2025-11-14 20:19_

---

_Branch deleted on 2025-11-14 20:19_

---
