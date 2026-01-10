```yaml
number: 10468
title: "Improve clarity of `PT006`'s error message"
type: pull_request
state: merged
author: augustelalande
labels: []
assignees: []
merged: true
base: main
head: pt006-message
created_at: 2024-03-19T04:02:35Z
updated_at: 2024-03-20T18:33:15Z
url: https://github.com/astral-sh/ruff/pull/10468
synced_at: 2026-01-10T22:47:02Z
```

# Improve clarity of `PT006`'s error message

---

_Pull request opened by @augustelalande on 2024-03-19 04:02_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Improve clarity of `PT006`'s error message. Resolves #10242.

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-03-19 04:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1066 -1066 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1056 -1056 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_cache.py#L35'>dev/breeze/tests/test_cache.py:35:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_cache.py#L35'>dev/breeze/tests/test_cache.py:35:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_docker_command_utils.py#L203'>dev/breeze/tests/test_docker_command_utils.py:203:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_docker_command_utils.py#L203'>dev/breeze/tests/test_docker_command_utils.py:203:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_exclude_from_matrix.py#L25'>dev/breeze/tests/test_exclude_from_matrix.py:25:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_exclude_from_matrix.py#L25'>dev/breeze/tests/test_exclude_from_matrix.py:25:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_general_utils.py#L25'>dev/breeze/tests/test_general_utils.py:25:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_general_utils.py#L25'>dev/breeze/tests/test_general_utils.py:25:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L126'>dev/breeze/tests/test_packages.py:126:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L126'>dev/breeze/tests/test_packages.py:126:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L162'>dev/breeze/tests/test_packages.py:162:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L162'>dev/breeze/tests/test_packages.py:162:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L227'>dev/breeze/tests/test_packages.py:227:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L227'>dev/breeze/tests/test_packages.py:227:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L328'>dev/breeze/tests/test_packages.py:328:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L328'>dev/breeze/tests/test_packages.py:328:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L339'>dev/breeze/tests/test_packages.py:339:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L339'>dev/breeze/tests/test_packages.py:339:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L350'>dev/breeze/tests/test_packages.py:350:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L350'>dev/breeze/tests/test_packages.py:350:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L390'>dev/breeze/tests/test_packages.py:390:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L390'>dev/breeze/tests/test_packages.py:390:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L426'>dev/breeze/tests/test_packages.py:426:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L426'>dev/breeze/tests/test_packages.py:426:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_provider_documentation.py#L121'>dev/breeze/tests/test_provider_documentation.py:121:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_provider_documentation.py#L121'>dev/breeze/tests/test_provider_documentation.py:121:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_provider_documentation.py#L156'>dev/breeze/tests/test_provider_documentation.py:156:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_provider_documentation.py#L156'>dev/breeze/tests/test_provider_documentation.py:156:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_provider_documentation.py#L221'>dev/breeze/tests/test_provider_documentation.py:221:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_provider_documentation.py#L221'>dev/breeze/tests/test_provider_documentation.py:221:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_provider_documentation.py#L84'>dev/breeze/tests/test_provider_documentation.py:84:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_provider_documentation.py#L84'>dev/breeze/tests/test_provider_documentation.py:84:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_provider_documentation.py#L96'>dev/breeze/tests/test_provider_documentation.py:96:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_provider_documentation.py#L96'>dev/breeze/tests/test_provider_documentation.py:96:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_pytest_args_for_test_types.py#L208'>dev/breeze/tests/test_pytest_args_for_test_types.py:208:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_pytest_args_for_test_types.py#L208'>dev/breeze/tests/test_pytest_args_for_test_types.py:208:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_pytest_args_for_test_types.py#L237'>dev/breeze/tests/test_pytest_args_for_test_types.py:237:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_pytest_args_for_test_types.py#L237'>dev/breeze/tests/test_pytest_args_for_test_types.py:237:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_pytest_args_for_test_types.py#L26'>dev/breeze/tests/test_pytest_args_for_test_types.py:26:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_pytest_args_for_test_types.py#L26'>dev/breeze/tests/test_pytest_args_for_test_types.py:26:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_selective_checks.py#L1067'>dev/breeze/tests/test_selective_checks.py:1067:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_selective_checks.py#L1067'>dev/breeze/tests/test_selective_checks.py:1067:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_selective_checks.py#L1235'>dev/breeze/tests/test_selective_checks.py:1235:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_selective_checks.py#L1235'>dev/breeze/tests/test_selective_checks.py:1235:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_selective_checks.py#L1363'>dev/breeze/tests/test_selective_checks.py:1363:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_selective_checks.py#L1363'>dev/breeze/tests/test_selective_checks.py:1363:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_selective_checks.py#L1448'>dev/breeze/tests/test_selective_checks.py:1448:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_selective_checks.py#L1448'>dev/breeze/tests/test_selective_checks.py:1448:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_selective_checks.py#L1561'>dev/breeze/tests/test_selective_checks.py:1561:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
... 2063 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+10 -10 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_named.py#L184'>tests/unit/bokeh/colors/test_named.py:184:26:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_named.py#L184'>tests/unit/bokeh/colors/test_named.py:184:26:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_validation__property.py#L195'>tests/unit/bokeh/core/property/test_validation__property.py:195:30:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_validation__property.py#L195'>tests/unit/bokeh/core/property/test_validation__property.py:195:30:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_validation__property.py#L196'>tests/unit/bokeh/core/property/test_validation__property.py:196:30:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_validation__property.py#L196'>tests/unit/bokeh/core/property/test_validation__property.py:196:30:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/document/test_callbacks__document.py#L140'>tests/unit/bokeh/document/test_callbacks__document.py:140:30:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/document/test_callbacks__document.py#L140'>tests/unit/bokeh/document/test_callbacks__document.py:140:30:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_defaults.py#L48'>tests/unit/bokeh/models/test_defaults.py:48:26:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_defaults.py#L48'>tests/unit/bokeh/models/test_defaults.py:48:26:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
... 10 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT006 | 2132 | 1066 | 1066 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1066 -1066 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1056 -1056 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_cache.py#L35'>dev/breeze/tests/test_cache.py:35:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_cache.py#L35'>dev/breeze/tests/test_cache.py:35:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_docker_command_utils.py#L203'>dev/breeze/tests/test_docker_command_utils.py:203:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_docker_command_utils.py#L203'>dev/breeze/tests/test_docker_command_utils.py:203:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_exclude_from_matrix.py#L25'>dev/breeze/tests/test_exclude_from_matrix.py:25:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_exclude_from_matrix.py#L25'>dev/breeze/tests/test_exclude_from_matrix.py:25:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_general_utils.py#L25'>dev/breeze/tests/test_general_utils.py:25:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_general_utils.py#L25'>dev/breeze/tests/test_general_utils.py:25:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L126'>dev/breeze/tests/test_packages.py:126:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L126'>dev/breeze/tests/test_packages.py:126:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L162'>dev/breeze/tests/test_packages.py:162:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L162'>dev/breeze/tests/test_packages.py:162:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L227'>dev/breeze/tests/test_packages.py:227:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L227'>dev/breeze/tests/test_packages.py:227:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L328'>dev/breeze/tests/test_packages.py:328:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L328'>dev/breeze/tests/test_packages.py:328:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L339'>dev/breeze/tests/test_packages.py:339:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L339'>dev/breeze/tests/test_packages.py:339:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L350'>dev/breeze/tests/test_packages.py:350:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L350'>dev/breeze/tests/test_packages.py:350:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L390'>dev/breeze/tests/test_packages.py:390:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L390'>dev/breeze/tests/test_packages.py:390:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L426'>dev/breeze/tests/test_packages.py:426:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_packages.py#L426'>dev/breeze/tests/test_packages.py:426:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_provider_documentation.py#L121'>dev/breeze/tests/test_provider_documentation.py:121:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_provider_documentation.py#L121'>dev/breeze/tests/test_provider_documentation.py:121:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_provider_documentation.py#L156'>dev/breeze/tests/test_provider_documentation.py:156:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_provider_documentation.py#L156'>dev/breeze/tests/test_provider_documentation.py:156:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_provider_documentation.py#L221'>dev/breeze/tests/test_provider_documentation.py:221:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_provider_documentation.py#L221'>dev/breeze/tests/test_provider_documentation.py:221:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_provider_documentation.py#L84'>dev/breeze/tests/test_provider_documentation.py:84:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_provider_documentation.py#L84'>dev/breeze/tests/test_provider_documentation.py:84:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_provider_documentation.py#L96'>dev/breeze/tests/test_provider_documentation.py:96:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_provider_documentation.py#L96'>dev/breeze/tests/test_provider_documentation.py:96:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_pytest_args_for_test_types.py#L208'>dev/breeze/tests/test_pytest_args_for_test_types.py:208:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_pytest_args_for_test_types.py#L208'>dev/breeze/tests/test_pytest_args_for_test_types.py:208:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_pytest_args_for_test_types.py#L237'>dev/breeze/tests/test_pytest_args_for_test_types.py:237:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_pytest_args_for_test_types.py#L237'>dev/breeze/tests/test_pytest_args_for_test_types.py:237:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_pytest_args_for_test_types.py#L26'>dev/breeze/tests/test_pytest_args_for_test_types.py:26:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_pytest_args_for_test_types.py#L26'>dev/breeze/tests/test_pytest_args_for_test_types.py:26:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_selective_checks.py#L1067'>dev/breeze/tests/test_selective_checks.py:1067:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_selective_checks.py#L1067'>dev/breeze/tests/test_selective_checks.py:1067:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_selective_checks.py#L1235'>dev/breeze/tests/test_selective_checks.py:1235:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_selective_checks.py#L1235'>dev/breeze/tests/test_selective_checks.py:1235:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_selective_checks.py#L1363'>dev/breeze/tests/test_selective_checks.py:1363:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_selective_checks.py#L1363'>dev/breeze/tests/test_selective_checks.py:1363:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_selective_checks.py#L1448'>dev/breeze/tests/test_selective_checks.py:1448:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_selective_checks.py#L1448'>dev/breeze/tests/test_selective_checks.py:1448:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/6b7a4e5f9f844d7b53ed1ae74bef448c99826f04/dev/breeze/tests/test_selective_checks.py#L1561'>dev/breeze/tests/test_selective_checks.py:1561:5:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
... 2063 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+10 -10 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_named.py#L184'>tests/unit/bokeh/colors/test_named.py:184:26:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_named.py#L184'>tests/unit/bokeh/colors/test_named.py:184:26:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_validation__property.py#L195'>tests/unit/bokeh/core/property/test_validation__property.py:195:30:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_validation__property.py#L195'>tests/unit/bokeh/core/property/test_validation__property.py:195:30:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_validation__property.py#L196'>tests/unit/bokeh/core/property/test_validation__property.py:196:30:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_validation__property.py#L196'>tests/unit/bokeh/core/property/test_validation__property.py:196:30:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/document/test_callbacks__document.py#L140'>tests/unit/bokeh/document/test_callbacks__document.py:140:30:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/document/test_callbacks__document.py#L140'>tests/unit/bokeh/document/test_callbacks__document.py:140:30:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_defaults.py#L48'>tests/unit/bokeh/models/test_defaults.py:48:26:</a> PT006 Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_defaults.py#L48'>tests/unit/bokeh/models/test_defaults.py:48:26:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
... 10 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT006 | 2132 | 1066 | 1066 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review requested from @AlexWaygood by @MichaReiser on 2024-03-19 08:47_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:92 on 2024-03-20 12:42_

