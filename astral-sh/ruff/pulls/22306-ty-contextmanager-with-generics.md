```yaml
number: 22306
title: "[ty] contextmanager with generics"
type: pull_request
state: closed
author: eclbg
labels:
  - ty
assignees: []
draft: true
base: main
head: contextmanager-types
created_at: 2025-12-30T18:21:40Z
updated_at: 2025-12-31T09:34:23Z
url: https://github.com/astral-sh/ruff/pull/22306
synced_at: 2026-01-12T15:57:46Z
```

# [ty] contextmanager with generics

---

_@eclbg_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @carljm by @eclbg on 2025-12-30 18:21_

---

_Review requested from @AlexWaygood by @eclbg on 2025-12-30 18:21_

---

_Review requested from @sharkdp by @eclbg on 2025-12-30 18:21_

---

_Review requested from @dcreager by @eclbg on 2025-12-30 18:21_

---

_Comment by @astral-sh-bot[bot] on 2025-12-30 18:23_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-30 18:24_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dulwich (https://github.com/dulwich/dulwich)
- dulwich/porcelain/__init__.py:643:16: error[invalid-return-type] Return type does not match returned value: expected `AbstractContextManager[T@open_repo | Repo, bool | None]`, found `_GeneratorContextManager[T@_noop_context_manager, None, None]`
+ dulwich/porcelain/__init__.py:643:16: error[invalid-return-type] Return type does not match returned value: expected `AbstractContextManager[T@open_repo | Repo, bool | None]`, found `_GeneratorContextManager[(str & BaseRepo) | (PathLike[str] & BaseRepo) | T@open_repo, None, None]`
- dulwich/porcelain/__init__.py:643:38: error[invalid-argument-type] Argument is incorrect: Expected `T@_noop_context_manager`, found `(str & BaseRepo) | (PathLike[str] & BaseRepo) | T@open_repo`
- dulwich/porcelain/__init__.py:697:16: error[invalid-return-type] Return type does not match returned value: expected `AbstractContextManager[T@open_repo_closing | Repo, bool | None]`, found `_GeneratorContextManager[T@_noop_context_manager, None, None]`
+ dulwich/porcelain/__init__.py:697:16: error[invalid-return-type] Return type does not match returned value: expected `AbstractContextManager[T@open_repo_closing | Repo, bool | None]`, found `_GeneratorContextManager[(str & BaseRepo) | (bytes & BaseRepo) | (PathLike[str] & BaseRepo) | T@open_repo_closing, None, None]`
- dulwich/porcelain/__init__.py:697:38: error[invalid-argument-type] Argument is incorrect: Expected `T@_noop_context_manager`, found `(str & BaseRepo) | (bytes & BaseRepo) | (PathLike[str] & BaseRepo) | T@open_repo_closing`
- Found 231 diagnostics
+ Found 229 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/addons/test_proxyserver.py:798:37: error[invalid-argument-type] Argument is incorrect: Expected `type[T@quic_connect]`, found `<class 'H3Client'>`
- test/mitmproxy/addons/test_proxyserver.py:832:17: error[invalid-argument-type] Argument is incorrect: Expected `type[T@quic_connect]`, found `<class 'QuicDatagramClient'>`
- test/mitmproxy/addons/test_proxyserver.py:877:37: error[invalid-argument-type] Argument is incorrect: Expected `type[T@quic_connect]`, found `<class 'H3Client'>`
- Found 2148 diagnostics
+ Found 2145 diagnostics

mypy (https://github.com/python/mypy)
- mypy/subtypes.py:187:26: error[invalid-argument-type] Argument is incorrect: Expected `list[tuple[T@pop_on_exit, T@pop_on_exit]]`, found `list[tuple[Type, Type]]`
- mypy/subtypes.py:187:71: error[invalid-argument-type] Argument is incorrect: Expected `T@pop_on_exit`, found `Type`
- mypy/subtypes.py:187:77: error[invalid-argument-type] Argument is incorrect: Expected `T@pop_on_exit`, found `Type`
- mypy/subtypes.py:226:26: error[invalid-argument-type] Argument is incorrect: Expected `list[tuple[T@pop_on_exit, T@pop_on_exit]]`, found `list[tuple[Type, Type]]`
- mypy/subtypes.py:226:70: error[invalid-argument-type] Argument is incorrect: Expected `T@pop_on_exit`, found `Type`
- mypy/subtypes.py:226:76: error[invalid-argument-type] Argument is incorrect: Expected `T@pop_on_exit`, found `Type`
- mypy/subtypes.py:1212:22: error[invalid-argument-type] Argument is incorrect: Expected `list[tuple[T@pop_on_exit, T@pop_on_exit]]`, found `Unknown | list[tuple[Instance, Instance]]`
- mypy/subtypes.py:1212:32: error[invalid-argument-type] Argument is incorrect: Expected `T@pop_on_exit`, found `Instance`
- mypy/subtypes.py:1212:38: error[invalid-argument-type] Argument is incorrect: Expected `T@pop_on_exit`, found `Instance`
- Found 1753 diagnostics
+ Found 1744 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_tests/test_highlevel_open_unix_stream.py:28:25: error[invalid-argument-type] Argument is incorrect: Expected `CloseT@close_on_error`, found `CloseMe`
- src/trio/_tests/test_highlevel_open_unix_stream.py:33:29: error[invalid-argument-type] Argument is incorrect: Expected `CloseT@close_on_error`, found `CloseMe`
- Found 483 diagnostics
+ Found 481 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
- xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
+ xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/relay/types.py:834:37: error[invalid-argument-type] Argument is incorrect: Expected `_T@aclosing`, found `AsyncIterable[NodeType@ListConnection]`
- strawberry/schema/schema.py:854:45: error[invalid-argument-type] Argument is incorrect: Expected `_T@aclosing`, found `AsyncIterator[ExecutionResult] & ~ExecutionResult`
- Found 358 diagnostics
+ Found 356 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-aws/prefect_aws/experimental/bundles/execute.py:52:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `None | Coroutine[Any, Any, None] | AwsCredentials`
+ src/integrations/prefect-aws/prefect_aws/experimental/bundles/execute.py:52:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `AwsCredentials | Coroutine[Any, Any, AwsCredentials]`
- src/integrations/prefect-aws/prefect_aws/experimental/bundles/upload.py:68:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `None | Coroutine[Any, Any, None] | AwsCredentials`
+ src/integrations/prefect-aws/prefect_aws/experimental/bundles/upload.py:68:10: warning[possibly-missing-attribute] Attribute `get_s3_client` may be missing on object of type `AwsCredentials | Coroutine[Any, Any, AwsCredentials]`
- src/integrations/prefect-aws/tests/test_s3.py:1141:24: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `credentials`
+ src/integrations/prefect-aws/tests/test_s3.py:1141:24: warning[possibly-missing-attribute] Attribute `credentials` may be missing on object of type `S3Bucket | Coroutine[Any, Any, S3Bucket]`
- src/integrations/prefect-azure/prefect_azure/experimental/bundles/execute.py:28:19: error[invalid-await] `None | Coroutine[Any, Any, None]` is not awaitable
+ src/integrations/prefect-azure/prefect_azure/experimental/bundles/execute.py:28:19: error[invalid-await] `AzureBlobStorageCredentials | Coroutine[Any, Any, AzureBlobStorageCredentials]` is not awaitable
- src/integrations/prefect-azure/prefect_azure/experimental/bundles/upload.py:38:19: error[invalid-await] `None | Coroutine[Any, Any, None]` is not awaitable
+ src/integrations/prefect-azure/prefect_azure/experimental/bundles/upload.py:38:19: error[invalid-await] `AzureBlobStorageCredentials | Coroutine[Any, Any, AzureBlobStorageCredentials]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/tests/cli/test_credentials.py:75:36: error[invalid-await] `None | Coroutine[Any, Any, None]` is not awaitable
+ src/integrations/prefect-dbt/tests/cli/test_credentials.py:75:36: error[invalid-await] `DbtCliProfile | Coroutine[Any, Any, DbtCliProfile]` is not awaitable
+ src/integrations/prefect-email/tests/test_credentials.py:91:12: warning[possibly-missing-attribute] Attribute `smtp_type` may be missing on object of type `EmailServerCredentials | Coroutine[Any, Any, EmailServerCredentials]`
+ src/integrations/prefect-email/tests/test_credentials.py:92:14: warning[possibly-missing-attribute] Attribute `get_server` may be missing on object of type `EmailServerCredentials | Coroutine[Any, Any, EmailServerCredentials]`
- src/integrations/prefect-email/tests/test_credentials.py:91:12: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `smtp_type`
+ src/integrations/prefect-email/tests/test_credentials.py:93:12: error[unresolved-attribute] Object of type `SMTP` has no attribute `port`
- src/integrations/prefect-email/tests/test_credentials.py:92:14: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `get_server`
- src/integrations/prefect-email/tests/test_credentials.py:108:12: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `smtp_type`
+ src/integrations/prefect-email/tests/test_credentials.py:108:12: warning[possibly-missing-attribute] Attribute `smtp_type` may be missing on object of type `EmailServerCredentials | Coroutine[Any, Any, EmailServerCredentials]`
+ src/integrations/prefect-email/tests/test_credentials.py:109:12: warning[possibly-missing-attribute] Attribute `verify` may be missing on object of type `EmailServerCredentials | Coroutine[Any, Any, EmailServerCredentials]`
+ src/integrations/prefect-email/tests/test_credentials.py:110:14: warning[possibly-missing-attribute] Attribute `get_server` may be missing on object of type `EmailServerCredentials | Coroutine[Any, Any, EmailServerCredentials]`
- src/integrations/prefect-email/tests/test_credentials.py:109:12: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `verify`
+ src/integrations/prefect-email/tests/test_credentials.py:111:12: error[unresolved-attribute] Object of type `SMTP` has no attribute `port`
- src/integrations/prefect-email/tests/test_credentials.py:110:14: error[unresolved-attribute] Object of type `None | Coroutine[Any, Any, None]` has no attribute `get_server`
- src/integrations/prefect-sqlalchemy/tests/test_database.py:447:18: error[invalid-context-manager] Object of type `None | Coroutine[Any, Any, None]` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
+ src/integrations/prefect-sqlalchemy/tests/test_database.py:447:18: error[invalid-context-manager] Object of type `SqlAlchemyConnector | Coroutine[Any, Any, SqlAlchemyConnector]` cannot be used with `with` because the methods `__enter__` and `__exit__` are possibly missing
- src/integrations/prefect-sqlalchemy/tests/test_database.py:466:18: error[invalid-context-manager] Object of type `None | Coroutine[Any, Any, None]` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
+ src/integrations/prefect-sqlalchemy/tests/test_database.py:466:18: error[invalid-context-manager] Object of type `SqlAlchemyConnector | Coroutine[Any, Any, SqlAlchemyConnector]` cannot be used with `with` because the methods `__enter__` and `__exit__` are possibly missing
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deployment.py:292:49: error[unresolved-attribute] Object of type `None` has no attribute `model_dump`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/deployments/steps/pull.py:288:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ReadableDeploymentStorage | WritableDeploymentStorage`, found `None`
+ src/prefect/deployments/steps/pull.py:288:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ReadableDeploymentStorage | WritableDeploymentStorage`, found `Block`
- src/prefect/flow_engine.py:696:32: error[unresolved-attribute] Object of type `None` has no attribute `client`
- src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1278:32: error[unresolved-attribute] Object of type `None` has no attribute `client`
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
- src/prefect/task_engine.py:821:32: error[unresolved-attribute] Object of type `None` has no attribute `client`
- src/prefect/testing/utilities.py:280:17: error[invalid-argument-type] Argument to function `assert_blocks_equal` is incorrect: Expected `Block`, found `ReadableFileSystem | None`
- src/prefect/testing/utilities.py:291:17: error[invalid-argument-type] Argument to function `assert_blocks_equal` is incorrect: Expected `Block`, found `ReadableFileSystem | None`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
- Found 5534 diagnostics
+ Found 5525 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/snowflake/__init__.py:574:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown | DataType]`
+ ibis/backends/snowflake/__init__.py:574:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Any, Unknown | DataType]`
- ibis/backends/snowflake/__init__.py:590:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown | DataType]`
+ ibis/backends/snowflake/__init__.py:590:13: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Any, Unknown | DataType]`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Self@iloc | Series[Any, Any], generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Self@iloc | SeriesHE[Any, Any], generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`

