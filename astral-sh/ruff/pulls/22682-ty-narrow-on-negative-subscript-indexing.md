```yaml
number: 22682
title: "[ty] Narrow on negative subscript indexing"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/neg-sub
created_at: 2026-01-18T15:47:13Z
updated_at: 2026-01-19T14:04:11Z
url: https://github.com/astral-sh/ruff/pull/22682
synced_at: 2026-01-19T14:36:03Z
```

# [ty] Narrow on negative subscript indexing

---

_@charliermarsh_

## Summary

Negative subscripts are also indicative of a thing being subcriptable:

```python
class Subscriptable:
    def __getitem__(self, key: int) -> int:
        return 42

class NotSubscriptable: ...

def _(x: list[Subscriptable | NotSubscriptable]):
    if not isinstance(x[-1], NotSubscriptable):
        # After narrowing, x[-1] excludes NotSubscriptable, which means subscripting works
        reveal_type(x[-1])  # revealed: Subscriptable & ~NotSubscriptable
        reveal_type(x[-1][0])  # revealed: int
```


---

_Label `ty` added by @charliermarsh on 2026-01-18 15:47_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 15:48_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-18 15:50_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyp (https://github.com/hauntsaninja/pyp)
+ pyp.py:429:34: error[invalid-argument-type] Argument to function `inner` is incorrect: Expected `list[stmt]`, found `object`
- pyp.py:429:34: error[unresolved-attribute] Object of type `stmt` has no attribute `body`
- pyp.py:432:27: error[unresolved-attribute] Object of type `stmt` has no attribute `value`
- pyp.py:433:26: error[unresolved-attribute] Object of type `stmt` has no attribute `value`
- pyp.py:439:75: error[unresolved-attribute] Object of type `stmt` has no attribute `value`
- Found 9 diagnostics
+ Found 6 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
- aioredis/client.py:3898:38: error[no-matching-overload] No overload of bound method `split` matches arguments
- Found 30 diagnostics
+ Found 29 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/test/cmd/develop.py:265:18: error[no-matching-overload] No overload matches arguments
- lib/spack/spack/util/gpg.py:353:9: error[no-matching-overload] No overload matches arguments
- lib/spack/spack/vendor/jinja2/compiler.py:1516:17: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `list[Any] | Expr`
+ lib/spack/spack/vendor/jinja2/compiler.py:1516:33: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Never`, found `str`
- lib/spack/spack/vendor/jinja2/environment.py:1612:17: error[no-matching-overload] No overload of bound method `writelines` matches arguments
- lib/spack/spack/version/version_types.py:1269:63: error[invalid-argument-type] Argument to function `_next_version_str_component` is incorrect: Expected `VersionStrComponent`, found `int | VersionStrComponent`
- lib/spack/spack/version/version_types.py:1271:35: error[unsupported-operator] Operator `+` is not supported between objects of type `int | VersionStrComponent` and `Literal[1]`
- lib/spack/spack/version/version_types.py:1291:63: error[invalid-argument-type] Argument to function `_prev_version_str_component` is incorrect: Expected `VersionStrComponent`, found `int | VersionStrComponent`
- lib/spack/spack/version/version_types.py:1293:35: error[unsupported-operator] Operator `-` is not supported between objects of type `int | VersionStrComponent` and `Literal[1]`
- Found 4346 diagnostics
+ Found 4339 diagnostics

jinja (https://github.com/pallets/jinja)
- src/jinja2/compiler.py:1534:17: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `list[Any] | Expr`
+ src/jinja2/compiler.py:1534:33: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Never`, found `str`

black (https://github.com/psf/black)
- src/black/linegen.py:1610:9: error[invalid-assignment] Object of type `Literal[""]` is not assignable to attribute `value` on type `Node | Leaf`
- src/blib2to3/pgen2/parse.py:367:13: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `list[Node | Leaf] | None`
- src/blib2to3/pgen2/parse.py:392:17: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `list[Node | Leaf] | None`
- Found 53 diagnostics
+ Found 50 diagnostics

kopf (https://github.com/nolar/kopf)
- kopf/_kits/hierarchies.py:44:24: error[no-matching-overload] No overload of bound method `setdefault` matches arguments
- kopf/_kits/hierarchies.py:82:24: error[no-matching-overload] No overload of bound method `setdefault` matches arguments
- kopf/_kits/hierarchies.py:125:30: error[no-matching-overload] No overload of bound method `setdefault` matches arguments
- kopf/_kits/hierarchies.py:139:17: error[no-matching-overload] No overload of bound method `setdefault` matches arguments
- kopf/_kits/hierarchies.py:181:25: error[no-matching-overload] No overload of bound method `setdefault` matches arguments
- kopf/_kits/hierarchies.py:185:25: error[no-matching-overload] No overload of bound method `setdefault` matches arguments
- kopf/_kits/hierarchies.py:232:30: error[no-matching-overload] No overload of bound method `get` matches arguments
- kopf/_kits/hierarchies.py:233:21: error[no-matching-overload] No overload of bound method `setdefault` matches arguments
- Found 294 diagnostics
+ Found 286 diagnostics

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
+ src/check_jsonschema/format_errors.py:41:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 27 diagnostics
+ Found 28 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- tests/test_pytypes.py:457:29: error[no-matching-overload] No overload of class `str` matches arguments
- Found 216 diagnostics
+ Found 215 diagnostics

mkosi (https://github.com/systemd/mkosi)
- mkosi/config.py:2490:9: error[no-matching-overload] No overload of bound method `pop` matches arguments
- Found 76 diagnostics
+ Found 75 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/routing.py:582:17: error[no-matching-overload] No overload of bound method `match` matches arguments
- Found 330 diagnostics
+ Found 329 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/v1/config.py:133:13: error[no-matching-overload] No overload of bound method `setdefault` matches arguments
- pydantic/v1/config.py:133:13: error[no-matching-overload] No overload of bound method `setdefault` matches arguments
- Found 3157 diagnostics
+ Found 3155 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
- schema_salad/metaschema.py:1037:24: error[no-matching-overload] No overload of bound method `get` matches arguments
- schema_salad/metaschema.py:1038:21: error[no-matching-overload] No overload of bound method `get` matches arguments
- schema_salad/metaschema.py:1039:21: error[no-matching-overload] No overload of bound method `get` matches arguments
- schema_salad/python_codegen_support.py:1034:24: error[no-matching-overload] No overload of bound method `get` matches arguments
- schema_salad/python_codegen_support.py:1035:21: error[no-matching-overload] No overload of bound method `get` matches arguments
- schema_salad/python_codegen_support.py:1036:21: error[no-matching-overload] No overload of bound method `get` matches arguments
- Found 419 diagnostics
+ Found 413 diagnostics

mypy (https://github.com/python/mypy)
- mypy/checker_shared.py:317:20: error[invalid-return-type] Return type does not match returned value: expected `TypeInfo | None`, found `TypeInfo | FuncItem | MypyFile`
- mypy/partially_defined.py:523:21: warning[possibly-missing-attribute] Attribute `intersection_update` may be missing on object of type `set[str] | None`
- mypy/semanal.py:4504:24: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `SymbolTable | None`
- mypy/semanal.py:5613:52: error[unsupported-operator] Operator `in` is not supported between objects of type `str` and `SymbolTable | None`
- mypy/semanal.py:6943:20: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `SymbolTable | None`
- mypy/server/astdiff.py:157:67: error[invalid-argument-type] Argument to function `compare_symbol_table_snapshots` is incorrect: Expected `dict[str, tuple[object, ...]]`, found `object`
+ mypy/server/astdiff.py:157:67: error[invalid-argument-type] Argument to function `compare_symbol_table_snapshots` is incorrect: Expected `dict[str, tuple[object, ...]]`, found `Top[dict[Unknown, Unknown]]`
- mypy/server/astdiff.py:157:78: error[invalid-argument-type] Argument to function `compare_symbol_table_snapshots` is incorrect: Expected `dict[str, tuple[object, ...]]`, found `object`
+ mypy/server/astdiff.py:157:78: error[invalid-argument-type] Argument to function `compare_symbol_table_snapshots` is incorrect: Expected `dict[str, tuple[object, ...]]`, found `Top[dict[Unknown, Unknown]]`
- mypyc/analysis/dataflow.py:131:17: error[unresolved-attribute] Object of type `Op` has no attribute `label`
- mypyc/ir/ops.py:118:16: error[invalid-return-type] Return type does not match returned value: expected `ControlOp`, found `Op`
- mypyc/ir/pprint.py:422:17: error[unresolved-attribute] Object of type `Op` has no attribute `label`
- Found 1744 diagnostics
+ Found 1736 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/mformat.py:722:76: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `WhitespaceNode | None`
- Found 2166 diagnostics
+ Found 2165 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/environment/collectors/toctree.py:170:41: error[unresolved-attribute] Object of type `Node` has no attribute `append`
- Found 346 diagnostics
+ Found 345 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- cwltool/checker.py:121:53: error[no-matching-overload] No overload of bound method `get` matches arguments
- cwltool/checker.py:121:53: error[no-matching-overload] No overload of bound method `get` matches arguments
- cwltool/checker.py:124:60: error[no-matching-overload] No overload of bound method `get` matches arguments
- cwltool/checker.py:124:60: error[no-matching-overload] No overload of bound method `get` matches arguments
- cwltool/command_line_tool.py:615:50: error[no-matching-overload] No overload of bound method `get` matches arguments
- cwltool/command_line_tool.py:620:32: error[no-matching-overload] No overload of bound method `get` matches arguments
- cwltool/process.py:1221:37: error[no-matching-overload] No overload of bound method `get` matches arguments
- Found 584 diagnostics
+ Found 577 diagnostics

zope.interface (https://github.com/zopefoundation/zope.interface)
- src/zope/interface/exceptions.py:219:23: error[no-matching-overload] No overload of class `str` matches arguments
- Found 422 diagnostics
+ Found 421 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/config/cc_rh_subscription.py:253:29: error[no-matching-overload] No overload of bound method `split` matches arguments
- Found 1174 diagnostics
+ Found 1173 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/conventions.py:349:12: error[no-matching-overload] No overload of bound method `get` matches arguments
- xarray/tests/test_backends_file_manager.py:173:5: error[no-matching-overload] No overload of bound method `write` matches arguments
- xarray/tests/test_backends_file_manager.py:176:5: error[no-matching-overload] No overload of bound method `write` matches arguments
- xarray/tests/test_backends_file_manager.py:179:5: error[no-matching-overload] No overload of bound method `write` matches arguments
- xarray/tests/test_backends_file_manager.py:198:5: error[no-matching-overload] No overload of bound method `write` matches arguments
- xarray/tests/test_backends_file_manager.py:200:5: error[no-matching-overload] No overload of bound method `write` matches arguments
- xarray/tests/test_backends_file_manager.py:202:5: error[no-matching-overload] No overload of bound method `write` matches arguments
- xarray/tests/test_backends_file_manager.py:214:5: error[no-matching-overload] No overload of bound method `write` matches arguments
- xarray/tests/test_backends_file_manager.py:271:5: error[no-matching-overload] No overload of bound method `write` matches arguments
- xarray/tests/test_backends_file_manager.py:287:5: error[no-matching-overload] No overload of bound method `write` matches arguments
- Found 1784 diagnostics
+ Found 1774 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 46 diagnostics
+ Found 47 diagnostics

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

altair (https://github.com/vega/altair)
- altair/vegalite/v6/api.py:913:5: error[no-matching-overload] No overload of bound method `update` matches arguments
- Found 1103 diagnostics
+ Found 1102 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/linear_model/_stochastic_gradient.py:785:30: error[no-matching-overload] No overload of bound method `reshape` matches arguments
- sklearn/linear_model/_stochastic_gradient.py:785:30: error[no-matching-overload] No overload of bound method `reshape` matches arguments
- sklearn/linear_model/_stochastic_gradient.py:785:30: error[no-matching-overload] No overload of bound method `reshape` matches arguments
- Found 2440 diagnostics
+ Found 2437 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- tests/contrib/integration_registry/registry_update_helpers/integration_registry_manager.py:161:24: error[no-matching-overload] No overload of function `getattr` matches arguments
- Found 8420 diagnostics
+ Found 8419 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`

jax (https://github.com/google/jax)
- jax/_src/pallas/fuser/block_spec.py:1771:21: error[invalid-argument-type] Argument to function `_block_size` is incorrect: Expected `Element | int | None`, found `Element | Squeezed | Blocked | ... omitted 4 union elements`
+ jax/_src/pallas/fuser/block_spec.py:1771:21: error[invalid-argument-type] Argument to function `_block_size` is incorrect: Expected `Element | int | None`, found `int | Blocked`
- jax/_src/pallas/fuser/block_spec.py:1772:21: error[invalid-argument-type] Argument to function `_block_size` is incorrect: Expected `Element | int | None`, found `Element | Squeezed | Blocked | ... omitted 4 union elements`
+ jax/_src/pallas/fuser/block_spec.py:1772:21: error[invalid-argument-type] Argument to function `_block_size` is incorrect: Expected `Element | int | None`, found `int | Blocked`

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/algorithms.py:552:34: error[no-matching-overload] No overload of bound method `astype` matches arguments
- pandas/core/algorithms.py:552:34: error[no-matching-overload] No overload of bound method `astype` matches arguments
- pandas/core/algorithms.py:583:18: error[no-matching-overload] No overload of bound method `astype` matches arguments
- pandas/core/algorithms.py:583:18: error[no-matching-overload] No overload of bound method `astype` matches arguments
- pandas/core/arrays/string_.py:804:22: error[no-matching-overload] No overload of bound method `astype` matches arguments
- pandas/core/dtypes/cast.py:1545:26: error[no-matching-overload] No overload of bound method `astype` matches arguments
- pandas/core/nanops.py:1557:30: error[no-matching-overload] No overload of bound method `astype` matches arguments
- pandas/core/nanops.py:1559:30: error[no-matching-overload] No overload of bound method `astype` matches arguments
- pandas/io/pytables.py:4837:26: error[no-matching-overload] No overload of bound method `reshape` matches arguments
- pandas/tests/series/test_cumulative.py:48:20: error[no-matching-overload] No overload matches arguments
- pandas/tests/series/test_cumulative.py:48:20: error[no-matching-overload] No overload matches arguments
- pandas/tests/series/test_cumulative.py:54:20: error[no-matching-overload] No overload matches arguments
- pandas/tests/series/test_cumulative.py:54:20: error[no-matching-overload] No overload matches arguments
- pandas/tests/series/test_cumulative.py:154:20: error[no-matching-overload] No overload matches arguments
- pandas/tests/series/test_cumulative.py:154:20: error[no-matching-overload] No overload matches arguments
- Found 3781 diagnostics
+ Found 3766 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/db/filtering.py:461:20: error[unresolved-attribute] Object of type `DBFilter` has no attribute `chain_id`
- Found 2057 diagnostics
+ Found 2056 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/anthropic/ai_task.py:60:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `Content`
- homeassistant/components/anthropic/entity.py:656:44: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `Content`
+ homeassistant/components/anthropic/entity.py:656:44: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
- homeassistant/components/anthropic/entity.py:668:64: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `Content`
+ homeassistant/components/anthropic/entity.py:668:64: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
- homeassistant/components/cloud/ai_task.py:132:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `Content`
- homeassistant/components/cloud/entity.py:461:44: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `Content`
+ homeassistant/components/cloud/entity.py:461:44: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
- homeassistant/components/cloud/entity.py:462:64: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `Content`
+ homeassistant/components/cloud/entity.py:462:64: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
- homeassistant/components/cloud/entity.py:463:31: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `Content`
+ homeassistant/components/cloud/entity.py:463:31: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
- homeassistant/components/conversation/chat_log.py:93:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `Content`
+ homeassistant/components/conversation/chat_log.py:93:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
- homeassistant/components/google_generative_ai_conversation/ai_task.py:93:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `Content`
- homeassistant/components/ollama/ai_task.py:60:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `Content`
- homeassistant/components/open_router/ai_task.py:61:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `Content`
- homeassistant/components/open_router/entity.py:264:44: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `Content`
+ homeassistant/components/open_router/entity.py:264:44: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
- homeassistant/components/open_router/entity.py:272:49: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `Content`
+ homeassistant/components/open_router/entity.py:272:49: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
- homeassistant/components/openai_conversation/ai_task.py:81:16: warning[possibly-missing-attribute] Attribute `content` may be missing on object of type `Content`
- homeassistant/components/openai_conversation/entity.py:602:44: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `Content`
+ homeassistant/components/openai_conversation/entity.py:602:44: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
- homeassistant/components/openai_conversation/entity.py:605:49: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `Content`
+ homeassistant/components/openai_conversation/entity.py:605:49: warning[possibly-missing-attribute] Attribute `attachments` may be missing on object of type `SystemContent | UserContent | AssistantContent | ToolResultContent`
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14509 diagnostics
+ Found 14502 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/sparse/linalg/_isolve/iterative.py:1003:19: error[no-matching-overload] No overload of function `vdot` matches arguments
- scipy/stats/_mstats_basic.py:667:16: error[no-matching-overload] No overload of bound method `any` matches arguments
- Found 8116 diagnostics
+ Found 8114 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-18 15:59_

---

_Review requested from @carljm by @charliermarsh on 2026-01-18 15:59_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-18 15:59_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-18 15:59_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-18 15:59_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/member.rs`:213 on 2026-01-18 16:04_

For completeness (though it's obviously unlikely to come up), shall we also handle unary `+`? `x[+5]`, etc.

---

_@AlexWaygood approved on 2026-01-18 16:04_

I can't believe this went unnoticed for so long 😅 Thank you!

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/semantic_index/member.rs`:213 on 2026-01-18 16:47_

Yeah will do. I was also gonna put some other variants in a separate PR.

---

_@charliermarsh reviewed on 2026-01-18 16:47_

---

_@AlexWaygood reviewed on 2026-01-18 17:13_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/member.rs`:213 on 2026-01-18 17:13_

yes, I guess ideally we'd also handle things like `++42` and `--42`... definitely puntable, this fixes loads of false positives as-is!

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-19 13:53_

---

_Review requested from @Gankra by @charliermarsh on 2026-01-19 13:53_

---

_Comment by @charliermarsh on 2026-01-19 13:54_

(Rebased on main and simplified the tests a bit so we can merge separately from the larger intersection PR.)

---

_Merged by @charliermarsh on 2026-01-19 14:04_

---

_Closed by @charliermarsh on 2026-01-19 14:04_

---

_Branch deleted on 2026-01-19 14:04_

---
