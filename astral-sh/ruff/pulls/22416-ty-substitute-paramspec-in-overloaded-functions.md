```yaml
number: 22416
title: "[ty] Substitute `ParamSpec` in overloaded functions"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: dhruv/paramspec-overload-2
created_at: 2026-01-06T10:46:28Z
updated_at: 2026-01-07T08:00:35Z
url: https://github.com/astral-sh/ruff/pull/22416
synced_at: 2026-01-12T15:57:49Z
```

# [ty] Substitute `ParamSpec` in overloaded functions

---

_@dhruvmanila_

## Summary

fixes: https://github.com/astral-sh/ty/issues/2027

This PR fixes a bug where the type mapping for a `ParamSpec` was not being applied in an overloaded function.

This PR also fixes https://github.com/astral-sh/ty/issues/2081 and reveals new diagnostics which doesn't look related to the bug:

```py
from prefect import flow, task

@task
def task_get() -> int:
    """Task get integer."""
    return 42

@task
def task_add(x: int, y: int) -> int:
    """Task add two integers."""
    print(f"Adding {x} and {y}")
    return x + y

@flow
def my_flow():
    """My flow."""
    x = 23
    future_y = task_get.submit()

	# error: [no-matching-overload]
    task_add(future_y, future_y)
	# error: [no-matching-overload]
    task_add(x, future_y)
```

The reason is that the type of `future_y` is `PrefectFuture[int]` while the type of `task_add` is `Task[(x: int, y: int), int]` which means that the assignment between `int` and `PrefectFuture[int]` fails which results in no overload matching. Pyright also raises the invalid argument type error on all three usages of `future_y` in those two calls.

## Test Plan

Add regression mdtest from the linked issue.


---

_Label `bug` added by @dhruvmanila on 2026-01-06 10:46_

---

_Label `ty` added by @dhruvmanila on 2026-01-06 10:46_

---

