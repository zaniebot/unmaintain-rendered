```yaml
number: 14770
title: "Remove `@` in `pytest.mark.parametrize` rule messages"
type: pull_request
state: merged
author: harupy
labels:
  - rule
assignees: []
merged: true
base: main
head: fix-parametrize-message
created_at: 2024-12-04T13:54:49Z
updated_at: 2024-12-04T15:08:05Z
url: https://github.com/astral-sh/ruff/pull/14770
synced_at: 2026-01-12T15:55:49Z
```

# Remove `@` in `pytest.mark.parametrize` rule messages

---

_@harupy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Related to #14515. The rules for `pytest.mark.parametrize` check calls now, not decorators. `@` in the message can be removed.

## Test Plan

<!-- How was it tested? -->

Existing tests

---

_@harupy reviewed on 2024-12-04 14:04_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:96 on 2024-12-04 14:04_

`flake8-pytest-style` uses `@pytest.mark.parametrize`:

https://github.com/m-burst/flake8-pytest-style/blob/2addab8b533ea101e25d022a37dc32d89db6c688/flake8_pytest_style/errors.py#L33

---

_Comment by @MichaReiser on 2024-12-04 14:08_

Removing it makes sense to me

---

_Comment by @harupy on 2024-12-04 14:16_

> Removing it makes sense to me

Thanks for the comment. `@` in the message is obviously harmless but seems a bit strange as ruff no longer checks decorators.

---

