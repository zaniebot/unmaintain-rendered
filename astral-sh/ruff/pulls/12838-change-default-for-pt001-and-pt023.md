```yaml
number: 12838
title: Change default for PT001 and PT023
type: pull_request
state: merged
author: MichaReiser
labels:
  - breaking
  - configuration
assignees: []
merged: true
base: ruff-0.6
head: change-default-for-pt001-pt023
created_at: 2024-08-12T10:08:36Z
updated_at: 2024-08-21T01:32:00Z
url: https://github.com/astral-sh/ruff/pull/12838
synced_at: 2026-01-12T15:55:42Z
```

# Change default for PT001 and PT023

---

_@MichaReiser_

## Summary

This PR promotes the changed defaults for PT001 and PT023 to not require parentheses for fixtures and marker decorators without arguments to stable. 


The default were changed in preview mode back in June (https://github.com/astral-sh/ruff/pull/12106). 

See https://github.com/astral-sh/ruff/issues/8796

## Test Plan

Updated snapshot tests


---

_Label `breaking` added by @MichaReiser on 2024-08-12 10:08_

---

_Label `configuration` added by @MichaReiser on 2024-08-12 10:08_

---

_Added to milestone `v0.6` by @MichaReiser on 2024-08-12 10:08_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:934 on 2024-08-12 10:10_

I think that was a bug. It's unclear to me why this rule would depend on enabling that setting.

---

_@MichaReiser reviewed on 2024-08-12 10:10_

---

_Comment by @github-actions[bot] on 2024-08-12 10:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+355 -957 violations, +2006 -0 fixes in 10 projects; 44 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+93 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/tests/analysis/test_nullpoint.py#L241'>tests/analysis/test_nullpoint.py:241:1:</a> PT023 [*] Use `@pytest.mark.slow` over `@pytest.mark.slow()`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/tests/analysis/test_nullpoint.py#L258'>tests/analysis/test_nullpoint.py:258:1:</a> PT023 [*] Use `@pytest.mark.slow` over `@pytest.mark.slow()`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/tests/analysis/test_nullpoint.py#L292'>tests/analysis/test_nullpoint.py:292:1:</a> PT023 [*] Use `@pytest.mark.slow` over `@pytest.mark.slow()`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/tests/analysis/test_nullpoint.py#L311'>tests/analysis/test_nullpoint.py:311:1:</a> PT023 [*] Use `@pytest.mark.slow` over `@pytest.mark.slow()`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/tests/analysis/test_nullpoint.py#L345'>tests/analysis/test_nullpoint.py:345:1:</a> PT023 [*] Use `@pytest.mark.slow` over `@pytest.mark.slow()`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/tests/analysis/test_nullpoint.py#L363'>tests/analysis/test_nullpoint.py:363:1:</a> PT023 [*] Use `@pytest.mark.slow` over `@pytest.mark.slow()`
... 55 additional changes omitted for rule PT023
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/tests/diagnostics/charged_particle_radiography/test_detector_stacks.py#L24'>tests/diagnostics/charged_particle_radiography/test_detector_stacks.py:24:1:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/tests/diagnostics/charged_particle_radiography/test_detector_stacks.py#L31'>tests/diagnostics/charged_particle_radiography/test_detector_stacks.py:31:1:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/tests/diagnostics/test_langmuir.py#L144'>tests/diagnostics/test_langmuir.py:144:1:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/tests/diagnostics/test_langmuir.py#L152'>tests/diagnostics/test_langmuir.py:152:1:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
... 83 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -316 violations, +1220 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/__init__.py#L108'>airflow/__init__.py:108:5:</a> RET506 Unnecessary `elif` after `raise` statement
+ <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/__init__.py#L108'>airflow/__init__.py:108:5:</a> RET506 [*] Unnecessary `elif` after `raise` statement
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/api/auth/backend/kerberos_auth.py#L128'>airflow/api/auth/backend/kerberos_auth.py:128:9:</a> RET505 Unnecessary `elif` after `return` statement
+ <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/api/auth/backend/kerberos_auth.py#L128'>airflow/api/auth/backend/kerberos_auth.py:128:9:</a> RET505 [*] Unnecessary `elif` after `return` statement
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/api/auth/backend/kerberos_auth.py#L165'>airflow/api/auth/backend/kerberos_auth.py:165:13:</a> RET505 Unnecessary `elif` after `return` statement
+ <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/api/auth/backend/kerberos_auth.py#L165'>airflow/api/auth/backend/kerberos_auth.py:165:13:</a> RET505 [*] Unnecessary `elif` after `return` statement
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/api_connexion/endpoints/config_endpoint.py#L119'>airflow/api_connexion/endpoints/config_endpoint.py:119:5:</a> RET505 Unnecessary `elif` after `return` statement
+ <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/api_connexion/endpoints/config_endpoint.py#L119'>airflow/api_connexion/endpoints/config_endpoint.py:119:5:</a> RET505 [*] Unnecessary `elif` after `return` statement
... 923 additional changes omitted for rule RET505
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L47'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:47:9:</a> RET506 Unnecessary `else` after `raise` statement
+ <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L47'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:47:9:</a> RET506 [*] Unnecessary `else` after `raise` statement
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/cli/commands/dag_command.py#L281'>airflow/cli/commands/dag_command.py:281:5:</a> RET506 Unnecessary `elif` after `raise` statement
+ <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/cli/commands/dag_command.py#L281'>airflow/cli/commands/dag_command.py:281:5:</a> RET506 [*] Unnecessary `elif` after `raise` statement
... 255 additional changes omitted for rule RET506
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/dag_processing/manager.py#L586'>airflow/dag_processing/manager.py:586:21:</a> RET508 Unnecessary `elif` after `break` statement
+ <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/dag_processing/manager.py#L586'>airflow/dag_processing/manager.py:586:21:</a> RET508 [*] Unnecessary `elif` after `break` statement
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/jobs/scheduler_job_runner.py#L585'>airflow/jobs/scheduler_job_runner.py:585:21:</a> RET507 Unnecessary `else` after `continue` statement
+ <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/jobs/scheduler_job_runner.py#L585'>airflow/jobs/scheduler_job_runner.py:585:21:</a> RET507 [*] Unnecessary `else` after `continue` statement
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/models/xcom_arg.py#L655'>airflow/models/xcom_arg.py:655:13:</a> RET508 Unnecessary `elif` after `break` statement
+ <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/models/xcom_arg.py#L655'>airflow/models/xcom_arg.py:655:13:</a> RET508 [*] Unnecessary `elif` after `break` statement
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/providers/amazon/aws/hooks/sagemaker.py#L1207'>airflow/providers/amazon/aws/hooks/sagemaker.py:1207:21:</a> RET508 Unnecessary `else` after `break` statement
+ <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/providers/amazon/aws/hooks/sagemaker.py#L1207'>airflow/providers/amazon/aws/hooks/sagemaker.py:1207:21:</a> RET508 [*] Unnecessary `else` after `break` statement
... 5 additional changes omitted for rule RET508
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/providers/amazon/aws/secrets/secrets_manager.py#L233'>airflow/providers/amazon/aws/secrets/secrets_manager.py:233:13:</a> RET507 Unnecessary `elif` after `continue` statement
+ <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/providers/amazon/aws/secrets/secrets_manager.py#L233'>airflow/providers/amazon/aws/secrets/secrets_manager.py:233:13:</a> RET507 [*] Unnecessary `elif` after `continue` statement
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py#L627'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor.py:627:13:</a> RET507 Unnecessary `elif` after `continue` statement
... 1513 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+71 -129 violations, +24 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/c016ca5ad977db0eb2e0a306827d584609936e33/RELEASING/verify_release.py#L52'>RELEASING/verify_release.py:52:5:</a> RET505 Unnecessary `else` after `return` statement
+ <a href='https://github.com/apache/superset/blob/c016ca5ad977db0eb2e0a306827d584609936e33/RELEASING/verify_release.py#L52'>RELEASING/verify_release.py:52:5:</a> RET505 [*] Unnecessary `else` after `return` statement
- <a href='https://github.com/apache/superset/blob/c016ca5ad977db0eb2e0a306827d584609936e33/RELEASING/verify_release.py#L93'>RELEASING/verify_release.py:93:9:</a> RET505 Unnecessary `elif` after `return` statement
+ <a href='https://github.com/apache/superset/blob/c016ca5ad977db0eb2e0a306827d584609936e33/RELEASING/verify_release.py#L93'>RELEASING/verify_release.py:93:9:</a> RET505 [*] Unnecessary `elif` after `return` statement
- <a href='https://github.com/apache/superset/blob/c016ca5ad977db0eb2e0a306827d584609936e33/scripts/build_docker.py#L67'>scripts/build_docker.py:67:5:</a> RET505 Unnecessary `elif` after `return` statement
+ <a href='https://github.com/apache/superset/blob/c016ca5ad977db0eb2e0a306827d584609936e33/scripts/build_docker.py#L67'>scripts/build_docker.py:67:5:</a> RET505 [*] Unnecessary `elif` after `return` statement
... 11 additional changes omitted for rule RET505
- <a href='https://github.com/apache/superset/blob/c016ca5ad977db0eb2e0a306827d584609936e33/superset/migrations/versions/2018-07-05_15-19_3dda56f1c4c6_migrate_num_period_compare_and_period_.py#L92'>superset/migrations/versions/2018-07-05_15-19_3dda56f1c4c6_migrate_num_period_compare_and_period_.py:92:5:</a> RET506 Unnecessary `elif` after `raise` statement
+ <a href='https://github.com/apache/superset/blob/c016ca5ad977db0eb2e0a306827d584609936e33/superset/migrations/versions/2018-07-05_15-19_3dda56f1c4c6_migrate_num_period_compare_and_period_.py#L92'>superset/migrations/versions/2018-07-05_15-19_3dda56f1c4c6_migrate_num_period_compare_and_period_.py:92:5:</a> RET506 [*] Unnecessary `elif` after `raise` statement
- <a href='https://github.com/apache/superset/blob/c016ca5ad977db0eb2e0a306827d584609936e33/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L334'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:334:13:</a> RET507 Unnecessary `elif` after `continue` statement
+ <a href='https://github.com/apache/superset/blob/c016ca5ad977db0eb2e0a306827d584609936e33/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L334'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:334:13:</a> RET507 [*] Unnecessary `elif` after `continue` statement
... 214 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -142 violations, +306 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/clustering/main.py#L88'>examples/server/app/clustering/main.py:88:5:</a> RET505 Unnecessary `elif` after `return` statement
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/clustering/main.py#L88'>examples/server/app/clustering/main.py:88:5:</a> RET505 [*] Unnecessary `elif` after `return` statement
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/checks.py#L105'>release/checks.py:105:9:</a> RET505 Unnecessary `else` after `return` statement
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/checks.py#L105'>release/checks.py:105:9:</a> RET505 [*] Unnecessary `else` after `return` statement
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/checks.py#L119'>release/checks.py:119:9:</a> RET505 Unnecessary `else` after `return` statement
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/checks.py#L119'>release/checks.py:119:9:</a> RET505 [*] Unnecessary `else` after `return` statement
... 239 additional changes omitted for rule RET505
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L169'>src/bokeh/application/application.py:169:9:</a> RET506 Unnecessary `elif` after `raise` statement
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/application.py#L169'>src/bokeh/application/application.py:169:9:</a> RET506 [*] Unnecessary `elif` after `raise` statement
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L154'>src/bokeh/application/handlers/directory.py:154:9:</a> RET506 Unnecessary `elif` after `raise` statement
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/directory.py#L154'>src/bokeh/application/handlers/directory.py:154:9:</a> RET506 [*] Unnecessary `elif` after `raise` statement
... 441 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+53 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/admin/tests/test_integration.py#L534'>admin/tests/test_integration.py:534:1:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/molecule/testinfra/app-code/test_securedrop_app_code.py#L55'>molecule/testinfra/app-code/test_securedrop_app_code.py:55:1:</a> PT023 [*] Use `@pytest.mark.skip_in_prod` over `@pytest.mark.skip_in_prod()`
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/molecule/testinfra/app-code/test_securedrop_app_code.py#L69'>molecule/testinfra/app-code/test_securedrop_app_code.py:69:1:</a> PT023 [*] Use `@pytest.mark.skip_in_prod` over `@pytest.mark.skip_in_prod()`
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/molecule/testinfra/app/test_app_network.py#L12'>molecule/testinfra/app/test_app_network.py:12:1:</a> PT023 [*] Use `@pytest.mark.skip_in_prod` over `@pytest.mark.skip_in_prod()`
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/molecule/testinfra/app/test_appenv.py#L18'>molecule/testinfra/app/test_appenv.py:18:1:</a> PT023 [*] Use `@pytest.mark.skip_in_prod` over `@pytest.mark.skip_in_prod()`
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/molecule/testinfra/app/test_appenv.py#L90'>molecule/testinfra/app/test_appenv.py:90:1:</a> PT023 [*] Use `@pytest.mark.skip_in_prod` over `@pytest.mark.skip_in_prod()`
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/molecule/testinfra/app/test_ossec_agent.py#L42'>molecule/testinfra/app/test_ossec_agent.py:42:1:</a> PT023 [*] Use `@pytest.mark.xfail` over `@pytest.mark.xfail()`
... 23 additional changes omitted for rule PT023
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/securedrop/tests/conftest.py#L102'>securedrop/tests/conftest.py:102:1:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/securedrop/tests/conftest.py#L122'>securedrop/tests/conftest.py:122:1:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/securedrop/tests/conftest.py#L151'>securedrop/tests/conftest.py:151:1:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
... 43 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/c6dd39bd834ed458bb627c294854832e21971f1a/test/conftest.py#L40'>test/conftest.py:40:1:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
+ <a href='https://github.com/pypa/cibuildwheel/blob/c6dd39bd834ed458bb627c294854832e21971f1a/test/conftest.py#L49'>test/conftest.py:49:1:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
+ <a href='https://github.com/pypa/cibuildwheel/blob/c6dd39bd834ed458bb627c294854832e21971f1a/unit_test/conftest.py#L22'>unit_test/conftest.py:22:1:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
+ <a href='https://github.com/pypa/cibuildwheel/blob/c6dd39bd834ed458bb627c294854832e21971f1a/unit_test/main_tests/conftest.py#L63'>unit_test/main_tests/conftest.py:63:1:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
+ <a href='https://github.com/pypa/cibuildwheel/blob/c6dd39bd834ed458bb627c294854832e21971f1a/unit_test/main_tests/conftest.py#L84'>unit_test/main_tests/conftest.py:84:1:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
+ <a href='https://github.com/pypa/cibuildwheel/blob/c6dd39bd834ed458bb627c294854832e21971f1a/unit_test/option_prepare_test.py#L32'>unit_test/option_prepare_test.py:32:1:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -370 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/3c6df26a10bf76e7fdfc45bdf3e21a8e947a75ff/rotkehlchen/tests/api/blockchain/test_substrate.py#L102'>rotkehlchen/tests/api/blockchain/test_substrate.py:102:1:</a> PT023 [*] Use `@pytest.mark.vcr()` over `@pytest.mark.vcr`
- <a href='https://github.com/rotki/rotki/blob/3c6df26a10bf76e7fdfc45bdf3e21a8e947a75ff/rotkehlchen/tests/api/blockchain/test_substrate.py#L51'>rotkehlchen/tests/api/blockchain/test_substrate.py:51:1:</a> PT023 [*] Use `@pytest.mark.vcr()` over `@pytest.mark.vcr`
- <a href='https://github.com/rotki/rotki/blob/3c6df26a10bf76e7fdfc45bdf3e21a8e947a75ff/rotkehlchen/tests/api/test_caching.py#L116'>rotkehlchen/tests/api/test_caching.py:116:1:</a> PT023 [*] Use `@pytest.mark.vcr()` over `@pytest.mark.vcr`
- <a href='https://github.com/rotki/rotki/blob/3c6df26a10bf76e7fdfc45bdf3e21a8e947a75ff/rotkehlchen/tests/api/test_current_assets_price.py#L311'>rotkehlchen/tests/api/test_current_assets_price.py:311:1:</a> PT023 [*] Use `@pytest.mark.vcr()` over `@pytest.mark.vcr`
- <a href='https://github.com/rotki/rotki/blob/3c6df26a10bf76e7fdfc45bdf3e21a8e947a75ff/rotkehlchen/tests/api/test_current_assets_price.py#L345'>rotkehlchen/tests/api/test_current_assets_price.py:345:1:</a> PT023 [*] Use `@pytest.mark.vcr()` over `@pytest.mark.vcr`
- <a href='https://github.com/rotki/rotki/blob/3c6df26a10bf76e7fdfc45bdf3e21a8e947a75ff/rotkehlchen/tests/api/test_history_events_export.py#L83'>rotkehlchen/tests/api/test_history_events_export.py:83:1:</a> PT023 [*] Use `@pytest.mark.vcr()` over `@pytest.mark.vcr`
... 337 additional changes omitted for rule PT023
- <a href='https://github.com/rotki/rotki/blob/3c6df26a10bf76e7fdfc45bdf3e21a8e947a75ff/rotkehlchen/tests/conftest.py#L107'>rotkehlchen/tests/conftest.py:107:5:</a> PT001 [*] Use `@pytest.fixture()` over `@pytest.fixture`
- <a href='https://github.com/rotki/rotki/blob/3c6df26a10bf76e7fdfc45bdf3e21a8e947a75ff/rotkehlchen/tests/fixtures/accounting.py#L73'>rotkehlchen/tests/fixtures/accounting.py:73:1:</a> PT001 [*] Use `@pytest.fixture()` over `@pytest.fixture`
- <a href='https://github.com/rotki/rotki/blob/3c6df26a10bf76e7fdfc45bdf3e21a8e947a75ff/rotkehlchen/tests/fixtures/accounting.py#L85'>rotkehlchen/tests/fixtures/accounting.py:85:1:</a> PT001 [*] Use `@pytest.fixture()` over `@pytest.fixture`
- <a href='https://github.com/rotki/rotki/blob/3c6df26a10bf76e7fdfc45bdf3e21a8e947a75ff/rotkehlchen/tests/fixtures/db.py#L125'>rotkehlchen/tests/fixtures/db.py:125:1:</a> PT001 [*] Use `@pytest.fixture()` over `@pytest.fixture`
... 360 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (+20 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build/blob/c81a2e5282ebcedae833da4ca33ee57089923d99/tests/conftest.py#L150'>tests/conftest.py:150:1:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
+ <a href='https://github.com/scikit-build/scikit-build/blob/c81a2e5282ebcedae833da4ca33ee57089923d99/tests/conftest.py#L157'>tests/conftest.py:157:1:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
+ <a href='https://github.com/scikit-build/scikit-build/blob/c81a2e5282ebcedae833da4ca33ee57089923d99/tests/test_distribution.py#L12'>tests/test_distribution.py:12:1:</a> PT023 [*] Use `@pytest.mark.isolated` over `@pytest.mark.isolated()`
+ <a href='https://github.com/scikit-build/scikit-build/blob/c81a2e5282ebcedae833da4ca33ee57089923d99/tests/test_distribution.py#L29'>tests/test_distribution.py:29:1:</a> PT023 [*] Use `@pytest.mark.isolated` over `@pytest.mark.isolated()`
+ <a href='https://github.com/scikit-build/scikit-build/blob/c81a2e5282ebcedae833da4ca33ee57089923d99/tests/test_hello_cpp.py#L174'>tests/test_hello_cpp.py:174:1:</a> PT023 [*] Use `@pytest.mark.deprecated` over `@pytest.mark.deprecated()`
+ <a href='https://github.com/scikit-build/scikit-build/blob/c81a2e5282ebcedae833da4ca33ee57089923d99/tests/test_hello_fortran.py#L22'>tests/test_hello_fortran.py:22:1:</a> PT023 [*] Use `@pytest.mark.fortran` over `@pytest.mark.fortran()`
+ <a href='https://github.com/scikit-build/scikit-build/blob/c81a2e5282ebcedae833da4ca33ee57089923d99/tests/test_hello_fortran.py#L30'>tests/test_hello_fortran.py:30:1:</a> PT023 [*] Use `@pytest.mark.fortran` over `@pytest.mark.fortran()`
+ <a href='https://github.com/scikit-build/scikit-build/blob/c81a2e5282ebcedae833da4ca33ee57089923d99/tests/test_hello_fortran.py#L61'>tests/test_hello_fortran.py:61:1:</a> PT023 [*] Use `@pytest.mark.fortran` over `@pytest.mark.fortran()`
... 13 additional changes omitted for rule PT023
... 12 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (8 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RET505 | 1582 | 0 | 0 | 1582 | 0 |
| PT023 | 661 | 196 | 465 | 0 | 0 |
| RET506 | 372 | 0 | 0 | 372 | 0 |
| PT004 | 355 | 0 | 355 | 0 | 0 |
| PT001 | 295 | 159 | 136 | 0 | 0 |
| RET507 | 36 | 0 | 0 | 36 | 0 |
| RET508 | 16 | 0 | 0 | 16 | 0 |
| PT005 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+38 -356 violations, +0 -0 fixes in 5 projects; 49 projects unchanged)

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
... 295 additional changes omitted for rule PT004
- <a href='https://github.com/apache/airflow/blob/3b42286aa478c405db42f5e7bbaf25f67adb8eb3/tests/providers/ssh/operators/test_ssh.py#L68'>tests/providers/ssh/operators/test_ssh.py:68:9:</a> PT005 Fixture `_patch_exec_ssh_client` returns a value, remove leading underscore
... 294 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -40 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/c016ca5ad977db0eb2e0a306827d584609936e33/tests/integration_tests/celery_tests.py#L71'>tests/integration_tests/celery_tests.py:71:5:</a> PT004 Fixture `setup_sqllab` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/c016ca5ad977db0eb2e0a306827d584609936e33/tests/integration_tests/charts/api_tests.py#L1274'>tests/integration_tests/charts/api_tests.py:1274:9:</a> PT004 Fixture `load_energy_charts` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/c016ca5ad977db0eb2e0a306827d584609936e33/tests/integration_tests/charts/api_tests.py#L89'>tests/integration_tests/charts/api_tests.py:89:9:</a> PT004 Fixture `clear_data_cache` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/c016ca5ad977db0eb2e0a306827d584609936e33/tests/integration_tests/charts/data/api_tests.py#L93'>tests/integration_tests/charts/data/api_tests.py:93:5:</a> PT004 Fixture `skip_by_backend` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/c016ca5ad977db0eb2e0a306827d584609936e33/tests/integration_tests/conftest.py#L120'>tests/integration_tests/conftest.py:120:5:</a> PT004 Fixture `setup_sample_data` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/c016ca5ad977db0eb2e0a306827d584609936e33/tests/integration_tests/core_tests.py#L79'>tests/integration_tests/core_tests.py:79:5:</a> PT004 Fixture `cleanup` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/c016ca5ad977db0eb2e0a306827d584609936e33/tests/integration_tests/dashboards/filter_state/api_tests.py#L57'>tests/integration_tests/dashboards/filter_state/api_tests.py:57:5:</a> PT004 Fixture `cache` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/c016ca5ad977db0eb2e0a306827d584609936e33/tests/integration_tests/explore/api_tests.py#L66'>tests/integration_tests/explore/api_tests.py:66:5:</a> PT004 Fixture `cache` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/c016ca5ad977db0eb2e0a306827d584609936e33/tests/integration_tests/explore/form_data/api_tests.py#L68'>tests/integration_tests/explore/form_data/api_tests.py:68:5:</a> PT004 Fixture `cache` does not return anything, add leading underscore
- <a href='https://github.com/apache/superset/blob/c016ca5ad977db0eb2e0a306827d584609936e33/tests/integration_tests/fixtures/birth_names_dashboard.py#L37'>tests/integration_tests/fixtures/birth_names_dashboard.py:37:5:</a> PT004 Fixture `load_birth_names_data` does not return anything, add leading underscore
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
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+34 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ doc/source/user_guide/style.ipynb:cell 113:31:89: E501 Line too long (90 > 88)
+ doc/source/user_guide/style.ipynb:cell 113:33:89: E501 Line too long (98 > 88)
+ doc/source/user_guide/style.ipynb:cell 123:5:56: E741 Ambiguous variable name: `l`
+ doc/source/user_guide/style.ipynb:cell 125:2:13: C408 Unnecessary `dict` call (rewrite as a literal)
+ doc/source/user_guide/style.ipynb:cell 125:4:13: C408 Unnecessary `dict` call (rewrite as a literal)
+ doc/source/user_guide/style.ipynb:cell 125:6:13: C408 Unnecessary `dict` call (rewrite as a literal)
+ doc/source/user_guide/style.ipynb:cell 125:8:13: C408 Unnecessary `dict` call (rewrite as a literal)
+ doc/source/user_guide/style.ipynb:cell 126:1:1: NPY002 Replace legacy `np.random.seed` call with `np.random.Generator`
+ doc/source/user_guide/style.ipynb:cell 126:2:1: PLW0127 Self-assignment of variable `cmap`
+ doc/source/user_guide/style.ipynb:cell 126:2:8: PLW0128 Redeclared variable `cmap` in assignment
... 24 additional changes omitted for project
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
<details><summary>Changes by rule (10 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT004 | 355 | 0 | 355 | 0 | 0 |
| E501 | 18 | 18 | 0 | 0 | 0 |
| NPY002 | 8 | 8 | 0 | 0 | 0 |
| RUF100 | 4 | 4 | 0 | 0 | 0 |
| C408 | 4 | 4 | 0 | 0 | 0 |
| E741 | 1 | 1 | 0 | 0 | 0 |
| PLW0127 | 1 | 1 | 0 | 0 | 0 |
| PLW0128 | 1 | 1 | 0 | 0 | 0 |
| F811 | 1 | 1 | 0 | 0 | 0 |
| PT005 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:35 on 2024-08-12 10:46_

It might be good to keep a mention of what the default is (just remove the mention of preview mode):

```suggestion
/// fixtures is fine, but it's best to be consistent. The rule defaults to
/// removing unnecessary parentheses, to match the documentation of the
/// official pytest projects.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:934 on 2024-08-12 10:51_

Yeah, that's odd. I tried digging through blame but the best I can conclude without spending too much time on it is that it's been this way for a long time xD

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/marks.rs`:15 on 2024-08-12 10:52_

```suggestion
/// setting.
///
/// The rule defaults to removing unnecessary parentheses,
/// to match the documentation of the official pytest projects.
```

---

_@AlexWaygood approved on 2024-08-12 10:53_

The big reduction in ecosystem hits is evidence that this is the way to go!

---

_Review requested from @charliermarsh by @MichaReiser on 2024-08-12 14:14_

---

_Merged by @MichaReiser on 2024-08-13 15:54_

---

_Closed by @MichaReiser on 2024-08-13 15:54_

---

_Branch deleted on 2024-08-13 15:54_

---

_Comment by @FinchPowers on 2024-08-21 01:22_

The documentation string was updated, but not the examples.

<img width="961" alt="Capture d’écran, le 2024-08-20 à 20 42 16" src="https://github.com/user-attachments/assets/494100ee-dd53-4aeb-9810-01f239d4caa6">

Fix in: https://github.com/astral-sh/ruff/pull/13019

---