_Comment by @astral-sh-bot[bot] on 2026-01-06 10:48_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-06 10:52_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-aws/tests/experimental/test_decorators.py:99:14: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-aws/tests/experimental/test_decorators.py:116:20: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-aws/tests/test_batch.py:122:24: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-aws/tests/test_client_waiter.py:93:26: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-aws/tests/test_client_waiter.py:101:26: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-aws/tests/test_client_waiter.py:119:26: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-aws/tests/test_s3.py:355:15: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-aws/tests/test_s3.py:365:15: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-aws/tests/test_s3.py:442:21: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-aws/tests/test_s3.py:449:21: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-aws/tests/test_s3.py:1183:26: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-aws/tests/test_s3.py:1199:26: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-aws/tests/test_s3.py:1219:19: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-aws/tests/test_s3.py:1240:19: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-azure/tests/experimental/tests_decorators.py:93:14: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-azure/tests/experimental/tests_decorators.py:112:20: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-azure/tests/test_blob_storage.py:20:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-azure/tests/test_blob_storage.py:42:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-azure/tests/test_blob_storage.py:62:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-azure/tests/test_blob_storage.py:77:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-azure/tests/test_blob_storage.py:92:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-azure/tests/test_blob_storage.py:107:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-azure/tests/test_cosmos_db.py:40:24: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-azure/tests/test_cosmos_db.py:60:24: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-azure/tests/test_ml_datastore.py:24:24: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-azure/tests/test_ml_datastore.py:44:24: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-azure/tests/test_ml_datastore.py:61:24: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-azure/tests/test_ml_datastore.py:80:24: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dask/tests/test_task_runners.py:104:31: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `PrefectFuture[Unknown]`
- src/integrations/prefect-dask/tests/test_task_runners.py:130:31: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `PrefectFuture[Unknown]`
- src/integrations/prefect-dask/tests/test_task_runners.py:178:31: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `PrefectFuture[Unknown]`
- src/integrations/prefect-dask/tests/test_task_runners.py:220:42: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `PrefectFuture[Unknown]`
- src/integrations/prefect-dask/tests/test_task_runners.py:250:42: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `PrefectFuture[Unknown]`
- src/integrations/prefect-dask/tests/test_task_runners.py:385:31: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `PrefectFuture[Unknown]`
- src/integrations/prefect-dask/tests/test_task_runners.py:418:24: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dask/tests/test_task_runners.py:459:31: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `list[Unknown | PrefectFuture[Unknown]]`
- src/integrations/prefect-dask/tests/test_task_runners.py:485:31: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `list[Unknown | PrefectFuture[Unknown]]`
- src/integrations/prefect-dask/tests/test_task_runners.py:515:31: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `PrefectFuture[Unknown]`
- src/integrations/prefect-dask/tests/test_task_runners.py:540:37: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `PrefectFuture[Unknown]`
+ src/integrations/prefect-databricks/prefect_databricks/flows.py:304:44: error[unresolved-attribute] Object of type `CoroutineType[Any, Any, dict[str, Any]]` has no attribute `get`
- src/integrations/prefect-databricks/prefect_databricks/flows.py:279:49: error[no-matching-overload] No overload matches arguments
+ src/integrations/prefect-databricks/prefect_databricks/flows.py:460:37: error[no-matching-overload] No overload of bound method `submit` matches arguments
- src/integrations/prefect-databricks/prefect_databricks/flows.py:301:21: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(run_id: int, databricks_credentials: DatabricksCredentials)`, found `DatabricksCredentials`
- src/integrations/prefect-databricks/prefect_databricks/flows.py:461:13: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(run_id: int, databricks_credentials: DatabricksCredentials, include_history: bool | None = None)`, found `int`
- src/integrations/prefect-databricks/prefect_databricks/flows.py:462:13: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(run_id: int, databricks_credentials: DatabricksCredentials, include_history: bool | None = None)`, found `DatabricksCredentials`
- src/integrations/prefect-databricks/prefect_databricks/flows.py:463:13: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(run_id: int, databricks_credentials: DatabricksCredentials, include_history: bool | None = None)`, found `list[Unknown]`
+ src/integrations/prefect-databricks/prefect_databricks/flows.py:652:18: error[not-subscriptable] Cannot subscript object of type `CoroutineType[Any, Any, dict[str, Any]]` with no `__getitem__` method
+ src/integrations/prefect-databricks/prefect_databricks/flows.py:680:44: error[unresolved-attribute] Object of type `CoroutineType[Any, Any, dict[str, Any]]` has no attribute `get`
- src/integrations/prefect-databricks/prefect_databricks/flows.py:631:24: error[no-matching-overload] No overload of bound method `submit` matches arguments
- src/integrations/prefect-databricks/prefect_databricks/flows.py:656:49: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-databricks/prefect_databricks/flows.py:677:21: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(run_id: int, databricks_credentials: DatabricksCredentials)`, found `DatabricksCredentials`
- src/integrations/prefect-databricks/tests/test_flows.py:165:44: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-databricks/tests/test_flows.py:196:24: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-databricks/tests/test_flows.py:245:19: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-databricks/tests/test_flows.py:283:19: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-databricks/tests/test_flows.py:322:19: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-databricks/tests/test_flows.py:360:19: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-databricks/tests/test_flows.py:401:24: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-databricks/tests/test_flows.py:452:24: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-databricks/tests/test_flows.py:499:24: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-databricks/tests/test_flows.py:544:19: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-databricks/tests/test_flows.py:584:19: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-databricks/tests/test_flows.py:625:19: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-databricks/tests/test_flows.py:677:24: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/prefect_dbt/cloud/jobs.py:351:39: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/prefect_dbt/cloud/jobs.py:360:40: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/prefect_dbt/cloud/jobs.py:369:47: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/prefect_dbt/cloud/jobs.py:397:34: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/prefect_dbt/cloud/jobs.py:474:45: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/prefect_dbt/cloud/jobs.py:598:29: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/prefect_dbt/cloud/jobs.py:606:29: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/prefect_dbt/cloud/jobs.py:624:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cli/test_commands.py:284:16: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cli/test_commands.py:299:16: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cli/test_commands.py:315:16: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cli/test_commands.py:333:9: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cli/test_commands.py:336:19: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cli/test_commands.py:371:9: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cli/test_commands.py:374:19: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cli/test_commands.py:389:16: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cli/test_commands.py:410:16: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cli/test_commands.py:424:16: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cli/test_commands.py:435:16: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cli/test_commands.py:450:16: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cli/test_commands.py:575:9: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cloud/test_jobs.py:116:30: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cloud/test_jobs.py:156:23: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cloud/test_jobs.py:179:23: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cloud/test_jobs.py:210:28: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cloud/test_jobs.py:245:28: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cloud/test_jobs.py:281:23: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cloud/test_jobs.py:315:23: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cloud/test_jobs.py:331:23: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cloud/test_jobs.py:360:23: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cloud/test_jobs.py:391:23: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cloud/test_jobs.py:425:28: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cloud/test_jobs.py:434:19: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cloud/test_jobs.py:558:23: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cloud/test_jobs.py:593:20: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cloud/test_jobs.py:848:23: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cloud/test_jobs.py:878:28: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cloud/test_jobs.py:916:23: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cloud/test_runs.py:74:40: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-dbt/tests/cloud/test_runs.py:78:41: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-docker/tests/experimental/test_decorators.py:106:14: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-docker/tests/experimental/test_decorators.py:123:20: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-docker/tests/test-project/flow.py:24:10: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-docker/tests/test-project/flow.py:31:9: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-email/tests/test_message.py:48:25: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/experimental/test_decorators.py:94:18: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/experimental/test_decorators.py:110:24: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/experimental/test_decorators.py:266:18: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/experimental/test_decorators.py:282:24: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/test_bigquery.py:27:16: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/test_bigquery.py:63:17: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/test_bigquery.py:81:17: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/test_bigquery.py:105:18: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/test_bigquery.py:124:18: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/test_bigquery.py:145:18: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/test_cloud_storage.py:41:16: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/test_cloud_storage.py:52:16: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/test_cloud_storage.py:72:16: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/test_cloud_storage.py:92:16: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/test_cloud_storage.py:103:16: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/test_credentials.py:273:23: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/test_secret_manager.py:17:16: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/test_secret_manager.py:26:9: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/test_secret_manager.py:27:16: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/test_secret_manager.py:38:16: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/test_secret_manager.py:50:16: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-gcp/tests/test_secret_manager.py:64:16: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-kubernetes/tests/experimental/test_decorator.py:98:14: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-kubernetes/tests/experimental/test_decorator.py:115:20: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-kubernetes/tests/test_flows.py:184:16: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-ray/tests/test_task_runners.py:221:31: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `PrefectFuture[Unknown]`
- src/integrations/prefect-ray/tests/test_task_runners.py:247:31: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `PrefectFuture[Unknown]`
- src/integrations/prefect-ray/tests/test_task_runners.py:295:31: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `PrefectFuture[Unknown]`
- src/integrations/prefect-ray/tests/test_task_runners.py:337:42: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `PrefectFuture[Unknown]`
- src/integrations/prefect-ray/tests/test_task_runners.py:367:42: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `PrefectFuture[Unknown]`
- src/integrations/prefect-ray/tests/test_task_runners.py:391:38: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `Unknown | int`
- src/integrations/prefect-ray/tests/test_task_runners.py:437:34: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `Literal["a"]`
- src/integrations/prefect-ray/tests/test_task_runners.py:438:34: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `Literal["b"]`
- src/integrations/prefect-ray/tests/test_task_runners.py:439:34: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `Literal["c"]`
- src/integrations/prefect-ray/tests/test_task_runners.py:446:34: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `Literal["d"]`
- src/integrations/prefect-ray/tests/test_task_runners.py:461:32: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `Literal[42]`
- src/integrations/prefect-ray/tests/test_task_runners.py:475:37: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `list[Unknown]`
- src/integrations/prefect-ray/tests/test_task_runners.py:476:37: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `list[Unknown]`
- src/integrations/prefect-ray/tests/test_task_runners.py:478:26: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `list[Unknown]`
- src/integrations/prefect-ray/tests/test_task_runners.py:479:26: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `list[Unknown]`
- src/integrations/prefect-ray/tests/test_task_runners.py:480:26: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `list[Unknown]`
- src/integrations/prefect-ray/tests/test_task_runners.py:529:37: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(...)`, found `PrefectFuture[Unknown]`
- src/integrations/prefect-shell/tests/test_commands.py:21:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands.py:34:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands.py:48:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands.py:60:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands.py:68:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands.py:76:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands.py:86:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands.py:94:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands.py:102:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands.py:138:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands_windows.py:20:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands_windows.py:36:21: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands_windows.py:55:21: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands_windows.py:76:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands_windows.py:89:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands_windows.py:102:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands_windows.py:114:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands_windows.py:122:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands_windows.py:133:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands_windows.py:149:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands_windows.py:160:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands_windows.py:172:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-shell/tests/test_commands_windows.py:204:22: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-sqlalchemy/tests/test_database.py:484:13: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-sqlalchemy/tests/test_database.py:485:24: error[no-matching-overload] No overload matches arguments
- src/integrations/prefect-sqlalchemy/tests/test_database.py:488:16: error[no-matching-overload] No overload matches arguments
- src/prefect/cli/shell.py:165:9: error[no-matching-overload] No overload matches arguments
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
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
- Found 5533 diagnostics
+ Found 5358 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/core/hydpytools.py:2628:55: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `DiGraph[Unknown]`
- hydpy/core/selectiontools.py:828:42: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `DiGraph[Unknown]`
- hydpy/core/selectiontools.py:828:49: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `Node | Element`
- hydpy/core/selectiontools.py:980:44: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `DiGraph[Unknown]`
- hydpy/core/selectiontools.py:980:51: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `Node | Element`
- hydpy/core/threadingtools.py:127:56: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `DiGraph[Unknown]`
- hydpy/core/threadingtools.py:127:63: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `Element`
- hydpy/core/threadingtools.py:247:59: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `DiGraph[Unknown]`
- hydpy/core/threadingtools.py:247:66: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `Element`
- hydpy/core/threadingtools.py:259:63: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `DiGraph[Unknown]`
- hydpy/core/threadingtools.py:259:70: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `Node`
- hydpy/models/hland/hland_derived.py:380:75: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `DiGraph[Unknown]`
- hydpy/models/hland/hland_derived.py:387:83: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `(...)`, found `DiGraph[Unknown]`
- Found 679 diagnostics
+ Found 666 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5131 diagnostics
+ Found 5134 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14474 diagnostics
+ Found 14473 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @dhruvmanila on 2026-01-06 14:59_