_Comment by @github-actions[bot] on 2024-12-04 14:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1665 -1665 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1502 -1502 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_cache.py#L35'>dev/breeze/tests/test_cache.py:35:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_cache.py#L35'>dev/breeze/tests/test_cache.py:35:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_docker_command_utils.py#L203'>dev/breeze/tests/test_docker_command_utils.py:203:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_docker_command_utils.py#L203'>dev/breeze/tests/test_docker_command_utils.py:203:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_exclude_from_matrix.py#L25'>dev/breeze/tests/test_exclude_from_matrix.py:25:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_exclude_from_matrix.py#L25'>dev/breeze/tests/test_exclude_from_matrix.py:25:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_general_utils.py#L25'>dev/breeze/tests/test_general_utils.py:25:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_general_utils.py#L25'>dev/breeze/tests/test_general_utils.py:25:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L126'>dev/breeze/tests/test_packages.py:126:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L126'>dev/breeze/tests/test_packages.py:126:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L164'>dev/breeze/tests/test_packages.py:164:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L164'>dev/breeze/tests/test_packages.py:164:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L238'>dev/breeze/tests/test_packages.py:238:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L238'>dev/breeze/tests/test_packages.py:238:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L340'>dev/breeze/tests/test_packages.py:340:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L340'>dev/breeze/tests/test_packages.py:340:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L351'>dev/breeze/tests/test_packages.py:351:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L351'>dev/breeze/tests/test_packages.py:351:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L362'>dev/breeze/tests/test_packages.py:362:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L362'>dev/breeze/tests/test_packages.py:362:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L402'>dev/breeze/tests/test_packages.py:402:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L402'>dev/breeze/tests/test_packages.py:402:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L438'>dev/breeze/tests/test_packages.py:438:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
... 2390 additional changes omitted for rule PT006
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/assets/test_s3.py#L75'>providers/tests/amazon/aws/assets/test_s3.py:75:33:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/assets/test_s3.py#L75'>providers/tests/amazon/aws/assets/test_s3.py:75:33:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/hooks/test_base_aws.py#L463'>providers/tests/amazon/aws/hooks/test_base_aws.py:463:99:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/hooks/test_base_aws.py#L463'>providers/tests/amazon/aws/hooks/test_base_aws.py:463:99:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/hooks/test_eks.py#L1203'>providers/tests/amazon/aws/hooks/test_eks.py:1203:13:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/hooks/test_eks.py#L1203'>providers/tests/amazon/aws/hooks/test_eks.py:1203:13:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/hooks/test_eks.py#L1215'>providers/tests/amazon/aws/hooks/test_eks.py:1215:13:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/hooks/test_eks.py#L1215'>providers/tests/amazon/aws/hooks/test_eks.py:1215:13:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/hooks/test_eks.py#L1227'>providers/tests/amazon/aws/hooks/test_eks.py:1227:13:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/hooks/test_eks.py#L1227'>providers/tests/amazon/aws/hooks/test_eks.py:1227:13:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/hooks/test_redshift_data.py#L452'>providers/tests/amazon/aws/hooks/test_redshift_data.py:452:9:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/hooks/test_redshift_data.py#L452'>providers/tests/amazon/aws/hooks/test_redshift_data.py:452:9:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/operators/test_ecs.py#L197'>providers/tests/amazon/aws/operators/test_ecs.py:197:13:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/operators/test_ecs.py#L197'>providers/tests/amazon/aws/operators/test_ecs.py:197:13:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/operators/test_ecs.py#L205'>providers/tests/amazon/aws/operators/test_ecs.py:205:13:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/operators/test_ecs.py#L205'>providers/tests/amazon/aws/operators/test_ecs.py:205:13:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/operators/test_ecs.py#L213'>providers/tests/amazon/aws/operators/test_ecs.py:213:13:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/operators/test_ecs.py#L213'>providers/tests/amazon/aws/operators/test_ecs.py:213:13:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/operators/test_ecs.py#L221'>providers/tests/amazon/aws/operators/test_ecs.py:221:13:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/operators/test_ecs.py#L221'>providers/tests/amazon/aws/operators/test_ecs.py:221:13:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/operators/test_ecs.py#L229'>providers/tests/amazon/aws/operators/test_ecs.py:229:13:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/operators/test_ecs.py#L229'>providers/tests/amazon/aws/operators/test_ecs.py:229:13:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
... 2959 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+136 -136 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/a3fd7423b016d27c674a4c6fdce5e384c1e82819/tests/integration_tests/access_tests.py#L84'>tests/integration_tests/access_tests.py:84:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/superset/blob/a3fd7423b016d27c674a4c6fdce5e384c1e82819/tests/integration_tests/access_tests.py#L84'>tests/integration_tests/access_tests.py:84:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/superset/blob/a3fd7423b016d27c674a4c6fdce5e384c1e82819/tests/integration_tests/charts/data/api_tests.py#L1458'>tests/integration_tests/charts/data/api_tests.py:1458:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/superset/blob/a3fd7423b016d27c674a4c6fdce5e384c1e82819/tests/integration_tests/charts/data/api_tests.py#L1458'>tests/integration_tests/charts/data/api_tests.py:1458:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/superset/blob/a3fd7423b016d27c674a4c6fdce5e384c1e82819/tests/integration_tests/charts/data/api_tests.py#L1483'>tests/integration_tests/charts/data/api_tests.py:1483:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/superset/blob/a3fd7423b016d27c674a4c6fdce5e384c1e82819/tests/integration_tests/charts/data/api_tests.py#L1483'>tests/integration_tests/charts/data/api_tests.py:1483:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/superset/blob/a3fd7423b016d27c674a4c6fdce5e384c1e82819/tests/integration_tests/db_engine_specs/hive_tests.py#L234'>tests/integration_tests/db_engine_specs/hive_tests.py:234:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/superset/blob/a3fd7423b016d27c674a4c6fdce5e384c1e82819/tests/integration_tests/db_engine_specs/hive_tests.py#L234'>tests/integration_tests/db_engine_specs/hive_tests.py:234:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/superset/blob/a3fd7423b016d27c674a4c6fdce5e384c1e82819/tests/integration_tests/extensions/metastore_cache_test.py#L102'>tests/integration_tests/extensions/metastore_cache_test.py:102:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/superset/blob/a3fd7423b016d27c674a4c6fdce5e384c1e82819/tests/integration_tests/extensions/metastore_cache_test.py#L102'>tests/integration_tests/extensions/metastore_cache_test.py:102:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
... 262 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+27 -27 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_named.py#L184'>tests/unit/bokeh/colors/test_named.py:184:26:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_named.py#L184'>tests/unit/bokeh/colors/test_named.py:184:26:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_validation__property.py#L195'>tests/unit/bokeh/core/property/test_validation__property.py:195:30:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_validation__property.py#L195'>tests/unit/bokeh/core/property/test_validation__property.py:195:30:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_validation__property.py#L196'>tests/unit/bokeh/core/property/test_validation__property.py:196:30:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_validation__property.py#L196'>tests/unit/bokeh/core/property/test_validation__property.py:196:30:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
... 15 additional changes omitted for rule PT006
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_visual.py#L237'>tests/unit/bokeh/core/property/test_visual.py:237:37:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_visual.py#L237'>tests/unit/bokeh/core/property/test_visual.py:237:37:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_visual.py#L308'>tests/unit/bokeh/core/property/test_visual.py:308:37:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_visual.py#L308'>tests/unit/bokeh/core/property/test_visual.py:308:37:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
... 44 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT006 | 2704 | 1352 | 1352 | 0 | 0 |
| PT007 | 626 | 313 | 313 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1715 -1715 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1550 -1550 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_cache.py#L35'>dev/breeze/tests/test_cache.py:35:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_cache.py#L35'>dev/breeze/tests/test_cache.py:35:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_docker_command_utils.py#L203'>dev/breeze/tests/test_docker_command_utils.py:203:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_docker_command_utils.py#L203'>dev/breeze/tests/test_docker_command_utils.py:203:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_exclude_from_matrix.py#L25'>dev/breeze/tests/test_exclude_from_matrix.py:25:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_exclude_from_matrix.py#L25'>dev/breeze/tests/test_exclude_from_matrix.py:25:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_general_utils.py#L25'>dev/breeze/tests/test_general_utils.py:25:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_general_utils.py#L25'>dev/breeze/tests/test_general_utils.py:25:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L126'>dev/breeze/tests/test_packages.py:126:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L126'>dev/breeze/tests/test_packages.py:126:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L164'>dev/breeze/tests/test_packages.py:164:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L164'>dev/breeze/tests/test_packages.py:164:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L238'>dev/breeze/tests/test_packages.py:238:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L238'>dev/breeze/tests/test_packages.py:238:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L340'>dev/breeze/tests/test_packages.py:340:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L340'>dev/breeze/tests/test_packages.py:340:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L351'>dev/breeze/tests/test_packages.py:351:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L351'>dev/breeze/tests/test_packages.py:351:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L362'>dev/breeze/tests/test_packages.py:362:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L362'>dev/breeze/tests/test_packages.py:362:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L402'>dev/breeze/tests/test_packages.py:402:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L402'>dev/breeze/tests/test_packages.py:402:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/dev/breeze/tests/test_packages.py#L438'>dev/breeze/tests/test_packages.py:438:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
... 2484 additional changes omitted for rule PT006
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/assets/test_s3.py#L75'>providers/tests/amazon/aws/assets/test_s3.py:75:33:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/assets/test_s3.py#L75'>providers/tests/amazon/aws/assets/test_s3.py:75:33:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/hooks/test_base_aws.py#L463'>providers/tests/amazon/aws/hooks/test_base_aws.py:463:99:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/hooks/test_base_aws.py#L463'>providers/tests/amazon/aws/hooks/test_base_aws.py:463:99:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/hooks/test_eks.py#L1203'>providers/tests/amazon/aws/hooks/test_eks.py:1203:13:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/hooks/test_eks.py#L1203'>providers/tests/amazon/aws/hooks/test_eks.py:1203:13:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/hooks/test_eks.py#L1215'>providers/tests/amazon/aws/hooks/test_eks.py:1215:13:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/hooks/test_eks.py#L1215'>providers/tests/amazon/aws/hooks/test_eks.py:1215:13:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/hooks/test_eks.py#L1227'>providers/tests/amazon/aws/hooks/test_eks.py:1227:13:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/hooks/test_eks.py#L1227'>providers/tests/amazon/aws/hooks/test_eks.py:1227:13:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/hooks/test_redshift_data.py#L452'>providers/tests/amazon/aws/hooks/test_redshift_data.py:452:9:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/hooks/test_redshift_data.py#L452'>providers/tests/amazon/aws/hooks/test_redshift_data.py:452:9:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/operators/test_ecs.py#L197'>providers/tests/amazon/aws/operators/test_ecs.py:197:13:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/operators/test_ecs.py#L197'>providers/tests/amazon/aws/operators/test_ecs.py:197:13:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/operators/test_ecs.py#L205'>providers/tests/amazon/aws/operators/test_ecs.py:205:13:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/operators/test_ecs.py#L205'>providers/tests/amazon/aws/operators/test_ecs.py:205:13:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/operators/test_ecs.py#L213'>providers/tests/amazon/aws/operators/test_ecs.py:213:13:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/operators/test_ecs.py#L213'>providers/tests/amazon/aws/operators/test_ecs.py:213:13:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/operators/test_ecs.py#L221'>providers/tests/amazon/aws/operators/test_ecs.py:221:13:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/operators/test_ecs.py#L221'>providers/tests/amazon/aws/operators/test_ecs.py:221:13:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/operators/test_ecs.py#L229'>providers/tests/amazon/aws/operators/test_ecs.py:229:13:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/apache/airflow/blob/2e8f456f9597b05bd7bbfb26dc59f823cee0b16f/providers/tests/amazon/aws/operators/test_ecs.py#L229'>providers/tests/amazon/aws/operators/test_ecs.py:229:13:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
... 3055 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+136 -136 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/a3fd7423b016d27c674a4c6fdce5e384c1e82819/tests/integration_tests/access_tests.py#L84'>tests/integration_tests/access_tests.py:84:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/superset/blob/a3fd7423b016d27c674a4c6fdce5e384c1e82819/tests/integration_tests/access_tests.py#L84'>tests/integration_tests/access_tests.py:84:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/superset/blob/a3fd7423b016d27c674a4c6fdce5e384c1e82819/tests/integration_tests/charts/data/api_tests.py#L1458'>tests/integration_tests/charts/data/api_tests.py:1458:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/superset/blob/a3fd7423b016d27c674a4c6fdce5e384c1e82819/tests/integration_tests/charts/data/api_tests.py#L1458'>tests/integration_tests/charts/data/api_tests.py:1458:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/superset/blob/a3fd7423b016d27c674a4c6fdce5e384c1e82819/tests/integration_tests/charts/data/api_tests.py#L1483'>tests/integration_tests/charts/data/api_tests.py:1483:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/superset/blob/a3fd7423b016d27c674a4c6fdce5e384c1e82819/tests/integration_tests/charts/data/api_tests.py#L1483'>tests/integration_tests/charts/data/api_tests.py:1483:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/superset/blob/a3fd7423b016d27c674a4c6fdce5e384c1e82819/tests/integration_tests/db_engine_specs/hive_tests.py#L234'>tests/integration_tests/db_engine_specs/hive_tests.py:234:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/superset/blob/a3fd7423b016d27c674a4c6fdce5e384c1e82819/tests/integration_tests/db_engine_specs/hive_tests.py#L234'>tests/integration_tests/db_engine_specs/hive_tests.py:234:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/apache/superset/blob/a3fd7423b016d27c674a4c6fdce5e384c1e82819/tests/integration_tests/extensions/metastore_cache_test.py#L102'>tests/integration_tests/extensions/metastore_cache_test.py:102:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/superset/blob/a3fd7423b016d27c674a4c6fdce5e384c1e82819/tests/integration_tests/extensions/metastore_cache_test.py#L102'>tests/integration_tests/extensions/metastore_cache_test.py:102:5:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
... 262 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+27 -27 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_named.py#L184'>tests/unit/bokeh/colors/test_named.py:184:26:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/colors/test_named.py#L184'>tests/unit/bokeh/colors/test_named.py:184:26:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_validation__property.py#L195'>tests/unit/bokeh/core/property/test_validation__property.py:195:30:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_validation__property.py#L195'>tests/unit/bokeh/core/property/test_validation__property.py:195:30:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_validation__property.py#L196'>tests/unit/bokeh/core/property/test_validation__property.py:196:30:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_validation__property.py#L196'>tests/unit/bokeh/core/property/test_validation__property.py:196:30:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
... 15 additional changes omitted for rule PT006
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_visual.py#L237'>tests/unit/bokeh/core/property/test_visual.py:237:37:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_visual.py#L237'>tests/unit/bokeh/core/property/test_visual.py:237:37:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_visual.py#L308'>tests/unit/bokeh/core/property/test_visual.py:308:37:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_visual.py#L308'>tests/unit/bokeh/core/property/test_visual.py:308:37:</a> PT007 Wrong values type in `pytest.mark.parametrize` expected `list` of `tuple`
... 44 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pypa/setuptools/blob/9692cde009af4651819d18a1e839d3b6e3fcd77d/pkg_resources/tests/test_working_set.py#L107'>pkg_resources/tests/test_working_set.py:107:9:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/pypa/setuptools/blob/9692cde009af4651819d18a1e839d3b6e3fcd77d/pkg_resources/tests/test_working_set.py#L107'>pkg_resources/tests/test_working_set.py:107:9:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
- <a href='https://github.com/pypa/setuptools/blob/9692cde009af4651819d18a1e839d3b6e3fcd77d/setuptools/tests/test_egg_info.py#L308'>setuptools/tests/test_egg_info.py:308:17:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/pypa/setuptools/blob/9692cde009af4651819d18a1e839d3b6e3fcd77d/setuptools/tests/test_egg_info.py#L308'>setuptools/tests/test_egg_info.py:308:17:</a> PT006 Wrong type passed to first argument of `pytest.mark.parametrize`; expected `tuple`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT006 | 2802 | 1401 | 1401 | 0 | 0 |
| PT007 | 628 | 314 | 314 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2024-12-04 15:00_

---

_@MichaReiser approved on 2024-12-04 15:01_

Thanks

---

_Merged by @MichaReiser on 2024-12-04 15:01_

---

_Closed by @MichaReiser on 2024-12-04 15:01_

---

_Branch deleted on 2024-12-04 15:08_

---
