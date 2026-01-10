```yaml
number: 21958
title: "[ty] propagate classmethod-ness through decorators returning Callables"
type: pull_request
state: merged
author: carljm
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: cjm/classmethod
created_at: 2025-12-13T04:36:02Z
updated_at: 2025-12-16T17:16:42Z
url: https://github.com/astral-sh/ruff/pull/21958
synced_at: 2026-01-10T16:42:11Z
```

# [ty] propagate classmethod-ness through decorators returning Callables

---

_Pull request opened by @carljm on 2025-12-13 04:36_

Fixes https://github.com/astral-sh/ty/issues/1787

## Summary

Allow method decorators returning Callables to presumptively propagate "classmethod-ness" in the same way that they already presumptively propagate "function-like-ness". We can't actually be sure that this is the case, based on the decorator's annotations, but (along with other type checkers) we heuristically assume it to be the case for decorators applied via decorator syntax.

## Test Plan

Added mdtest.


---

_Label `ty` added by @carljm on 2025-12-13 04:36_

---

_Comment by @astral-sh-bot[bot] on 2025-12-13 04:38_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-13 04:39_

---

_Comment by @astral-sh-bot[bot] on 2025-12-13 04:40_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/unittest.py:583:14: error[missing-argument] No argument provided for required parameter `cls`
- testing/test_config.py:1441:14: error[missing-argument] No argument provided for required parameter `cls`
- testing/test_config.py:1763:10: error[missing-argument] No argument provided for required parameter `cls`
- testing/test_faulthandler.py:216:10: error[missing-argument] No argument provided for required parameter `cls`
- testing/test_monkeypatch.py:426:10: error[missing-argument] No argument provided for required parameter `cls`
- Found 440 diagnostics
+ Found 435 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- cki_lib/inttests/cluster.py:78:31: error[missing-argument] No argument provided for required parameter `cls`
- cki_lib/inttests/mariadb.py:22:31: error[missing-argument] No argument provided for required parameter `cls`
- cki_lib/inttests/mariadb.py:29:14: error[missing-argument] No argument provided for required parameter `namespace`
- cki_lib/inttests/mariadb.py:29:32: error[invalid-argument-type] Argument is incorrect: Expected `type[KubernetesCluster]`, found `Literal["mariadb"]`
- cki_lib/inttests/minio.py:22:31: error[missing-argument] No argument provided for required parameter `cls`
- cki_lib/inttests/minio.py:29:14: error[missing-argument] No argument provided for required parameter `namespace`
- cki_lib/inttests/minio.py:29:32: error[invalid-argument-type] Argument is incorrect: Expected `type[KubernetesCluster]`, found `Literal["minio"]`
- cki_lib/inttests/rabbitmq.py:36:31: error[missing-argument] No argument provided for required parameter `cls`
- cki_lib/inttests/rabbitmq.py:43:14: error[missing-argument] No argument provided for required parameter `namespace`
- cki_lib/inttests/rabbitmq.py:43:32: error[invalid-argument-type] Argument is incorrect: Expected `type[KubernetesCluster]`, found `Literal["rabbitmq"]`
- cki_lib/inttests/remote_responses.py:123:31: error[missing-argument] No argument provided for required parameter `cls`
- cki_lib/inttests/remote_responses.py:135:14: error[missing-argument] No argument provided for required parameter `namespace`
- cki_lib/inttests/remote_responses.py:135:32: error[invalid-argument-type] Argument is incorrect: Expected `type[KubernetesCluster]`, found `Literal["remote-responses"]`
- cki_lib/inttests/sqs.py:29:31: error[missing-argument] No argument provided for required parameter `cls`
- cki_lib/inttests/sqs.py:37:14: error[missing-argument] No argument provided for required parameter `namespace`
- cki_lib/inttests/sqs.py:37:32: error[invalid-argument-type] Argument is incorrect: Expected `type[KubernetesCluster]`, found `Literal["sqs"]`
- cki_lib/inttests/vault.py:22:31: error[missing-argument] No argument provided for required parameter `cls`
- cki_lib/inttests/vault.py:29:14: error[missing-argument] No argument provided for required parameter `namespace`
- cki_lib/inttests/vault.py:29:32: error[invalid-argument-type] Argument is incorrect: Expected `type[KubernetesCluster]`, found `Literal["vault"]`
- tests/kcidb/test_kcidb_file.py:83:14: error[missing-argument] No argument provided for required parameter `kcidb_data`
- Found 259 diagnostics
+ Found 239 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- test/with_dummyserver/test_chunked_transfer.py:299:14: error[missing-argument] No argument provided for required parameter `monkeypatch`
- test/with_dummyserver/test_chunked_transfer.py:299:36: error[invalid-argument-type] Argument is incorrect: Expected `type[ConnectionMarker]`, found `MonkeyPatch`
- test/with_dummyserver/test_chunked_transfer.py:327:14: error[missing-argument] No argument provided for required parameter `monkeypatch`
- test/with_dummyserver/test_chunked_transfer.py:327:36: error[invalid-argument-type] Argument is incorrect: Expected `type[ConnectionMarker]`, found `MonkeyPatch`
- Found 272 diagnostics
+ Found 268 diagnostics