I think the phrase "name format" is still a little confusing here. (I use this decorator a fair bit, and I didn't know that the first argument was called the "names argument".) Maybe something like this?

```suggestion
        if *single_argument {
            return format!("Wrong type passed to argument 1 of `@pytest.mark.parametrize`; expected `str`");
        }
        format!("Wrong type passed to argument 1 of `@pytest.mark.parametrize`; expected `{expected}`")
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:103 on 2024-03-20 12:42_

```suggestion
            return Some(format!("Use a string for the first argument"));
        }
        Some(format!("Use a `{expected}` for the first argument"))
```

---

_@AlexWaygood reviewed on 2024-03-20 12:43_

Thanks!

---

_@augustelalande reviewed on 2024-03-20 17:07_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:92 on 2024-03-20 17:07_

Done

---

_@augustelalande reviewed on 2024-03-20 17:07_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:103 on 2024-03-20 17:07_

Done

---

_Comment by @augustelalande on 2024-03-20 17:07_

Thanks @AlexWaygood good suggestions.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/types.rs`:27 on 2024-03-20 17:09_

```suggestion
            Self::Csv => write!(f, "comma-separated list of strings"),
```

---

_@AlexWaygood reviewed on 2024-03-20 17:09_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/flake8_pytest_style/types.rs`:27 on 2024-03-20 17:13_

Done

---

_@augustelalande reviewed on 2024-03-20 17:13_

---

_@augustelalande reviewed on 2024-03-20 17:14_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/flake8_pytest_style/types.rs`:27 on 2024-03-20 17:14_

