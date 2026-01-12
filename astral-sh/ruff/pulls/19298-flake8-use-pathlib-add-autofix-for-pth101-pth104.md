```yaml
number: 19298
title: "[`flake8-use-pathlib`] Add autofix for `PTH101`, `PTH104`, `PTH105`, `PTH121`"
type: pull_request
state: closed
author: chirizxc
labels:
  - fixes
  - preview
assignees: []
base: main
head: feat/autofixes
created_at: 2025-07-12T06:58:37Z
updated_at: 2025-07-17T16:09:32Z
url: https://github.com/astral-sh/ruff/pull/19298
synced_at: 2026-01-12T15:56:36Z
```

# [`flake8-use-pathlib`] Add autofix for `PTH101`, `PTH104`, `PTH105`, `PTH121`

---

_@chirizxc_

## Summary
Part of https://github.com/astral-sh/ruff/issues/2331

## Test Plan

`cargo nextest run flake8_use_pathlib`


---

_Comment by @chirizxc on 2025-07-12 07:03_

most test in [`full_name.py`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/resources/test/fixtures/flake8_use_pathlib/full_name.py#L11) return `TypeError: rename() missing required argument 'dst' (pos 2)`

---

_Comment by @github-actions[bot] on 2025-07-12 07:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +48 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +30 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/a24962c84ddbdb5887853cace240a0821b733e15/airflow-core/src/airflow/configuration.py#L2131'>airflow-core/src/airflow/configuration.py:2131:9:</a> PTH101 [*] `os.chmod()` should be replaced by `Path.chmod()`
- <a href='https://github.com/apache/airflow/blob/a24962c84ddbdb5887853cace240a0821b733e15/airflow-core/src/airflow/configuration.py#L2131'>airflow-core/src/airflow/configuration.py:2131:9:</a> PTH101 `os.chmod()` should be replaced by `Path.chmod()`
+ <a href='https://github.com/apache/airflow/blob/a24962c84ddbdb5887853cace240a0821b733e15/airflow-core/src/airflow/dag_processing/bundles/base.py#L391'>airflow-core/src/airflow/dag_processing/bundles/base.py:391:13:</a> PTH105 [*] `os.replace()` should be replaced by `Path.replace()`
- <a href='https://github.com/apache/airflow/blob/a24962c84ddbdb5887853cace240a0821b733e15/airflow-core/src/airflow/dag_processing/bundles/base.py#L391'>airflow-core/src/airflow/dag_processing/bundles/base.py:391:13:</a> PTH105 `os.replace()` should be replaced by `Path.replace()`
+ <a href='https://github.com/apache/airflow/blob/a24962c84ddbdb5887853cace240a0821b733e15/airflow-core/src/airflow/utils/log/file_task_handler.py#L845'>airflow-core/src/airflow/utils/log/file_task_handler.py:845:17:</a> PTH101 [*] `os.chmod()` should be replaced by `Path.chmod()`
- <a href='https://github.com/apache/airflow/blob/a24962c84ddbdb5887853cace240a0821b733e15/airflow-core/src/airflow/utils/log/file_task_handler.py#L845'>airflow-core/src/airflow/utils/log/file_task_handler.py:845:17:</a> PTH101 `os.chmod()` should be replaced by `Path.chmod()`
+ <a href='https://github.com/apache/airflow/blob/a24962c84ddbdb5887853cace240a0821b733e15/airflow-core/tests/unit/core/test_impersonation_tests.py#L71'>airflow-core/tests/unit/core/test_impersonation_tests.py:71:17:</a> PTH101 [*] `os.chmod()` should be replaced by `Path.chmod()`
- <a href='https://github.com/apache/airflow/blob/a24962c84ddbdb5887853cace240a0821b733e15/airflow-core/tests/unit/core/test_impersonation_tests.py#L71'>airflow-core/tests/unit/core/test_impersonation_tests.py:71:17:</a> PTH101 `os.chmod()` should be replaced by `Path.chmod()`
+ <a href='https://github.com/apache/airflow/blob/a24962c84ddbdb5887853cace240a0821b733e15/airflow-core/tests/unit/core/test_impersonation_tests.py#L80'>airflow-core/tests/unit/core/test_impersonation_tests.py:80:21:</a> PTH101 [*] `os.chmod()` should be replaced by `Path.chmod()`
- <a href='https://github.com/apache/airflow/blob/a24962c84ddbdb5887853cace240a0821b733e15/airflow-core/tests/unit/core/test_impersonation_tests.py#L80'>airflow-core/tests/unit/core/test_impersonation_tests.py:80:21:</a> PTH101 `os.chmod()` should be replaced by `Path.chmod()`
+ <a href='https://github.com/apache/airflow/blob/a24962c84ddbdb5887853cace240a0821b733e15/airflow-core/tests/unit/core/test_impersonation_tests.py#L87'>airflow-core/tests/unit/core/test_impersonation_tests.py:87:13:</a> PTH101 [*] `os.chmod()` should be replaced by `Path.chmod()`
- <a href='https://github.com/apache/airflow/blob/a24962c84ddbdb5887853cace240a0821b733e15/airflow-core/tests/unit/core/test_impersonation_tests.py#L87'>airflow-core/tests/unit/core/test_impersonation_tests.py:87:13:</a> PTH101 `os.chmod()` should be replaced by `Path.chmod()`
+ <a href='https://github.com/apache/airflow/blob/a24962c84ddbdb5887853cace240a0821b733e15/dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py#L104'>dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py:104:13:</a> PTH101 [*] `os.chmod()` should be replaced by `Path.chmod()`
... 14 additional changes omitted for rule PTH101
+ <a href='https://github.com/apache/airflow/blob/a24962c84ddbdb5887853cace240a0821b733e15/dev/breeze/src/airflow_breeze/utils/reproducible.py#L144'>dev/breeze/src/airflow_breeze/utils/reproducible.py:144:9:</a> PTH104 [*] `os.rename()` should be replaced by `Path.rename()`
- <a href='https://github.com/apache/airflow/blob/a24962c84ddbdb5887853cace240a0821b733e15/dev/breeze/src/airflow_breeze/utils/reproducible.py#L144'>dev/breeze/src/airflow_breeze/utils/reproducible.py:144:9:</a> PTH104 `os.rename()` should be replaced by `Path.rename()`
+ <a href='https://github.com/apache/airflow/blob/a24962c84ddbdb5887853cace240a0821b733e15/scripts/in_container/run_migration_reference.py#L212'>scripts/in_container/run_migration_reference.py:212:9:</a> PTH104 [*] `os.rename()` should be replaced by `Path.rename()`
- <a href='https://github.com/apache/airflow/blob/a24962c84ddbdb5887853cace240a0821b733e15/scripts/in_container/run_migration_reference.py#L212'>scripts/in_container/run_migration_reference.py:212:9:</a> PTH104 `os.rename()` should be replaced by `Path.rename()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/848eccdfc5401cc674edf70c86c5512adc4b0699/src/latch_cli/utils/__init__.py#L278'>src/latch_cli/utils/__init__.py:278:9:</a> PTH101 [*] `os.chmod()` should be replaced by `Path.chmod()`
- <a href='https://github.com/latchbio/latch/blob/848eccdfc5401cc674edf70c86c5512adc4b0699/src/latch_cli/utils/__init__.py#L278'>src/latch_cli/utils/__init__.py:278:9:</a> PTH101 `os.chmod()` should be replaced by `Path.chmod()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +16 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/3fecbe41c52d1a3f0b11b99a6c4017e4afaa4aa0/scripts/lib/zulip_tools.py#L60'>scripts/lib/zulip_tools.py:60:9:</a> PTH104 [*] `os.rename()` should be replaced by `Path.rename()`
- <a href='https://github.com/zulip/zulip/blob/3fecbe41c52d1a3f0b11b99a6c4017e4afaa4aa0/scripts/lib/zulip_tools.py#L60'>scripts/lib/zulip_tools.py:60:9:</a> PTH104 `os.rename()` should be replaced by `Path.rename()`
+ <a href='https://github.com/zulip/zulip/blob/3fecbe41c52d1a3f0b11b99a6c4017e4afaa4aa0/scripts/lib/zulip_tools.py#L730'>scripts/lib/zulip_tools.py:730:5:</a> PTH104 [*] `os.rename()` should be replaced by `Path.rename()`
- <a href='https://github.com/zulip/zulip/blob/3fecbe41c52d1a3f0b11b99a6c4017e4afaa4aa0/scripts/lib/zulip_tools.py#L730'>scripts/lib/zulip_tools.py:730:5:</a> PTH104 `os.rename()` should be replaced by `Path.rename()`
+ <a href='https://github.com/zulip/zulip/blob/3fecbe41c52d1a3f0b11b99a6c4017e4afaa4aa0/tools/lib/provision_inner.py#L158'>tools/lib/provision_inner.py:158:9:</a> PTH104 [*] `os.rename()` should be replaced by `Path.rename()`
- <a href='https://github.com/zulip/zulip/blob/3fecbe41c52d1a3f0b11b99a6c4017e4afaa4aa0/tools/lib/provision_inner.py#L158'>tools/lib/provision_inner.py:158:9:</a> PTH104 `os.rename()` should be replaced by `Path.rename()`
+ <a href='https://github.com/zulip/zulip/blob/3fecbe41c52d1a3f0b11b99a6c4017e4afaa4aa0/zerver/management/commands/export_usermessage_batch.py#L46'>zerver/management/commands/export_usermessage_batch.py:46:17:</a> PTH104 [*] `os.rename()` should be replaced by `Path.rename()`
- <a href='https://github.com/zulip/zulip/blob/3fecbe41c52d1a3f0b11b99a6c4017e4afaa4aa0/zerver/management/commands/export_usermessage_batch.py#L46'>zerver/management/commands/export_usermessage_batch.py:46:17:</a> PTH104 `os.rename()` should be replaced by `Path.rename()`
+ <a href='https://github.com/zulip/zulip/blob/3fecbe41c52d1a3f0b11b99a6c4017e4afaa4aa0/zerver/management/commands/export_usermessage_batch.py#L60'>zerver/management/commands/export_usermessage_batch.py:60:17:</a> PTH104 [*] `os.rename()` should be replaced by `Path.rename()`
- <a href='https://github.com/zulip/zulip/blob/3fecbe41c52d1a3f0b11b99a6c4017e4afaa4aa0/zerver/management/commands/export_usermessage_batch.py#L60'>zerver/management/commands/export_usermessage_batch.py:60:17:</a> PTH104 `os.rename()` should be replaced by `Path.rename()`
+ <a href='https://github.com/zulip/zulip/blob/3fecbe41c52d1a3f0b11b99a6c4017e4afaa4aa0/zerver/management/commands/fetch_tor_exit_nodes.py#L70'>zerver/management/commands/fetch_tor_exit_nodes.py:70:9:</a> PTH104 [*] `os.rename()` should be replaced by `Path.rename()`
- <a href='https://github.com/zulip/zulip/blob/3fecbe41c52d1a3f0b11b99a6c4017e4afaa4aa0/zerver/management/commands/fetch_tor_exit_nodes.py#L70'>zerver/management/commands/fetch_tor_exit_nodes.py:70:9:</a> PTH104 `os.rename()` should be replaced by `Path.rename()`
+ <a href='https://github.com/zulip/zulip/blob/3fecbe41c52d1a3f0b11b99a6c4017e4afaa4aa0/zerver/tornado/event_queue.py#L714'>zerver/tornado/event_queue.py:714:9:</a> PTH104 [*] `os.rename()` should be replaced by `Path.rename()`
- <a href='https://github.com/zulip/zulip/blob/3fecbe41c52d1a3f0b11b99a6c4017e4afaa4aa0/zerver/tornado/event_queue.py#L714'>zerver/tornado/event_queue.py:714:9:</a> PTH104 `os.rename()` should be replaced by `Path.rename()`
+ <a href='https://github.com/zulip/zulip/blob/3fecbe41c52d1a3f0b11b99a6c4017e4afaa4aa0/zerver/worker/base.py#L151'>zerver/worker/base.py:151:13:</a> PTH104 [*] `os.rename()` should be replaced by `Path.rename()`
- <a href='https://github.com/zulip/zulip/blob/3fecbe41c52d1a3f0b11b99a6c4017e4afaa4aa0/zerver/worker/base.py#L151'>zerver/worker/base.py:151:13:</a> PTH104 `os.rename()` should be replaced by `Path.rename()`
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PTH101 | 26 | 0 | 0 | 26 | 0 |
| PTH104 | 20 | 0 | 0 | 20 | 0 |
| PTH105 | 2 | 0 | 0 | 2 | 0 |

</p>
</details>




---

_Label `fixes` added by @MichaReiser on 2025-07-14 09:14_

---

_Label `preview` added by @MichaReiser on 2025-07-14 09:14_

---

_Review requested from @ntBre by @MichaReiser on 2025-07-14 09:14_

---

_Comment by @chirizxc on 2025-07-14 17:01_

@ntBre, this PR breaks past behavior when it did diagnostics even for functions with one argument, I don't think this behavior was correct

---

_Closed by @chirizxc on 2025-07-17 16:09_

---
