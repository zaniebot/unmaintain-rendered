```yaml
number: 22268
title: Update Rust crate insta to v1.45.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/insta-1.x-lockfile
created_at: 2025-12-29T16:14:21Z
updated_at: 2025-12-29T16:37:21Z
url: https://github.com/astral-sh/ruff/pull/22268
synced_at: 2026-01-12T15:57:45Z
```

# Update Rust crate insta to v1.45.0

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change | Pending |
|---|---|---|---|---|
| [insta](https://insta.rs/) ([source](https://redirect.github.com/mitsuhiko/insta)) | workspace.dependencies | minor | `1.43.2` -> `1.45.0` | `1.45.1` |

---

### Release Notes

<details>
<summary>mitsuhiko/insta (insta)</summary>

### [`v1.45.0`](https://redirect.github.com/mitsuhiko/insta/blob/HEAD/CHANGELOG.md#1450)

[Compare Source](https://redirect.github.com/mitsuhiko/insta/compare/1.44.3...1.45.0)

- Add external diff tool support via `INSTA_DIFF_TOOL` environment variable. When set, insta uses the specified tool (e.g., `delta`, `difftastic`) to display snapshot diffs instead of the built-in diff. The tool is invoked as `<tool> <old_file> <new_file>`. [#&#8203;844](https://redirect.github.com/mitsuhiko/insta/issues/844)
- Add `test.disable_nextest_doctest` config option to `insta.yaml`, allowing users to silence the nextest doctest warning via config instead of passing `--dnd` every time. [#&#8203;842](https://redirect.github.com/mitsuhiko/insta/issues/842)
- Skip non-insta snapshot files in unreferenced detection. Projects using both insta and other snapshot tools (like vitest or jest) can now use `--unreferenced=reject` without false positives on `.snap` files from other tools. [#&#8203;846](https://redirect.github.com/mitsuhiko/insta/issues/846)
- Collect warnings from tests for display after run. Ensures deprecation warnings are visible even when nextest suppresses stdout/stderr from passing tests. [#&#8203;840](https://redirect.github.com/mitsuhiko/insta/issues/840)
- Update TOML serialization to be up-to-date and backwards-compatible. [#&#8203;834](https://redirect.github.com/mitsuhiko/insta/issues/834)
- Support `clippy::needless_raw_strings` lint by only using raw strings when content contains backslashes or quotes. [#&#8203;828](https://redirect.github.com/mitsuhiko/insta/issues/828)

### [`v1.44.3`](https://redirect.github.com/mitsuhiko/insta/blob/HEAD/CHANGELOG.md#1443)

[Compare Source](https://redirect.github.com/mitsuhiko/insta/compare/1.44.2...1.44.3)

- Fix a regression in 1.44.2 where merge conflict detection was too aggressive, incorrectly flagging snapshot content containing `======` or similar patterns as conflicts. [#&#8203;832](https://redirect.github.com/mitsuhiko/insta/issues/832)
- Fix a regression in 1.42.2 where inline snapshot updates would corrupt the file when code preceded the macro (e.g., `let output = assert_snapshot!(...)`). [#&#8203;833](https://redirect.github.com/mitsuhiko/insta/issues/833)

### [`v1.44.2`](https://redirect.github.com/mitsuhiko/insta/blob/HEAD/CHANGELOG.md#1442)

[Compare Source](https://redirect.github.com/mitsuhiko/insta/compare/1.44.1...1.44.2)

- Fix a rare backward compatibility issue where inline snapshots using an uncommon legacy format (single-line content stored in multiline raw strings) could fail to match after 1.44.0. [#&#8203;830](https://redirect.github.com/mitsuhiko/insta/issues/830)
- Handle merge conflicts in snapshot files gracefully. When a snapshot file contains git merge conflict markers, insta now detects them and treats the snapshot as missing, allowing tests to continue and create a new pending snapshot for review. [#&#8203;829](https://redirect.github.com/mitsuhiko/insta/issues/829)
- Skip nextest\_doctest tests when cargo-nextest is not installed. [#&#8203;826](https://redirect.github.com/mitsuhiko/insta/issues/826)
- Fix functional tests failing under nextest due to inherited `NEXTEST_RUN_ID` environment variable. [#&#8203;824](https://redirect.github.com/mitsuhiko/insta/issues/824)

### [`v1.44.1`](https://redirect.github.com/mitsuhiko/insta/blob/HEAD/CHANGELOG.md#1441)

[Compare Source](https://redirect.github.com/mitsuhiko/insta/compare/1.44.0...1.44.1)

- Add `--dnd` alias for `--disable-nextest-doctest` flag to make it easier to silence the deprecation warning. [#&#8203;822](https://redirect.github.com/mitsuhiko/insta/issues/822)
- Update cargo-dist to 0.30.2 and fix Windows runner to use windows-2022. [#&#8203;821](https://redirect.github.com/mitsuhiko/insta/issues/821)

### [`v1.44.0`](https://redirect.github.com/mitsuhiko/insta/blob/HEAD/CHANGELOG.md#1440)

[Compare Source](https://redirect.github.com/mitsuhiko/insta/compare/1.43.2...1.44.0)

- Added non-interactive snapshot review and reject modes for use in non-TTY environments
  (LLMs, CI pipelines, scripts). `cargo insta review --snapshot <path>` and
  `cargo insta reject --snapshot <path>` now work without a terminal. Enhanced
  `pending-snapshots` output with usage instructions and workspace-relative paths. [#&#8203;815](https://redirect.github.com/mitsuhiko/insta/issues/815)
- Add `--disable-nextest-doctest` flag to `cargo insta test` to disable running doctests with
  nextest. Shows a deprecation warning when nextest is used with doctests without this flag, to prepare `cargo insta` to no longer run
  a separate doctest process when using nextest in the future. [#&#8203;803](https://redirect.github.com/mitsuhiko/insta/issues/803)
- Add ergonomic `--test-runner-fallback` / `--no-test-runner-fallback` flags to `cargo insta test`. [#&#8203;811](https://redirect.github.com/mitsuhiko/insta/issues/811)
- Apply redactions to snapshot metadata. [#&#8203;813](https://redirect.github.com/mitsuhiko/insta/issues/813)
- Remove confusing 'previously unseen snapshot' message. [#&#8203;812](https://redirect.github.com/mitsuhiko/insta/issues/812)
- Speed up JSON float rendering. [#&#8203;806](https://redirect.github.com/mitsuhiko/insta/issues/806) ([@&#8203;nyurik](https://redirect.github.com/nyurik))
- Allow globset version up to 0.4.16. [#&#8203;810](https://redirect.github.com/mitsuhiko/insta/issues/810) ([@&#8203;g0hl1n](https://redirect.github.com/g0hl1n))
- Improve documentation. [#&#8203;814](https://redirect.github.com/mitsuhiko/insta/issues/814) ([@&#8203;tshepang](https://redirect.github.com/tshepang))
- We no longer trim starting newlines during assertions, which allows asserting
  the number of leading newlines match. Existing assertions with different
  leading newlines will pass and print a warning suggesting running with
  `--force-update-snapshots`.  They may fail in the future.  (Note that we still
  currently allow differing *trailing* newlines, though may adjust this in the
  future).  [#&#8203;563](https://redirect.github.com/mitsuhiko/insta/issues/563)

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/ruff).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0Mi41OS4wIiwidXBkYXRlZEluVmVyIjoiNDIuNTkuMCIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2025-12-29 16:14_

---

_Comment by @astral-sh-bot[bot] on 2025-12-29 16:15_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-29 16:17_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2802 diagnostics
+ Found 2799 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
+ sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
- sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
+ sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
- sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
+ sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14450 diagnostics
+ Found 14449 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Merged by @MichaReiser on 2025-12-29 16:37_

---

_Closed by @MichaReiser on 2025-12-29 16:37_

---

_Branch deleted on 2025-12-29 16:37_

---
