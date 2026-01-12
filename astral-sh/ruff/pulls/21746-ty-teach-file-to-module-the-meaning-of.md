```yaml
number: 21746
title: "[ty] teach `file_to_module` the meaning of desperation as well"
type: pull_request
state: closed
author: Gankra
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: gankra/workup
head: gankra/workup2
created_at: 2025-12-02T02:08:57Z
updated_at: 2025-12-02T14:06:18Z
url: https://github.com/astral-sh/ruff/pull/21746
synced_at: 2026-01-12T15:57:32Z
```

# [ty] teach `file_to_module` the meaning of desperation as well

---

_@Gankra_

## Summary

This is a followup to #21745 that also makes relative imports work if the path is well-formed relative to the pyproject.toml (which is the case in pyx). With these two changes combined all workspace-related import issues vanish in pyx.

## Test Plan

The tests don't currently reflect the practical effect of this result because I didn't specifically check in a workspace test where the invalid-module-name can be dodges by clamping to a pyproject.toml (I assumed I would just implement fully allowing invalid names, which is more broadly useful).


---

_Label `ty` added by @Gankra on 2025-12-02 02:08_

---

_Label `ecosystem-analyzer` added by @Gankra on 2025-12-02 02:08_

---

_Comment by @astral-sh-bot[bot] on 2025-12-02 02:11_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-02 02:12_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 496 diagnostics
+ Found 498 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-aws/infra/tests/test_worker_stack.py:4:8: error[unresolved-import] Cannot resolve imported module `..worker.events_stack`
- src/integrations/prefect-aws/infra/tests/test_worker_stack.py:5:8: error[unresolved-import] Cannot resolve imported module `..worker.service_stack`
- src/integrations/prefect-aws/prefect_aws/__init__.py:1:1: error[unresolved-import] Cannot resolve imported module `.`
- src/integrations/prefect-aws/prefect_aws/__init__.py:2:7: error[unresolved-import] Cannot resolve imported module `.credentials`
- src/integrations/prefect-aws/prefect_aws/__init__.py:3:7: error[unresolved-import] Cannot resolve imported module `.client_parameters`
- src/integrations/prefect-aws/prefect_aws/__init__.py:4:7: error[unresolved-import] Cannot resolve imported module `.lambda_function`
- src/integrations/prefect-aws/prefect_aws/__init__.py:5:7: error[unresolved-import] Cannot resolve imported module `.s3`
- src/integrations/prefect-aws/prefect_aws/__init__.py:6:7: error[unresolved-import] Cannot resolve imported module `.secrets_manager`
- src/integrations/prefect-aws/prefect_aws/__init__.py:7:7: error[unresolved-import] Cannot resolve imported module `.workers`
+ src/integrations/prefect-aws/prefect_aws/__init__.py:1:15: error[unresolved-import] Module `prefect_aws` has no member `_version`
- src/integrations/prefect-aws/prefect_aws/_cli/ecs_worker.py:12:7: error[unresolved-import] Cannot resolve imported module `.utils`
- src/integrations/prefect-aws/prefect_aws/_cli/main.py:5:7: error[unresolved-import] Cannot resolve imported module `.ecs_worker`
- src/integrations/prefect-aws/prefect_aws/experimental/__init__.py:1:7: error[unresolved-import] Cannot resolve imported module `.decorators`
- src/integrations/prefect-aws/prefect_aws/workers/__init__.py:1:7: error[unresolved-import] Cannot resolve imported module `.ecs_worker`
+ src/integrations/prefect-aws/tests/conftest.py:27:9: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr | None`, found `Literal["secret_access_key"]`
+ src/integrations/prefect-aws/tests/deployments/test_steps.py:215:13: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr | None`, found `Literal["BlockSecret"]`
+ src/integrations/prefect-aws/tests/deployments/test_steps.py:219:13: error[invalid-argument-type] Argument is incorrect: Expected `AwsClientParameters`, found `dict[Unknown | str, Unknown | str | bool | dict[Unknown | str, Unknown | int]]`
+ src/integrations/prefect-aws/tests/test_s3.py:482:56: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr | None`, found `Literal["testing"]`
+ src/integrations/prefect-aws/tests/test_s3.py:488:39: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["minioadmin"]`
+ src/integrations/prefect-aws/tests/test_s3.py:764:54: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["password"]`
- src/integrations/prefect-azure/prefect_azure/__init__.py:1:7: error[unresolved-import] Cannot resolve imported module `.credentials`
- src/integrations/prefect-azure/prefect_azure/__init__.py:8:7: error[unresolved-import] Cannot resolve imported module `.workers.container_instance`
- src/integrations/prefect-azure/prefect_azure/__init__.py:9:7: error[unresolved-import] Cannot resolve imported module `.blob_storage`
- src/integrations/prefect-azure/prefect_azure/__init__.py:10:7: error[unresolved-import] Cannot resolve imported module `.repository`
- src/integrations/prefect-azure/prefect_azure/experimental/__init__.py:1:7: error[unresolved-import] Cannot resolve imported module `.decorators`
- src/integrations/prefect-azure/prefect_azure/experimental/bundles/__init__.py:1:7: error[unresolved-import] Cannot resolve imported module `.upload`
- src/integrations/prefect-azure/prefect_azure/experimental/bundles/__init__.py:2:7: error[unresolved-import] Cannot resolve imported module `.execute`
+ src/integrations/prefect-azure/tests/test_aci_worker.py:50:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str, str]`, found `tuple[str | None, str, str | None]`
+ src/integrations/prefect-azure/tests/test_aci_worker.py:52:9: warning[possibly-missing-attribute] Attribute `get_secret_value` may be missing on object of type `SecretStr | None`
+ src/integrations/prefect-azure/tests/test_aci_worker.py:172:30: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr | None`, found `Literal["test_client_secret"]`
- src/integrations/prefect-azure/tests/test_aci_worker.py:166:9: error[unresolved-attribute] Module `prefect_azure` has no member `credentials`
- src/integrations/prefect-azure/tests/test_aci_worker.py:229:9: error[unresolved-attribute] Module `prefect_azure` has no member `credentials`
- src/integrations/prefect-azure/tests/test_aci_worker.py:246:9: error[unresolved-attribute] Module `prefect_azure` has no member `workers`
+ src/integrations/prefect-azure/tests/test_aci_worker.py:246:9: error[unresolved-attribute] Module `prefect_azure.workers` has no member `container_instance`
- src/integrations/prefect-azure/tests/test_aci_worker.py:320:9: error[unresolved-attribute] Module `prefect_azure` has no member `credentials`
- src/integrations/prefect-azure/tests/test_aci_worker.py:330:9: error[unresolved-attribute] Module `prefect_azure` has no member `credentials`
- src/integrations/prefect-azure/tests/test_aci_worker.py:337:9: error[unresolved-attribute] Module `prefect_azure` has no member `credentials`
- src/integrations/prefect-azure/tests/test_aci_worker.py:383:9: error[unresolved-attribute] Module `prefect_azure` has no member `credentials`
- src/integrations/prefect-azure/tests/test_aci_worker.py:756:9: error[unresolved-attribute] Module `prefect_azure` has no member `workers`
+ src/integrations/prefect-azure/tests/test_aci_worker.py:756:9: error[unresolved-attribute] Module `prefect_azure.workers` has no member `container_instance`
+ src/integrations/prefect-azure/tests/test_aci_worker.py:997:13: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr | None`, found `Literal["my-client-secret"]`
- src/integrations/prefect-azure/tests/test_aci_worker.py:1000:13: error[unresolved-attribute] Module `prefect_azure` has no member `workers`
+ src/integrations/prefect-azure/tests/test_aci_worker.py:1000:13: error[unresolved-attribute] Module `prefect_azure.workers` has no member `container_instance`
- src/integrations/prefect-bitbucket/prefect_bitbucket/__init__.py:3:1: error[unresolved-import] Cannot resolve imported module `.`
+ src/integrations/prefect-bitbucket/prefect_bitbucket/__init__.py:3:15: error[unresolved-import] Module `prefect_bitbucket` has no member `_version`
- src/integrations/prefect-bitbucket/prefect_bitbucket/__init__.py:4:7: error[unresolved-import] Cannot resolve imported module `.credentials`
- src/integrations/prefect-bitbucket/prefect_bitbucket/__init__.py:5:7: error[unresolved-import] Cannot resolve imported module `.repository`
- src/integrations/prefect-bitbucket/tests/test_repository.py:88:29: error[unresolved-attribute] Module `prefect_bitbucket` has no member `repository`
- src/integrations/prefect-bitbucket/tests/test_repository.py:103:29: error[unresolved-attribute] Module `prefect_bitbucket` has no member `repository`
- src/integrations/prefect-bitbucket/tests/test_repository.py:120:29: error[unresolved-attribute] Module `prefect_bitbucket` has no member `repository`
- src/integrations/prefect-bitbucket/tests/test_repository.py:135:29: error[unresolved-attribute] Module `prefect_bitbucket` has no member `repository`
- src/integrations/prefect-bitbucket/tests/test_repository.py:162:29: error[unresolved-attribute] Module `prefect_bitbucket` has no member `repository`
- src/integrations/prefect-bitbucket/tests/test_repository.py:192:29: error[unresolved-attribute] Module `prefect_bitbucket` has no member `repository`
- src/integrations/prefect-bitbucket/tests/test_repository.py:214:29: error[unresolved-attribute] Module `prefect_bitbucket` has no member `repository`
- src/integrations/prefect-bitbucket/tests/test_repository.py:227:21: error[unresolved-attribute] Module `prefect_bitbucket` has no member `repository`
- src/integrations/prefect-bitbucket/tests/test_repository.py:253:29: error[unresolved-attribute] Module `prefect_bitbucket` has no member `repository`
- src/integrations/prefect-bitbucket/tests/test_repository.py:266:21: error[unresolved-attribute] Module `prefect_bitbucket` has no member `repository`
- src/integrations/prefect-dask/prefect_dask/__init__.py:1:1: error[unresolved-import] Cannot resolve imported module `.`
- src/integrations/prefect-dask/prefect_dask/__init__.py:2:7: error[unresolved-import] Cannot resolve imported module `.task_runners`
+ src/integrations/prefect-dask/prefect_dask/__init__.py:1:15: error[unresolved-import] Module `prefect_dask` has no member `_version`
- src/integrations/prefect-dask/prefect_dask/__init__.py:3:7: error[unresolved-import] Cannot resolve imported module `.client` - did you mean `client`?
- src/integrations/prefect-dask/prefect_dask/__init__.py:4:7: error[unresolved-import] Cannot resolve imported module `.utils`
+ src/integrations/prefect-dask/tests/test_task_runners.py:100:10: error[no-matching-overload] No overload matches arguments
+ src/integrations/prefect-dask/tests/test_task_runners.py:126:10: error[no-matching-overload] No overload matches arguments
+ src/integrations/prefect-dask/tests/test_task_runners.py:217:10: error[no-matching-overload] No overload matches arguments
+ src/integrations/prefect-dask/tests/test_task_runners.py:315:10: error[no-matching-overload] No overload matches arguments
+ src/integrations/prefect-dask/tests/test_task_runners.py:361:10: error[no-matching-overload] No overload matches arguments
+ src/integrations/prefect-dask/tests/test_task_runners.py:382:10: error[no-matching-overload] No overload matches arguments
+ src/integrations/prefect-dask/tests/test_task_runners.py:416:14: error[no-matching-overload] No overload matches arguments
+ src/integrations/prefect-dask/tests/test_task_runners.py:511:10: error[no-matching-overload] No overload matches arguments
+ src/integrations/prefect-dask/tests/test_utils.py:25:10: error[no-matching-overload] No overload matches arguments
+ src/integrations/prefect-dask/tests/test_utils.py:33:10: error[no-matching-overload] No overload matches arguments
+ src/integrations/prefect-dask/tests/test_utils.py:71:10: error[no-matching-overload] No overload matches arguments
+ src/integrations/prefect-dask/tests/test_utils.py:81:18: error[invalid-context-manager] Object of type `_AsyncGeneratorContextManager[Unknown, None]` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
+ src/integrations/prefect-dask/tests/test_utils.py:84:10: error[no-matching-overload] No overload matches arguments
+ src/integrations/prefect-dask/tests/test_utils.py:98:10: error[no-matching-overload] No overload matches arguments
+ src/integrations/prefect-dask/tests/test_utils.py:109:10: error[no-matching-overload] No overload matches arguments
+ src/integrations/prefect-dask/tests/test_utils.py:111:18: error[invalid-context-manager] Object of type `_AsyncGeneratorContextManager[Unknown, None]` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
- src/integrations/prefect-databricks/prefect_databricks/__init__.py:1:7: error[unresolved-import] Cannot resolve imported module `.credentials`
- src/integrations/prefect-databricks/prefect_databricks/__init__.py:2:1: error[unresolved-import] Cannot resolve imported module `.`
+ src/integrations/prefect-databricks/prefect_databricks/__init__.py:2:15: error[unresolved-import] Module `prefect_databricks` has no member `_version`
+ src/integrations/prefect-databricks/tests/test_credentials.py:7:52: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["token_value"]`
+ src/integrations/prefect-databricks/tests/test_rest.py:29:9: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["token_value"]`
- src/integrations/prefect-dbt/prefect_dbt/__init__.py:1:1: error[unresolved-import] Cannot resolve imported module `.`
+ src/integrations/prefect-dbt/prefect_dbt/__init__.py:1:15: error[unresolved-import] Module `prefect_dbt` has no member `_version`
- src/integrations/prefect-dbt/prefect_dbt/__init__.py:3:7: error[unresolved-import] Cannot resolve imported module `.core`
- src/integrations/prefect-dbt/prefect_dbt/__init__.py:4:7: error[unresolved-import] Cannot resolve imported module `.cloud`
- src/integrations/prefect-dbt/prefect_dbt/cli/__init__.py:10:7: error[unresolved-import] Cannot resolve imported module `.credentials`
- src/integrations/prefect-dbt/prefect_dbt/cli/__init__.py:11:7: error[unresolved-import] Cannot resolve imported module `.commands`
- src/integrations/prefect-dbt/prefect_dbt/cli/__init__.py:13:7: error[unresolved-import] Cannot resolve imported module `.configs.base`
- src/integrations/prefect-dbt/prefect_dbt/cli/__init__.py:20:11: error[unresolved-import] Cannot resolve imported module `.configs.snowflake`
- src/integrations/prefect-dbt/prefect_dbt/cli/__init__.py:25:11: error[unresolved-import] Cannot resolve imported module `.configs.bigquery`
- src/integrations/prefect-dbt/prefect_dbt/cli/__init__.py:30:11: error[unresolved-import] Cannot resolve imported module `.configs.postgres`
- src/integrations/prefect-dbt/prefect_dbt/cli/configs/__init__.py:1:7: error[unresolved-import] Cannot resolve imported module `.base`
- src/integrations/prefect-dbt/prefect_dbt/cli/configs/__init__.py:4:11: error[unresolved-import] Cannot resolve imported module `.snowflake`
- src/integrations/prefect-dbt/prefect_dbt/cli/configs/__init__.py:9:11: error[unresolved-import] Cannot resolve imported module `.bigquery`
- src/integrations/prefect-dbt/prefect_dbt/cli/configs/__init__.py:14:11: error[unresolved-import] Cannot resolve imported module `.postgres`
- src/integrations/prefect-dbt/prefect_dbt/cloud/__init__.py:1:7: error[unresolved-import] Cannot resolve imported module `.credentials`
- src/integrations/prefect-dbt/prefect_dbt/cloud/__init__.py:2:7: error[unresolved-import] Cannot resolve imported module `.jobs`
- src/integrations/prefect-dbt/prefect_dbt/core/__init__.py:1:7: error[unresolved-import] Cannot resolve imported module `.runner`
- src/integrations/prefect-dbt/prefect_dbt/core/__init__.py:2:7: error[unresolved-import] Cannot resolve imported module `.settings`
+ src/integrations/prefect-dbt/tests/cli/configs/test_snowflake.py:100:9: error[unknown-argument] Argument `schema` does not match any known parameter
+ src/integrations/prefect-dbt/tests/cli/test_commands.py:482:13: error[invalid-argument-type] Argument is incorrect: Expected `SnowflakeTargetConfigs | BigQueryTargetConfigs | PostgresTargetConfigs | TargetConfigs`, found `dict[Unknown | str, Unknown | str | int]`
+ src/integrations/prefect-dbt/tests/cli/test_credentials.py:18:9: error[invalid-argument-type] Argument is incorrect: Expected `SnowflakeTargetConfigs | BigQueryTargetConfigs | PostgresTargetConfigs | TargetConfigs`, found `dict[str, str] | TargetConfigs`
+ src/integrations/prefect-dbt/tests/cli/test_credentials.py:19:9: error[invalid-argument-type] Argument is incorrect: Expected `GlobalConfigs | None`, found `dict[str, bool] | GlobalConfigs`
+ src/integrations/prefect-dbt/tests/cli/test_credentials.py:30:45: error[invalid-argument-type] Argument is incorrect: Expected `SnowflakeTargetConfigs | BigQueryTargetConfigs | PostgresTargetConfigs | TargetConfigs`, found `dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str]]`
+ src/integrations/prefect-dbt/tests/cli/test_credentials.py:40:9: error[invalid-argument-type] Argument is incorrect: Expected `SnowflakeTargetConfigs | BigQueryTargetConfigs | PostgresTargetConfigs | TargetConfigs`, found `dict[str, str]`
+ src/integrations/prefect-dbt/tests/cli/test_credentials.py:41:9: error[invalid-argument-type] Argument is incorrect: Expected `GlobalConfigs | None`, found `dict[str, bool]`
+ src/integrations/prefect-dbt/tests/cloud/test_cloud_credentials.py:11:32: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["my_api_key"]`
+ src/integrations/prefect-dbt/tests/cloud/test_cloud_credentials.py:34:42: error[invalid-argument-type] Argument to bound method `get_client` is incorrect: Expected `Literal["administrative", "metadata"]`, found `Literal["blorp"]`
- src/integrations/prefect-docker/prefect_docker/__init__.py:1:1: error[unresolved-import] Cannot resolve imported module `.`
+ src/integrations/prefect-docker/prefect_docker/__init__.py:1:15: error[unresolved-import] Module `prefect_docker` has no member `_version`
- src/integrations/prefect-docker/prefect_docker/__init__.py:2:7: error[unresolved-import] Cannot resolve imported module `.host`
- src/integrations/prefect-docker/prefect_docker/__init__.py:3:7: error[unresolved-import] Cannot resolve imported module `.credentials`
- src/integrations/prefect-docker/prefect_docker/__init__.py:4:7: error[unresolved-import] Cannot resolve imported module `.worker`
- src/integrations/prefect-docker/prefect_docker/experimental/__init__.py:1:7: error[unresolved-import] Cannot resolve imported module `.decorators`
- src/integrations/prefect-email/prefect_email/__init__.py:1:1: error[unresolved-import] Cannot resolve imported module `.`
+ src/integrations/prefect-email/prefect_email/__init__.py:1:15: error[unresolved-import] Module `prefect_email` has no member `_version`
- src/integrations/prefect-email/prefect_email/__init__.py:2:7: error[unresolved-import] Cannot resolve imported module `.credentials`
- src/integrations/prefect-email/prefect_email/__init__.py:3:7: error[unresolved-import] Cannot resolve imported module `.message`
+ src/integrations/prefect-email/prefect_email/message.py:89:5: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["From"]` and value of type `str | None` on object of type `MIMEMultipart`
- src/integrations/prefect-gcp/prefect_gcp/__init__.py:1:1: error[unresolved-import] Cannot resolve imported module `.`
+ src/integrations/prefect-gcp/prefect_gcp/__init__.py:1:15: error[unresolved-import] Module `prefect_gcp` has no member `_version`
- src/integrations/prefect-gcp/prefect_gcp/__init__.py:2:7: error[unresolved-import] Cannot resolve imported module `.bigquery`
- src/integrations/prefect-gcp/prefect_gcp/__init__.py:3:7: error[unresolved-import] Cannot resolve imported module `.cloud_storage`
- src/integrations/prefect-gcp/prefect_gcp/__init__.py:4:7: error[unresolved-import] Cannot resolve imported module `.credentials`
- src/integrations/prefect-gcp/prefect_gcp/__init__.py:5:7: error[unresolved-import] Cannot resolve imported module `.secret_manager`
- src/integrations/prefect-gcp/prefect_gcp/__init__.py:6:7: error[unresolved-import] Cannot resolve imported module `.workers.vertex`
- src/integrations/prefect-gcp/prefect_gcp/__init__.py:7:7: error[unresolved-import] Cannot resolve imported module `.workers.cloud_run`
- src/integrations/prefect-gcp/prefect_gcp/__init__.py:8:7: error[unresolved-import] Cannot resolve imported module `.workers.cloud_run_v2`
- src/integrations/prefect-gcp/prefect_gcp/experimental/__init__.py:1:7: error[unresolved-import] Cannot resolve imported module `.decorators`
- src/integrations/prefect-gcp/prefect_gcp/experimental/bundles/__init__.py:1:7: error[unresolved-import] Cannot resolve imported module `.upload`
- src/integrations/prefect-gcp/prefect_gcp/experimental/bundles/__init__.py:2:7: error[unresolved-import] Cannot resolve imported module `.execute`
+ src/integrations/prefect-gcp/tests/test_credentials.py:64:13: error[invalid-argument-type] Argument is incorrect: Expected `Path | None`, found `Literal["~/does_not/exist"]`
+ src/integrations/prefect-gcp/tests/test_credentials.py:70:12: warning[possibly-missing-attribute] Attribute `get_secret_value` may be missing on object of type `SecretDict | None`
+ src/integrations/prefect-gcp/tests/test_credentials.py:80:24: error[invalid-argument-type] Argument is incorrect: Expected `SecretDict | None`, found `Literal["not json"]`
- src/integrations/prefect-github/prefect_github/__init__.py:2:1: error[unresolved-import] Cannot resolve imported module `.`
+ src/integrations/prefect-github/prefect_github/__init__.py:2:15: error[unresolved-import] Module `prefect_github` has no member `_version`
- src/integrations/prefect-github/prefect_github/__init__.py:3:7: error[unresolved-import] Cannot resolve imported module `.credentials`
- src/integrations/prefect-github/prefect_github/__init__.py:4:7: error[unresolved-import] Cannot resolve imported module `.repository`
+ src/integrations/prefect-github/prefect_github/repository.py:69:27: warning[possibly-missing-attribute] Attribute `get_secret_value` may be missing on object of type `SecretStr | None`
+ src/integrations/prefect-github/tests/test_credentials.py:16:37: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr | None`, found `Literal["my-github-token"]`
- src/integrations/prefect-github/tests/test_repository.py:32:29: error[unresolved-attribute] Module `prefect_github` has no member `repository`
- src/integrations/prefect-github/tests/test_repository.py:48:29: error[unresolved-attribute] Module `prefect_github` has no member `repository`
- src/integrations/prefect-github/tests/test_repository.py:62:29: error[unresolved-attribute] Module `prefect_github` has no member `repository`
+ src/integrations/prefect-github/tests/test_repository.py:63:40: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr | None`, found `Literal["XYZ"]`
- src/integrations/prefect-github/tests/test_repository.py:126:29: error[unresolved-attribute] Module `prefect_github` has no member `repository`
- src/integrations/prefect-github/tests/test_repository.py:139:21: error[unresolved-attribute] Module `prefect_github` has no member `repository`
- src/integrations/prefect-github/tests/test_repository.py:166:29: error[unresolved-attribute] Module `prefect_github` has no member `repository`
- src/integrations/prefect-github/tests/test_repository.py:179:21: error[unresolved-attribute] Module `prefect_github` has no member `repository`
- src/integrations/prefect-gitlab/prefect_gitlab/__init__.py:1:1: error[unresolved-import] Cannot resolve imported module `.`
+ src/integrations/prefect-gitlab/prefect_gitlab/__init__.py:1:15: error[unresolved-import] Module `prefect_gitlab` has no member `_version`
- src/integrations/prefect-gitlab/prefect_gitlab/__init__.py:2:7: error[unresolved-import] Cannot resolve imported module `.credentials`
- src/integrations/prefect-gitlab/prefect_gitlab/__init__.py:3:7: error[unresolved-import] Cannot resolve imported module `.repositories`
- src/integrations/prefect-gitlab/tests/test_repositories.py:71:29: error[unresolved-attribute] Module `prefect_gitlab` has no member `repositories`
- src/integrations/prefect-gitlab/tests/test_repositories.py:84:29: error[unresolved-attribute] Module `prefect_gitlab` has no member `repositories`
- src/integrations/prefect-gitlab/tests/test_repositories.py:99:29: error[unresolved-attribute] Module `prefect_gitlab` has no member `repositories`
- src/integrations/prefect-gitlab/tests/test_repositories.py:124:29: error[unresolved-attribute] Module `prefect_gitlab` has no member `repositories`
- src/integrations/prefect-gitlab/tests/test_repositories.py:149:29: error[unresolved-attribute] Module `prefect_gitlab` has no member `repositories`
- src/integrations/prefect-gitlab/tests/test_repositories.py:172:29: error[unresolved-attribute] Module `prefect_gitlab` has no member `repositories`
- src/integrations/prefect-gitlab/tests/test_repositories.py:185:21: error[unresolved-attribute] Module `prefect_gitlab` has no member `repositories`
- src/integrations/prefect-gitlab/tests/test_repositories.py:211:29: error[unresolved-attribute] Module `prefect_gitlab` has no member `repositories`
- src/integrations/prefect-gitlab/tests/test_repositories.py:224:21: error[unresolved-attribute] Module `prefect_gitlab` has no member `repositories`
- src/integrations/prefect-gitlab/tests/test_repositories.py:247:29: error[unresolved-attribute] Module `prefect_gitlab` has no member `repositories`
- src/integrations/prefect-kubernetes/prefect_kubernetes/__init__.py:1:1: error[unresolved-import] Cannot resolve imported module `.`
+ src/integrations/prefect-kubernetes/prefect_kubernetes/__init__.py:1:15: error[unresolved-import] Module `prefect_kubernetes` has no member `_version`
- src/integrations/prefect-kubernetes/prefect_kubernetes/experimental/__init__.py:1:7: error[unresolved-import] Cannot resolve imported module `.decorators`
- src/integrations/prefect-ray/prefect_ray/__init__.py:1:1: error[unresolved-import] Cannot resolve imported module `.`
- src/integrations/prefect-ray/prefect_ray/__init__.py:6:7: error[unresolved-import] Cannot resolve imported module `.task_runners`
+ src/integrations/prefect-ray/prefect_ray/__init__.py:1:15: error[unresolved-import] Module `prefect_ray` has no member `_version`
+ src/integrations/prefect-ray/tests/test_task_runners.py:435:10: error[no-matching-overload] No overload matches arguments
+ src/integrations/prefect-ray/tests/test_task_runners.py:457:10: error[no-matching-overload] No overload matches arguments
+ src/integrations/prefect-ray/tests/test_task_runners.py:472:10: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-redis/prefect_redis/__init__.py:1:7: error[unresolved-import] Cannot resolve imported module `.blocks`
- src/integrations/prefect-redis/prefect_redis/__init__.py:2:7: error[unresolved-import] Cannot resolve imported module `.tasks`
- src/integrations/prefect-redis/prefect_redis/__init__.py:9:7: error[unresolved-import] Cannot resolve imported module `.locking`
- src/integrations/prefect-redis/prefect_redis/__init__.py:10:1: error[unresolved-import] Cannot resolve imported module `.`
+ src/integrations/prefect-redis/prefect_redis/__init__.py:10:15: error[unresolved-import] Module `prefect_redis` has no member `_version`
- src/integrations/prefect-redis/tests/test_messaging.py:80:12: error[invalid-return-type] Return type does not match returned value: expected `Publisher`, found `prefect.server.utilities.messaging.Publisher`
+ src/integrations/prefect-redis/tests/test_messaging.py:80:12: error[invalid-return-type] Return type does not match returned value: expected `prefect_redis.messaging.Publisher`, found `prefect.server.utilities.messaging.Publisher`
- src/integrations/prefect-redis/tests/test_messaging.py:86:12: error[invalid-return-type] Return type does not match returned value: expected `Consumer`, found `prefect.server.utilities.messaging.Consumer`
+ src/integrations/prefect-redis/tests/test_messaging.py:86:12: error[invalid-return-type] Return type does not match returned value: expected `prefect_redis.messaging.Consumer`, found `prefect.server.utilities.messaging.Consumer`
- src/integrations/prefect-redis/tests/test_messaging.py:98:12: error[invalid-return-type] Return type does not match returned value: expected `Consumer`, found `prefect.server.utilities.messaging.Consumer`
+ src/integrations/prefect-redis/tests/test_messaging.py:98:12: error[invalid-return-type] Return type does not match returned value: expected `prefect_redis.messaging.Consumer`, found `prefect.server.utilities.messaging.Consumer`
- src/integrations/prefect-redis/tests/test_messaging.py:104:12: error[invalid-return-type] Return type does not match returned value: expected `Publisher`, found `prefect.server.utilities.messaging.Publisher`
+ src/integrations/prefect-redis/tests/test_messaging.py:104:12: error[invalid-return-type] Return type does not match returned value: expected `prefect_redis.messaging.Publisher`, found `prefect.server.utilities.messaging.Publisher`
- src/integrations/prefect-shell/prefect_shell/__init__.py:1:1: error[unresolved-import] Cannot resolve imported module `.`
+ src/integrations/prefect-shell/prefect_shell/__init__.py:1:15: error[unresolved-import] Module `prefect_shell` has no member `_version`
- src/integrations/prefect-shell/prefect_shell/__init__.py:2:7: error[unresolved-import] Cannot resolve imported module `.commands`
- src/integrations/prefect-slack/prefect_slack/__init__.py:1:1: error[unresolved-import] Cannot resolve imported module `.`
+ src/integrations/prefect-slack/prefect_slack/__init__.py:1:15: error[unresolved-import] Module `prefect_slack` has no member `_version`
- src/integrations/prefect-slack/prefect_slack/__init__.py:2:7: error[unresolved-import] Cannot resolve imported module `.credentials`
+ src/integrations/prefect-slack/tests/test_credentials.py:11:40: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["xoxb-xxxx"]`
+ src/integrations/prefect-slack/tests/test_credentials.py:16:22: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["https://hooks.slack.com/xxxx"]`
+ src/integrations/prefect-slack/tests/test_credentials.py:37:26: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["http://wherever"]`
+ src/integrations/prefect-slack/tests/test_credentials.py:62:26: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["http://wherever"]`
+ src/integrations/prefect-slack/tests/test_credentials.py:76:28: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["http://test"]`
+ src/integrations/prefect-slack/tests/test_credentials.py:92:28: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["http://test"]`
+ src/integrations/prefect-slack/tests/test_credentials.py:110:28: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["http://test"]`
+ src/integrations/prefect-slack/tests/test_notification_block.py:24:26: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["http://wherever"]`
+ src/integrations/prefect-slack/tests/test_notification_block.py:49:26: error[invalid-argument-type] Argument is incorrect: Expected `SecretStr`, found `Literal["http://wherever"]`
- src/integrations/prefect-snowflake/prefect_snowflake/__init__.py:1:1: error[unresolved-import] Cannot resolve imported module `.`
+ src/integrations/prefect-snowflake/prefect_snowflake/__init__.py:1:15: error[unresolved-import] Module `prefect_snowflake` has no member `_version`
- src/integrations/prefect-sqlalchemy/prefect_sqlalchemy/__init__.py:1:1: error[unresolved-import] Cannot resolve imported module `.`
+ src/integrations/prefect-sqlalchemy/prefect_sqlalchemy/__init__.py:1:15: error[unresolved-import] Module `prefect_sqlalchemy` has no member `_version`
- src/integrations/prefect-sqlalchemy/prefect_sqlalchemy/__init__.py:2:7: error[unresolved-import] Cannot resolve imported module `.credentials`
- src/integrations/prefect-sqlalchemy/prefect_sqlalchemy/__init__.py:7:7: error[unresolved-import] Cannot resolve imported module `.database`
- Found 5012 diagnostics
+ Found 4965 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
+ src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 39 diagnostics
+ Found 41 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1207:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5814 diagnostics
+ Found 5815 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
- misc/dbt-materialize/dbt/adapters/materialize/connections.py:27:7: error[unresolved-import] Cannot resolve imported module `.__version__`
- misc/mcp-materialize-agents/mcp_materialize_agents/mcp_materialize_agents/__init__.py:36:7: error[unresolved-import] Cannot resolve imported module `.config`
- misc/mcp-materialize/mcp_materialize/__init__.py:52:7: error[unresolved-import] Cannot resolve imported module `.config`
- misc/mcp-materialize/mcp_materialize/__init__.py:53:7: error[unresolved-import] Cannot resolve imported module `.mz_client`
- Found 3371 diagnostics
+ Found 3367 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-02 02:17_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-import` | 0 | 74 | 17 |
| `unresolved-attribute` | 0 | 33 | 3 |
| `invalid-argument-type` | 33 | 0 | 0 |
| `no-matching-overload` | 17 | 0 | 0 |
| `invalid-return-type` | 1 | 0 | 4 |
| `possibly-missing-attribute` | 3 | 0 | 0 |
| `unsupported-base` | 3 | 0 | 0 |
| `invalid-context-manager` | 2 | 0 | 0 |
| `invalid-assignment` | 1 | 0 | 0 |
| `unknown-argument` | 1 | 0 | 0 |
| **Total** | **61** | **107** | **24** |

**[Full report with detailed diff](https://gankra-workup2.ecosystem-663.pages.dev/diff)** ([timing results](https://gankra-workup2.ecosystem-663.pages.dev/timing))




---

_Comment by @Gankra on 2025-12-02 02:24_

Most of those results look good and correct

---

_Comment by @Gankra on 2025-12-02 14:06_

This is now included in the base PR.

---

_Closed by @Gankra on 2025-12-02 14:06_

---
