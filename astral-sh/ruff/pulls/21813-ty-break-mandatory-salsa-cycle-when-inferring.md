```yaml
number: 21813
title: "[ty] Break mandatory salsa cycle when inferring function definitions"
type: pull_request
state: closed
author: dcreager
labels:
  - performance
  - ty
assignees: []
draft: true
base: main
head: dcreager/break-the-cycle
created_at: 2025-12-05T14:41:44Z
updated_at: 2025-12-11T20:01:07Z
url: https://github.com/astral-sh/ruff/pull/21813
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Break mandatory salsa cycle when inferring function definitions

---

_Pull request opened by @dcreager on 2025-12-05 14:41_

This is a first stab at fixing astral-sh/ty#1729. Pushing it up in draft form to get a linkable PR number for it.

---

_Label `internal` added by @dcreager on 2025-12-05 14:41_

---

_Label `ty` added by @dcreager on 2025-12-05 14:41_

---

_Comment by @astral-sh-bot[bot] on 2025-12-05 14:43_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-10 13:07:04.057970492 +0000
+++ new-output.txt	2025-12-10 13:07:07.711980161 +0000
@@ -150,7 +150,7 @@
 callables_protocol.py:169:7: error[invalid-assignment] Object of type `def cb8_bad1(x: int) -> Any` is not assignable to `Proto8`
 callables_protocol.py:186:5: error[invalid-assignment] Object of type `Literal["str"]` is not assignable to attribute `other_attribute` of type `int`
 callables_protocol.py:187:5: error[unresolved-attribute] Unresolved attribute `xxx` on type `Proto9[P@decorator1, R@decorator1]`.
-callables_protocol.py:197:7: error[unresolved-attribute] Object of type `Proto9[(x: int), Unknown] | Proto9[Divergent, Unknown]` has no attribute `other_attribute2`
+callables_protocol.py:197:7: error[unresolved-attribute] Object of type `Proto9[(x: int), Unknown]` has no attribute `other_attribute2`
 callables_protocol.py:238:8: error[invalid-assignment] Object of type `def cb11_bad1(x: int, y: str, /) -> Any` is not assignable to `Proto11`
 callables_protocol.py:260:8: error[invalid-assignment] Object of type `def cb12_bad1(*args: Any, *, kwarg0: Any) -> None` is not assignable to `Proto12`
 callables_protocol.py:284:27: error[invalid-assignment] Object of type `def cb13_no_default(path: str) -> str` is not assignable to `Proto13_Default`

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-05 14:45_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/_code/code.py:501:21: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ src/_pytest/_code/code.py:507:24: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ src/_pytest/_code/code.py:551:25: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ src/_pytest/_code/code.py:590:45: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ src/_pytest/_code/code.py:596:23: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ src/_pytest/recwarn.py:190:23: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
- src/_pytest/unittest.py:583:14: error[missing-argument] No argument provided for required parameter `cls`
- testing/test_config.py:1441:14: error[missing-argument] No argument provided for required parameter `cls`
- testing/test_config.py:1763:10: error[missing-argument] No argument provided for required parameter `cls`
- testing/test_faulthandler.py:216:10: error[missing-argument] No argument provided for required parameter `cls`
- testing/test_monkeypatch.py:426:10: error[missing-argument] No argument provided for required parameter `cls`
- Found 443 diagnostics
+ Found 444 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/schemas.py:118:28: error[unresolved-attribute] Object of type `property` has no attribute `BaseTransport`
+ src/schemathesis/schemas.py:118:28: error[unresolved-attribute] Object of type `property | property` has no attribute `BaseTransport`

urllib3 (https://github.com/urllib3/urllib3)
- test/with_dummyserver/test_chunked_transfer.py:299:14: error[missing-argument] No argument provided for required parameter `monkeypatch`
- test/with_dummyserver/test_chunked_transfer.py:299:36: error[invalid-argument-type] Argument is incorrect: Expected `Self@mark`, found `MonkeyPatch`
- test/with_dummyserver/test_chunked_transfer.py:327:14: error[missing-argument] No argument provided for required parameter `monkeypatch`
- test/with_dummyserver/test_chunked_transfer.py:327:36: error[invalid-argument-type] Argument is incorrect: Expected `Self@mark`, found `MonkeyPatch`
- Found 273 diagnostics
+ Found 269 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- cki_lib/inttests/cluster.py:78:31: error[missing-argument] No argument provided for required parameter `cls`
- cki_lib/inttests/mariadb.py:22:31: error[missing-argument] No argument provided for required parameter `cls`
- cki_lib/inttests/mariadb.py:29:14: error[missing-argument] No argument provided for required parameter `namespace`
- cki_lib/inttests/mariadb.py:29:32: error[invalid-argument-type] Argument is incorrect: Expected `Self@k8s_namespace`, found `Literal["mariadb"]`
- cki_lib/inttests/minio.py:22:31: error[missing-argument] No argument provided for required parameter `cls`
- cki_lib/inttests/minio.py:29:14: error[missing-argument] No argument provided for required parameter `namespace`
- cki_lib/inttests/minio.py:29:32: error[invalid-argument-type] Argument is incorrect: Expected `Self@k8s_namespace`, found `Literal["minio"]`
- cki_lib/inttests/rabbitmq.py:36:31: error[missing-argument] No argument provided for required parameter `cls`
- cki_lib/inttests/rabbitmq.py:43:14: error[missing-argument] No argument provided for required parameter `namespace`
- cki_lib/inttests/rabbitmq.py:43:32: error[invalid-argument-type] Argument is incorrect: Expected `Self@k8s_namespace`, found `Literal["rabbitmq"]`
- cki_lib/inttests/remote_responses.py:123:31: error[missing-argument] No argument provided for required parameter `cls`
- cki_lib/inttests/remote_responses.py:135:14: error[missing-argument] No argument provided for required parameter `namespace`
- cki_lib/inttests/remote_responses.py:135:32: error[invalid-argument-type] Argument is incorrect: Expected `Self@k8s_namespace`, found `Literal["remote-responses"]`
- cki_lib/inttests/sqs.py:29:31: error[missing-argument] No argument provided for required parameter `cls`
- cki_lib/inttests/sqs.py:37:14: error[missing-argument] No argument provided for required parameter `namespace`
- cki_lib/inttests/sqs.py:37:32: error[invalid-argument-type] Argument is incorrect: Expected `Self@k8s_namespace`, found `Literal["sqs"]`
- cki_lib/inttests/vault.py:22:31: error[missing-argument] No argument provided for required parameter `cls`
- cki_lib/inttests/vault.py:29:14: error[missing-argument] No argument provided for required parameter `namespace`
- cki_lib/inttests/vault.py:29:32: error[invalid-argument-type] Argument is incorrect: Expected `Self@k8s_namespace`, found `Literal["vault"]`
- tests/kcidb/test_kcidb_file.py:83:14: error[missing-argument] No argument provided for required parameter `kcidb_data`
- Found 271 diagnostics
+ Found 251 diagnostics

operator (https://github.com/canonical/operator)
- ops/_private/harness.py:378:48: error[unresolved-attribute] Object of type `property` has no attribute `Container`
+ ops/_private/harness.py:378:48: error[unresolved-attribute] Object of type `property | property` has no attribute `Container`
- ops/_private/harness.py:401:24: error[unresolved-attribute] Object of type `property` has no attribute `Model`
+ ops/_private/harness.py:401:24: error[unresolved-attribute] Object of type `property | property` has no attribute `Model`
- ops/_private/harness.py:406:28: error[unresolved-attribute] Object of type `property` has no attribute `Framework`
+ ops/_private/harness.py:406:28: error[unresolved-attribute] Object of type `property | property` has no attribute `Framework`
- ops/_private/harness.py:2144:36: error[unresolved-attribute] Object of type `property` has no attribute `CloudSpec`
+ ops/_private/harness.py:2144:36: error[unresolved-attribute] Object of type `property | property` has no attribute `CloudSpec`
- ops/model.py:2544:65: error[unresolved-attribute] Object of type `property` has no attribute `Client`
+ ops/model.py:2544:65: error[unresolved-attribute] Object of type `property | property` has no attribute `Client`
- ops/model.py:2635:22: error[unresolved-attribute] Object of type `property` has no attribute `LayerDict`
+ ops/model.py:2635:22: error[unresolved-attribute] Object of type `property | property` has no attribute `LayerDict`
- ops/model.py:2635:41: error[unresolved-attribute] Object of type `property` has no attribute `Layer`
+ ops/model.py:2635:41: error[unresolved-attribute] Object of type `property | property` has no attribute `Layer`
- ops/model.py:2654:27: error[unresolved-attribute] Object of type `property` has no attribute `Plan`
+ ops/model.py:2654:27: error[unresolved-attribute] Object of type `property | property` has no attribute `Plan`
- ops/model.py:2663:65: error[unresolved-attribute] Object of type `property` has no attribute `ServiceInfo`
+ ops/model.py:2663:65: error[unresolved-attribute] Object of type `property | property` has no attribute `ServiceInfo`
- ops/model.py:2673:49: error[unresolved-attribute] Object of type `property` has no attribute `ServiceInfo`
+ ops/model.py:2673:49: error[unresolved-attribute] Object of type `property | property` has no attribute `ServiceInfo`
- ops/model.py:2687:41: error[unresolved-attribute] Object of type `property` has no attribute `CheckLevel`
+ ops/model.py:2687:41: error[unresolved-attribute] Object of type `property | property` has no attribute `CheckLevel`
- ops/model.py:2700:45: error[unresolved-attribute] Object of type `property` has no attribute `CheckInfo`
+ ops/model.py:2700:45: error[unresolved-attribute] Object of type `property | property` has no attribute `CheckInfo`
- ops/model.py:2827:15: error[unresolved-attribute] Object of type `property` has no attribute `FileInfo`
+ ops/model.py:2827:15: error[unresolved-attribute] Object of type `property | property` has no attribute `FileInfo`
- ops/model.py:3007:46: error[unresolved-attribute] Object of type `property` has no attribute `FileInfo`
+ ops/model.py:3007:46: error[unresolved-attribute] Object of type `property | property` has no attribute `FileInfo`
- ops/model.py:3048:46: error[unresolved-attribute] Object of type `property` has no attribute `FileInfo`
+ ops/model.py:3048:46: error[unresolved-attribute] Object of type `property | property` has no attribute `FileInfo`
- ops/model.py:3049:20: error[unresolved-attribute] Object of type `property` has no attribute `FileInfo`
+ ops/model.py:3049:20: error[unresolved-attribute] Object of type `property | property` has no attribute `FileInfo`
- ops/model.py:3191:10: error[unresolved-attribute] Object of type `property` has no attribute `ExecProcess`
+ ops/model.py:3191:10: error[unresolved-attribute] Object of type `property | property` has no attribute `ExecProcess`
- ops/model.py:3212:10: error[unresolved-attribute] Object of type `property` has no attribute `ExecProcess`
+ ops/model.py:3212:10: error[unresolved-attribute] Object of type `property | property` has no attribute `ExecProcess`
- ops/model.py:3231:10: error[unresolved-attribute] Object of type `property` has no attribute `ExecProcess`
+ ops/model.py:3231:10: error[unresolved-attribute] Object of type `property | property` has no attribute `ExecProcess`
- ops/model.py:3283:38: error[unresolved-attribute] Object of type `property` has no attribute `Notice`
+ ops/model.py:3283:38: error[unresolved-attribute] Object of type `property | property` has no attribute `Notice`
- ops/model.py:3301:16: error[unresolved-attribute] Object of type `property` has no attribute `NoticesUsers`
+ ops/model.py:3301:16: error[unresolved-attribute] Object of type `property | property` has no attribute `NoticesUsers`
- ops/model.py:3303:25: error[unresolved-attribute] Object of type `property` has no attribute `NoticeType`
+ ops/model.py:3303:25: error[unresolved-attribute] Object of type `property | property` has no attribute `NoticeType`
- ops/model.py:3305:15: error[unresolved-attribute] Object of type `property` has no attribute `Notice`
+ ops/model.py:3305:15: error[unresolved-attribute] Object of type `property | property` has no attribute `Notice`
- ops/model.py:3322:25: error[unresolved-attribute] Object of type `property` has no attribute `Client`
+ ops/model.py:3322:25: error[unresolved-attribute] Object of type `property | property` has no attribute `Client`

trio (https://github.com/python-trio/trio)
- src/trio/_channel.py:610:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@as_safe_channel) -> AbstractAsyncContextManager[ReceiveChannel[T@as_safe_channel], bool | None]`, found `((self, *args: P@as_safe_channel.args, **kwargs: P@as_safe_channel.kwargs) -> _AsyncGeneratorContextManager[Unknown, None]) | Unknown`
+ src/trio/_channel.py:610:12: error[invalid-return-type] Return type does not match returned value: expected `(**P@as_safe_channel) -> AbstractAsyncContextManager[ReceiveChannel[T@as_safe_channel], bool | None]`, found `(self, *args: P@as_safe_channel.args, **kwargs: P@as_safe_channel.kwargs) -> _AsyncGeneratorContextManager[Unknown, None]`
+ src/trio/testing/_raises_group.py:50:21: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ src/trio/testing/_raises_group.py:54:24: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ src/trio/testing/_raises_group.py:60:25: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ src/trio/testing/_raises_group.py:73:23: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
- Found 495 diagnostics
+ Found 499 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/plugins/worksearch/code.py:1207:9: error[invalid-argument-type] Argument to function `run_solr_query` is incorrect: Expected `Literal["UNLABELLED", "BOOK_SEARCH", "BOOK_SEARCH_API", "BOOK_SEARCH_FACETS", "BOOK_CAROUSEL", ... omitted 11 literals]`, found `str`
- Found 1137 diagnostics
+ Found 1136 diagnostics

