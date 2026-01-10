```yaml
number: 22252
title: Update astral-sh/setup-uv action to v7.1.6
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/astral-sh-setup-uv-7.x
created_at: 2025-12-29T16:11:40Z
updated_at: 2025-12-29T16:37:28Z
url: https://github.com/astral-sh/ruff/pull/22252
synced_at: 2026-01-10T16:36:18Z
```

# Update astral-sh/setup-uv action to v7.1.6

---

_Pull request opened by @renovate on 2025-12-29 16:11_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [astral-sh/setup-uv](https://redirect.github.com/astral-sh/setup-uv) | action | patch | `v7.1.4` -> `v7.1.6` |

---

### Release Notes

<details>
<summary>astral-sh/setup-uv (astral-sh/setup-uv)</summary>

### [`v7.1.6`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v7.1.6): 🌈 add OS version to cache key to prevent binary incompatibility

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v7.1.5...v7.1.6)

##### Changes

This release will invalidate your cache existing keys!

The os version e.g. `ubuntu-22.04` is now part of the cache key. This prevents failing builds when a cache got populated with wheels built with different tools (e.g. glibc) than are present on the runner where the cache got restored.

##### 🐛 Bug fixes

- feat: add OS version to cache key to prevent binary incompatibility [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;716](https://redirect.github.com/astral-sh/setup-uv/issues/716))

##### 🧰 Maintenance

- chore: update known checksums for 0.9.17 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;714](https://redirect.github.com/astral-sh/setup-uv/issues/714))

##### ⬆️ Dependency updates

- Bump actions/checkout from 5.0.0 to 6.0.1 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;712](https://redirect.github.com/astral-sh/setup-uv/issues/712))
- Bump actions/setup-node from 6.0.0 to 6.1.0 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;715](https://redirect.github.com/astral-sh/setup-uv/issues/715))

### [`v7.1.5`](https://redirect.github.com/astral-sh/setup-uv/releases/tag/v7.1.5): 🌈 allow setting `cache-local-path` without `enable-cache: true`

[Compare Source](https://redirect.github.com/astral-sh/setup-uv/compare/v7.1.4...v7.1.5)

##### Changes

[#&#8203;612](https://redirect.github.com/astral-sh/setup-uv/pull/612) fixed a faulty behavior where this action set `UV_CACHE_DIR` even though `enable-cache` was `false`. It also fixed the cases were the cache dir is already configured in a settings file like `pyproject.toml` or `UV_CACHE_DIR` was already set.  Here the action shouldn't overwrite or set `UV_CACHE_DIR`.

These fixes introduced an unwanted behavior: You can still set `cache-local-path` but this action didn't do anything. This release fixes that.

You can now use `cache-local-path` to automatically set `UV_CACHE_DIR` even when `enable-cache` is `false` (or gets set to false by default e.g. on self-hosted runners)

```yaml
- name: This is now possible
  uses: astral-sh/setup-uv@v7
  with:
    enable-cache: false
    cache-local-path: "/path/to/cache"
```

##### 🐛 Bug fixes

- allow cache-local-path w/o enable-cache [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;707](https://redirect.github.com/astral-sh/setup-uv/issues/707))

##### 🧰 Maintenance

- set biome files.maxSize to 2MiB [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;708](https://redirect.github.com/astral-sh/setup-uv/issues/708))
- chore: update known checksums for 0.9.16 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;706](https://redirect.github.com/astral-sh/setup-uv/issues/706))
- chore: update known checksums for 0.9.15 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;704](https://redirect.github.com/astral-sh/setup-uv/issues/704))
- chore: use `npm ci --ignore-scripts` everywhere [@&#8203;woodruffw](https://redirect.github.com/woodruffw) ([#&#8203;699](https://redirect.github.com/astral-sh/setup-uv/issues/699))
- chore: update known checksums for 0.9.14 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;700](https://redirect.github.com/astral-sh/setup-uv/issues/700))
- chore: update known checksums for 0.9.13 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;694](https://redirect.github.com/astral-sh/setup-uv/issues/694))
- chore: update known checksums for 0.9.12 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;693](https://redirect.github.com/astral-sh/setup-uv/issues/693))
- chore: update known checksums for 0.9.11 @&#8203;[github-actions\[bot\]](https://redirect.github.com/apps/github-actions) ([#&#8203;688](https://redirect.github.com/astral-sh/setup-uv/issues/688))

##### ⬆️ Dependency updates

- Bump peter-evans/create-pull-request from 7.0.8 to 7.0.9 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;695](https://redirect.github.com/astral-sh/setup-uv/issues/695))
- bump dependencies [@&#8203;eifinger](https://redirect.github.com/eifinger) ([#&#8203;709](https://redirect.github.com/astral-sh/setup-uv/issues/709))
- Bump github/codeql-action from 4.30.9 to 4.31.6 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;698](https://redirect.github.com/astral-sh/setup-uv/issues/698))
- Bump zizmorcore/zizmor-action from 0.2.0 to 0.3.0 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;696](https://redirect.github.com/astral-sh/setup-uv/issues/696))
- Bump eifinger/actionlint-action from 1.9.2 to 1.9.3 @&#8203;[dependabot\[bot\]](https://redirect.github.com/apps/dependabot) ([#&#8203;690](https://redirect.github.com/astral-sh/setup-uv/issues/690))

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

_Label `internal` added by @renovate[bot] on 2025-12-29 16:11_

---

_Comment by @astral-sh-bot[bot] on 2025-12-29 16:14_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
xarray (https://github.com/pydata/xarray)
- xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`

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
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- Found 1843 diagnostics
+ Found 1844 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14449 diagnostics
+ Found 14450 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-29 16:28_


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

_@woodruffw approved on 2025-12-29 16:37_

---

_Merged by @woodruffw on 2025-12-29 16:37_

---

_Closed by @woodruffw on 2025-12-29 16:37_

---

_Branch deleted on 2025-12-29 16:37_

---
