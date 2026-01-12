```yaml
number: 22208
title: "[ty] Fix `super()` with TypeVar-annotated `self` and `cls` parameter"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: charlie/super
created_at: 2025-12-26T15:07:18Z
updated_at: 2026-01-08T00:56:11Z
url: https://github.com/astral-sh/ruff/pull/22208
synced_at: 2026-01-12T15:57:44Z
```

# [ty] Fix `super()` with TypeVar-annotated `self` and `cls` parameter

---

_@charliermarsh_

## Summary

This PR fixes `super()` handling when the first parameter (`self` or `cls`) is annotated with a TypeVar, like `Self`. 

Previously, `super()` would incorrectly resolve TypeVars to their bounds before creating the `BoundSuperType`. So if you had `self: Self` where `Self` is bounded by `Parent`, we'd process `Parent` as a `NominalInstance` and end up with `SuperOwnerKind::Instance(Parent)`.

As a result:

```python
class Parent:
    @classmethod
    def create(cls) -> Self:
        return cls()

class Child(Parent):
    @classmethod
    def create(cls) -> Self:
        return super().create()  # Error: Argument type `Self@create` does not satisfy upper bound `Parent`
```

We now track two additional variants on `SuperOwnerKind` for TypeVar owners:

- `InstanceTypeVar`: for instance methods where self is a TypeVar (e.g., `self: Self`).
- `ClassTypeVar`: for classmethods where `cls` is a `TypeVar` wrapped in `type[...]` (e.g., `cls: type[Self]`).

Closes https://github.com/astral-sh/ty/issues/2122.


---

_Label `bug` added by @charliermarsh on 2025-12-26 15:07_

---

_Label `ty` added by @charliermarsh on 2025-12-26 15:07_

---

_Comment by @astral-sh-bot[bot] on 2025-12-26 15:09_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2026-01-08 00:47:54.303348418 +0000
+++ new-output.txt	2026-01-08 00:47:54.651349892 +0000
@@ -210,7 +210,7 @@
 constructors_call_init.py:130:9: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
 constructors_call_metaclass.py:39:1: error[type-assertion-failure] Type `int | Meta2` does not match asserted type `Class2`
 constructors_call_metaclass.py:39:13: error[missing-argument] No argument provided for required parameter `x` of function `__new__`
-constructors_call_metaclass.py:46:16: error[invalid-super-argument] `T@__call__` is not an instance or subclass of `<class 'Meta3'>` in `super(<class 'Meta3'>, T@__call__)` call
+constructors_call_metaclass.py:46:16: error[invalid-super-argument] `type[T@__call__]` is not an instance or subclass of `<class 'Meta3'>` in `super(<class 'Meta3'>, type[T@__call__])` call
 constructors_call_metaclass.py:54:1: error[missing-argument] No argument provided for required parameter `x` of function `__new__`
 constructors_call_metaclass.py:68:1: error[missing-argument] No argument provided for required parameter `x` of function `__new__`
 constructors_call_new.py:21:13: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `int`, found `float`

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-26 15:10_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/test_slots.py:985:24: error[unresolved-attribute] Object of type `<super: <class 'B'>, B>` has no attribute `__getattr__`
+ tests/test_slots.py:985:24: error[unresolved-attribute] Object of type `<super: <class 'B'>, Self@__getattr__>` has no attribute `__getattr__`