Although I'm still not happy with this. To me a "comma-seperated list of strings" would be `['string1', 'string2']`

---

_@augustelalande reviewed on 2024-03-20 17:15_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/flake8_pytest_style/types.rs`:27 on 2024-03-20 17:15_

Maybe "string of comma-seperated values"

---

_@AlexWaygood reviewed on 2024-03-20 17:17_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/types.rs`:27 on 2024-03-20 17:17_

Oh I see. Yes, "string of comma-separated values" works well, I think! Good call.

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/flake8_pytest_style/types.rs`:27 on 2024-03-20 17:19_

Ok done

---

_@augustelalande reviewed on 2024-03-20 17:19_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:94 on 2024-03-20 17:28_

Hmm, I don't think we should put backticks around `{expected}` here, since `string of comma-separated values` is a prose description rather than code. I'd remove the backticks here...

```suggestion
        format!("Wrong type passed to first argument of `@pytest.mark.parametrize`; expected {expected}")
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/types.rs`:29 on 2024-03-20 17:29_

...And I'd add backticks here for the two enum variants where it specifically makes sense to add backticks:

```suggestion
            Self::Tuple => write!(f, "`tuple`"),
            Self::List => write!(f, "`list`"),
```

---

_@AlexWaygood reviewed on 2024-03-20 17:29_

Nearly there!

---

_@AlexWaygood reviewed on 2024-03-20 17:31_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/types.rs`:27 on 2024-03-20 17:31_

