```yaml
number: 22333
title: "[ty] Override `__file__` to str when applicable on imported modules"
type: pull_request
state: merged
author: sinon
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: type-module-dunder-file-str
created_at: 2026-01-01T20:21:52Z
updated_at: 2026-01-15T18:37:15Z
url: https://github.com/astral-sh/ruff/pull/22333
synced_at: 2026-01-15T18:49:33Z
```

# [ty] Override `__file__` to str when applicable on imported modules

---

_@sinon_

## Summary

Resolves: https://github.com/astral-sh/ty/issues/860

Typeshed defines `__file__` as `str | None` but when `ty` has successfully imported a module we can assumed the modules `__file__` is str.

This is already the case for the `__file__` of the local/current file: https://github.com/astral-sh/ruff/pull/18071

This expands to handle cases like `from mod.a import __file__ as path_of_a`

Notes:
This feels like this logic should ideally be centralized and have the current file and imported files operate in the same way (i.e they all pre-populate a set of known symbol overrides) as currently there is overrides for current file and imported files spread across a few parts of `place.rs` but felt like that refactoring is beyond my current experience with the codebase so simple fix only 😆 


## Test Plan

<!-- How was it tested? -->

Added new mdtest, not sure if there is an existing test file which this can be merged into instead

### Tasks

- ~Failing mdtest which indicates the `__file__` == `str` might not hold up on stubs, need to look into this further and see if this needs to expand the handling.~
- ~Another failing test for the case where a module overrides `__file__` (setting to `None` in the tests case) which the current is ignoring~
- Should we care about `sys` and other similar statically linked std library modules that won't have `__file__`? pyrefly and pyright both reveal `str` in this case even though it will be an error at runtime, so probably fine

## Ecosystem review

A bigger impact than I was expecting though most look good ~though `prefect` stands out as a bit different to the rest.~ 

