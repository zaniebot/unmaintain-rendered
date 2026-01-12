```yaml
number: 12837
title: Deprecate PT004 and PT005
type: pull_request
state: merged
author: MichaReiser
labels:
  - breaking
assignees: []
merged: true
base: ruff-0.6
head: deprecate-pt004-pt005
created_at: 2024-08-12T09:49:35Z
updated_at: 2024-08-12T14:18:21Z
url: https://github.com/astral-sh/ruff/pull/12837
synced_at: 2026-01-12T15:55:42Z
```

# Deprecate PT004 and PT005

---

_@MichaReiser_

## Summary

Deprecates the pytest rules PT004 and PT005. 

See https://github.com/astral-sh/ruff/issues/8796





---

_@MichaReiser reviewed on 2024-08-12 09:50_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:175 on 2024-08-12 09:50_

If someone more familiar with pytest has a better explanation, please comment.

---

_Label `breaking` added by @MichaReiser on 2024-08-12 09:50_

---

_Comment by @github-actions[bot] on 2024-08-12 10:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -356 violations, +0 -0 fixes in 3 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -316 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/helm_tests/airflow_aux/test_pod_template_file.py#L48'>helm_tests/airflow_aux/test_pod_template_file.py:48:9:</a> PT004 Fixture `setup_test_cases` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/kubernetes_tests/conftest.py#L27'>kubernetes_tests/conftest.py:27:5:</a> PT004 Fixture `initialize_providers_manager` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/kubernetes_tests/test_base.py#L58'>kubernetes_tests/test_base.py:58:9:</a> PT004 Fixture `base_tests_setup` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/kubernetes_tests/test_kubernetes_pod_operator.py#L89'>kubernetes_tests/test_kubernetes_pod_operator.py:89:5:</a> PT004 Fixture `mock_get_connection` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/kubernetes_tests/test_kubernetes_pod_operator.py#L98'>kubernetes_tests/test_kubernetes_pod_operator.py:98:9:</a> PT004 Fixture `setup_tests` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/always/test_pandas.py#L31'>tests/always/test_pandas.py:31:9:</a> PT004 Fixture `setup_test_cases` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/always/test_pandas.py#L48'>tests/always/test_pandas.py:48:9:</a> PT004 Fixture `setup_test_cases` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/always/test_providers_manager.py#L57'>tests/always/test_providers_manager.py:57:9:</a> PT004 Fixture `inject_fixtures` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/conftest.py#L64'>tests/api_connexion/conftest.py:64:5:</a> PT004 Fixture `set_auto_role_public` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_config_endpoint.py#L238'>tests/api_connexion/endpoints/test_config_endpoint.py:238:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_config_endpoint.py#L70'>tests/api_connexion/endpoints/test_config_endpoint.py:70:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_connection_endpoint.py#L61'>tests/api_connexion/endpoints/test_connection_endpoint.py:61:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_dag_endpoint.py#L122'>tests/api_connexion/endpoints/test_dag_endpoint.py:122:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_dag_parsing.py#L67'>tests/api_connexion/endpoints/test_dag_parsing.py:67:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_dag_run_endpoint.py#L132'>tests/api_connexion/endpoints/test_dag_run_endpoint.py:132:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_dag_source_endpoint.py#L74'>tests/api_connexion/endpoints/test_dag_source_endpoint.py:74:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_dag_stats_endpoint.py#L62'>tests/api_connexion/endpoints/test_dag_stats_endpoint.py:62:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_dag_warning_endpoint.py#L67'>tests/api_connexion/endpoints/test_dag_warning_endpoint.py:67:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_dataset_endpoint.py#L655'>tests/api_connexion/endpoints/test_dataset_endpoint.py:655:9:</a> PT004 Fixture `time_freezer` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_dataset_endpoint.py#L835'>tests/api_connexion/endpoints/test_dataset_endpoint.py:835:9:</a> PT004 Fixture `time_freezer` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_dataset_endpoint.py#L84'>tests/api_connexion/endpoints/test_dataset_endpoint.py:84:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_event_log_endpoint.py#L102'>tests/api_connexion/endpoints/test_event_log_endpoint.py:102:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_extra_link_endpoint.py#L67'>tests/api_connexion/endpoints/test_extra_link_endpoint.py:67:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
... 293 additional changes omitted for rule PT004
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/providers/ssh/operators/test_ssh.py#L68'>tests/providers/ssh/operators/test_ssh.py:68:9:</a> PT005 Fixture `_patch_exec_ssh_client` returns a value, remove leading underscore
... 292 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -40 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/celery_tests.py#L71'>tests/integration_tests/celery_tests.py:71:5:</a> PT004 Fixture `setup_sqllab` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/charts/api_tests.py#L1274'>tests/integration_tests/charts/api_tests.py:1274:9:</a> PT004 Fixture `load_energy_charts` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/charts/api_tests.py#L89'>tests/integration_tests/charts/api_tests.py:89:9:</a> PT004 Fixture `clear_data_cache` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/charts/data/api_tests.py#L93'>tests/integration_tests/charts/data/api_tests.py:93:5:</a> PT004 Fixture `skip_by_backend` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/conftest.py#L120'>tests/integration_tests/conftest.py:120:5:</a> PT004 Fixture `setup_sample_data` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/core_tests.py#L79'>tests/integration_tests/core_tests.py:79:5:</a> PT004 Fixture `cleanup` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/dashboards/filter_state/api_tests.py#L57'>tests/integration_tests/dashboards/filter_state/api_tests.py:57:5:</a> PT004 Fixture `cache` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/explore/api_tests.py#L66'>tests/integration_tests/explore/api_tests.py:66:5:</a> PT004 Fixture `cache` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/explore/form_data/api_tests.py#L68'>tests/integration_tests/explore/form_data/api_tests.py:68:5:</a> PT004 Fixture `cache` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/fixtures/birth_names_dashboard.py#L37'>tests/integration_tests/fixtures/birth_names_dashboard.py:37:5:</a> PT004 Fixture `load_birth_names_data` does not return anything, add leading underscore
... 30 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/agronholm/anyio">agronholm/anyio</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT004 | 355 | 0 | 355 | 0 | 0 |
| PT005 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -356 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -316 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/helm_tests/airflow_aux/test_pod_template_file.py#L48'>helm_tests/airflow_aux/test_pod_template_file.py:48:9:</a> PT004 Fixture `setup_test_cases` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/kubernetes_tests/conftest.py#L27'>kubernetes_tests/conftest.py:27:5:</a> PT004 Fixture `initialize_providers_manager` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/kubernetes_tests/test_base.py#L58'>kubernetes_tests/test_base.py:58:9:</a> PT004 Fixture `base_tests_setup` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/kubernetes_tests/test_kubernetes_pod_operator.py#L89'>kubernetes_tests/test_kubernetes_pod_operator.py:89:5:</a> PT004 Fixture `mock_get_connection` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/kubernetes_tests/test_kubernetes_pod_operator.py#L98'>kubernetes_tests/test_kubernetes_pod_operator.py:98:9:</a> PT004 Fixture `setup_tests` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/always/test_pandas.py#L31'>tests/always/test_pandas.py:31:9:</a> PT004 Fixture `setup_test_cases` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/always/test_pandas.py#L48'>tests/always/test_pandas.py:48:9:</a> PT004 Fixture `setup_test_cases` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/always/test_providers_manager.py#L57'>tests/always/test_providers_manager.py:57:9:</a> PT004 Fixture `inject_fixtures` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/conftest.py#L64'>tests/api_connexion/conftest.py:64:5:</a> PT004 Fixture `set_auto_role_public` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_config_endpoint.py#L238'>tests/api_connexion/endpoints/test_config_endpoint.py:238:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_config_endpoint.py#L70'>tests/api_connexion/endpoints/test_config_endpoint.py:70:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_connection_endpoint.py#L61'>tests/api_connexion/endpoints/test_connection_endpoint.py:61:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_dag_endpoint.py#L122'>tests/api_connexion/endpoints/test_dag_endpoint.py:122:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_dag_parsing.py#L67'>tests/api_connexion/endpoints/test_dag_parsing.py:67:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_dag_run_endpoint.py#L132'>tests/api_connexion/endpoints/test_dag_run_endpoint.py:132:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_dag_source_endpoint.py#L74'>tests/api_connexion/endpoints/test_dag_source_endpoint.py:74:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_dag_stats_endpoint.py#L62'>tests/api_connexion/endpoints/test_dag_stats_endpoint.py:62:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_dag_warning_endpoint.py#L67'>tests/api_connexion/endpoints/test_dag_warning_endpoint.py:67:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_dataset_endpoint.py#L655'>tests/api_connexion/endpoints/test_dataset_endpoint.py:655:9:</a> PT004 Fixture `time_freezer` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_dataset_endpoint.py#L835'>tests/api_connexion/endpoints/test_dataset_endpoint.py:835:9:</a> PT004 Fixture `time_freezer` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_dataset_endpoint.py#L84'>tests/api_connexion/endpoints/test_dataset_endpoint.py:84:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/api_connexion/endpoints/test_event_log_endpoint.py#L102'>tests/api_connexion/endpoints/test_event_log_endpoint.py:102:9:</a> PT004 Fixture `setup_attrs` does not return anything, add leading underscore
... 294 additional changes omitted for rule PT004
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/providers/ssh/operators/test_ssh.py#L68'>tests/providers/ssh/operators/test_ssh.py:68:9:</a> PT005 Fixture `_patch_exec_ssh_client` returns a value, remove leading underscore
... 293 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -40 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/celery_tests.py#L71'>tests/integration_tests/celery_tests.py:71:5:</a> PT004 Fixture `setup_sqllab` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/charts/api_tests.py#L1274'>tests/integration_tests/charts/api_tests.py:1274:9:</a> PT004 Fixture `load_energy_charts` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/charts/api_tests.py#L89'>tests/integration_tests/charts/api_tests.py:89:9:</a> PT004 Fixture `clear_data_cache` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/charts/data/api_tests.py#L93'>tests/integration_tests/charts/data/api_tests.py:93:5:</a> PT004 Fixture `skip_by_backend` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/conftest.py#L120'>tests/integration_tests/conftest.py:120:5:</a> PT004 Fixture `setup_sample_data` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/core_tests.py#L79'>tests/integration_tests/core_tests.py:79:5:</a> PT004 Fixture `cleanup` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/dashboards/filter_state/api_tests.py#L57'>tests/integration_tests/dashboards/filter_state/api_tests.py:57:5:</a> PT004 Fixture `cache` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/explore/api_tests.py#L66'>tests/integration_tests/explore/api_tests.py:66:5:</a> PT004 Fixture `cache` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/explore/form_data/api_tests.py#L68'>tests/integration_tests/explore/form_data/api_tests.py:68:5:</a> PT004 Fixture `cache` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/57a4199f527c6bf197245f313ee28e0734c60368/tests/integration_tests/fixtures/birth_names_dashboard.py#L37'>tests/integration_tests/fixtures/birth_names_dashboard.py:37:5:</a> PT004 Fixture `load_birth_names_data` does not return anything, add leading underscore
... 30 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/molecule/testinfra/ossec/test_journalist_mail.py#L14'>molecule/testinfra/ossec/test_journalist_mail.py:14:45:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PT004`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/3c6df26a10bf76e7fdfc45bdf3e21a8e947a75ff/rotkehlchen/tests/fixtures/globaldb.py#L200'>rotkehlchen/tests/fixtures/globaldb.py:200:52:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PT004`)
+ <a href='https://github.com/rotki/rotki/blob/3c6df26a10bf76e7fdfc45bdf3e21a8e947a75ff/rotkehlchen/tests/fixtures/oracles.py#L20'>rotkehlchen/tests/fixtures/oracles.py:20:118:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PT004`)
+ <a href='https://github.com/rotki/rotki/blob/3c6df26a10bf76e7fdfc45bdf3e21a8e947a75ff/rotkehlchen/tests/fixtures/thegraph.py#L12'>rotkehlchen/tests/fixtures/thegraph.py:12:67:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PT004`)
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT004 | 355 | 0 | 355 | 0 | 0 |
| RUF100 | 4 | 4 | 0 | 0 | 0 |
| PT005 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:175 on 2024-08-12 11:21_