python-chess (https://github.com/niklasf/python-chess)
- chess/__init__.py:3962:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `Board`
- chess/variant.py:808:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `ThreeCheckBoard`
- chess/variant.py:814:20: error[invalid-return-type] Return type does not match returned value: expected `Self@root`, found `ThreeCheckBoard`
- chess/variant.py:822:16: error[invalid-return-type] Return type does not match returned value: expected `Self@mirror`, found `ThreeCheckBoard`
- chess/variant.py:1061:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `CrazyhouseBoard`
- chess/variant.py:1067:20: error[invalid-return-type] Return type does not match returned value: expected `Self@root`, found `CrazyhouseBoard`
- chess/variant.py:1075:16: error[invalid-return-type] Return type does not match returned value: expected `Self@mirror`, found `CrazyhouseBoard`
- Found 12 diagnostics
+ Found 5 diagnostics

beartype (https://github.com/beartype/beartype)
+ beartype/typing/_typingpep544.py:520:31: error[unresolved-attribute] Object of type `<super: <class 'Protocol'>, type[Self@__class_getitem__]>` has no attribute `__class_getitem__`
- Found 497 diagnostics
+ Found 498 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- scrapy/spiders/crawl.py:219:16: error[invalid-return-type] Return type does not match returned value: expected `Self@from_crawler`, found `CrawlSpider`
- scrapy/spiders/sitemap.py:45:16: error[invalid-return-type] Return type does not match returned value: expected `Self@from_crawler`, found `SitemapSpider`
- scrapy/utils/testsite.py:18:9: error[unresolved-attribute] Object of type `<super: <class 'SiteTest'>, SiteTest>` has no attribute `setUp`
+ scrapy/utils/testsite.py:18:9: error[unresolved-attribute] Object of type `<super: <class 'SiteTest'>, Self@setUp>` has no attribute `setUp`
- scrapy/utils/testsite.py:23:9: error[unresolved-attribute] Object of type `<super: <class 'SiteTest'>, SiteTest>` has no attribute `tearDown`
+ scrapy/utils/testsite.py:23:9: error[unresolved-attribute] Object of type `<super: <class 'SiteTest'>, Self@tearDown>` has no attribute `tearDown`
- tests/test_dupefilters.py:33:9: error[unresolved-attribute] Unresolved attribute `method` on type `FromCrawlerRFPDupeFilter`.
+ tests/test_dupefilters.py:33:9: error[unresolved-attribute] Unresolved attribute `method` on type `Self@from_crawler`.
- tests/test_pipeline_media.py:179:16: error[unresolved-attribute] Object of type `<super: <class 'MockedMediaPipeline'>, MockedMediaPipeline>` has no attribute `download`
+ tests/test_pipeline_media.py:179:16: error[unresolved-attribute] Object of type `<super: <class 'MockedMediaPipeline'>, Self@download>` has no attribute `download`
- tests/test_pipeline_media.py:455:17: error[unresolved-attribute] Unresolved attribute `store_uri` on type `Pipeline`.
+ tests/test_pipeline_media.py:455:17: error[unresolved-attribute] Unresolved attribute `store_uri` on type `Self@from_crawler`.
- Found 1784 diagnostics
+ Found 1782 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/doctest.py:282:16: error[invalid-return-type] Return type does not match returned value: expected `Self@from_parent`, found `DoctestItem`
- src/_pytest/main.py:549:16: error[invalid-return-type] Return type does not match returned value: expected `Self@from_parent`, found `Dir`
- src/_pytest/nodes.py:627:16: error[invalid-return-type] Return type does not match returned value: expected `Self@from_parent`, found `FSCollector`
- src/_pytest/python.py:752:16: error[invalid-return-type] Return type does not match returned value: expected `Self@from_parent`, found `Class`
- src/_pytest/python.py:1639:16: error[invalid-return-type] Return type does not match returned value: expected `Self@from_parent`, found `Function`
- src/_pytest/subtests.py:132:16: error[invalid-return-type] Return type does not match returned value: expected `Self@_new`, found `SubtestReport`
- Found 433 diagnostics
+ Found 427 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
+ github/GithubRetry.py:90:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 298 diagnostics
+ Found 299 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/generation/stateful/state_machine.py:200:13: error[unresolved-attribute] Object of type `<super: <class 'APIStateMachine'>, APIStateMachine>` has no attribute `_add_result_to_targets`
+ src/schemathesis/generation/stateful/state_machine.py:200:13: error[unresolved-attribute] Object of type `<super: <class 'APIStateMachine'>, Self@_add_result_to_targets>` has no attribute `_add_result_to_targets`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/commands/slash.py:1065:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `BaseSlashCommand`
- tanjun/commands/slash.py:1233:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `SlashCommandGroup`
- Found 136 diagnostics
+ Found 134 diagnostics

zope.interface (https://github.com/zopefoundation/zope.interface)
- src/zope/interface/adapter.py:769:9: error[unresolved-attribute] Object of type `<super: <class 'AdapterLookupBase'>, AdapterLookupBase>` has no attribute `changed`
+ src/zope/interface/adapter.py:769:9: error[unresolved-attribute] Object of type `<super: <class 'AdapterLookupBase'>, Self@changed>` has no attribute `changed`

discord.py (https://github.com/Rapptz/discord.py)
- discord/ui/modal.py:273:16: error[invalid-return-type] Return type does not match returned value: expected `Self@add_item`, found `Modal`
- Found 552 diagnostics
+ Found 551 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/dataset.py:409:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__eq__`, found `Dataset`
+ xarray/core/datatree.py:550:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[str, Self@__init__] | None`, found `Mapping[str, DataTree] | None`
- xarray/core/datatree.py:921:16: error[invalid-return-type] Return type does not match returned value: expected `Self@_copy_node`, found `DataTree`
- xarray/core/datatree.py:1938:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__eq__`, found `DataTree`
- xarray/core/treenode.py:729:16: error[invalid-return-type] Return type does not match returned value: expected `Self@_copy_node`, found `NamedNode`
- xarray/core/variable.py:2729:16: error[invalid-return-type] Return type does not match returned value: expected `Self@chunk`, found `Variable`
- Found 1765 diagnostics
+ Found 1761 diagnostics

setuptools (https://github.com/pypa/setuptools)
- setuptools/__init__.py:71:30: error[unresolved-attribute] Object of type `<super: <class 'MinimalDistribution'>, MinimalDistribution>` has no attribute `_split_standard_project_metadata`
+ setuptools/__init__.py:71:30: error[unresolved-attribute] Object of type `<super: <class 'MinimalDistribution'>, Self@_get_project_config_files>` has no attribute `_split_standard_project_metadata`
- setuptools/_vendor/zipp/__init__.py:95:41: error[unresolved-attribute] Object of type `<super: <class 'SanitizedNames'>, SanitizedNames>` has no attribute `namelist`
+ setuptools/_vendor/zipp/__init__.py:95:41: error[unresolved-attribute] Object of type `<super: <class 'SanitizedNames'>, Self@namelist>` has no attribute `namelist`
- setuptools/command/sdist.py:114:9: error[unresolved-attribute] Object of type `<super: <class 'sdist'>, sdist>` has no attribute `_add_defaults_optional`
+ setuptools/command/sdist.py:114:9: error[unresolved-attribute] Object of type `<super: <class 'sdist'>, Self@_add_defaults_optional>` has no attribute `_add_defaults_optional`
- setuptools/command/sdist.py:158:13: error[unresolved-attribute] Object of type `<super: <class 'sdist'>, sdist>` has no attribute `_add_defaults_data_files`
+ setuptools/command/sdist.py:158:13: error[unresolved-attribute] Object of type `<super: <class 'sdist'>, Self@_add_defaults_data_files>` has no attribute `_add_defaults_data_files`
- setuptools/config/pyprojecttoml.py:441:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__enter__`, found `_EnsurePackagesDiscovered`
- Found 1274 diagnostics
+ Found 1273 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-aws/prefect_aws/workers/ecs_worker.py:1615:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__aenter__`, found `ECSWorker`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/_internal/_logging.py:31:16: error[invalid-return-type] Return type does not match returned value: expected `Self@getChild`, found `SafeLogger`
- src/prefect/blocks/core.py:1641:16: error[invalid-return-type] Return type does not match returned value: expected `Self@model_validate`, found `Block`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/context.py:217:16: error[invalid-return-type] Return type does not match returned value: expected `Self@model_copy`, found `ContextModel`
- src/prefect/context.py:265:20: error[invalid-return-type] Return type does not match returned value: expected `Self@__enter__`, found `SyncClientContext`
- src/prefect/context.py:323:20: error[invalid-return-type] Return type does not match returned value: expected `Self@__aenter__`, found `AsyncClientContext`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/events/worker.py:130:16: error[invalid-return-type] Return type does not match returned value: expected `Self@instance`, found `EventsWorker`
- src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/prefect/logging/handlers.py:92:16: error[invalid-return-type] Return type does not match returned value: expected `Self@instance`, found `APILogWorker`
- src/prefect/server/events/schemas/automations.py:623:16: error[invalid-return-type] Return type does not match returned value: expected `Self@model_validate`, found `Automation`
- src/prefect/server/orchestration/rules.py:353:16: error[invalid-return-type] Return type does not match returned value: expected `Self@safe_copy`, found `FlowOrchestrationContext`
- src/prefect/server/orchestration/rules.py:514:16: error[invalid-return-type] Return type does not match returned value: expected `Self@safe_copy`, found `TaskOrchestrationContext`
- src/prefect/server/schemas/core.py:1162:16: error[invalid-return-type] Return type does not match returned value: expected `Self@model_validate`, found `WorkPool`
- src/prefect/server/schemas/responses.py:330:16: error[invalid-return-type] Return type does not match returned value: expected `Self@model_validate`, found `FlowRunResponse`
- src/prefect/server/schemas/responses.py:524:16: error[invalid-return-type] Return type does not match returned value: expected `Self@model_validate`, found `DeploymentResponse`
- src/prefect/server/schemas/responses.py:553:16: error[invalid-return-type] Return type does not match returned value: expected `Self@model_validate`, found `WorkQueueResponse`
- src/prefect/server/schemas/responses.py:598:16: error[invalid-return-type] Return type does not match returned value: expected `Self@model_validate`, found `WorkerResponse`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | T@resolve_variables | float | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | T@resolve_variables | float | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`
- Found 5382 diagnostics
+ Found 5361 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/contrib/internal/gevent/greenlet.py:21:9: error[unresolved-attribute] Object of type `<super: <class 'TracingMixin'>, TracingMixin>` has no attribute `run`
+ ddtrace/contrib/internal/gevent/greenlet.py:21:9: error[unresolved-attribute] Object of type `<super: <class 'TracingMixin'>, Self@run>` has no attribute `run`
- ddtrace/internal/http.py:37:16: error[unresolved-attribute] Object of type `<super: <class 'HTTPConnectionMixin'>, HTTPConnectionMixin>` has no attribute `request`
+ ddtrace/internal/http.py:37:16: error[unresolved-attribute] Object of type `<super: <class 'HTTPConnectionMixin'>, Self@request>` has no attribute `request`
- tests/contrib/httplib/test_httplib.py:41:9: error[unresolved-attribute] Object of type `<super: <class 'HTTPLibBaseMixin'>, HTTPLibBaseMixin>` has no attribute `setUp`
+ tests/contrib/httplib/test_httplib.py:41:9: error[unresolved-attribute] Object of type `<super: <class 'HTTPLibBaseMixin'>, Self@setUp>` has no attribute `setUp`
- tests/contrib/httplib/test_httplib.py:49:9: error[unresolved-attribute] Object of type `<super: <class 'HTTPLibBaseMixin'>, HTTPLibBaseMixin>` has no attribute `tearDown`
+ tests/contrib/httplib/test_httplib.py:49:9: error[unresolved-attribute] Object of type `<super: <class 'HTTPLibBaseMixin'>, Self@tearDown>` has no attribute `tearDown`
- tests/contrib/mysql/test_mysql.py:23:9: error[unresolved-attribute] Object of type `<super: <class 'MySQLCore'>, MySQLCore>` has no attribute `tearDown`
+ tests/contrib/mysql/test_mysql.py:23:9: error[unresolved-attribute] Object of type `<super: <class 'MySQLCore'>, Self@tearDown>` has no attribute `tearDown`
- tests/contrib/mysqldb/test_mysqldb.py:26:9: error[unresolved-attribute] Object of type `<super: <class 'MySQLCore'>, MySQLCore>` has no attribute `setUp`
+ tests/contrib/mysqldb/test_mysqldb.py:26:9: error[unresolved-attribute] Object of type `<super: <class 'MySQLCore'>, Self@setUp>` has no attribute `setUp`
- tests/contrib/mysqldb/test_mysqldb.py:31:9: error[unresolved-attribute] Object of type `<super: <class 'MySQLCore'>, MySQLCore>` has no attribute `tearDown`
+ tests/contrib/mysqldb/test_mysqldb.py:31:9: error[unresolved-attribute] Object of type `<super: <class 'MySQLCore'>, Self@tearDown>` has no attribute `tearDown`
- tests/contrib/pymysql/test_pymysql.py:36:9: error[unresolved-attribute] Object of type `<super: <class 'PyMySQLCore'>, PyMySQLCore>` has no attribute `setUp`
+ tests/contrib/pymysql/test_pymysql.py:36:9: error[unresolved-attribute] Object of type `<super: <class 'PyMySQLCore'>, Self@setUp>` has no attribute `setUp`
- tests/contrib/pymysql/test_pymysql.py:40:9: error[unresolved-attribute] Object of type `<super: <class 'PyMySQLCore'>, PyMySQLCore>` has no attribute `tearDown`
+ tests/contrib/pymysql/test_pymysql.py:40:9: error[unresolved-attribute] Object of type `<super: <class 'PyMySQLCore'>, Self@tearDown>` has no attribute `tearDown`
- tests/contrib/pyodbc/test_pyodbc.py:20:9: error[unresolved-attribute] Object of type `<super: <class 'PyODBCTest'>, PyODBCTest>` has no attribute `setUp`
+ tests/contrib/pyodbc/test_pyodbc.py:20:9: error[unresolved-attribute] Object of type `<super: <class 'PyODBCTest'>, Self@setUp>` has no attribute `setUp`
- tests/contrib/pyodbc/test_pyodbc.py:24:9: error[unresolved-attribute] Object of type `<super: <class 'PyODBCTest'>, PyODBCTest>` has no attribute `tearDown`
+ tests/contrib/pyodbc/test_pyodbc.py:24:9: error[unresolved-attribute] Object of type `<super: <class 'PyODBCTest'>, Self@tearDown>` has no attribute `tearDown`
- tests/contrib/requests/test_requests.py:40:9: error[unresolved-attribute] Object of type `<super: <class 'BaseRequestTestCase'>, BaseRequestTestCase>` has no attribute `setUp`
+ tests/contrib/requests/test_requests.py:40:9: error[unresolved-attribute] Object of type `<super: <class 'BaseRequestTestCase'>, Self@setUp>` has no attribute `setUp`
- tests/contrib/requests/test_requests.py:49:9: error[unresolved-attribute] Object of type `<super: <class 'BaseRequestTestCase'>, BaseRequestTestCase>` has no attribute `tearDown`
+ tests/contrib/requests/test_requests.py:49:9: error[unresolved-attribute] Object of type `<super: <class 'BaseRequestTestCase'>, Self@tearDown>` has no attribute `tearDown`
- tests/contrib/sqlalchemy/mixins.py:54:9: error[unresolved-attribute] Object of type `<super: <class 'SQLAlchemyTestBase'>, SQLAlchemyTestBase>` has no attribute `setUp`
+ tests/contrib/sqlalchemy/mixins.py:54:9: error[unresolved-attribute] Object of type `<super: <class 'SQLAlchemyTestBase'>, Self@setUp>` has no attribute `setUp`
- tests/contrib/sqlalchemy/mixins.py:73:9: error[unresolved-attribute] Object of type `<super: <class 'SQLAlchemyTestBase'>, SQLAlchemyTestBase>` has no attribute `tearDown`
+ tests/contrib/sqlalchemy/mixins.py:73:9: error[unresolved-attribute] Object of type `<super: <class 'SQLAlchemyTestBase'>, Self@tearDown>` has no attribute `tearDown`

altair (https://github.com/vega/altair)
- altair/utils/schemapi.py:1155:28: error[unresolved-attribute] Object of type `<super: <class 'SchemaBase'>, SchemaBase>` has no attribute `__getattr__`
+ altair/utils/schemapi.py:1155:28: error[unresolved-attribute] Object of type `<super: <class 'SchemaBase'>, Self@__getattr__>` has no attribute `__getattr__`
- altair/vegalite/v6/api.py:4122:19: error[invalid-super-argument] `_TSchemaBase@from_dict` is not an instance or subclass of `<class 'Chart'>` in `super(<class 'Chart'>, _TSchemaBase@from_dict)` call
+ altair/vegalite/v6/api.py:4122:19: error[invalid-super-argument] `type[_TSchemaBase@from_dict]` is not an instance or subclass of `<class 'Chart'>` in `super(<class 'Chart'>, type[_TSchemaBase@from_dict])` call
- altair/vegalite/v6/schema/channels.py:224:16: error[unresolved-attribute] Object of type `<super: <class 'FieldChannelMixin'>, FieldChannelMixin>` has no attribute `to_dict`
+ altair/vegalite/v6/schema/channels.py:224:16: error[unresolved-attribute] Object of type `<super: <class 'FieldChannelMixin'>, Self@to_dict>` has no attribute `to_dict`
- altair/vegalite/v6/schema/channels.py:247:16: warning[possibly-missing-attribute] Attribute `to_dict` may be missing on object of type `<super: <class 'ValueChannelMixin'>, ValueChannelMixin> | <super: <class 'ValueChannelMixin'>, Unknown>`
+ altair/vegalite/v6/schema/channels.py:247:16: warning[possibly-missing-attribute] Attribute `to_dict` may be missing on object of type `<super: <class 'ValueChannelMixin'>, Self@to_dict> | <super: <class 'ValueChannelMixin'>, Unknown>`
- altair/vegalite/v6/schema/channels.py:265:16: error[unresolved-attribute] Object of type `<super: <class 'DatumChannelMixin'>, DatumChannelMixin>` has no attribute `to_dict`
+ altair/vegalite/v6/schema/channels.py:265:16: error[unresolved-attribute] Object of type `<super: <class 'DatumChannelMixin'>, Self@to_dict>` has no attribute `to_dict`

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/server/contexts.py:381:16: error[unresolved-attribute] Object of type `<super: <class '_RequestProxy'>, _RequestProxy>` has no attribute `__getattr__`
+ src/bokeh/server/contexts.py:381:16: error[unresolved-attribute] Object of type `<super: <class '_RequestProxy'>, Self@__getattr__>` has no attribute `__getattr__`

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/sql/compilers/datafusion.py:540:16: error[unresolved-attribute] Object of type `<super: <class 'DataFusionCompiler'>, DataFusionCompiler>` has no attribute `visit_GroupConcat`
+ ibis/backends/sql/compilers/datafusion.py:540:16: error[unresolved-attribute] Object of type `<super: <class 'DataFusionCompiler'>, Self@visit_GroupConcat>` has no attribute `visit_GroupConcat`
- ibis/backends/sql/compilers/impala.py:273:13: error[unresolved-attribute] Object of type `<super: <class 'ImpalaCompiler'>, ImpalaCompiler>` has no attribute `visit_DateAdd`
- ibis/backends/sql/compilers/impala.py:284:13: error[unresolved-attribute] Object of type `<super: <class 'ImpalaCompiler'>, ImpalaCompiler>` has no attribute `visit_TimestampAdd`
+ ibis/backends/sql/compilers/impala.py:273:13: error[unresolved-attribute] Object of type `<super: <class 'ImpalaCompiler'>, Self@visit_DateAdd>` has no attribute `visit_DateAdd`
+ ibis/backends/sql/compilers/impala.py:284:13: error[unresolved-attribute] Object of type `<super: <class 'ImpalaCompiler'>, Self@visit_TimestampAdd>` has no attribute `visit_TimestampAdd`
- ibis/backends/sql/compilers/mysql.py:254:16: error[unresolved-attribute] Object of type `<super: <class 'MySQLCompiler'>, MySQLCompiler>` has no attribute `visit_Equals`
+ ibis/backends/sql/compilers/mysql.py:254:16: error[unresolved-attribute] Object of type `<super: <class 'MySQLCompiler'>, Self@visit_Equals>` has no attribute `visit_Equals`
- ibis/backends/sql/compilers/oracle.py:112:16: error[unresolved-attribute] Object of type `<super: <class 'OracleCompiler'>, OracleCompiler>` has no attribute `visit_Equals`
+ ibis/backends/sql/compilers/oracle.py:112:16: error[unresolved-attribute] Object of type `<super: <class 'OracleCompiler'>, Self@visit_Equals>` has no attribute `visit_Equals`
- ibis/backends/sql/compilers/postgres.py:525:16: error[unresolved-attribute] Object of type `<super: <class 'PostgresCompiler'>, PostgresCompiler>` has no attribute `visit_JSONGetItem`
+ ibis/backends/sql/compilers/postgres.py:525:16: error[unresolved-attribute] Object of type `<super: <class 'PostgresCompiler'>, Self@visit_JSONGetItem>` has no attribute `visit_JSONGetItem`
- ibis/common/typing.py:243:16: error[unresolved-attribute] Object of type `<super: <class 'DefaultTypeVars'>, <class 'DefaultTypeVars'>>` has no attribute `__class_getitem__`
+ ibis/common/typing.py:243:16: error[unresolved-attribute] Object of type `<super: <class 'DefaultTypeVars'>, type[Self@__class_getitem__]>` has no attribute `__class_getitem__`

sympy (https://github.com/sympy/sympy)
- sympy/diffgeom/diffgeom.py:2250:16: error[unresolved-attribute] Object of type `<super: <class '_deprecated_container'>, _deprecated_container>` has no attribute `__iter__`
+ sympy/diffgeom/diffgeom.py:2250:16: error[unresolved-attribute] Object of type `<super: <class '_deprecated_container'>, Self@__iter__>` has no attribute `__iter__`
- sympy/diffgeom/diffgeom.py:2254:16: error[unresolved-attribute] Object of type `<super: <class '_deprecated_container'>, _deprecated_container>` has no attribute `__getitem__`
+ sympy/diffgeom/diffgeom.py:2254:16: error[unresolved-attribute] Object of type `<super: <class '_deprecated_container'>, Self@__getitem__>` has no attribute `__getitem__`
- sympy/diffgeom/diffgeom.py:2258:16: error[unresolved-attribute] Object of type `<super: <class '_deprecated_container'>, _deprecated_container>` has no attribute `__contains__`
+ sympy/diffgeom/diffgeom.py:2258:16: error[unresolved-attribute] Object of type `<super: <class '_deprecated_container'>, Self@__contains__>` has no attribute `__contains__`
- sympy/functions/elementary/miscellaneous.py:639:22: error[unresolved-attribute] Object of type `<super: <class 'MinMaxBase'>, MinMaxBase>` has no attribute `fdiff`
+ sympy/functions/elementary/miscellaneous.py:639:22: error[unresolved-attribute] Object of type `<super: <class 'MinMaxBase'>, Self@_eval_derivative>` has no attribute `fdiff`
- sympy/matrices/expressions/matexpr.py:232:20: error[unresolved-attribute] Object of type `<super: <class 'MatrixExpr'>, MatrixExpr>` has no attribute `_eval_derivative`
+ sympy/matrices/expressions/matexpr.py:232:20: error[unresolved-attribute] Object of type `<super: <class 'MatrixExpr'>, Self@_eval_derivative>` has no attribute `_eval_derivative`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/base.py:482:13: error[unresolved-attribute] Object of type `<super: <class 'BaseEstimator'>, BaseEstimator>` has no attribute `__setstate__`
+ sklearn/base.py:482:13: error[unresolved-attribute] Object of type `<super: <class 'BaseEstimator'>, Self@__setstate__>` has no attribute `__setstate__`
- sklearn/base.py:545:16: error[unresolved-attribute] Object of type `<super: <class 'ClassifierMixin'>, ClassifierMixin>` has no attribute `__sklearn_tags__`
+ sklearn/base.py:545:16: error[unresolved-attribute] Object of type `<super: <class 'ClassifierMixin'>, Self@__sklearn_tags__>` has no attribute `__sklearn_tags__`
- sklearn/base.py:615:16: error[unresolved-attribute] Object of type `<super: <class 'RegressorMixin'>, RegressorMixin>` has no attribute `__sklearn_tags__`
+ sklearn/base.py:615:16: error[unresolved-attribute] Object of type `<super: <class 'RegressorMixin'>, Self@__sklearn_tags__>` has no attribute `__sklearn_tags__`
- sklearn/base.py:688:16: error[unresolved-attribute] Object of type `<super: <class 'ClusterMixin'>, ClusterMixin>` has no attribute `__sklearn_tags__`
+ sklearn/base.py:688:16: error[unresolved-attribute] Object of type `<super: <class 'ClusterMixin'>, Self@__sklearn_tags__>` has no attribute `__sklearn_tags__`
- sklearn/base.py:858:16: error[unresolved-attribute] Object of type `<super: <class 'TransformerMixin'>, TransformerMixin>` has no attribute `__sklearn_tags__`
+ sklearn/base.py:858:16: error[unresolved-attribute] Object of type `<super: <class 'TransformerMixin'>, Self@__sklearn_tags__>` has no attribute `__sklearn_tags__`
- sklearn/base.py:1039:16: error[unresolved-attribute] Object of type `<super: <class 'DensityMixin'>, DensityMixin>` has no attribute `__sklearn_tags__`
+ sklearn/base.py:1039:16: error[unresolved-attribute] Object of type `<super: <class 'DensityMixin'>, Self@__sklearn_tags__>` has no attribute `__sklearn_tags__`
- sklearn/base.py:1086:16: error[unresolved-attribute] Object of type `<super: <class 'OutlierMixin'>, OutlierMixin>` has no attribute `__sklearn_tags__`
+ sklearn/base.py:1086:16: error[unresolved-attribute] Object of type `<super: <class 'OutlierMixin'>, Self@__sklearn_tags__>` has no attribute `__sklearn_tags__`
- sklearn/base.py:1178:16: error[unresolved-attribute] Object of type `<super: <class 'MultiOutputMixin'>, MultiOutputMixin>` has no attribute `__sklearn_tags__`
+ sklearn/base.py:1178:16: error[unresolved-attribute] Object of type `<super: <class 'MultiOutputMixin'>, Self@__sklearn_tags__>` has no attribute `__sklearn_tags__`
- sklearn/base.py:1187:16: error[unresolved-attribute] Object of type `<super: <class '_UnstableArchMixin'>, _UnstableArchMixin>` has no attribute `__sklearn_tags__`
+ sklearn/base.py:1187:16: error[unresolved-attribute] Object of type `<super: <class '_UnstableArchMixin'>, Self@__sklearn_tags__>` has no attribute `__sklearn_tags__`
- sklearn/ensemble/_hist_gradient_boosting/predictor.py:134:13: error[unresolved-attribute] Object of type `<super: <class 'TreePredictor'>, TreePredictor>` has no attribute `__setstate__`
+ sklearn/ensemble/_hist_gradient_boosting/predictor.py:134:13: error[unresolved-attribute] Object of type `<super: <class 'TreePredictor'>, Self@__setstate__>` has no attribute `__setstate__`
- sklearn/externals/_arff.py:521:21: error[unresolved-attribute] Object of type `<super: <class '_DataListMixin'>, _DataListMixin>` has no attribute `decode_rows`
+ sklearn/externals/_arff.py:521:21: error[unresolved-attribute] Object of type `<super: <class '_DataListMixin'>, Self@decode_rows>` has no attribute `decode_rows`
- sklearn/model_selection/_split.py:91:16: error[unresolved-attribute] Object of type `<super: <class '_UnsupportedGroupCVMixin'>, _UnsupportedGroupCVMixin>` has no attribute `split`
+ sklearn/model_selection/_split.py:91:16: error[unresolved-attribute] Object of type `<super: <class '_UnsupportedGroupCVMixin'>, Self@split>` has no attribute `split`
- sklearn/neighbors/_base.py:1395:16: error[unresolved-attribute] Object of type `<super: <class 'RadiusNeighborsMixin'>, RadiusNeighborsMixin>` has no attribute `__sklearn_tags__`
+ sklearn/neighbors/_base.py:1395:16: error[unresolved-attribute] Object of type `<super: <class 'RadiusNeighborsMixin'>, Self@__sklearn_tags__>` has no attribute `__sklearn_tags__`

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/arrays/_mixins.py:246:16: error[invalid-return-type] Return type does not match returned value: expected `Self@_concat_same_type`, found `NDArrayBackedExtensionArray`
- pandas/core/arrays/arrow/array.py:1371:16: error[invalid-return-type] Return type does not match returned value: expected `Self@_pad_or_backfill`, found `ArrowExtensionArray`
- pandas/core/arrays/arrow/array.py:1386:20: error[invalid-return-type] Return type does not match returned value: expected `Self@fillna`, found `ArrowExtensionArray`
- pandas/core/arrays/arrow/array.py:1414:16: error[invalid-return-type] Return type does not match returned value: expected `Self@fillna`, found `ArrowExtensionArray`
- pandas/core/arrays/boolean.py:330:16: error[invalid-return-type] Return type does not match returned value: expected `Self@_simple_new`, found `BooleanArray`
- pandas/core/arrays/categorical.py:385:16: error[invalid-return-type] Return type does not match returned value: expected `Self@_simple_new`, found `Categorical`
- pandas/core/arrays/categorical.py:2567:16: error[invalid-return-type] Return type does not match returned value: expected `Self@unique`, found `Categorical`
- pandas/core/arrays/datetimelike.py:1588:16: error[invalid-return-type] Return type does not match returned value: expected `Self@_quantile`, found `DatetimeLikeArrayMixin`
- pandas/core/arrays/datetimelike.py:2539:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `TimelikeOps`
- pandas/core/arrays/datetimelike.py:2598:16: error[invalid-return-type] Return type does not match returned value: expected `Self@take`, found `TimelikeOps`
- pandas/core/arrays/datetimes.py:335:16: error[invalid-return-type] Return type does not match returned value: expected `Self@_simple_new`, found `DatetimeArray`
- pandas/core/arrays/numpy_.py:389:16: error[invalid-return-type] Return type does not match returned value: expected `Self@take`, found `NumpyExtensionArray`
- pandas/core/arrays/string_.py:601:16: error[invalid-return-type] Return type does not match returned value: expected `Self@view`, found `BaseStringArray`
- pandas/core/arrays/timedeltas.py:243:16: error[invalid-return-type] Return type does not match returned value: expected `Self@_simple_new`, found `TimedeltaArray`
- pandas/core/indexes/base.py:2906:16: error[invalid-return-type] Return type does not match returned value: expected `Self@drop_duplicates`, found `Index`
- pandas/core/indexes/datetimelike.py:928:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[Self@_wrap_join_result, ndarray[tuple[Any, ...], dtype[signedinteger[_64Bit]]] | None, ndarray[tuple[Any, ...], dtype[signedinteger[_64Bit]]] | None]`, found `tuple[DatetimeTimedeltaMixin | Unknown, ndarray[tuple[Any, ...], dtype[signedinteger[_64Bit]]] | None, ndarray[tuple[Any, ...], dtype[signedinteger[_64Bit]]] | None]`
- pandas/core/indexes/datetimelike.py:1036:16: error[invalid-return-type] Return type does not match returned value: expected `Self@delete`, found `DatetimeTimedeltaMixin`
- pandas/core/indexes/range.py:815:20: error[invalid-return-type] Return type does not match returned value: expected `Self@sort_values | tuple[Self@sort_values, ndarray[tuple[Any, ...], dtype[Any]] | RangeIndex]`, found `RangeIndex | tuple[RangeIndex, ndarray[tuple[Any, ...], dtype[Any]]]`
- pandas/core/series.py:5542:16: error[invalid-return-type] Return type does not match returned value: expected `Self@rename_axis | None`, found `Series | None`
- Found 3785 diagnostics
+ Found 3766 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/db/filtering.py:1038:16: error[invalid-return-type] Return type does not match returned value: expected `Self@make`, found `HistoryEventWithTxRefFilterQuery`
- rotkehlchen/db/filtering.py:1147:16: error[invalid-return-type] Return type does not match returned value: expected `Self@make`, found `HistoryEventWithCounterpartyFilterQuery`
- rotkehlchen/db/filtering.py:1228:16: error[invalid-return-type] Return type does not match returned value: expected `Self@make`, found `SolanaEventFilterQuery`
- rotkehlchen/db/filtering.py:1343:16: error[invalid-return-type] Return type does not match returned value: expected `Self@make`, found `EvmEventFilterQuery`
- rotkehlchen/db/filtering.py:1440:16: error[invalid-return-type] Return type does not match returned value: expected `Self@make`, found `EthStakingEventFilterQuery`
- Found 2089 diagnostics
+ Found 2084 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
- misc/python/materialize/mz_version.py:66:16: error[invalid-return-type] Return type does not match returned value: expected `T@parse`, found `TypedVersionBase`
- Found 311 diagnostics
+ Found 310 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/sparse/_matrix.py:146:16: error[unresolved-attribute] Object of type `<super: <class 'spmatrix'>, spmatrix>` has no attribute `todense`
+ scipy/sparse/_matrix.py:146:16: error[unresolved-attribute] Object of type `<super: <class 'spmatrix'>, Self@todense>` has no attribute `todense`
- scipy/sparse/tests/test_base.py:5709:21: error[unresolved-attribute] Object of type `<super: <class '_NonCanonicalMixin'>, _NonCanonicalMixin>` has no attribute `spcreator`
+ scipy/sparse/tests/test_base.py:5709:21: error[unresolved-attribute] Object of type `<super: <class '_NonCanonicalMixin'>, Self@spcreator>` has no attribute `spcreator`


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @charliermarsh on 2025-12-26 15:11_

---

_Marked ready for review by @charliermarsh on 2025-12-26 15:14_

---

_Review requested from @carljm by @charliermarsh on 2025-12-26 15:14_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-26 15:14_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-26 15:14_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-26 15:14_

---

_Comment by @astral-sh-bot[bot] on 2025-12-26 15:16_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 1 | 64 | 1 |
| `unresolved-attribute` | 1 | 0 | 58 |
| `invalid-await` | 0 | 0 | 6 |
| `invalid-argument-type` | 3 | 1 | 0 |
| `unused-ignore-comment` | 2 | 2 | 0 |
| `invalid-super-argument` | 0 | 0 | 1 |
| `possibly-missing-attribute` | 0 | 0 | 1 |
| **Total** | **7** | **67** | **67** |


**[Full report with detailed diff](https://196e9d3d.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://196e9d3d.ty-ecosystem-ext.pages.dev/timing))



---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/snapshots/super.md_-_Super_-_Basic_Usage_-_Implicit_Super_Objec…_(f9e5e48e3a4a4c12).snap`:178 on 2026-01-06 18:07_

We are losing some useful context in a lot of these diagnostics, that might help a user understand why the typevar isn't valid. Is it possible to restore this extra diagnostic context, while keeping the fix?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/bound_super.rs`:254 on 2026-01-06 18:36_

This code appears three different times -- extract a helper method? (Or do it up front once when constructing the bound super instance)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/bound_super.rs`:251 on 2026-01-06 19:03_

I feel like the logic here is a bit "loose" in a way that could bite us in the future (and is causing at least one undesirable behavior change in the tests). If I'm understanding right, the `NominalInstance` branch here should only ever occur if we have `SuperOwnerKind::InstanceTypeVar`, and the fallback branch should only ever occur if we have `SuperOwnerKind::ClassTypeVar`. If we had, for example, a `SuperOwnerKind::InstanceTypeVar` with a `SubclassOf` or `ClassLiteral` type in the upper bound, it would be wrong to treat that as if it were an instance-type upper bound class.

It feels to me like maybe we should be eagerly checking the upper bound when we create a typevar super, erroring if it doesn't look valid (this would be the `None` case below returning an empty MRO -- it seems a bit questionable to silently fall back when faced with a typevar upper bound that makes no sense for `super` -- this is causing the behavior change in `method10` in the tests), and maybe even just storing the extracted class then, so we don't have to re-extract it in multiple different places and deal with all the possibilities that we already checked up-front?


---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/bound_super.rs`:283 on 2026-01-06 19:06_

The name and docstring here seem misleading, given that I see this method used more generally (e.g. in `walk_bound_super_type` below)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/bound_super.rs`:530 on 2026-01-06 19:13_

I think the root cause of the lost diagnostic context mentioned in the snapshots is that now `typevar_context` is always `None`, whereas before we used to use `delegate_with_error_mapped` with a `Some` error context (in the old `Type::TypeVar` branch code that's removed in this PR.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/bound_super.rs`:384 on 2026-01-06 19:13_

Here and below is where we used to get typevar contextual information added to our diagnostics, and now we don't anymore.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/bound_super.rs`:332 on 2026-01-06 19:14_

In the current code in this PR, we never pass in a non-None `error_context` here. Maybe this will change if we fix the lost error context; if it doesn't, we should simplify this method (we probably only need `delegate_to` in that case, not `delegate_with_error_mapped`)

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/class/super.md`:242 on 2026-01-06 19:28_

I don't think this behavior change is desirable, but it may not be important, so we could potentially leave it as a TODO.

I think the preferable behavior here would be to create the union of bound-super instances, like before, where each element in the union acts as one of the constrained types -- but the binding_type for each element of the union still uses the original constrained typevar.

I think doing more checking of the bounds/constraints of a typevar up-front, and storing the extracted class type to use, as suggested in other comments, would make it pretty easy to handle this case, too.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/class/super.md`:265 on 2026-01-06 19:29_

I think this behavior change is also not desirable. `S` could be a callable type which is not a subclass of `Foo`, so this could fail at runtime. Erroring is better here.

The root cause here is our permissive handling of un-recognized bounds, which is mentioned in other comments. Early validation of bounds/constraints could help restore this error.

---

_@carljm requested changes on 2026-01-06 19:32_

Nice fix! I think there are currently a few undesired behavior changes coming along with it, and some effectively-unused code left behind.

---

_@AlexWaygood reviewed on 2026-01-06 19:46_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/super.md_-_Super_-_Basic_Usage_-_Implicit_Super_Objec…_(f9e5e48e3a4a4c12).snap`:161 on 2026-01-06 19:46_

I'll co-sign @carljm's comment below that it seems sad to lose these subidagnostics -- especially the last one, which gives you a hint on how to fix this common error!

---

_Comment by @charliermarsh on 2026-01-06 19:46_

Thank you! Will investigate.

---

_@AlexWaygood reviewed on 2026-01-06 20:29_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/super.md_-_Super_-_Basic_Usage_-_Implicit_Super_Objec…_(f9e5e48e3a4a4c12).snap`:5 on 2026-01-06 20:29_

can you update your installation of `cargo insta` and run `cargo insta test --force-update-snapshots`? The latest version of `cargo insta` causes loads of annoying warnings to be printed when running tests locally if the snapshot has the "old format" (the old format does not have this newline between the two dashed lines)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/bound_super.rs`:225 on 2026-01-06 20:32_

```suggestion
            None => ClassType::object(db),
```

---

_@AlexWaygood reviewed on 2026-01-06 20:32_

---

_Comment by @charliermarsh on 2026-01-06 20:33_

@AlexWaygood -- sorry still working through the feedback, I will re-request!

---

_Converted to draft by @charliermarsh on 2026-01-06 20:33_

---

_Marked ready for review by @charliermarsh on 2026-01-06 20:54_

---

_Review requested from @carljm by @charliermarsh on 2026-01-06 20:54_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-06 20:54_

---

_Comment by @charliermarsh on 2026-01-06 20:55_

Okay, I think this is closer?

---

_@charliermarsh reviewed on 2026-01-06 20:57_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/bound_super.rs`:362 on 2026-01-06 20:57_

This was already present, just moved up...

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/type_ordering.rs`:200 on 2026-01-06 22:14_

Missed this the first time through -- this doesn't look quite right. We should distinguish `InstanceTypeVar` from `ClassTypeVar` here, not conflate them. That is, we should extend the existing pattern above for the pre-existing enum variants, without introducing any union matches.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/bound_super.rs`:433 on 2026-01-06 22:26_

I don't think the fallback `_ => bound.to_class_type(db),` is correct here. We've "unwrapped" the subclass-of type; the typevar within it should have a `NominalInstance` bound. Tests all pass with this change:

```suggestion
                            if let Type::NominalInstance(instance) = bound {
                                SuperOwnerKind::ClassTypeVar(bound_typevar, instance.class(db))
                            } else {
                                return Err(BoundSuperError::AbstractOwnerType {
                                    owner_type,
                                    pivot_class: pivot_class_type,
                                    typevar_context: Some(typevar),
                                });
                            }
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/bound_super.rs`:447 on 2026-01-06 22:27_

nit

```suggestion
                            SuperOwnerKind::ClassTypeVar(bound_typevar, ClassType::object(db))
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/bound_super.rs`:532 on 2026-01-06 22:28_

```suggestion
                        SuperOwnerKind::InstanceTypeVar(bound_typevar, ClassType::object(db))
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/bound_super.rs`:544 on 2026-01-06 22:33_

Here again, I don't think this match arm is right. It's at least not needed to pass any current test. I think if we did support a class-literal type (or subclass-of type) as upper bound here, the correct thing to do might be to create a `ClassTypeVar` then, not an `InstanceTypeVar`? But I think we can leave consideration of that case to a separate PR, if it ever comes up in practice.

```suggestion
                        if let Type::NominalInstance(instance) = bound {
                            SuperOwnerKind::InstanceTypeVar(bound_typevar, instance.class(db))
                        } else {
                            return Err(BoundSuperError::AbstractOwnerType {
                                owner_type,
                                pivot_class: pivot_class_type,
                                typevar_context: Some(typevar),
                            });
                        }

```

---

_@carljm reviewed on 2026-01-06 22:35_

---

_Comment by @carljm on 2026-01-06 22:44_

There are some newly-added diagnostics in the ecosystem report here that I think would be worth understanding...

---

_Comment by @charliermarsh on 2026-01-06 23:06_

I will persevere

---

_Comment by @charliermarsh on 2026-01-06 23:24_

(Gonna push your fixes first, then review the diagnostics...)

---

_Comment by @charliermarsh on 2026-01-07 00:47_

For the ecosystem changes...

1. The PyGithub change is a now-unused type ignore, so that seems fine.

2. The new xarray diagnostic appears to be valid? Here's Claude's analysis (I also added a test to demonstrate it):

```
  The Type Contract:
  - TreeNode.__init__ has signature: def __init__(self, children: Mapping[str, Self] | None = None)
  - DataTree.__init__ has signature: def __init__(self, children: Mapping[str, DataTree] | None = None)

  The Problem:
  When DataTree.__init__ calls super().__init__(children=children):
  1. The parent TreeNode.__init__ expects Mapping[str, Self]
  2. Self represents "the exact type of the instance being constructed"
  3. DataTree.__init__ passes Mapping[str, DataTree] (a concrete type)

  Why This Matters:
  If someone subclasses DataTree:
  class MyDataTree(DataTree):
      pass

  mt = MyDataTree(children={"child": some_data_tree})

  In this case:
  - Self in TreeNode.__init__ would be MyDataTree
  - TreeNode expects Mapping[str, MyDataTree]
  - But DataTree.__init__ only accepts/passes Mapping[str, DataTree]
  - This is a type mismatch because Mapping is invariant in its value type

  The Fix (in xarray):
  xarray should change DataTree.__init__ to use Self instead of the concrete DataTree:
  def __init__(self, children: Mapping[str, Self] | None = None):
```

3. The beartype diagnostic was a false positive; I pushed a test case and a fix...


---

_Review requested from @carljm by @charliermarsh on 2026-01-07 00:47_

---

_Comment by @carljm on 2026-01-07 01:54_

The one part of the Claude analysis on xarray that looks wrong is that `Mapping` is covariant in its value type, not invariant. But I don't think this actually invalidates any of the rest of the analysis -- passing a `Mapping[str, DataTree]` when a `Mapping[str, MyDataTree]` is expected is also invalid under covariance! So I think the analysis is right, that's a true positive.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/class/super.md`:280 on 2026-01-07 02:00_

Ideally I'd love to see some analysis of how other type checkers handle these two cases, and whether they can ever work at runtime, to know whether this is a TODO for further improvement, or the correct behavior.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/class/super.md`:691 on 2026-01-07 02:02_

It seems like xarray actually used `Mapping`, but your test below uses `dict`. That's a significant difference since `Mapping` is covariant in its value type, but `dict` is invariant. I don't think that changes the behavior in this test, but using `Mapping` would make the test "stronger" since it would show that invariance is not actually the problem.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/bound_super.rs`:430 on 2026-01-07 02:27_

This is not really the right error kind here -- we'll throw this now for typevars with various upper bounds that are not necessarily abstract/structural at all (as in the `type[]` examples you added in the tests), they are just "types we can't extract a class from".

I'm actually wondering if the right thing here is not to error, but to fall back to the behavior from before this PR (where we just delegate to the bound). I think in most(?) cases that we don't handle as `ClassTypeVar`, that will just end up throwing an error later -- but it will be a more accurate error than this one. And if it doesn't throw an error, that means we've got a typevar whose upper bound is actually a valid subclass of the pivot type -- treating such a case as the upper bound at least won't be any worse than our behavior today.

---

_@carljm reviewed on 2026-01-07 02:31_

Ok sorry I keep finding new things to comment on here, not trying to be annoying... I think this is looking pretty good, just the errors we are throwing on the typevar cases we aren't handling don't look right, and I'm not clear whether they should necessarily be errors or not.

---

_@charliermarsh reviewed on 2026-01-07 02:44_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/class/super.md`:280 on 2026-01-07 02:44_

I hate to just dump Claude here but after making the change you suggested two comments below...

```
  method11 (self: S where S: type[Foo[int]]):
  - ✅ No error
  - Revealed: <super: <class 'Foo'>, <class 'Foo[int]'>>
  - Comment in test: "Delegates to the bound to resolve the super type"

  method12 (cls: type[S] where S: type[Foo[int]]):
  - ❌ Error: "type[S@method12] is a type variable with an abstract/structural type"
  - Revealed: Unknown

  Comparison with Other Type Checkers

  | Case                        | Pyright                              | Mypy                           | Pyrefly               | ty (updated)           |
  |-----------------------------|--------------------------------------|--------------------------------|-----------------------|------------------------|
  | method11 error on signature | ✅ "self must be supertype of class" | ✅ "erased type not supertype" | ❌                    | ❌                     |
  | method11 error on super()   | ❌                                   | ❌                             | ❌                    | ❌                     |
  | method11 revealed type      | object                               | super                          | super[object, Foo[T]] | <super: Foo, Foo[int]> |
  | method12 error on signature | ✅ "cls must be supertype of class"  | ✅ "erased type not supertype" | ❌                    | ❌                     |
  | method12 error on super()   | ❌                                   | ❌                             | ❌                    | ✅                     |
  | method12 revealed type      | type[object]                         | super                          | super[object, Foo]    | Unknown                |

  Analysis

  1. method11 is now handled well by ty: It extracts Foo[int] from type[Foo[int]] and builds a valid super object. This is reasonable - if someone writes self: S where S: type[Foo[int]], ty interprets the bound to mean "self is a class of type Foo[int]".
  2. method12 still errors: This is because cls: type[S] where S: type[Foo[int]] means cls would be type[type[Foo[int]]] - a metaclass. The bound type[Foo[int]] is "abstract/structural" from ty's perspective when wrapped in another type[].
  3. Runtime consideration: Both patterns can technically work at runtime since Python's super() just uses whatever value is passed. But semantically:
    - method11 is weird (why annotate self as a class?) but ty handles it
    - method12 is very weird (cls would be a metaclass of a metaclass)

  Recommendation for Carl's Question

  method11: ty's updated behavior is correct and arguably better than pyright/mypy. It provides a useful type by extracting the class from the bound, rather than falling back to object.

  method12: ty's error is reasonable because type[type[Foo[int]]] is an unusual/abstract type that doesn't have a clear nominal class to use for super(). However:
  - Pyright/mypy error on the signature instead (which is also valid)
  - The error could potentially be improved to mention that type[type[X]] is the issue

  This behavior seems correct - method12 represents a genuinely problematic pattern that should be flagged. The question is just whether the error message could be more precise about why it's problematic (the double type[] wrapping creates a metaclass-of-metaclass situation).
```

---

_@charliermarsh reviewed on 2026-01-07 03:02_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/class/super.md`:280 on 2026-01-07 03:02_

Sorry, updated analysis:

```
  Current ty behavior:

  | Case                                            | Error on super() | Revealed Type                              |
  |-------------------------------------------------|------------------|--------------------------------------------|
  | method11 (self: S where S: type[Foo[int]])      | ❌ No error      | <super: <class 'Foo'>, <class 'Foo[int]'>> |
  | method12 (cls: type[S] where S: type[Foo[int]]) | ❌ No error      | <super: <class 'Foo'>, Unknown>            |

  Updated comparison:

  | Case                        | Pyright      | Mypy  | ty (updated)           |
  |-----------------------------|--------------|-------|------------------------|
  | method11 error on signature | ✅           | ✅    | ❌                     |
  | method11 error on super()   | ❌           | ❌    | ❌                     |
  | method11 revealed type      | object       | super | <super: Foo, Foo[int]> |
  | method12 error on signature | ✅           | ✅    | ❌                     |
  | method12 error on super()   | ❌           | ❌    | ❌ (was ✅)            |
  | method12 revealed type      | type[object] | super | <super: Foo, Unknown>  |

  Analysis:

  The change now follows Carl's suggestion: instead of throwing AbstractOwnerType immediately, we delegate to the bound. For method12:
  - type[S] where S: type[Foo[int]] means we need type[type[Foo[int]]]
  - We can't construct that, so we fall back to type[Unknown]
  - type[Unknown] is a valid dynamic type, so super() succeeds with Unknown as the owner

  This is consistent with Carl's rationale: "if it doesn't throw an error, that means we've got a typevar whose upper bound is actually a valid subclass of the pivot type -- treating such a case as the upper bound at least won't be any worse than our behavior today."

  The behavior matches what pyright/mypy do (no error on super() itself), though they error on the signature which ty doesn't currently check.
```

---

_@charliermarsh reviewed on 2026-01-07 03:02_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/bound_super.rs`:430 on 2026-01-07 03:02_

I'm not certain that I got this right.

---

_@charliermarsh reviewed on 2026-01-07 03:03_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/class/super.md`:280 on 2026-01-07 03:03_

Should we be validating the signature in these cases? (Is that tracked elsewhere?)

---

_@charliermarsh reviewed on 2026-01-07 03:07_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/resources/mdtest/class/super.md`:280 on 2026-01-07 03:07_

I see a few TODOs that suggest we _do_ want to do this (validate that the `self/cls` annotation is a supertype of the enclosing class), but that seems like a separate concern from this PR.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/class/super.md`:280 on 2026-01-08 00:30_

Yeah, we should validate the `self` / `cls` annotation, and yeah that's separate from this PR.

Thanks for the analysis! I'm happy with this behavior.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/bound_super.rs`:240 on 2026-01-08 00:33_

I suspect this maybe shouldn't be a no-op, but I think it's fine to just leave a TODO and wait for a real repro to arrive.

```suggestion
                // TODO: we might need to normalize the nested class here?
                Some(self)
```

---

_@carljm approved on 2026-01-08 00:35_

Looks good to me! Thanks for persevering :)

---

_Comment by @charliermarsh on 2026-01-08 00:46_

Thank you for the great reviews! Sorry for all the back-and-forth.

---

_Merged by @charliermarsh on 2026-01-08 00:56_

---

_Closed by @charliermarsh on 2026-01-08 00:56_

---

_Branch deleted on 2026-01-08 00:56_

---