setuptools (https://github.com/pypa/setuptools)
- setuptools/_vendor/autocommand/autoparse.py:306:5: error[invalid-assignment] Object of type `Unknown & ~None` is not assignable to attribute `func` on type `_Wrapped[(...), Unknown, (argv=None), Unknown] | _Wrapped[(...), Unknown, Divergent, Unknown]`
- setuptools/_vendor/autocommand/autoparse.py:307:5: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `parser` on type `_Wrapped[(...), Unknown, (argv=None), Unknown] | _Wrapped[(...), Unknown, Divergent, Unknown]`
+ setuptools/_vendor/autocommand/autoparse.py:306:5: error[unresolved-attribute] Unresolved attribute `func` on type `_Wrapped[(...), Unknown, (argv=None), Unknown]`.
+ setuptools/_vendor/autocommand/autoparse.py:307:5: error[unresolved-attribute] Unresolved attribute `parser` on type `_Wrapped[(...), Unknown, (argv=None), Unknown]`.
- setuptools/build_meta.py:300:18: error[missing-argument] No argument provided for required parameter `cls`
- setuptools/tests/test_wheel.py:677:13: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `str | dict[Unknown | str, Unknown] | dict[str, list[Unknown | str]] | Unknown`
- Found 1284 diagnostics
+ Found 1282 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/integrations/prefect-aws/prefect_aws/experimental/bundles/execute.py:48:27: error[missing-argument] No argument provided for required parameter `name`
+ src/integrations/prefect-aws/prefect_aws/experimental/bundles/upload.py:64:27: error[missing-argument] No argument provided for required parameter `name`
- src/integrations/prefect-aws/tests/test_s3.py:442:21: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-aws/tests/test_s3.py:449:21: error[no-matching-overload] No overload matches arguments
+ src/integrations/prefect-aws/tests/test_s3.py:1140:18: error[missing-argument] No argument provided for required parameter `name`
- src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:225:27: error[missing-argument] No argument provided for required parameter `cls`
- src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:250:15: error[missing-argument] No argument provided for required parameter `cls`
+ src/integrations/prefect-azure/prefect_azure/experimental/bundles/execute.py:28:19: error[missing-argument] No argument provided for required parameter `name`
+ src/integrations/prefect-azure/prefect_azure/experimental/bundles/execute.py:30:17: error[unknown-argument] Argument `_sync` does not match any known parameter
+ src/integrations/prefect-azure/prefect_azure/experimental/bundles/upload.py:38:19: error[missing-argument] No argument provided for required parameter `name`
+ src/integrations/prefect-azure/prefect_azure/experimental/bundles/upload.py:40:17: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/integrations/prefect-azure/tests/test_aci_worker.py:110:15: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-azure/tests/test_aci_worker.py:111:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[Unknown | str, Unknown | dict[str, Any]]`
- src/integrations/prefect-azure/tests/test_aci_worker.py:1021:20: error[missing-argument] No argument provided for required parameter `cls`
- src/integrations/prefect-dbt/prefect_dbt/cloud/jobs.py:1123:22: error[unresolved-attribute] Object of type `((trigger_job_run_options: TriggerJobRunOptions | None = None) -> Unknown | Coroutine[Any, Any, Unknown]) | ((trigger_job_run_options: Divergent = Divergent) -> Unknown | Coroutine[Any, Any, Unknown])` has no attribute `aio`
+ src/integrations/prefect-dbt/prefect_dbt/cloud/jobs.py:1123:22: error[unresolved-attribute] Object of type `(trigger_job_run_options: TriggerJobRunOptions | None = None) -> Unknown | Coroutine[Any, Any, Unknown]` has no attribute `aio`
+ src/integrations/prefect-dbt/tests/cli/test_credentials.py:75:36: error[missing-argument] No argument provided for required parameter `name`
- src/integrations/prefect-docker/prefect_docker/worker.py:559:31: error[missing-argument] No argument provided for required parameter `cls`
- src/integrations/prefect-docker/tests/test-project/flow.py:31:9: error[no-matching-overload] No overload matches arguments
+ src/integrations/prefect-email/tests/test_credentials.py:90:19: error[missing-argument] No argument provided for required parameter `name`
+ src/integrations/prefect-email/tests/test_credentials.py:107:19: error[missing-argument] No argument provided for required parameter `name`
+ src/integrations/prefect-gcp/prefect_gcp/experimental/bundles/execute.py:28:13: error[missing-argument] No argument provided for required parameter `name`
+ src/integrations/prefect-gcp/prefect_gcp/experimental/bundles/execute.py:30:17: error[unknown-argument] Argument `_sync` does not match any known parameter
+ src/integrations/prefect-gcp/prefect_gcp/experimental/bundles/upload.py:52:13: error[missing-argument] No argument provided for required parameter `name`
+ src/integrations/prefect-gcp/prefect_gcp/experimental/bundles/upload.py:54:17: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/integrations/prefect-gcp/tests/test_cloud_run_worker.py:421:19: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-gcp/tests/test_cloud_run_worker.py:421:75: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-gcp/tests/test_cloud_run_worker.py:442:19: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-gcp/tests/test_cloud_run_worker.py:442:75: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-gcp/tests/test_cloud_run_worker.py:475:19: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-gcp/tests/test_cloud_run_worker.py:475:75: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-gcp/tests/test_vertex_worker.py:66:19: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-gcp/tests/test_vertex_worker.py:67:17: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-gcp/tests/test_vertex_worker.py:94:19: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-gcp/tests/test_vertex_worker.py:95:17: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1261:24: error[missing-argument] No argument provided for required parameter `cls`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1294:19: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1295:17: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1316:19: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1317:17: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1353:19: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1354:17: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1387:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1388:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1435:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1436:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1489:22: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1490:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1613:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1614:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1640:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1641:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1694:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1695:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1762:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1763:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1814:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1815:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1866:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1867:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1909:23: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1910:21: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1949:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1950:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1992:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1993:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2039:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2040:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2080:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2081:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2106:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2107:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2149:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2150:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2172:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2173:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2208:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2209:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2255:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2256:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2314:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2315:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2360:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2361:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2393:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2394:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2468:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2469:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2522:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2523:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2545:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2546:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2589:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2590:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2613:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2614:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2637:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2638:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2655:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2656:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
- src/integrations/prefect-snowflake/tests/experimental/test_spcs_worker.py:142:39: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-snowflake/tests/experimental/test_spcs_worker.py:143:9: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[Unknown | str, Unknown | dict[str, Any]]`
- src/integrations/prefect-snowflake/tests/experimental/test_spcs_worker.py:266:20: error[missing-argument] No argument provided for required parameter `cls`
- src/integrations/prefect-snowflake/tests/experimental/test_spcs_worker.py:927:20: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-snowflake/tests/experimental/test_spcs_worker.py:927:69: error[invalid-argument-type] Argument is incorrect: Expected `Self@from_template_and_values`, found `dict[str, Any]`
+ src/integrations/prefect-sqlalchemy/tests/test_database.py:447:18: error[missing-argument] No argument provided for required parameter `name`
+ src/integrations/prefect-sqlalchemy/tests/test_database.py:466:18: error[missing-argument] No argument provided for required parameter `name`
- src/integrations/prefect-sqlalchemy/tests/test_database.py:488:16: error[no-matching-overload] No overload matches arguments
- src/prefect/blocks/core.py:1261:39: error[missing-argument] No argument provided for required parameter `block_document_id`
- src/prefect/blocks/core.py:1261:69: error[invalid-argument-type] Argument is incorrect: Expected `Self@_get_block_document_by_id`, found `str | UUID`
- src/prefect/blocks/core.py:1264:43: error[missing-argument] No argument provided for required parameter `block_document_id`
- src/prefect/cli/deploy/_actions.py:65:19: error[missing-argument] No argument provided for required parameter `name`
- src/prefect/cli/deploy/_actions.py:65:32: error[invalid-argument-type] Argument is incorrect: Expected `Self@aload`, found `str`
- src/prefect/cli/deployment.py:288:20: error[missing-argument] No argument provided for required parameter `ref`
- src/prefect/cli/deployment.py:288:40: error[invalid-argument-type] Argument is incorrect: Expected `Self@load_from_ref`, found `UUID & ~AlwaysFalsy`
- src/prefect/deployments/steps/pull.py:265:23: error[missing-argument] No argument provided for required parameter `name`
- src/prefect/deployments/steps/pull.py:265:35: error[invalid-argument-type] Argument is incorrect: Expected `Self@aload`, found `str`
- src/prefect/flow_engine.py:319:36: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/prefect/flow_engine.py:623:36: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/prefect/flow_engine.py:695:18: error[missing-argument] No argument provided for required parameter `cls`
- src/prefect/flow_engine.py:906:36: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/prefect/flow_engine.py:1207:36: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/prefect/flows.py:593:24: error[missing-argument] No argument provided for required parameter `ref`
- src/prefect/flows.py:593:58: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/prefect/results.py:144:23: error[missing-argument] No argument provided for required parameter `name`
+ src/prefect/results.py:181:17: error[missing-argument] No argument provided for required parameter `name`
- src/prefect/results.py:144:35: error[invalid-argument-type] Argument is incorrect: Expected `Self@aload`, found `str`
- src/prefect/results.py:224:23: error[missing-argument] No argument provided for required parameter `name`
+ src/prefect/results.py:181:44: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/prefect/results.py:524:38: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/prefect/results.py:536:44: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/prefect/results.py:629:51: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/prefect/results.py:647:57: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/prefect/results.py:812:57: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/prefect/results.py:826:57: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/prefect/server/models/flow_runs.py:582:15: error[missing-argument] No argument provided for required parameter `db`
- src/prefect/server/models/task_runs.py:556:15: error[missing-argument] No argument provided for required parameter `db`
- src/prefect/task_engine.py:777:36: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/prefect/task_engine.py:820:18: error[missing-argument] No argument provided for required parameter `cls`
- src/prefect/task_engine.py:1383:36: error[unknown-argument] Argument `_sync` does not match any known parameter
- src/prefect/testing/utilities.py:263:28: error[missing-argument] No argument provided for required parameter `name`
- src/prefect/testing/utilities.py:263:40: error[invalid-argument-type] Argument is incorrect: Expected `Self@aload`, found `str`
- src/prefect/testing/utilities.py:274:28: error[missing-argument] No argument provided for required parameter `name`
- src/prefect/testing/utilities.py:274:40: error[invalid-argument-type] Argument is incorrect: Expected `Self@aload`, found `str`
- src/prefect/workers/base.py:895:31: error[missing-argument] No argument provided for required parameter `cls`
- src/prefect/workers/base.py:1417:31: error[missing-argument] No argument provided for required parameter `cls`
- Found 5533 diagnostics
+ Found 5425 diagnostics

xarray (https://github.com/pydata/xarray)
+ xarray/core/indexing.py:411:29: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ xarray/core/indexing.py:417:24: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
- xarray/tests/test_backends.py:2580:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@roundtrip`, found `Self@roundtrip`
- xarray/tests/test_backends.py:5267:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@roundtrip`, found `Self@roundtrip`
- xarray/tests/test_namedarray.py:449:19: error[no-matching-overload] No overload of bound method `_new` matches arguments
- Found 1758 diagnostics
+ Found 1757 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- tests/testing/internal/pytest/test_pytest_bdd.py:98:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_bdd.py:149:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_bdd.py:205:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_bdd.py:237:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_benchmark.py:34:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_codeowners.py:46:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_ddtrace_tags.py:31:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_itr.py:47:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_itr.py:103:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_itr.py:167:18: error[missing-argument] No argument provided for required parameter `cls`
- Found 8473 diagnostics
+ Found 8463 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 41 diagnostics
+ Found 42 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/conftest.py:2123:10: error[missing-argument] No argument provided for required parameter `cls`
+ pandas/core/arrays/string_.py:225:23: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ pandas/core/dtypes/dtypes.py:1366:39: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ pandas/core/dtypes/dtypes.py:1401:23: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ pandas/core/dtypes/dtypes.py:1551:23: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
- Found 3763 diagnostics
+ Found 3766 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
+ django-stubs/contrib/gis/gdal/envelope.pyi:19:21: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/envelope.pyi:21:21: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/envelope.pyi:23:24: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:106:25: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:106:47: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:108:24: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:108:46: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:111:42: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:114:24: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:114:30: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:134:25: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:134:31: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:134:37: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:136:24: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:136:30: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:136:36: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:149:25: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:151:24: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/collections.pyi:11:25: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/collections.pyi:13:24: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/coordseq.pyi:10:36: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/coordseq.pyi:32:24: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/linestring.pyi:9:36: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/linestring.pyi:12:25: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/linestring.pyi:12:31: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/linestring.pyi:14:24: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/linestring.pyi:14:30: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/polygon.pyi:12:30: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/polygon.pyi:18:25: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/polygon.pyi:18:31: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/polygon.pyi:18:37: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/polygon.pyi:20:24: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/polygon.pyi:20:30: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/polygon.pyi:20:36: error[invalid-type-form] Invalid subscript of object of type `property` in type expression
- Found 438 diagnostics
+ Found 472 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/expr/types/relations.py:3668:13: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Self@join`, found `Table`
- Found 4525 diagnostics
+ Found 4524 diagnostics

jax (https://github.com/google/jax)
- jax/_src/clusters/k8s_cluster.py:130:10: error[missing-argument] No argument provided for required parameter `cls`
- jax/_src/clusters/k8s_cluster.py:140:10: error[missing-argument] No argument provided for required parameter `cls`
- jax/_src/clusters/k8s_cluster.py:148:10: error[missing-argument] No argument provided for required parameter `cls`
- jax/_src/interpreters/pxla.py:1360:10: error[missing-argument] No argument provided for required parameter `runner`
- jax/_src/interpreters/pxla.py:1360:38: error[invalid-argument-type] Argument is incorrect: Expected `Self@trace`, found `Unknown | PGLEProfiler | None`
- Found 2721 diagnostics
+ Found 2716 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/externals/array_api_compat/_internal.py:49:9: error[invalid-assignment] Object of type `Signature` is not assignable to attribute `__signature__` on type `_Wrapped[(...), Unknown, (*args: object, **kwargs: object), Unknown] | _Wrapped[(...), Unknown, Divergent, Unknown]`
+ sklearn/externals/array_api_compat/_internal.py:49:9: error[unresolved-attribute] Unresolved attribute `__signature__` on type `_Wrapped[(...), Unknown, (*args: object, **kwargs: object), Unknown]`.

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/models/evap/evap_model.py:8166:14: error[missing-argument] No argument provided for required parameter `constants`
- hydpy/models/evap/evap_model.py:8167:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@modify_constants`, found `Constants | None`
- hydpy/models/evap/evap_model.py:8168:12: error[missing-argument] No argument provided for required parameter `constants`
- hydpy/models/evap/evap_model.py:8169:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@modify_rows`, found `Constants | None`
- hydpy/models/evap/evap_model.py:8170:12: error[missing-argument] No argument provided for required parameter `refindices`
- hydpy/models/evap/evap_model.py:8171:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@modify_refindices`, found `NameParameter | None`
- hydpy/models/evap/evap_model.py:8172:12: error[missing-argument] No argument provided for required parameter `refweights`
- hydpy/models/evap/evap_model.py:8173:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@modify_refweights`, found `Parameter | None`
- hydpy/models/evap/evap_model.py:8174:12: error[missing-argument] No argument provided for required parameter `refweights`
- hydpy/models/evap/evap_model.py:8175:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@modify_refweights`, found `Parameter | None`
- hydpy/models/evap/evap_model.py:8176:12: error[missing-argument] No argument provided for required parameter `refweights`
- hydpy/models/evap/evap_model.py:8177:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@modify_refweights`, found `Parameter | None`
- hydpy/models/evap/evap_model.py:8178:12: error[missing-argument] No argument provided for required parameter `refweights`
- hydpy/models/evap/evap_model.py:8179:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@modify_refweights`, found `Parameter | None`
- hydpy/models/meteo/meteo_model.py:2871:14: error[missing-argument] No argument provided for required parameter `refindices`
- hydpy/models/meteo/meteo_model.py:2872:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@modify_refindices`, found `NameParameter | None`
- hydpy/models/meteo/meteo_model.py:2873:12: error[missing-argument] No argument provided for required parameter `refweights`
- hydpy/models/meteo/meteo_model.py:2874:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@modify_refweights`, found `Parameter | None`
- hydpy/models/meteo/meteo_model.py:2875:12: error[missing-argument] No argument provided for required parameter `refweights`
- hydpy/models/meteo/meteo_model.py:2876:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@modify_refweights`, found `Parameter | None`
- hydpy/models/meteo/meteo_model.py:2877:12: error[missing-argument] No argument provided for required parameter `refweights`
- hydpy/models/meteo/meteo_model.py:2878:13: error[invalid-argument-type] Argument is incorrect: Expected `Self@modify_refweights`, found `Parameter | None`
- Found 704 diagnostics
+ Found 682 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/indexes/test_index_float.py:113:13: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `Index[Any]`
- tests/series/test_series_float.py:54:13: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Series[list[Unknown | float]]`
+ tests/series/test_series_float.py:54:13: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Series[Any]`
- tests/series/test_series_float.py:110:13: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Unknown`
- tests/series/test_series_float.py:110:25: error[no-matching-overload] No overload of bound method `astype` matches arguments
- tests/series/test_series_float.py:112:15: error[no-matching-overload] No overload of bound method `astype` matches arguments
- Found 5545 diagnostics
+ Found 5541 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/stats/_axis_nan_policy.py:713:9: error[invalid-assignment] Object of type `Signature` is not assignable to attribute `__signature__` on type `_Wrapped[(...), Unknown, (*args, *, _no_deco=Literal[False], **kwds), Unknown] | _Wrapped[(...), Unknown, Divergent, Unknown]`
+ scipy/stats/_axis_nan_policy.py:713:9: error[unresolved-attribute] Unresolved attribute

... (truncated 21 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct metadata = ~11MB
+     struct metadata = ~10MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     struct fields = ~21MB
+     struct fields = ~22MB
-     memo fields = ~176MB
+     memo fields = ~185MB

prefect (https://github.com/PrefectHQ/prefect)
-     struct metadata = ~52MB
+     struct metadata = ~49MB
-     memo metadata = ~176MB
+     memo metadata = ~167MB


```

</details>




---

_Comment by @dcreager on 2025-12-05 14:56_

The main blocker here is that the params/return type annotations for a function might be inferred in one of three different places. That means that `Signature::from_function` has to [(indirectly)](https://github.com/astral-sh/ruff/blob/e42cdf84957b178dc7e37aed0f2b1f4d5f0c272a/crates/ty_python_semantic/src/types/signatures.rs#L1988) [decide](https://github.com/astral-sh/ruff/blob/e42cdf84957b178dc7e37aed0f2b1f4d5f0c272a/crates/ty_python_semantic/src/types.rs#L189-L200) where to get the annotation type from. Any function that (a) has a parameter or return type annotation, (b) does not have any PEP-695 generics, and (c) does not have deferred annotations will hit the branch that immediately creates a salsa cycle by [calling `infer_definition_types`](https://github.com/astral-sh/ruff/blob/e42cdf84957b178dc7e37aed0f2b1f4d5f0c272a/crates/ty_python_semantic/src/types.rs#L191).

To handle this, I think we want to update `TypeInferenceBuilder` to create a `Parameters` instance as part of `infer_parameters`. For this cyclic case, we call `infer_parameters` in `infer_function_definition`, and can pass that `Parameters` in to the `FunctionLiteral` that we create. Then, when its `signature` method gets called, if we passed in a `Parameters` directly, we just use it. If not (because there is a PEP-695 scope, or annotations were deferred), we won't have passed in a `Parameters` directly, and we still have to fall back on looking up the parameters indirectly, but (I think) neither of those involve an immediate obvious cycle.

---

_Comment by @codspeed-hq[bot] on 2025-12-05 15:00_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fbreak-the-cycle?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21813 will **improve performances by 8.8%**

<sub>Comparing <code>dcreager/break-the-cycle</code> (0fd5172) with <code>main</code> (ff7086d)</sub>



### Summary

`⚡ 5` improvements  
`✅ 17` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | WallTime | [`` small[tanjun] ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fbreak-the-cycle?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Btanjun%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 2.8 s | 2.7 s | +6.28% |
| ⚡ | WallTime | [`` medium[pandas] ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fbreak-the-cycle?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bpandas%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 70.5 s | 64.8 s | +8.8% |
| ⚡ | Simulation | [`` anyio ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fbreak-the-cycle?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Aanyio%3A%3Aproject%3A%3Aanyio&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.4 s | 1.4 s | +4.33% |
| ⚡ | Simulation | [`` ty_check_file[incremental] ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fbreak-the-cycle?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Acheck_file%3A%3Abenchmark_incremental%3A%3Aty_check_file%5Bincremental%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 6.9 ms | 6.7 ms | +4.01% |
| ⚡ | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fbreak-the-cycle?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.3 s | 1.3 s | +5.93% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fbreak-the-cycle?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Label `internal` removed by @MichaReiser on 2025-12-05 15:01_

---

_Label `performance` added by @MichaReiser on 2025-12-05 15:01_

---

_Comment by @dcreager on 2025-12-05 21:53_

This is failing locally on the `cycle` mdtest. That has pathological examples like

```py
def f(x=f): ...
```

and

```py
class C:
    def f(self: "C"):
        def inner_a(positional=self.a):
            return
        self.a = inner_a
```

We're definitely not creating salsa cycles in the normal case anymore, but these are now starting to fail with "too many cycle iterations". The issue is that the default value keeps growing, with each salsa cycle adding one more level of depth to the type.

The way we've solved this for e.g. typevar bounds/constraints/defaults is to evaluate them lazily...which unfortunately is exactly what we're trying to get away from with this PR! :sob: 

I will have to have a think about this.

---

_Comment by @dcreager on 2025-12-05 23:32_

Ah, it looks like I might just need to learn about `recursive_type_normalized` and use that!

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:551 on 2025-12-09 00:10_

I suspect that this is one of the main reasons for the memory increase and slowdown. The idea of extra is that we only create it in rare instances. However, assuming I understand your change correctly, we now create an extra for every function definition of which there are many. 

Could we maybe use a type inference context instead? 

---

_@MichaReiser reviewed on 2025-12-09 00:10_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-12-10 06:40_

---

_Comment by @dcreager on 2025-12-11 20:01_

Went with a different approach instead #21906 

---

_Closed by @dcreager on 2025-12-11 20:01_

---

_Branch deleted on 2025-12-11 20:01_

---