This seems fine to me, though the line's a little long ;)

```suggestion
/// ## Deprecation
/// Marking fixtures that do not return a value with an underscore
/// isn't a practice recommended by the pytest community.
///
```

I'd also remove "By convention, " from the beginning of the "Why is this bad?" paragraphs. It doesn't seem like there has ever been a convention supporting these rules.

```diff
diff --git a/crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs b/crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs
index 7272b78bd..628992b58 100644
--- a/crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs
+++ b/crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs
@@ -178,7 +178,7 @@ impl AlwaysFixableViolation for PytestExtraneousScopeFunction {
 /// with a leading underscore.
 ///
 /// ## Why is this bad?
-/// By convention, fixtures that don't return a value should be named with a
+/// Fixtures that don't return a value should be named with a
 /// leading underscore, while fixtures that do return a value should not.
 ///
 /// This rule ignores abstract fixtures and generators.
@@ -238,7 +238,7 @@ impl Violation for PytestMissingFixtureNameUnderscore {
 /// leading underscore.
 ///
 /// ## Why is this bad?
-/// By convention, fixtures that don't return a value should be named with a
+/// Fixtures that don't return a value should be named with a
 /// leading underscore, while fixtures that do return a value should not.
 ///
 /// This rule ignores abstract fixtures.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:234 on 2024-08-12 11:22_

```suggestion
/// Marking fixtures that do not return a value with an underscore
/// isn't a practice recommended by the pytest community.
```

---

_@AlexWaygood approved on 2024-08-12 11:22_

---

_@MichaReiser reviewed on 2024-08-12 14:04_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:175 on 2024-08-12 14:04_

I prefer to keep the description as is. By convention doesn't mean that it has to be a blessed convention ;) It can be your personal convention. I also don't see the point of improving the documentation of a deprecated rule.

---

_@AlexWaygood reviewed on 2024-08-12 14:11_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:175 on 2024-08-12 14:11_

> I prefer to keep the description as is. By convention doesn't mean that it has to be a blessed convention ;) It can be your personal convention. I also don't see the point of improving the documentation of a deprecated rule.

While that's strictly-speaking true, that seems like quite a legalistic interpretation to me :P that's not how I understood the phrase in the docs, and I doubt it's how many of our users would understand it either.

> I also don't see the point of improving the documentation of a deprecated rule.

Hmm, that logic seems circular to me. Users might not know that it's deprecated (or why it's deprecated) until they read the documentation that tells them about the rule. I think documentation is still really important here so that we can clearly convey why we no longer endorse the rule's recommendations

---

_Merged by @MichaReiser on 2024-08-12 14:13_

---

_Closed by @MichaReiser on 2024-08-12 14:13_

---

_Branch deleted on 2024-08-12 14:13_

---