Thanks!

---

_@augustelalande reviewed on 2024-03-20 18:01_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/flake8_pytest_style/types.rs`:29 on 2024-03-20 18:01_

Done. I couldn't do this exactly, because the value was used somewhere else where it wouldn't make sense.

---

_@AlexWaygood reviewed on 2024-03-20 18:06_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:99 on 2024-03-20 18:06_

I'd write this as so:

```suggestion
        let expected_string = {
	        if *single_argument {
	            "`str`".to_string();
	        } else {
	            match expected {
	                types::ParametrizeNameType::Csv => format!("a {expected}"),
	                types::ParametrizeNameType::Tuple | types::ParametrizeNameType::List => {
	                    format!("`{expected}`");
	                }
	            }
	        }
	    };
```

---

_@AlexWaygood reviewed on 2024-03-20 18:08_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:118 on 2024-03-20 18:08_

```suggestion
        let expected_string = {
	        if *single_argument {
	            "string".to_string();
	        } else {
	            match expected {
	                types::ParametrizeNameType::Csv => format!("{expected}"),
	                types::ParametrizeNameType::Tuple | types::ParametrizeNameType::List => {
	                    format!("`{expected}`");
	                }
	            }
	        }
	    };
```

---

_@AlexWaygood approved on 2024-03-20 18:18_

LGTM, thanks!

---

_Merged by @AlexWaygood on 2024-03-20 18:22_

---

_Closed by @AlexWaygood on 2024-03-20 18:22_

---

_Branch deleted on 2024-03-20 18:24_

---