setuptools (https://github.com/pypa/setuptools)
- setuptools/build_meta.py:300:18: error[missing-argument] No argument provided for required parameter `cls`
- Found 1263 diagnostics
+ Found 1262 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-aws/prefect_aws/experimental/bundles/execute.py:48:27: error[missing-argument] No argument provided for required parameter `name`
- src/integrations/prefect-aws/prefect_aws/experimental/bundles/execute.py:48:47: error[invalid-argument-type] Argument is incorrect: Expected `type[Self@aload]`, found `str`
- src/integrations/prefect-aws/prefect_aws/experimental/bundles/upload.py:64:27: error[missing-argument] No argument provided for required parameter `name`
- src/integrations/prefect-aws/tests/test_s3.py:1140:18: error[missing-argument] No argument provided for required parameter `name`
- src/integrations/prefect-aws/tests/test_s3.py:1140:32: error[invalid-argument-type] Argument is incorrect: Expected `type[Self@aload]`, found `Literal["round-tripper"]`
- src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:225:27: error[missing-argument] No argument provided for required parameter `cls`
- src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:250:15: error[missing-argument] No argument provided for required parameter `cls`
- src/integrations/prefect-azure/prefect_azure/experimental/bundles/execute.py:28:19: error[missing-argument] No argument provided for required parameter `name`
- src/integrations/prefect-azure/prefect_azure/experimental/bundles/upload.py:38:19: error[missing-argument] No argument provided for required parameter `name`
- src/integrations/prefect-azure/tests/test_aci_worker.py:110:15: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-azure/tests/test_aci_worker.py:111:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[Unknown | str, Unknown | dict[str, Any]]`
- src/integrations/prefect-azure/tests/test_aci_worker.py:1021:20: error[missing-argument] No argument provided for required parameter `cls`
- src/integrations/prefect-dbt/tests/cli/test_commands.py:584:18: error[missing-argument] No argument provided for required parameter `cls`
- src/integrations/prefect-dbt/tests/cli/test_commands.py:605:24: error[missing-argument] No argument provided for required parameter `cls`
- src/integrations/prefect-dbt/tests/cli/test_commands.py:624:24: error[missing-argument] No argument provided for required parameter `cls`
- src/integrations/prefect-dbt/tests/cli/test_commands.py:643:24: error[missing-argument] No argument provided for required parameter `cls`
- src/integrations/prefect-dbt/tests/cli/test_commands.py:662:24: error[missing-argument] No argument provided for required parameter `cls`
- src/integrations/prefect-dbt/tests/cli/test_commands.py:681:24: error[missing-argument] No argument provided for required parameter `cls`
- src/integrations/prefect-dbt/tests/cli/test_commands.py:702:24: error[missing-argument] No argument provided for required parameter `cls`
- src/integrations/prefect-dbt/tests/cli/test_commands.py:732:24: error[missing-argument] No argument provided for required parameter `cls`
- src/integrations/prefect-dbt/tests/cli/test_commands.py:756:24: error[missing-argument] No argument provided for required parameter `cls`
- src/integrations/prefect-dbt/tests/cli/test_credentials.py:75:36: error[missing-argument] No argument provided for required parameter `name`
- src/integrations/prefect-dbt/tests/cli/test_credentials.py:75:55: error[invalid-argument-type] Argument is incorrect: Expected `type[Self@aload]`, found `str`
- src/integrations/prefect-docker/prefect_docker/worker.py:559:31: error[missing-argument] No argument provided for required parameter `cls`
- src/integrations/prefect-email/tests/test_credentials.py:90:19: error[missing-argument] No argument provided for required parameter `name`
- src/integrations/prefect-email/tests/test_credentials.py:90:47: error[invalid-argument-type] Argument is incorrect: Expected `type[Self@aload]`, found `Literal["email-credentials"]`
- src/integrations/prefect-email/tests/test_credentials.py:107:19: error[missing-argument] No argument provided for required parameter `name`
- src/integrations/prefect-email/tests/test_credentials.py:107:47: error[invalid-argument-type] Argument is incorrect: Expected `type[Self@aload]`, found `Literal["email-credentials"]`
- src/integrations/prefect-gcp/prefect_gcp/experimental/bundles/execute.py:28:13: error[missing-argument] No argument provided for required parameter `name`
- src/integrations/prefect-gcp/prefect_gcp/experimental/bundles/execute.py:29:17: error[invalid-argument-type] Argument is incorrect: Expected `type[Self@aload]`, found `str`
- src/integrations/prefect-gcp/prefect_gcp/experimental/bundles/upload.py:52:13: error[missing-argument] No argument provided for required parameter `name`
- src/integrations/prefect-gcp/tests/test_cloud_run_worker.py:421:19: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-gcp/tests/test_cloud_run_worker.py:421:75: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-gcp/tests/test_cloud_run_worker.py:442:19: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-gcp/tests/test_cloud_run_worker.py:442:75: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-gcp/tests/test_cloud_run_worker.py:475:19: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-gcp/tests/test_cloud_run_worker.py:475:75: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-gcp/tests/test_vertex_worker.py:66:19: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-gcp/tests/test_vertex_worker.py:67:17: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-gcp/tests/test_vertex_worker.py:94:19: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-gcp/tests/test_vertex_worker.py:95:17: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1261:24: error[missing-argument] No argument provided for required parameter `cls`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1294:19: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1295:17: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1316:19: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1317:17: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1353:19: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1354:17: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1387:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1388:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1435:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1436:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1489:22: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1490:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1613:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1614:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1640:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1641:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1694:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1695:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1762:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1763:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1814:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1815:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1866:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1867:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1909:23: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1910:21: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1949:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1950:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1992:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:1993:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2039:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2040:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2080:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2081:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2106:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2107:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2149:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2150:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2172:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2173:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2208:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2209:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2255:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2256:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2314:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2315:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2360:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2361:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2393:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2394:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2468:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2469:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2522:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2523:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2545:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2546:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2589:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2590:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2613:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2614:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2637:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2638:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2655:31: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-kubernetes/tests/test_worker.py:2656:13: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-snowflake/tests/experimental/test_spcs_worker.py:142:39: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-snowflake/tests/experimental/test_spcs_worker.py:143:9: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[Unknown | str, Unknown | dict[str, Any]]`
- src/integrations/prefect-snowflake/tests/experimental/test_spcs_worker.py:266:20: error[missing-argument] No argument provided for required parameter `cls`
- src/integrations/prefect-snowflake/tests/experimental/test_spcs_worker.py:927:20: error[missing-argument] No argument provided for required parameter `values`
- src/integrations/prefect-snowflake/tests/experimental/test_spcs_worker.py:927:69: error[invalid-argument-type] Argument is incorrect: Expected `type[BaseJobConfiguration]`, found `dict[str, Any]`
- src/integrations/prefect-sqlalchemy/tests/test_database.py:447:18: error[missing-argument] No argument provided for required parameter `name`
- src/integrations/prefect-sqlalchemy/tests/test_database.py:447:43: error[invalid-argument-type] Argument is incorrect: Expected `type[Self@aload]`, found `str`
- src/integrations/prefect-sqlalchemy/tests/test_database.py:466:18: error[missing-argument] No argument provided for required parameter `name`
- src/integrations/prefect-sqlalchemy/tests/test_database.py:466:43: error[invalid-argument-type] Argument is incorrect: Expected `type[Self@aload]`, found `str`
- src/prefect/artifacts.py:233:51: error[invalid-argument-type] Argument is incorrect: Expected `type[Artifact]`, found `str | None`
- src/prefect/blocks/core.py:1261:39: error[missing-argument] No argument provided for required parameter `block_document_id`
- src/prefect/blocks/core.py:1261:69: error[invalid-argument-type] Argument is incorrect: Expected `type[Block]`, found `str | UUID`
- src/prefect/blocks/core.py:1264:43: error[missing-argument] No argument provided for required parameter `block_document_id`
- src/prefect/cli/deploy/_actions.py:65:19: error[missing-argument] No argument provided for required parameter `name`
- src/prefect/cli/deploy/_actions.py:65:32: error[invalid-argument-type] Argument is incorrect: Expected `type[Self@aload]`, found `str`
- src/prefect/cli/deployment.py:288:20: error[missing-argument] No argument provided for required parameter `ref`
- src/prefect/deployments/steps/pull.py:265:23: error[missing-argument] No argument provided for required parameter `name`
- src/prefect/deployments/steps/pull.py:265:35: error[invalid-argument-type] Argument is incorrect: Expected `type[Self@aload]`, found `str`
- src/prefect/flow_engine.py:695:18: error[missing-argument] No argument provided for required parameter `cls`
- src/prefect/flow_engine.py:1277:24: error[missing-argument] No argument provided for required parameter `cls`
- src/prefect/flows.py:593:24: error[missing-argument] No argument provided for required parameter `ref`
- src/prefect/flows.py:938:17: error[missing-argument] No argument provided for required parameter `cls`
- src/prefect/results.py:144:23: error[missing-argument] No argument provided for required parameter `name`
- src/prefect/results.py:144:35: error[invalid-argument-type] Argument is incorrect: Expected `type[Self@aload]`, found `str`
- src/prefect/results.py:181:17: error[missing-argument] No argument provided for required parameter `name`
- src/prefect/results.py:181:28: error[invalid-argument-type] Argument is incorrect: Expected `type[Self@aload]`, found `str`
- src/prefect/results.py:224:23: error[missing-argument] No argument provided for required parameter `name`
- src/prefect/server/api/server.py:701:49: error[missing-argument] No argument provided for required parameter `cls`
- src/prefect/server/services/base.py:106:20: error[missing-argument] No argument provided for required parameter `cls`
- src/prefect/task_engine.py:820:18: error[missing-argument] No argument provided for required parameter `cls`
- src/prefect/task_engine.py:1426:24: error[missing-argument] No argument provided for required parameter `cls`
- src/prefect/testing/utilities.py:263:28: error[missing-argument] No argument provided for required parameter `name`
- src/prefect/testing/utilities.py:263:40: error[invalid-argument-type] Argument is incorrect: Expected `type[Self@aload]`, found `str`
- src/prefect/testing/utilities.py:274:28: error[missing-argument] No argument provided for required parameter `name`
- src/prefect/testing/utilities.py:274:40: error[invalid-argument-type] Argument is incorrect: Expected `type[Self@aload]`, found `str`
- src/prefect/workers/base.py:895:31: error[missing-argument] No argument provided for required parameter `cls`
- src/prefect/workers/base.py:1417:31: error[missing-argument] No argument provided for required parameter `cls`
- Found 5526 diagnostics
+ Found 5383 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- tests/testing/internal/pytest/test_pytest_bdd.py:99:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_bdd.py:150:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_bdd.py:206:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_bdd.py:238:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_benchmark.py:34:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_codeowners.py:46:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_cov.py:27:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_cov.py:48:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_custom_tags.py:32:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_custom_tags.py:69:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_custom_tags.py:111:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_ddtrace_tags.py:31:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_ddtrace_tags.py:58:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_efd.py:46:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_efd.py:105:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_efd.py:150:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_efd.py:186:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_efd.py:230:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_itr.py:47:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_itr.py:103:18: error[missing-argument] No argument provided for required parameter `cls`
- tests/testing/internal/pytest/test_pytest_itr.py:167:18: error[missing-argument] No argument provided for required parameter `cls`
- Found 8330 diagnostics
+ Found 8309 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/models/evap/evap_model.py:8166:14: error[missing-argument] No argument provided for required parameter `constants`
- hydpy/models/evap/evap_model.py:8167:13: error[invalid-argument-type] Argument is incorrect: Expected `type[NameParameter]`, found `Constants | None`
- hydpy/models/evap/evap_model.py:8168:12: error[missing-argument] No argument provided for required parameter `constants`
- hydpy/models/evap/evap_model.py:8169:13: error[invalid-argument-type] Argument is incorrect: Expected `type[KeywordParameter2D]`, found `Constants | None`
- hydpy/models/evap/evap_model.py:8170:12: error[missing-argument] No argument provided for required parameter `refindices`
- hydpy/models/evap/evap_model.py:8171:13: error[invalid-argument-type] Argument is incorrect: Expected `type[ZipParameter]`, found `NameParameter | None`
- hydpy/models/evap/evap_model.py:8172:12: error[missing-argument] No argument provided for required parameter `refweights`
- hydpy/models/evap/evap_model.py:8173:13: error[invalid-argument-type] Argument is incorrect: Expected `type[Variable]`, found `Parameter | None`
- hydpy/models/evap/evap_model.py:8174:12: error[missing-argument] No argument provided for required parameter `refweights`
- hydpy/models/evap/evap_model.py:8175:13: error[invalid-argument-type] Argument is incorrect: Expected `type[Variable]`, found `Parameter | None`
- hydpy/models/evap/evap_model.py:8176:12: error[missing-argument] No argument provided for required parameter `refweights`
- hydpy/models/evap/evap_model.py:8177:13: error[invalid-argument-type] Argument is incorrect: Expected `type[Variable]`, found `Parameter | None`
- hydpy/models/evap/evap_model.py:8178:12: error[missing-argument] No argument provided for required parameter `refweights`
- hydpy/models/evap/evap_model.py:8179:13: error[invalid-argument-type] Argument is incorrect: Expected `type[Variable]`, found `Parameter | None`
- hydpy/models/meteo/meteo_model.py:2871:14: error[missing-argument] No argument provided for required parameter `refindices`
- hydpy/models/meteo/meteo_model.py:2872:13: error[invalid-argument-type] Argument is incorrect: Expected `type[ZipParameter]`, found `NameParameter | None`
- hydpy/models/meteo/meteo_model.py:2873:12: error[missing-argument] No argument provided for required parameter `refweights`
- hydpy/models/meteo/meteo_model.py:2874:13: error[invalid-argument-type] Argument is incorrect: Expected `type[Variable]`, found `Parameter | None`
- hydpy/models/meteo/meteo_model.py:2875:12: error[missing-argument] No argument provided for required parameter `refweights`
- hydpy/models/meteo/meteo_model.py:2876:13: error[invalid-argument-type] Argument is incorrect: Expected `type[Variable]`, found `Parameter | None`
- hydpy/models/meteo/meteo_model.py:2877:12: error[missing-argument] No argument provided for required parameter `refweights`
- hydpy/models/meteo/meteo_model.py:2878:13: error[invalid-argument-type] Argument is incorrect: Expected `type[Variable]`, found `Parameter | None`
- Found 693 diagnostics
+ Found 671 diagnostics

jax (https://github.com/google/jax)
- jax/_src/clusters/k8s_cluster.py:130:10: error[missing-argument] No argument provided for required parameter `cls`
- jax/_src/clusters/k8s_cluster.py:140:10: error[missing-argument] No argument provided for required parameter `cls`
- jax/_src/clusters/k8s_cluster.py:148:10: error[missing-argument] No argument provided for required parameter `cls`
- jax/_src/interpreters/pxla.py:1361:10: error[missing-argument] No argument provided for required parameter `runner`
- jax/_src/interpreters/pxla.py:1361:38: error[invalid-argument-type] Argument is incorrect: Expected `type[PGLEProfiler]`, found `Unknown | PGLEProfiler | None`
- Found 2730 diagnostics
+ Found 2725 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/conftest.py:2123:10: error[missing-argument] No argument provided for required parameter `cls`
- Found 3714 diagnostics
+ Found 3713 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` removed by @carljm on 2025-12-13 04:46_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-13 04:46_

---

_Label `ecosystem-analyzer` removed by @carljm on 2025-12-13 04:50_

---

_Marked ready for review by @carljm on 2025-12-13 04:50_

---

_Review requested from @AlexWaygood by @carljm on 2025-12-13 04:50_

---

_Review requested from @sharkdp by @carljm on 2025-12-13 04:50_

---

_Review requested from @dcreager by @carljm on 2025-12-13 04:50_

---

_Comment by @astral-sh-bot[bot] on 2025-12-13 04:56_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `missing-argument` | 0 | 146 | 0 |
| `invalid-argument-type` | 0 | 76 | 0 |
| **Total** | **0** | **222** | **0** |

**[Full report with detailed diff](https://cjm-classmethod.ecosystem-663.pages.dev/diff)** ([timing results](https://cjm-classmethod.ecosystem-663.pages.dev/timing))




---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-13 05:06_

---

_Comment by @codspeed-hq[bot] on 2025-12-13 05:23_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fclassmethod?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21958 will **not alter performance**

<sub>Comparing <code>cjm/classmethod</code> (88c12ca) with <code>main</code> (7f7485d)</sub>



### Summary

`✅ 52` untouched  





---

_Comment by @astral-sh-bot[bot] on 2025-12-13 05:24_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@dhruvmanila approved on 2025-12-15 08:58_

This sounds like a reasonable approach.

I was wondering about what Alex mentioned last Friday which is whether we could start modeling these decorators directly using the typeshed's definition. I think it might be possible for `staticmethod` but `classmethod` requires `Concatenate` support. But even after that I'm not sure how could we signal ty to pass the implicit `cls` argument to the callable.

---

_Comment by @AlexWaygood on 2025-12-15 09:00_

Yeah, I think Carl's analysis demonstrates that we will need some version of this even if we go with the approach I outlined on Friday (which may still be worth doing on its own merits after `Concatenate` support has landed!)

---

_Review requested from @MichaReiser by @carljm on 2025-12-16 16:17_

---

_Label `ecosystem-analyzer` removed by @carljm on 2025-12-16 16:23_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-16 16:23_

---

_Merged by @carljm on 2025-12-16 17:16_

---

_Closed by @carljm on 2025-12-16 17:16_

---

_Branch deleted on 2025-12-16 17:16_

---
