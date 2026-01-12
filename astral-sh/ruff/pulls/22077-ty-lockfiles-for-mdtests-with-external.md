```yaml
number: 22077
title: "[ty] Lockfiles for mdtests with external dependencies"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/mdtest-lockfiles
created_at: 2025-12-19T10:51:18Z
updated_at: 2025-12-19T13:46:19Z
url: https://github.com/astral-sh/ruff/pull/22077
synced_at: 2026-01-12T15:57:41Z
```

# [ty] Lockfiles for mdtests with external dependencies

---

_@sharkdp_

## Summary

Add lockfiles for all mdtests which make use of external dependencies. When running tests normally, we use this lockfile when creating the temporary venv using `uv sync --locked`. A new `MDTEST_UPGRADE_LOCKFILES` environment variable is used to switch to a mode in which those lockfiles can be updated or regenerated. When using the Python mdtest runner, this environment variable is automatically set (because we use this command while developing, not to simulate exactly what happens in CI). A command-line flag is provided to opt out of this.

## Test Plan

### Using the mdtest runner

#### Adding a new test (no lockfile yet)

* Removed `attrs.lock` to simulate this
* Ran `uv run crates/ty_python_semantic/mdtest.py -e external/`. The lockfile is generated and the test succeeds.

#### Upgrading/downgrading a dependency

* Changed pydantic requirement from `pydantic==2.12.2` to `pydantic==2.12.5` (also tested with `2.12.0`)
* Ran `uv run crates/ty_python_semantic/mdtest.py -e external/`. The lockfile is updated and the test succeeds.

### Using cargo

#### Adding a new test (no lockfile yet)

* Removed `attrs.lock` to simulate this
* Ran `MDTEST_EXTERNAL=1 cargo test -p ty_python_semantic --test mdtest mdtest__external` "naively", which outputs:
   > Failed to setup in-memory virtual environment with dependencies: Lockfile not found at '/home/shark/ruff/crates/ty_python_semantic/resources/mdtest/external/attrs.lock'. Run with `MDTEST_UPGRADE_LOCKFILES=1` to generate it.
* Ran `MDTEST_UPGRADE_LOCKFILES=1 MDTEST_EXTERNAL=1 cargo test -p ty_python_semantic --test mdtest mdtest__external`. The lockfile is updated and the test succeeds.

#### Upgrading/downgrading a dependency

* Changed pydantic requirement from `pydantic==2.12.2` to `pydantic==2.12.5` (also tested with `2.12.0`)
* Ran `MDTEST_EXTERNAL=1 cargo test -p ty_python_semantic --test mdtest mdtest__external` "naively", which outputs a similar error message as above.
* Ran the command suggested in the error message (`MDTEST_EXTERNAL=1 MDTEST_UPGRADE_LOCKFILES=1 cargo test -p ty_python_semantic --test mdtest mdtest__external`). The lockfile is updated and the test succeeds.

---

_Label `testing` added by @sharkdp on 2025-12-19 10:51_

---

_Label `ty` added by @sharkdp on 2025-12-19 10:51_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 10:52_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-19 10:54_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables` is not assignable to `dict[str, Any]`
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

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 43 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any], generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any], generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1223:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5085 diagnostics
+ Found 5086 diagnostics

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:295:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:298:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:301:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2802 diagnostics
+ Found 2799 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14416 diagnostics
+ Found 14415 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @sharkdp on 2025-12-19 11:12_

---

_Review requested from @carljm by @sharkdp on 2025-12-19 11:12_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-12-19 11:12_

---

_Review requested from @dcreager by @sharkdp on 2025-12-19 11:12_

---

_Review requested from @MichaReiser by @sharkdp on 2025-12-19 11:12_

---

_@MichaReiser reviewed on 2025-12-19 11:51_

---

_Review comment by @MichaReiser on `crates/ty_test/src/external_dependencies.rs`:17 on 2025-12-19 11:51_

Noo `Path` 😆 

---

_@MichaReiser reviewed on 2025-12-19 11:52_

---

_Review comment by @MichaReiser on `crates/ty_test/src/external_dependencies.rs`:17 on 2025-12-19 11:52_

It should be easy to use `SystemPath` or `Utf8Path` here, given that `lockfile_path` is an `Utf8PathBuf`

---

_Review comment by @MichaReiser on `crates/ty_test/src/external_dependencies.rs`:134 on 2025-12-19 11:54_

Should this fail if the lock file is missing?

---

_@MichaReiser approved on 2025-12-19 11:54_

Nice

---

_Review comment by @AlexWaygood on `crates/ty_test/src/external_dependencies.rs`:109 on 2025-12-19 12:22_

uff, maybe we should ask uv to add a dedicated error code for this so we don't need to parse their stderr output 😆

---

_@AlexWaygood reviewed on 2025-12-19 12:22_

---

_@AlexWaygood approved on 2025-12-19 12:23_

Looks reasonable to me, thank you!

---

_@sharkdp reviewed on 2025-12-19 12:23_

---

_Review comment by @sharkdp on `crates/ty_test/src/external_dependencies.rs`:109 on 2025-12-19 12:23_

Yeah. It's not great... but it's also not the end of the world if this detection fails to work.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/mdtest.py`:1 on 2025-12-19 12:26_

If you pass `--enable-external`, the default is that it will upgrade a lockfile. What does that mean exactly? Does that mean that it will, by default, bump the dependency pins in that lockfile? If so then that feels like an odd default -- I think you want the dependency pins to remain the same by default, otherwise there's not much point in a lockfile?

---

_@AlexWaygood reviewed on 2025-12-19 12:26_

---

_@sharkdp reviewed on 2025-12-19 12:40_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/mdtest.py`:1 on 2025-12-19 12:40_

Hm, my understanding was different. Quoting from https://docs.astral.sh/uv/concepts/projects/sync/#upgrading-locked-package-versions:

> With an existing `uv.lock` file, uv will prefer the previously locked versions of packages when running `uv sync` and `uv lock`. Package versions will only change if the project's dependency constraints exclude the previous, locked version.

This means that the lockfile will not change *unless you change the version constraints in the Markdown file*. And in that latter case, it seemed like a good default to make that opt-out when running via mdtest.py.

In this sense `--locked` does *not* lead to a different environment in the case of success (which is different from `cargo`, right?).

I'll clarify with someone from the uv team :smile: 

---

_@AlexWaygood reviewed on 2025-12-19 12:44_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/mdtest.py`:1 on 2025-12-19 12:44_

To be clear, I definitely don't consider myself an expert on uv's lockfiles 😄 but whatever the case, I think it might be good to add a comment saying exactly what the default is for this flag, and what that actually means in terms of the practical impliciations (even if it's just a link to the uv docs). I can never remember exactly what "upgrading" a uv lockfile implies.

---

_@sharkdp reviewed on 2025-12-19 13:19_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/mdtest.py`:1 on 2025-12-19 13:19_

I changed the help text to:
```
  --no-lockfile-upgrades
        By default, lockfiles will be upgraded when dependency requirements in the
        Markdown test change. Set this flag to never upgrade any lockfiles.
```

I also checked with Konsti that my understanding here is correct. If you don't change the requirements, the default option should also never change and lockfiles. And you will still get reproducible venvs.

---

_Merged by @sharkdp on 2025-12-19 13:29_

---

_Closed by @sharkdp on 2025-12-19 13:29_

---

_Branch deleted on 2025-12-19 13:29_

---

_@AlexWaygood reviewed on 2025-12-19 13:46_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/mdtest.py`:1 on 2025-12-19 13:46_

thank you

---