- `no-matching-overload` - look good these are cases like the original bug where a `__file__` was getting passed into `dirname` and other similar overloadded methods which did not expect to ever receive a `None`.
- `invalid-argument-type` - similarly these look generally correct, mainly cases where a method was expecting something pathlike and didn't want the `| None`
- `possibly-missing-attribute` on string method calls (`split`, `strip`, `rstrip`, etc.)  - generally looks good, these are cases where `string` methods were unhappy with being called with `str | None` but are now working.
- `jax` has a few that don't appear to match the above cases but looks more like a non-determinism case
- ~`prefect` the cases that stand out as potentially problematic are the new `invalid-await`~ `invalid-await` cases have went after now respecting the override definition
- [`mesonbuild`](https://github.com/mesonbuild/meson/blob/4a9075b863c7a5002e51a129a4efa7281aafa0b2/mesonbuild/minstall.py#L25-L30) ~case is interesting as the `except ImportError` isn't experienced by `ty` but only when `mesonbuild` is run on windows. Not sure it's a case to handle in `ty` though 🤔 as the cases where libraries are writing code in expectation that the import succeeds is much more common.~ Resolved by respecting the explicit definition
- New diagnostic on [scikit-learn](https://github.com/scikit-learn/scikit-learn/blob/fafe37e414cf22448e861371c537fa2c476639b7/sklearn/utils/_testing.py#L936) but I am not seeing the connection in the code to `__file__` 🤔 Ah so I think this is because of a [no-matching-overload] going away at https://github.com/scikit-learn/scikit-learn/blob/fafe37e414cf22448e861371c537fa2c476639b7/sklearn/utils/_testing.py#L926 which means `kwargs` is getting a narrower type definition. 



---

_Review requested from @carljm by @sinon on 2026-01-01 20:21_

---

_Review requested from @AlexWaygood by @sinon on 2026-01-01 20:21_

---

_Review requested from @sharkdp by @sinon on 2026-01-01 20:21_

---

_Review requested from @dcreager by @sinon on 2026-01-01 20:21_

---

_Comment by @astral-sh-bot[bot] on 2026-01-01 20:23_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-01 20:24_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pip (https://github.com/pypa/pip)
- src/pip/_internal/build_env.py:70:27: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- src/pip/_internal/utils/virtualenv.py:87:36: error[no-matching-overload] No overload of function `abspath` matches arguments
- Found 590 diagnostics
+ Found 588 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/test/concretization/core.py:4504:25: error[no-matching-overload] No overload of function `dirname` matches arguments
- lib/spack/spack/test/schema.py:244:34: error[no-matching-overload] No overload of function `dirname` matches arguments
- Found 4341 diagnostics
+ Found 4339 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
- src/bandersnatch/tests/test_main.py:69:29: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- src/bandersnatch/tests/test_main.py:138:25: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- Found 80 diagnostics
+ Found 78 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/_code/code.py:1526:20: warning[possibly-missing-attribute] Attribute `rstrip` may be missing on object of type `str | None`
- src/_pytest/_code/code.py:1530:20: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- src/_pytest/fixtures.py:1937:20: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- src/_pytest/nodes.py:54:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- testing/code/test_excinfo.py:187:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- Found 421 diagnostics
+ Found 416 diagnostics

paasta (https://github.com/yelp/paasta)
- paasta_tools/api/api.py:100:23: error[no-matching-overload] No overload of function `dirname` matches arguments
- paasta_tools/utils.py:2364:20: error[no-matching-overload] No overload of function `dirname` matches arguments
- Found 1099 diagnostics
+ Found 1097 diagnostics

flake8 (https://github.com/pycqa/flake8)
- tests/unit/plugins/pycodestyle_test.py:32:15: error[invalid-argument-type] Argument to function `open` is incorrect: Expected `int | str | bytes | PathLike[str] | PathLike[bytes]`, found `str | None`
- Found 38 diagnostics
+ Found 37 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/tools/web/test_app.py:120:27: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- test/mitmproxy/tools/web/test_app.py:120:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- Found 2145 diagnostics
+ Found 2143 diagnostics

mypy (https://github.com/python/mypy)
- mypy/modulefinder.py:857:17: error[no-matching-overload] No overload of function `check_output` matches arguments
- mypy/test/testmypyc.py:16:24: warning[possibly-missing-attribute] Attribute `endswith` may be missing on object of type `str | None`
- mypy/test/testtypes.py:1605:17: warning[possibly-missing-attribute] Attribute `endswith` may be missing on object of type `str | None`
- mypy/test/testtypes.py:1607:19: error[invalid-argument-type] Argument to function `open` is incorrect: Expected `int | str | bytes | PathLike[str] | PathLike[bytes]`, found `str | None`
- Found 1744 diagnostics
+ Found 1740 diagnostics

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

artigraph (https://github.com/artigraph/artigraph)
- tests/arti/internal/test_utils.py:56:20: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- tests/arti/internal/test_utils.py:56:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- Found 147 diagnostics
+ Found 145 diagnostics

comtypes (https://github.com/enthought/comtypes)
- comtypes/server/register.py:317:60: error[invalid-argument-type] Argument to function `_iter_inproc_ctx_entries` is incorrect: Expected `str`, found `str | None`
- Found 400 diagnostics
+ Found 399 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- mkdocs/tests/config/config_options_legacy_tests.py:857:22: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `str | None`
- mkdocs/tests/config/config_options_tests.py:1070:22: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `str | None`
- mkdocs/tests/config/config_tests.py:98:38: error[no-matching-overload] No overload of function `dirname` matches arguments
- mkdocs/tests/theme_tests.py:11:30: error[no-matching-overload] No overload of function `dirname` matches arguments
- Found 227 diagnostics
+ Found 223 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/minstall.py:30:17: error[invalid-assignment] Object of type `None` is not assignable to `str`
- run_tests.py:202:27: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`

pyodide (https://github.com/pyodide/pyodide)
- pyodide-build/pyodide_build/pypabuild.py:266:5: error[no-matching-overload] No overload of function `copy2` matches arguments
- pyodide-build/pyodide_build/pypabuild.py:271:5: error[no-matching-overload] No overload of function `copy2` matches arguments
- pyodide-build/pyodide_build/pywasmcross.py:20:21: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- src/tests/test_stdlib_fixes.py:302:28: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- Found 939 diagnostics
+ Found 935 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/plugins/admin/code.py:186:29: error[no-matching-overload] No overload of function `dirname` matches arguments
- Found 1147 diagnostics
+ Found 1146 diagnostics

xarray (https://github.com/pydata/xarray)
- doc/conf.py:377:36: error[no-matching-overload] No overload of function `dirname` matches arguments
- xarray/core/utils.py:1236:20: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- Found 1763 diagnostics
+ Found 1761 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- cwltool/job.py:1033:29: error[invalid-argument-type] Argument to function `copyfile` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `str | None`
- cwltool/job.py:1036:29: error[invalid-argument-type] Argument to function `copyfile` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `str | None`
- Found 254 diagnostics
+ Found 252 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- tests/helpers.py:11:17: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- tests/integration_tests/modules/test_combined.py:340:27: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- tests/unittests/config/test_cc_ntp.py:274:37: error[no-matching-overload] No overload of function `realpath` matches arguments
- Found 1172 diagnostics
+ Found 1169 diagnostics

setuptools (https://github.com/pypa/setuptools)
- setuptools/_distutils/tests/test_sysconfig.py:296:42: error[no-matching-overload] No overload of function `dirname` matches arguments
- setuptools/command/setopt.py:23:29: error[no-matching-overload] No overload of function `dirname` matches arguments
- Found 1133 diagnostics
+ Found 1131 diagnostics

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
- src/prefect/server/database/alembic_commands.py:44:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- src/prefect/server/database/orm_models.py:1592:18: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- src/prefect/server/database/orm_models.py:1608:18: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
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
- Found 5408 diagnostics
+ Found 5405 diagnostics

pycryptodome (https://github.com/Legrandin/pycryptodome)
- lib/Crypto/SelfTest/Hash/test_BLAKE2.py:319:20: error[no-matching-overload] No overload of function `dirname` matches arguments
- lib/Crypto/SelfTest/Hash/test_BLAKE2.py:377:20: error[no-matching-overload] No overload of function `dirname` matches arguments
- lib/Crypto/SelfTest/Hash/test_BLAKE2.py:437:20: error[no-matching-overload] No overload of function `dirname` matches arguments
- lib/Crypto/SelfTest/Protocol/test_HPKE.py:339:24: error[no-matching-overload] No overload of function `dirname` matches arguments
- lib/Crypto/SelfTest/PublicKey/test_import_Curve25519.py:62:20: error[no-matching-overload] No overload of function `dirname` matches arguments
- lib/Crypto/SelfTest/PublicKey/test_import_Curve448.py:35:20: error[no-matching-overload] No overload of function `dirname` matches arguments
- lib/Crypto/SelfTest/PublicKey/test_import_ECC.py:68:20: error[no-matching-overload] No overload of function `dirname` matches arguments
- lib/Crypto/SelfTest/PublicKey/test_import_RSA.py:54:20: error[no-matching-overload] No overload of function `dirname` matches arguments
- lib/Crypto/SelfTest/loader.py:165:20: error[no-matching-overload] No overload of function `dirname` matches arguments
- lib/Crypto/SelfTest/loader.py:188:20: error[no-matching-overload] No overload of function `dirname` matches arguments
- Found 1336 diagnostics
+ Found 1326 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- src/scikit_build_core/builder/builder.py:186:27: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- Found 48 diagnostics
+ Found 46 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/tests/test_common.py:133:21: error[no-matching-overload] No overload of function `dirname` matches arguments
- sklearn/tests/test_common.py:154:21: error[no-matching-overload] No overload of function `dirname` matches arguments
- sklearn/tests/test_common.py:181:21: error[no-matching-overload] No overload of function `dirname` matches arguments
- sklearn/tests/test_docstring_parameters.py:42:21: error[no-matching-overload] No overload of function `dirname` matches arguments
- sklearn/tests/test_min_dependencies_readme.py:102:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- sklearn/tests/test_min_dependencies_readme.py:205:27: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- sklearn/utils/_testing.py:926:35: error[no-matching-overload] No overload of function `dirname` matches arguments
+ sklearn/utils/_testing.py:936:13: error[invalid-assignment] Cannot assign to a subscript on an object of type `str`
- Found 2429 diagnostics
+ Found 2423 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/bootstrap/sitecustomize.py:74:23: error[no-matching-overload] No overload of function `dirname` matches arguments
- ddtrace/commands/ddtrace_run.py:95:16: error[no-matching-overload] No overload of function `dirname` matches arguments
- ddtrace/internal/utils/config.py:19:12: error[no-matching-overload] No overload of function `basename` matches arguments
- lib-injection/sources/sitecustomize.py:513:62: error[no-matching-overload] No overload of function `dirname` matches arguments
- tests/commands/test_runner.py:25:19: error[no-matching-overload] No overload of function `dirname` matches arguments
- tests/internal/test_auto.py:88:33: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- Found 8419 diagnostics
+ Found 8413 diagnostics

altair (https://github.com/vega/altair)
- tests/vegalite/v6/test_theme.py:1086:17: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- Found 1060 diagnostics
+ Found 1059 diagnostics

jax (https://github.com/google/jax)
- jax/_src/source_info_util.py:53:5: error[no-matching-overload] No overload of function `dirname` matches arguments
- jax/_src/traceback_util.py:40:20: error[invalid-argument-type] Argument to function `register_exclusion` is incorrect: Expected `str`, found `str | None`
- Found 2857 diagnostics
+ Found 2855 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/util/_exceptions.py:45:15: error[no-matching-overload] No overload of function `dirname` matches arguments
- Found 3758 diagnostics
+ Found 3757 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/util/warnings.py:74:15: error[no-matching-overload] No overload of function `dirname` matches arguments
- Found 878 diagnostics
+ Found 877 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/pyspark/tests/conftest.py:289:30: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- ibis/backends/snowflake/tests/test_udf.py:183:21: error[no-matching-overload] No overload of function `dirname` matches arguments
- Found 4607 diagnostics
+ Found 4605 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- Pythonwin/pywin/scintilla/control.py:25:26: error[no-matching-overload] No overload of function `split` matches arguments
- Pythonwin/pywin/scintilla/control.py:34:26: error[no-matching-overload] No overload of function `split` matches arguments
- Pythonwin/pywin/test/test_exe.py:19:21: error[no-matching-overload] No overload of function `dirname` matches arguments
- com/win32com/server/register.py:237:29: error[no-matching-overload] No overload of function `dirname` matches arguments
- com/win32com/server/register.py:240:37: error[no-matching-overload] No overload of function `basename` matches arguments
- com/win32com/test/util.py:50:17: error[no-matching-overload] No overload of function `basename` matches arguments
- com/win32comext/shell/demos/servers/empty_volume_cache.py:77:52: error[no-matching-overload] No overload of function `dirname` matches arguments
- win32/Lib/win32serviceutil.py:51:26: error[no-matching-overload] No overload of function `dirname` matches arguments
- win32/Lib/win32serviceutil.py:144:17: error[no-matching-overload] No overload of function `split` matches arguments
- win32/scripts/pywin32_postinstall.py:362:29: error[no-matching-overload] No overload of function `dirname` matches arguments
- win32/scripts/pywin32_postinstall.py:363:31: error[no-matching-overload] No overload of function `dirname` matches arguments
- win32/scripts/pywin32_postinstall.py:364:28: error[no-matching-overload] No overload of function `dirname` matches arguments
- win32/scripts/regsetup.py:405:25: error[no-matching-overload] No overload of function `split` matches arguments
- win32/scripts/setup_d.py:26:21: error[no-matching-overload] No overload of function `basename` matches arguments
- win32/test/testall.py:141:11: error[no-matching-overload] No overload of function `dirname` matches arguments
- win32/test/testall.py:143:33: error[no-matching-overload] No overload of function `basename` matches arguments
- Found 2710 diagnostics
+ Found 2694 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1823 diagnostics
+ Found 1825 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2053 diagnostics
+ Found 2054 diagnostics

scipy (https://github.com/scipy/scipy)
- doc/source/conf.py:501:45: error[no-matching-overload] No overload of function `dirname` matches arguments
- scipy/_lib/cobyqa/doc/source/conf.py:216:30: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- scipy/_lib/cobyqa/doc/source/conf.py:216:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- scipy/_lib/tests/test_warnings.py:80:17: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- scipy/_lib/tests/test_warnings.py:80:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- scipy/fftpack/tests/test_import.py:23:21: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- scipy/fftpack/tests/test_import.py:23:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- tools/check_xp_untested.py:73:41: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- tools/check_xp_untested.py:73:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
- Found 8117 diagnostics
+ Found 8108 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Converted to draft by @sinon on 2026-01-01 20:25_

---

_Label `ty` added by @ntBre on 2026-01-01 20:32_

---

_Label `ecosystem-analyzer` added by @MichaReiser on 2026-01-05 09:59_

---

_Comment by @MichaReiser on 2026-01-05 10:03_

Thank you for working on this. There are a few failing tests. Would you mind taking a look at them.

---

_Comment by @astral-sh-bot[bot] on 2026-01-05 10:04_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `no-matching-overload` | 0 | 56 | 0 |
| `invalid-argument-type` | 0 | 40 | 0 |
| `possibly-missing-attribute` | 0 | 5 | 0 |
| `invalid-return-type` | 0 | 1 | 3 |
| `invalid-assignment` | 2 | 0 | 0 |
| **Total** | **2** | **102** | **3** |


**[Full report with detailed diff](https://36919980.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://36919980.ty-ecosystem-ext.pages.dev/timing))



---

_@quencs reviewed on 2026-01-05 10:09_

_

---

_Marked ready for review by @sinon on 2026-01-05 20:47_

---

_Comment by @sinon on 2026-01-06 10:21_

Finished reviewing the eco-system impact (see PR description) I think everything looks good, so this should be ready for further review.

---

_Renamed from "[ty] Override `__file__` to str on imported modules" to "[ty] Override `__file__` to str when applicable on imported modules" by @sinon on 2026-01-08 15:14_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/place.rs`:1087 on 2026-01-14 12:38_

This comment is _nearly_ accurate!

The way it works at runtime is that a pure-Python non-namespace-package module will always have `__file__` set to a string; a namespace-package module will always have `__file__` set to `None`, and a C extension might not have it set at all:

```pycon
% uv pip install google-cloud-ndb           
Using Python 3.13.8 environment at: test-env
Resolved 22 packages in 647ms
Prepared 9 packages in 1.07s
Installed 21 packages in 92ms
 + certifi==2026.1.4
 + charset-normalizer==3.4.4
 + google-api-core==2.29.0
 + google-auth==2.47.0
 + google-cloud-core==2.5.0
 + google-cloud-datastore==2.23.0
 + google-cloud-ndb==2.4.0
 + googleapis-common-protos==1.72.0
 + grpcio==1.76.0
 + grpcio-status==1.76.0
 + idna==3.11
 + proto-plus==1.27.0
 + protobuf==6.33.4
 + pyasn1==0.6.1
 + pyasn1-modules==0.4.2
 + pymemcache==4.0.0
 + pytz==2025.2
 + redis==6.4.0
 + requests==2.32.5
 + rsa==4.9.1
 + urllib3==2.6.3
(test-env) ~/dev % py
Python 3.13.8 (main, Oct 10 2025, 12:46:18) [Clang 20.1.4 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import google
>>> google.__file__ is None  # `google` is a namespace package
True
>>> 
>>> import itertools
>>> itertools.__file__  # `itertools` is a C extension
Traceback (most recent call last):
  File "<python-input-3>", line 1, in <module>
    itertools.__file__
AttributeError: module 'itertools' has no attribute '__file__'. Did you mean: '__name__'?
```

It looks to me like your code is correct for the namespace package case, because you only apply the special case here if `module.file(db)` is `Some()`, and `module.file(db)` will only be `None` if it's a namespace package. But the comment here needs to be revised, and you could also consider adding an explicit test for the namespace-package case. (Namespace packages are pure-Python packages that have no `__init__.py(i)` file in them -- they are _just_ directories that contain other Python modules/packages.)

We could also possibly get rid of the exemption for stubs that you have here? The rationale for treating stubs separately is that they might represent C extensions, and C extensions won't necessarily set `__file__`. But that doesn't affect the _type_ of the `__file__` member, it just affects whether the module _has_ the `__file__` member to begin with. We could consider emitting a `possibly-unresolved-attribute` warning about that, but I think it would probably be too noisy: just because a module is represented by a stub file doesn't mean that it _definitely_ is a C extension, it just _could_ be a C extension.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/place.rs`:1117 on 2026-01-14 12:41_

nit: you could reduce indentation here by using let-chains :-)

---

_@AlexWaygood reviewed on 2026-01-14 12:41_

Thank you, and sorry for the delayed review!!

This looks pretty good, but the comment needs to be updated and I think we should add a test for namespace packages -- see my comment inline

---

_Assigned to @AlexWaygood by @AlexWaygood on 2026-01-14 12:41_

---

_@sinon reviewed on 2026-01-14 13:38_

---

_Review comment by @sinon on `crates/ty_python_semantic/src/place.rs`:1087 on 2026-01-14 13:38_

> We could also possibly get rid of the exemption for stubs that you have here?

Thanks for the added context, I mainly took this behaviour steer from an existing tests comment: https://github.com/astral-sh/ruff/blob/853bb006269e39f55c0d6cde5a05861d8202bbfe/crates/ty_python_semantic/resources/mdtest/scopes/moduletype_attrs.md?plain=1#L102-L104 so will update the comment on the existing test as that does become `str` (which matches runtime)


---

_@AlexWaygood reviewed on 2026-01-14 13:41_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/place.rs`:1087 on 2026-01-14 13:41_

thanks, yeah, that makes sense!

---

_@sinon reviewed on 2026-01-14 20:13_

---

_Review comment by @sinon on `crates/ty_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:102 on 2026-01-14 20:13_

@AlexWaygood I opted to remove this comment as I think it's wrong? This isn't importing a stub and checking it's `__file__` it's doing it on the stdlib `typing` module. Further down this file there is some stub files mocked.

Hopefully the new md tests for `__file__` cover this, let me know if I should expand this test with some.

---

_@sinon reviewed on 2026-01-14 20:20_

---

_Review comment by @sinon on `crates/ty_python_semantic/src/place.rs`:1135 on 2026-01-14 20:20_

This is existing behaviour on `main` probably preferable `str | None` (or could we infer `None`?), let me know if I should add a TODO, Issue or try and tackle in this PR.

pyright is `str | None`, pyrefly/mypy is `str` and ty/zuban emit unknown-attr errors.

---

_@AlexWaygood reviewed on 2026-01-14 21:23_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/dunder_file_attribute.md`:63 on 2026-01-14 21:23_

Ah -- no, _namespace_ packages do have `__file__`, but `__file__` is set to `None` for namespace packages. So I guess your logic in `crates/ty_python_semantic/src/place.rs` isn't _quite_ right. It's C extensions that don't necessarily have `__file__` set at all -- but we don't have a great way of knowing whether a module is a C extension or not right now.

---

_@AlexWaygood reviewed on 2026-01-14 21:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:102 on 2026-01-14 21:24_

I think we _could_ keep _some_ version of the comment that states that for a stub file, we don't know whether it's a C extension or pure-Python module, and if it's a C extension then `__file__` might be unset entirely? But yes, I agree that the current version of the comment is more confusing than helpful

---

_Review request for @carljm removed by @carljm on 2026-01-15 00:23_

---

_@AlexWaygood reviewed on 2026-01-15 16:38_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/dunder_file_attribute.md`:63 on 2026-01-15 16:38_

Looking at this more deeply, it seems we don't understand any `types.ModuleType` attributes as being available on namespace packages right now! Which is something we should fix, but not in this PR. So I'll adjust the comments in this PR and land it.

Thank you!!

---

_@AlexWaygood approved on 2026-01-15 17:02_

---

_Merged by @AlexWaygood on 2026-01-15 17:08_

---

_Closed by @AlexWaygood on 2026-01-15 17:08_

---

_@sinon reviewed on 2026-01-15 17:38_

---

_Review comment by @sinon on `crates/ty_python_semantic/resources/mdtest/import/dunder_file_attribute.md`:63 on 2026-01-15 17:38_

Thanks for landing this @AlexWaygood 😁 

Yeah, as I started looking into this there is a lot of places similar to: 
```
let Some(module) = resolve_module(self.db, self.file, &module_name) else {
    continue;
};
```
so only `Module::File` is being considered and `Module::Namespace` is skipped.

Seems like a `known_namespace_symbol` or similar is needed in `place.rs` to handle a scaled back version of the logic being done on modules 🤔

I might continue poking around in this area and if I make some progress will create an Issue to track the `TODO` from the mdtest

---

_@AlexWaygood reviewed on 2026-01-15 17:47_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/dunder_file_attribute.md`:63 on 2026-01-15 17:47_

Oh, sorry, I'm about to make a PR to fix this up 😶 I should have said!!

---

_@AlexWaygood reviewed on 2026-01-15 18:14_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/dunder_file_attribute.md`:63 on 2026-01-15 18:14_

PR: https://github.com/astral-sh/ruff/pull/22606

---

_@sinon reviewed on 2026-01-15 18:37_

---

_Review comment by @sinon on `crates/ty_python_semantic/resources/mdtest/import/dunder_file_attribute.md`:63 on 2026-01-15 18:37_

Nice 🚀 no worries still a good opportunity to checkout your PR and learn some more of the dataflows

---