zulip (https://github.com/zulip/zulip)
- zerver/data_import/slack.py:1798:13: error[invalid-argument-type] Argument to function `convert_slack_workspace_messages` is incorrect: Expected `(UploadFileRequest, /) -> None`, found `(ParallelRecordType@run_parallel_queue, /) -> None`
- zerver/lib/export.py:2288:9: error[invalid-argument-type] Argument is incorrect: Expected `(ParallelRecordType@run_parallel_queue, /) -> None`, found `def _save_s3_key_to_file(key_name: str) -> None`
- zerver/lib/export.py:2299:65: error[invalid-argument-type] Argument to function `iterate_attachments` is incorrect: Expected `(str, /) -> Any`, found `(ParallelRecordType@run_parallel_queue, /) -> None`
- zerver/lib/parallel.py:54:9: error[invalid-argument-type] Argument is incorrect: Expected `(ParallelRecordType@run_parallel_queue, /) -> None`, found `(ParallelRecordType@run_parallel, /) -> None`
- zerver/lib/parallel.py:63:20: error[invalid-argument-type] Argument is incorrect: Expected `ParallelRecordType@run_parallel_queue`, found `ParallelRecordType@run_parallel`
- zerver/tests/test_parallel.py:135:21: error[invalid-argument-type] Argument is incorrect: Expected `ParallelRecordType@run_parallel_queue`, found `Literal[100]`
- zerver/tests/test_parallel.py:143:21: error[invalid-argument-type] Argument is incorrect: Expected `ParallelRecordType@run_parallel_queue`, found `Literal[101]`
- zerver/tests/test_parallel.py:144:21: error[invalid-argument-type] Argument is incorrect: Expected `ParallelRecordType@run_parallel_queue`, found `Literal[102]`
- Found 3663 diagnostics
+ Found 3655 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1228:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5125 diagnostics
+ Found 5126 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Renamed from "contextmanager with generics" to "[ty] contextmanager with generics" by @AlexWaygood on 2025-12-30 18:25_

---

_Label `ty` added by @AlexWaygood on 2025-12-30 18:26_

---

_Review requested from @MichaReiser by @eclbg on 2025-12-31 08:53_

---

_Converted to draft by @eclbg on 2025-12-31 09:00_

---

_Comment by @eclbg on 2025-12-31 09:01_

Sorry for the noise, very much a draft for now.

---

_Comment by @codspeed-hq[bot] on 2025-12-31 09:04_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/eclbg%3Acontextmanager-types?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22306 will **degrade performance by 5.94%**

<sub>Comparing <code>eclbg:contextmanager-types</code> (67f12ee) with <code>main</code> (758926e)</sub>



### Summary

`❌ 1` regression  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/eclbg%3Acontextmanager-types?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` colour_science ``](https://codspeed.io/astral-sh/ruff/branches/eclbg%3Acontextmanager-types?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Acolour_science&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 94.7 s | 100.7 s | -5.94% |

[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/eclbg%3Acontextmanager-types?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @eclbg on 2025-12-31 09:24_

I'll close this for now and reopen when I'm sure it's worth a look

---

_Closed by @eclbg on 2025-12-31 09:24_

---
