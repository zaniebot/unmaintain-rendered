```yaml
number: 21118
title: "[`ruff`] Implement `direct-member-import` (RUF066)"
type: pull_request
state: closed
author: revmischa
labels:
  - rule
  - needs-decision
assignees: []
base: main
head: feature/ruff066-direct-member-import
created_at: 2025-10-29T03:06:59Z
updated_at: 2025-10-29T17:19:56Z
url: https://github.com/astral-sh/ruff/pull/21118
synced_at: 2026-01-10T16:59:49Z
```

# [`ruff`] Implement `direct-member-import` (RUF066)

---

_Pull request opened by @revmischa on 2025-10-29 03:06_

## Summary

Implements **RUF066** (`direct-member-import`) to enforce importing modules instead of specific members, following [section 2.2 of the Google Python Style Guide](https://google.github.io/styleguide/pyguide.html#22-imports).

This rule helps make code origins more explicit by flagging `from module import member` patterns and encouraging `import module` instead.

## Rule Behavior

**Flags:**
```python
from os import path  # RUF066
from pathlib import Path  # RUF066
from collections import defaultdict  # RUF066
from json import loads, dumps  # RUF066 (reports twice)
```

**Exemptions** (per Google Style Guide 2.2.4.1):
- `typing` and `typing_extensions` modules - type hints are commonly imported directly
- `collections.abc` module - abstract base classes  
- `six.moves` module - Python 2/3 compatibility
- Relative imports (e.g., `from . import foo`)
- Wildcard imports (e.g., `from module import *`)

**Example violations:**
```python
# Bad - violates RUF066
from os import path
result = path.join("a", "b")

# Good - passes RUF066  
import os
result = os.path.join("a", "b")
```

## Implementation Details

- **Rule location:** `crates/ruff_linter/src/rules/ruff/rules/direct_member_import.rs`
- **Rule code:** RUF066
- **Preview mode:** `preview_since = "0.15.0"`
- **Integration:** Checks `StmtImportFrom` nodes in AST analysis
- **Test fixture:** `crates/ruff_linter/resources/test/fixtures/ruff/RUF066.py`

## Test Plan

- ✅ Compiles successfully with `cargo build`
- ✅ Added comprehensive test fixture covering violations and exemptions
- 🔲 Snapshot tests (require `cargo insta test --accept`)
- 🔲 Full validation suite (clippy, schema update, pre-commit)

## References

- Google Python Style Guide: https://google.github.io/styleguide/pyguide.html#22-imports
- Exemptions follow section 2.2.4.1 exactly

---

_Marked ready for review by @revmischa on 2025-10-29 04:04_

---

_Comment by @ntBre on 2025-10-29 15:08_

Thanks for working on this! I believe this is related to issues https://github.com/astral-sh/ruff/issues/5841 and https://github.com/astral-sh/ruff/issues/3045, although one of the examples in the summary here is more restrictive than those in the issues.

However, these are both marked as `needs-decision` and we aren't prioritizing adding new rules at the moment, so I don't think we can move this forward currently.

---

_Label `rule` added by @ntBre on 2025-10-29 15:08_

---

_Label `needs-decision` added by @ntBre on 2025-10-29 15:08_

---

_Comment by @github-actions[bot] on 2025-10-29 15:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -227 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -227 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/src/airflow/cli/commands/dag_command.py#L240'>airflow-core/src/airflow/cli/commands/dag_command.py:240:34:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/src/airflow/cli/commands/dag_command.py#L388'>airflow-core/src/airflow/cli/commands/dag_command.py:388:32:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/src/airflow/cli/commands/dag_command.py#L403'>airflow-core/src/airflow/cli/commands/dag_command.py:403:29:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/src/airflow/cli/commands/dag_command.py#L413'>airflow-core/src/airflow/cli/commands/dag_command.py:413:46:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/src/airflow/cli/commands/dag_command.py#L413'>airflow-core/src/airflow/cli/commands/dag_command.py:413:96:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/src/airflow/cli/commands/dag_command.py#L547'>airflow-core/src/airflow/cli/commands/dag_command.py:547:30:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/src/airflow/cli/commands/dag_command.py#L578'>airflow-core/src/airflow/cli/commands/dag_command.py:578:34:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/src/airflow/cli/commands/dag_command.py#L616'>airflow-core/src/airflow/cli/commands/dag_command.py:616:25:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/src/airflow/dag_processing/dagbag.py#L155'>airflow-core/src/airflow/dag_processing/dagbag.py:155:36:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/src/airflow/dag_processing/dagbag.py#L245'>airflow-core/src/airflow/dag_processing/dagbag.py:245:30:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/src/airflow/dag_processing/dagbag.py#L561'>airflow-core/src/airflow/dag_processing/dagbag.py:561:28:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/tests/integration/otel/dags/otel_test_dag.py#L87'>airflow-core/tests/integration/otel/dags/otel_test_dag.py:87:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_between_tasks.py#L152'>airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_between_tasks.py:152:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_in_task.py#L145'>airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_in_task.py:145:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/tests/unit/dag_processing/test_processor.py#L541'>airflow-core/tests/unit/dag_processing/test_processor.py:541:11:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/tests/unit/utils/test_db_cleanup.py#L684'>airflow-core/tests/unit/utils/test_db_cleanup.py:684:15:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/devel-common/src/tests_common/pytest_plugin.py#L1251'>devel-common/src/tests_common/pytest_plugin.py:1251:24:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/devel-common/src/tests_common/pytest_plugin.py#L2805'>devel-common/src/tests_common/pytest_plugin.py:2805:16:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/performance/src/performance_dags/performance_dag/performance_dag.py#L235'>performance/src/performance_dags/performance_dag/performance_dag.py:235:11:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/providers/airbyte/tests/system/airbyte/example_airbyte_trigger_job.py#L33'>providers/airbyte/tests/system/airbyte/example_airbyte_trigger_job.py:33:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/providers/amazon/tests/unit/amazon/aws/operators/test_s3.py#L651'>providers/amazon/tests/unit/amazon/aws/operators/test_s3.py:651:15:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/providers/amazon/tests/unit/amazon/aws/transfers/test_base.py#L42'>providers/amazon/tests/unit/amazon/aws/transfers/test_base.py:42:20:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/providers/amazon/tests/unit/amazon/aws/transfers/test_dynamodb_to_s3.py#L268'>providers/amazon/tests/unit/amazon/aws/transfers/test_dynamodb_to_s3.py:268:15:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/providers/apache/druid/tests/system/apache/druid/example_druid.py#L31'>providers/apache/druid/tests/system/apache/druid/example_druid.py:31:6:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/providers/apache/flink/tests/unit/apache/flink/operators/test_flink_kubernetes.py#L201'>providers/apache/flink/tests/unit/apache/flink/operators/test_flink_kubernetes.py:201:20:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/providers/apache/flink/tests/unit/apache/flink/sensors/test_flink_kubernetes.py#L887'>providers/apache/flink/tests/unit/apache/flink/sensors/test_flink_kubernetes.py:887:20:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
... 199 additional changes omitted for rule AIR311
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/providers/openlineage/tests/unit/openlineage/plugins/test_listener.py#L123'>providers/openlineage/tests/unit/openlineage/plugins/test_listener.py:123:13:</a> AIR301 `create_dagrun` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/providers/openlineage/tests/unit/openlineage/plugins/test_listener.py#L966'>providers/openlineage/tests/unit/openlineage/plugins/test_listener.py:966:13:</a> AIR301 `create_dagrun` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/providers/openlineage/tests/unit/openlineage/utils/test_utils.py#L1715'>providers/openlineage/tests/unit/openlineage/utils/test_utils.py:1715:18:</a> AIR301 `create_dagrun` is removed in Airflow 3.0
... 198 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR311 | 224 | 0 | 224 | 0 | 0 |
| AIR301 | 3 | 0 | 3 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+186776 -227 violations, +0 -0 fixes in 28 projects; 27 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+535 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/872e3cc4d8bacdbf7ed45c5de25a527f15bec0e3/disnake/__main__.py#L7'>disnake/__main__.py:7:21:</a> RUF066 Prefer importing the module `pathlib` instead of member `Path`
+ <a href='https://github.com/DisnakeDev/disnake/blob/872e3cc4d8bacdbf7ed45c5de25a527f15bec0e3/disnake/abc.py#L3'>disnake/abc.py:3:24:</a> RUF066 Prefer importing the module `__future__` instead of member `annotations`
+ <a href='https://github.com/DisnakeDev/disnake/blob/872e3cc4d8bacdbf7ed45c5de25a527f15bec0e3/disnake/abc.py#L57'>disnake/abc.py:57:26:</a> RUF066 Prefer importing the module `datetime` instead of member `datetime`
+ <a href='https://github.com/DisnakeDev/disnake/blob/872e3cc4d8bacdbf7ed45c5de25a527f15bec0e3/disnake/abc.py#L7'>disnake/abc.py:7:17:</a> RUF066 Prefer importing the module `abc` instead of member `ABC`
+ <a href='https://github.com/DisnakeDev/disnake/blob/872e3cc4d8bacdbf7ed45c5de25a527f15bec0e3/disnake/activity.py#L3'>disnake/activity.py:3:24:</a> RUF066 Prefer importing the module `__future__` instead of member `annotations`
+ <a href='https://github.com/DisnakeDev/disnake/blob/872e3cc4d8bacdbf7ed45c5de25a527f15bec0e3/disnake/app_commands.py#L3'>disnake/app_commands.py:3:24:</a> RUF066 Prefer importing the module `__future__` instead of member `annotations`
+ <a href='https://github.com/DisnakeDev/disnake/blob/872e3cc4d8bacdbf7ed45c5de25a527f15bec0e3/disnake/app_commands.py#L7'>disnake/app_commands.py:7:17:</a> RUF066 Prefer importing the module `abc` instead of member `ABC`
+ <a href='https://github.com/DisnakeDev/disnake/blob/872e3cc4d8bacdbf7ed45c5de25a527f15bec0e3/disnake/appinfo.py#L3'>disnake/appinfo.py:3:24:</a> RUF066 Prefer importing the module `__future__` instead of member `annotations`
+ <a href='https://github.com/DisnakeDev/disnake/blob/872e3cc4d8bacdbf7ed45c5de25a527f15bec0e3/disnake/application_role_connection.py#L3'>disnake/application_role_connection.py:3:24:</a> RUF066 Prefer importing the module `__future__` instead of member `annotations`
+ <a href='https://github.com/DisnakeDev/disnake/blob/872e3cc4d8bacdbf7ed45c5de25a527f15bec0e3/disnake/asset.py#L3'>disnake/asset.py:3:24:</a> RUF066 Prefer importing the module `__future__` instead of member `annotations`
... 525 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+6372 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/.github/scripts/download_pretrained.py#L10'>.github/scripts/download_pretrained.py:10:5:</a> RUF066 Prefer importing the module `rasa.nlu.utils.hugging_face.registry` instead of member `model_weights_defaults`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/.github/scripts/download_pretrained.py#L11'>.github/scripts/download_pretrained.py:11:5:</a> RUF066 Prefer importing the module `rasa.nlu.utils.hugging_face.registry` instead of member `model_class_dict`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/.github/scripts/download_pretrained.py#L6'>.github/scripts/download_pretrained.py:6:26:</a> RUF066 Prefer importing the module `transformers` instead of member `AutoTokenizer`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/.github/scripts/download_pretrained.py#L6'>.github/scripts/download_pretrained.py:6:41:</a> RUF066 Prefer importing the module `transformers` instead of member `TFAutoModel`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/.github/scripts/mr_generate_summary.py#L4'>.github/scripts/mr_generate_summary.py:4:25:</a> RUF066 Prefer importing the module `collections` instead of member `defaultdict`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/.github/scripts/mr_generate_summary.py#L7'>.github/scripts/mr_generate_summary.py:7:21:</a> RUF066 Prefer importing the module `pathlib` instead of member `Path`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/.github/scripts/mr_publish_results.py#L10'>.github/scripts/mr_publish_results.py:10:35:</a> RUF066 Prefer importing the module `datadog_api_client.v1` instead of member `ApiClient`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/.github/scripts/mr_publish_results.py#L10'>.github/scripts/mr_publish_results.py:10:46:</a> RUF066 Prefer importing the module `datadog_api_client.v1` instead of member `Configuration`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/.github/scripts/mr_publish_results.py#L11'>.github/scripts/mr_publish_results.py:11:51:</a> RUF066 Prefer importing the module `datadog_api_client.v1.api.metrics_api` instead of member `MetricsApi`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/.github/scripts/mr_publish_results.py#L12'>.github/scripts/mr_publish_results.py:12:57:</a> RUF066 Prefer importing the module `datadog_api_client.v1.model.metrics_payload` instead of member `MetricsPayload`
... 6362 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+43917 -227 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/docs/conf.py#L21'>airflow-core/docs/conf.py:21:24:</a> RUF066 Prefer importing the module `__future__` instead of member `annotations`
+ <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/docs/conf.py#L27'>airflow-core/docs/conf.py:27:21:</a> RUF066 Prefer importing the module `pathlib` instead of member `Path`
+ <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/docs/conf.py#L31'>airflow-core/docs/conf.py:31:5:</a> RUF066 Prefer importing the module `docs.utils.conf_constants` instead of member `AIRFLOW_CORE_DOC_STATIC_PATH`
+ <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/docs/conf.py#L32'>airflow-core/docs/conf.py:32:5:</a> RUF066 Prefer importing the module `docs.utils.conf_constants` instead of member `AIRFLOW_CORE_DOCKER_COMPOSE_PATH`
+ <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/docs/conf.py#L33'>airflow-core/docs/conf.py:33:5:</a> RUF066 Prefer importing the module `docs.utils.conf_constants` instead of member `AIRFLOW_CORE_ROOT_PATH`
+ <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/docs/conf.py#L34'>airflow-core/docs/conf.py:34:5:</a> RUF066 Prefer importing the module `docs.utils.conf_constants` instead of member `AIRFLOW_CORE_SRC_PATH`
... 43912 additional changes omitted for rule RUF066
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/src/airflow/cli/commands/dag_command.py#L240'>airflow-core/src/airflow/cli/commands/dag_command.py:240:34:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/src/airflow/cli/commands/dag_command.py#L388'>airflow-core/src/airflow/cli/commands/dag_command.py:388:32:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/src/airflow/cli/commands/dag_command.py#L403'>airflow-core/src/airflow/cli/commands/dag_command.py:403:29:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/src/airflow/cli/commands/dag_command.py#L413'>airflow-core/src/airflow/cli/commands/dag_command.py:413:46:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
- <a href='https://github.com/apache/airflow/blob/15e0b7501d9a66491597f77626de639403effa77/airflow-core/src/airflow/cli/commands/dag_command.py#L413'>airflow-core/src/airflow/cli/commands/dag_command.py:413:96:</a> AIR311 `airflow.DAG` is removed in Airflow 3.0; It still works in Airflow 3.0 but is expected to be removed in a future version.
... 44133 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+13465 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/99b61143f644a2582a78a216574267fe036ee726/RELEASING/changelog.py#L21'>RELEASING/changelog.py:21:25:</a> RUF066 Prefer importing the module `dataclasses` instead of member `dataclass`
+ <a href='https://github.com/apache/superset/blob/99b61143f644a2582a78a216574267fe036ee726/RELEASING/changelog.py#L25'>RELEASING/changelog.py:25:24:</a> RUF066 Prefer importing the module `click.core` instead of member `Context`
+ <a href='https://github.com/apache/superset/blob/99b61143f644a2582a78a216574267fe036ee726/RELEASING/changelog.py#L28'>RELEASING/changelog.py:28:24:</a> RUF066 Prefer importing the module `github` instead of member `BadCredentialsException`
+ <a href='https://github.com/apache/superset/blob/99b61143f644a2582a78a216574267fe036ee726/RELEASING/changelog.py#L28'>RELEASING/changelog.py:28:49:</a> RUF066 Prefer importing the module `github` instead of member `Github`
+ <a href='https://github.com/apache/superset/blob/99b61143f644a2582a78a216574267fe036ee726/RELEASING/changelog.py#L28'>RELEASING/changelog.py:28:57:</a> RUF066 Prefer importing the module `github` instead of member `PullRequest`
+ <a href='https://github.com/apache/superset/blob/99b61143f644a2582a78a216574267fe036ee726/RELEASING/changelog.py#L28'>RELEASING/changelog.py:28:70:</a> RUF066 Prefer importing the module `github` instead of member `Repository`
+ <a href='https://github.com/apache/superset/blob/99b61143f644a2582a78a216574267fe036ee726/RELEASING/generate_email.py#L20'>RELEASING/generate_email.py:20:24:</a> RUF066 Prefer importing the module `click.core` instead of member `Context`
+ <a href='https://github.com/apache/superset/blob/99b61143f644a2582a78a216574267fe036ee726/docker/pythonpath_dev/superset_config.py#L27'>docker/pythonpath_dev/superset_config.py:27:30:</a> RUF066 Prefer importing the module `celery.schedules` instead of member `crontab`
+ <a href='https://github.com/apache/superset/blob/99b61143f644a2582a78a216574267fe036ee726/docker/pythonpath_dev/superset_config.py#L28'>docker/pythonpath_dev/superset_config.py:28:52:</a> RUF066 Prefer importing the module `flask_caching.backends.filesystemcache` instead of member `FileSystemCache`
+ <a href='https://github.com/apache/superset/blob/99b61143f644a2582a78a216574267fe036ee726/docker/pythonpath_dev/superset_config_docker_light.py#L21'>docker/pythonpath_dev/superset_config_docker_light.py:21:52:</a> RUF066 Prefer importing the module `flask_caching.backends.filesystemcache` instead of member `FileSystemCache`
... 13455 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+1933 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L119'>check_proxy.py:119:27:</a> RUF066 Prefer importing the module `distutils` instead of member `dir_util`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L125'>check_proxy.py:125:39:</a> RUF066 Prefer importing the module `shared_utils.colorful` instead of member `log亮黄`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L125'>check_proxy.py:125:46:</a> RUF066 Prefer importing the module `shared_utils.colorful` instead of member `log亮绿`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L125'>check_proxy.py:125:53:</a> RUF066 Prefer importing the module `shared_utils.colorful` instead of member `log亮红`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L179'>check_proxy.py:179:29:</a> RUF066 Prefer importing the module `toolbox` instead of member `get_conf`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L195'>check_proxy.py:195:47:</a> RUF066 Prefer importing the module `shared_utils.colorful` instead of member `log亮黄`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L1'>check_proxy.py:1:20:</a> RUF066 Prefer importing the module `loguru` instead of member `logger`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L206'>check_proxy.py:206:45:</a> RUF066 Prefer importing the module `toolbox` instead of member `trimmed_format_exc`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L217'>check_proxy.py:217:33:</a> RUF066 Prefer importing the module `toolbox` instead of member `trimmed_format_exc`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L226'>check_proxy.py:226:25:</a> RUF066 Prefer importing the module `toolbox` instead of member `ProxyNetworkActivate`
... 1923 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+6220 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/docserver.py#L34'>docs/bokeh/docserver.py:34:21:</a> RUF066 Prefer importing the module `pathlib` instead of member `Path`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/docserver.py#L37'>docs/bokeh/docserver.py:37:19:</a> RUF066 Prefer importing the module `flask` instead of member `redirect`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/docserver.py#L37'>docs/bokeh/docserver.py:37:29:</a> RUF066 Prefer importing the module `flask` instead of member `url_for`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/docserver.py#L38'>docs/bokeh/docserver.py:38:32:</a> RUF066 Prefer importing the module `tornado.httpserver` instead of member `HTTPServer`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/docserver.py#L39'>docs/bokeh/docserver.py:39:28:</a> RUF066 Prefer importing the module `tornado.ioloop` instead of member `IOLoop`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/docserver.py#L40'>docs/bokeh/docserver.py:40:26:</a> RUF066 Prefer importing the module `tornado.wsgi` instead of member `WSGIContainer`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/docserver.py#L42'>docs/bokeh/docserver.py:42:32:</a> RUF066 Prefer importing the module `bokeh.util.tornado` instead of member `fixup_windows_event_loop_policy`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/conf.py#L10'>docs/bokeh/source/conf.py:10:22:</a> RUF066 Prefer importing the module `datetime` instead of member `date`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/conf.py#L12'>docs/bokeh/source/conf.py:12:25:</a> RUF066 Prefer importing the module `sphinx.util` instead of member `logging`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/source/conf.py#L15'>docs/bokeh/source/conf.py:15:19:</a> RUF066 Prefer importing the module `bokeh` instead of member `__version__`
... 6210 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+3226 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/918f75fae54968cc804ca3396cef93dadeba6542/.github/workflows/algolia/configure-algolia-api.py#L1'>.github/workflows/algolia/configure-algolia-api.py:1:24:</a> RUF066 Prefer importing the module `__future__` instead of member `annotations`
+ <a href='https://github.com/ibis-project/ibis/blob/918f75fae54968cc804ca3396cef93dadeba6542/.github/workflows/algolia/configure-algolia-api.py#L5'>.github/workflows/algolia/configure-algolia-api.py:5:41:</a> RUF066 Prefer importing the module `algoliasearch.search_client` instead of member `SearchClient`
+ <a href='https://github.com/ibis-project/ibis/blob/918f75fae54968cc804ca3396cef93dadeba6542/.github/workflows/algolia/upload-algolia-api.py#L1'>.github/workflows/algolia/upload-algolia-api.py:1:24:</a> RUF066 Prefer importing the module `__future__` instead of member `annotations`
+ <a href='https://github.com/ibis-project/ibis/blob/918f75fae54968cc804ca3396cef93dadeba6542/.github/workflows/algolia/upload-algolia-api.py#L7'>.github/workflows/algolia/upload-algolia-api.py:7:23:</a> RUF066 Prefer importing the module `functools` instead of member `partial`
+ <a href='https://github.com/ibis-project/ibis/blob/918f75fae54968cc804ca3396cef93dadeba6542/.github/workflows/algolia/upload-algolia-api.py#L9'>.github/workflows/algolia/upload-algolia-api.py:9:41:</a> RUF066 Prefer importing the module `algoliasearch.search_client` instead of member `SearchClient`
+ <a href='https://github.com/ibis-project/ibis/blob/918f75fae54968cc804ca3396cef93dadeba6542/.github/workflows/algolia/upload-algolia.py#L1'>.github/workflows/algolia/upload-algolia.py:1:24:</a> RUF066 Prefer importing the module `__future__` instead of member `annotations`
+ <a href='https://github.com/ibis-project/ibis/blob/918f75fae54968cc804ca3396cef93dadeba6542/.github/workflows/algolia/upload-algolia.py#L5'>.github/workflows/algolia/upload-algolia.py:5:28:</a> RUF066 Prefer importing the module `urllib.request` instead of member `urlopen`
+ <a href='https://github.com/ibis-project/ibis/blob/918f75fae54968cc804ca3396cef93dadeba6542/.github/workflows/algolia/upload-algolia.py#L7'>.github/workflows/algolia/upload-algolia.py:7:41:</a> RUF066 Prefer importing the module `algoliasearch.search_client` instead of member `SearchClient`
+ <a href='https://github.com/ibis-project/ibis/blob/918f75fae54968cc804ca3396cef93dadeba6542/ci/make_geography_db.py#L16'>ci/make_geography_db.py:16:24:</a> RUF066 Prefer importing the module `__future__` instead of member `annotations`
+ <a href='https://github.com/ibis-project/ibis/blob/918f75fae54968cc804ca3396cef93dadeba6542/ci/make_geography_db.py#L21'>ci/make_geography_db.py:21:21:</a> RUF066 Prefer importing the module `pathlib` instead of member `Path`
... 3216 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+11148 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/78a2f86f70524a5aa660a36ee9c73d557f254050/libs/cli/langchain_cli/__init__.py#L3'>libs/cli/langchain_cli/__init__.py:3:36:</a> RUF066 Prefer importing the module `langchain_cli._version` instead of member `__version__`
+ <a href='https://github.com/langchain-ai/langchain/blob/78a2f86f70524a5aa660a36ee9c73d557f254050/libs/cli/langchain_cli/_version.py#L1'>libs/cli/langchain_cli/_version.py:1:23:</a> RUF066 Prefer importing the module `importlib` instead of member `metadata`
+ <a href='https://github.com/langchain-ai/langchain/blob/78a2f86f70524a5aa660a36ee9c73d557f254050/libs/cli/langchain_cli/cli.py#L10'>libs/cli/langchain_cli/cli.py:10:38:</a> RUF066 Prefer importing the module `langchain_cli.namespaces` instead of member `app`
+ <a href='https://github.com/langchain-ai/langchain/blob/78a2f86f70524a5aa660a36ee9c73d557f254050/libs/cli/langchain_cli/cli.py#L11'>libs/cli/langchain_cli/cli.py:11:38:</a> RUF066 Prefer importing the module `langchain_cli.namespaces` instead of member `integration`
+ <a href='https://github.com/langchain-ai/langchain/blob/78a2f86f70524a5aa660a36ee9c73d557f254050/libs/cli/langchain_cli/cli.py#L12'>libs/cli/langchain_cli/cli.py:12:38:</a> RUF066 Prefer importing the module `langchain_cli.namespaces` instead of member `template`
+ <a href='https://github.com/langchain-ai/langchain/blob/78a2f86f70524a5aa660a36ee9c73d557f254050/libs/cli/langchain_cli/cli.py#L13'>libs/cli/langchain_cli/cli.py:13:46:</a> RUF066 Prefer importing the module `langchain_cli.namespaces.migrate` instead of member `main`
+ <a href='https://github.com/langchain-ai/langchain/blob/78a2f86f70524a5aa660a36ee9c73d557f254050/libs/cli/langchain_cli/cli.py#L14'>libs/cli/langchain_cli/cli.py:14:42:</a> RUF066 Prefer importing the module `langchain_cli.utils.packages` instead of member `get_langserve_export`
+ <a href='https://github.com/langchain-ai/langchain/blob/78a2f86f70524a5aa660a36ee9c73d557f254050/libs/cli/langchain_cli/cli.py#L14'>libs/cli/langchain_cli/cli.py:14:64:</a> RUF066 Prefer importing the module `langchain_cli.utils.packages` instead of member `get_package_root`
+ <a href='https://github.com/langchain-ai/langchain/blob/78a2f86f70524a5aa660a36ee9c73d557f254050/libs/cli/langchain_cli/cli.py#L3'>libs/cli/langchain_cli/cli.py:3:24:</a> RUF066 Prefer importing the module `__future__` instead of member `annotations`
+ <a href='https://github.com/langchain-ai/langchain/blob/78a2f86f70524a5aa660a36ee9c73d557f254050/libs/cli/langchain_cli/cli.py#L9'>libs/cli/langchain_cli/cli.py:9:36:</a> RUF066 Prefer importing the module `langchain_cli._version` instead of member `__version__`
... 11138 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF066 | 186776 | 186776 | 0 | 0 | 0 |
| AIR311 | 224 | 0 | 224 | 0 | 0 |
| AIR301 | 3 | 0 | 3 | 0 | 0 |

</p>
</details>




---

_Closed by @MichaReiser on 2025-10-29 15:36_

---

_@justinchuby reviewed on 2025-10-29 16:50_

---

_Review comment by @justinchuby on `crates/ruff_linter/resources/test/fixtures/ruff/RUF066.py`:9 on 2025-10-29 16:50_

Looks like `__future__` needs to be included

---

_@revmischa reviewed on 2025-10-29 17:10_

---

_Review comment by @revmischa on `crates/ruff_linter/resources/test/fixtures/ruff/RUF066.py`:9 on 2025-10-29 17:10_

I don't mind to continue working on this if it's something that could get accepted one day but the PR is closed for now

---

_@justinchuby reviewed on 2025-10-29 17:19_

---

_Review comment by @justinchuby on `crates/ruff_linter/resources/test/fixtures/ruff/RUF066.py`:9 on 2025-10-29 17:19_

Yeah a bit disappointing to see

---