---

_Review requested from @carljm by @dhruvmanila on 2026-01-06 14:59_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2026-01-06 14:59_

---

_Review requested from @sharkdp by @dhruvmanila on 2026-01-06 14:59_

---

_Review requested from @dcreager by @dhruvmanila on 2026-01-06 14:59_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:221 on 2026-01-06 15:11_

Not in scope for this PR, but I want to take a quick stab at seeing if we can remove the redundancy between `Specialization` and `PartialSpecialization`. This is getting silly that you/we have to copy-paste this much!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:243 on 2026-01-06 15:21_

If I'm reading the [docs](https://docs.rs/smallvec/latest/smallvec/macro.smallvec_inline.html) right, `smallvec_inline!` only works here because `result.overloads` happens to be a `SmallVec` with 1 stack-reserved element. If we were to change the type of `result.overloads`, or even the size of the `SmallVec`, this would not work anymore. Is it worth using something more future-proof, like wrapping in `Either`? `flat_map` just needs to return an iterator.

```rust
Either::Right([signature.apply_type_mapping_impl(
    db,
    type_mapping,
    tcx,
    visitor,
)]
```


---

_@dcreager approved on 2026-01-06 15:22_

---

_@MichaReiser reviewed on 2026-01-06 15:28_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/signatures.rs`:243 on 2026-01-06 15:28_

I think that's something we can deal with when we change the type. Seems unlikely that we change the type and increasing the size of the small vec isn't an issue.

---

_@dcreager reviewed on 2026-01-06 15:30_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:243 on 2026-01-06 15:30_

Fair

---

_@dhruvmanila reviewed on 2026-01-06 16:03_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:221 on 2026-01-06 16:03_

Yes, definitely that would be useful!

---

_@dcreager reviewed on 2026-01-06 19:11_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/signatures.rs`:221 on 2026-01-06 19:11_

https://github.com/astral-sh/ruff/pull/22422

(That will have a merge conflict with this PR, so I'm happy to defer to you and let you merge this first. Then I can rebase/fix the conflicts on 22422)

---

_@dhruvmanila reviewed on 2026-01-07 07:45_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:221 on 2026-01-07 07:45_

I merged that PR and rebased this, will merge it now.

---

_Merged by @dhruvmanila on 2026-01-07 08:00_

---

_Closed by @dhruvmanila on 2026-01-07 08:00_

---

_Branch deleted on 2026-01-07 08:00_

---
