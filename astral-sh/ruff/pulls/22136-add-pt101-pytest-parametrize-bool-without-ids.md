```yaml
number: 22136
title: "Add PT101: \"pytest parametrize bool without ids\""
type: pull_request
state: open
author: akx
labels:
  - rule
  - needs-decision
assignees: []
base: main
head: pt-ids-for-vague-parametrizes
created_at: 2025-12-22T09:21:17Z
updated_at: 2025-12-22T22:09:24Z
url: https://github.com/astral-sh/ruff/pull/22136
synced_at: 2026-01-12T15:57:42Z
```

# Add PT101: "pytest parametrize bool without ids"

---

_@akx_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR proposes a new lint rule, PT101 (101 to separate it from the upstream PT rules), which flags `pytest.mark.parametrize` uses like `@pytest.mark.parametrize("flag", [True, False])`, which show up unhelpfully as `test_foo[False]`, `test_foo[True]`, and suggests to add `ids=(...)` to such parametrizations.

## Test Plan

Tests within, and I also ran this on our work repo and it seems I have some 200 issues to fix... 😄 


---

_Comment by @astral-sh-bot[bot] on 2025-12-22 09:26_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2906 -0 violations, +0 -0 fixes in 19 projects; 36 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+14 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/tests/interactions/test_base.py#L100'>tests/interactions/test_base.py:100:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/tests/interactions/test_base.py#L34'>tests/interactions/test_base.py:34:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/tests/test_abc.py#L145'>tests/test_abc.py:145:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/tests/test_http.py#L8'>tests/test_http.py:8:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/tests/test_permissions.py#L34'>tests/test_permissions.py:34:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/tests/test_permissions.py#L52'>tests/test_permissions.py:52:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/tests/test_permissions.py#L70'>tests/test_permissions.py:70:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/tests/test_permissions.py#L83'>tests/test_permissions.py:83:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/tests/test_utils.py#L1008'>tests/test_utils.py:1008:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/tests/test_utils.py#L331'>tests/test_utils.py:331:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+16 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/analysis/test_fit_functions.py#L108'>tests/analysis/test_fit_functions.py:108:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/analysis/test_fit_functions.py#L32'>tests/analysis/test_fit_functions.py:32:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/analysis/time_series/test_conditioanl_averaging.py#L249'>tests/analysis/time_series/test_conditioanl_averaging.py:249:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/analysis/time_series/test_excess_statistics.py#L10'>tests/analysis/time_series/test_excess_statistics.py:10:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/diagnostics/test_langmuir.py#L289'>tests/diagnostics/test_langmuir.py:289:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/formulary/test_lengths.py#L150'>tests/formulary/test_lengths.py:150:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/formulary/test_speeds.py#L186'>tests/formulary/test_speeds.py:186:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/particles/test_ionization_collection.py#L868'>tests/particles/test_ionization_collection.py:868:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/particles/test_ionization_collection.py#L869'>tests/particles/test_ionization_collection.py:869:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/particles/test_ionization_collection.py#L870'>tests/particles/test_ionization_collection.py:870:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+532 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/6bce6707908c571ef22a8295bfa871d3b0c2df9e/airflow-core/tests/integration/cli/commands/test_celery_command.py#L58'>airflow-core/tests/integration/cli/commands/test_celery_command.py:58:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/airflow/blob/6bce6707908c571ef22a8295bfa871d3b0c2df9e/airflow-core/tests/unit/api_fastapi/auth/managers/simple/test_simple_auth_manager.py#L143'>airflow-core/tests/unit/api_fastapi/auth/managers/simple/test_simple_auth_manager.py:143:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/airflow/blob/6bce6707908c571ef22a8295bfa871d3b0c2df9e/airflow-core/tests/unit/api_fastapi/auth/managers/simple/test_simple_auth_manager.py#L172'>airflow-core/tests/unit/api_fastapi/auth/managers/simple/test_simple_auth_manager.py:172:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/airflow/blob/6bce6707908c571ef22a8295bfa871d3b0c2df9e/airflow-core/tests/unit/api_fastapi/auth/managers/simple/test_simple_auth_manager.py#L199'>airflow-core/tests/unit/api_fastapi/auth/managers/simple/test_simple_auth_manager.py:199:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/airflow/blob/6bce6707908c571ef22a8295bfa871d3b0c2df9e/airflow-core/tests/unit/api_fastapi/auth/managers/simple/test_simple_auth_manager.py#L218'>airflow-core/tests/unit/api_fastapi/auth/managers/simple/test_simple_auth_manager.py:218:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/airflow/blob/6bce6707908c571ef22a8295bfa871d3b0c2df9e/airflow-core/tests/unit/api_fastapi/auth/managers/simple/test_simple_auth_manager.py#L244'>airflow-core/tests/unit/api_fastapi/auth/managers/simple/test_simple_auth_manager.py:244:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/airflow/blob/6bce6707908c571ef22a8295bfa871d3b0c2df9e/airflow-core/tests/unit/api_fastapi/auth/managers/test_base_auth_manager.py#L254'>airflow-core/tests/unit/api_fastapi/auth/managers/test_base_auth_manager.py:254:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/airflow/blob/6bce6707908c571ef22a8295bfa871d3b0c2df9e/airflow-core/tests/unit/api_fastapi/auth/managers/test_base_auth_manager.py#L276'>airflow-core/tests/unit/api_fastapi/auth/managers/test_base_auth_manager.py:276:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/airflow/blob/6bce6707908c571ef22a8295bfa871d3b0c2df9e/airflow-core/tests/unit/api_fastapi/auth/managers/test_base_auth_manager.py#L296'>airflow-core/tests/unit/api_fastapi/auth/managers/test_base_auth_manager.py:296:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/airflow/blob/6bce6707908c571ef22a8295bfa871d3b0c2df9e/airflow-core/tests/unit/api_fastapi/auth/managers/test_base_auth_manager.py#L316'>airflow-core/tests/unit/api_fastapi/auth/managers/test_base_auth_manager.py:316:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
... 522 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+50 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/c0bcf28947577c0fb4f3599baa98c05d2046fe47/superset-extensions-cli/tests/test_cli_validate.py#L109'>superset-extensions-cli/tests/test_cli_validate.py:109:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/superset/blob/c0bcf28947577c0fb4f3599baa98c05d2046fe47/superset-extensions-cli/tests/test_templates.py#L89'>superset-extensions-cli/tests/test_templates.py:89:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/superset/blob/c0bcf28947577c0fb4f3599baa98c05d2046fe47/tests/integration_tests/access_tests.py#L121'>tests/integration_tests/access_tests.py:121:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/superset/blob/c0bcf28947577c0fb4f3599baa98c05d2046fe47/tests/integration_tests/security/analytics_db_safety_tests.py#L26'>tests/integration_tests/security/analytics_db_safety_tests.py:26:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/superset/blob/c0bcf28947577c0fb4f3599baa98c05d2046fe47/tests/integration_tests/sqla_models_tests.py#L1031'>tests/integration_tests/sqla_models_tests.py:1031:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/superset/blob/c0bcf28947577c0fb4f3599baa98c05d2046fe47/tests/integration_tests/sqla_models_tests.py#L794'>tests/integration_tests/sqla_models_tests.py:794:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/superset/blob/c0bcf28947577c0fb4f3599baa98c05d2046fe47/tests/integration_tests/sqla_models_tests.py#L887'>tests/integration_tests/sqla_models_tests.py:887:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/superset/blob/c0bcf28947577c0fb4f3599baa98c05d2046fe47/tests/integration_tests/sqla_models_tests.py#L933'>tests/integration_tests/sqla_models_tests.py:933:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/superset/blob/c0bcf28947577c0fb4f3599baa98c05d2046fe47/tests/unit_tests/connectors/sqla/models_test.py#L610'>tests/unit_tests/connectors/sqla/models_test.py:610:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/superset/blob/c0bcf28947577c0fb4f3599baa98c05d2046fe47/tests/unit_tests/datasets/commands/importers/v1/import_test.py#L747'>tests/unit_tests/datasets/commands/importers/v1/import_test.py:747:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
... 40 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_checkbox_group.py#L49'>tests/integration/widgets/test_checkbox_group.py:49:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_radio_group.py#L49'>tests/integration/widgets/test_radio_group.py:49:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_validation__property.py#L303'>tests/unit/bokeh/core/property/test_validation__property.py:303:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/embed/test_util__embed.py#L135'>tests/unit/bokeh/embed/test_util__embed.py:135:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test__legends.py#L101'>tests/unit/bokeh/plotting/test__legends.py:101:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test__legends.py#L136'>tests/unit/bokeh/plotting/test__legends.py:136:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test__legends.py#L76'>tests/unit/bokeh/plotting/test__legends.py:76:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test_contour.py#L118'>tests/unit/bokeh/plotting/test_contour.py:118:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test_contour.py#L82'>tests/unit/bokeh/plotting/test_contour.py:82:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/server/test_server__server.py#L462'>tests/unit/bokeh/server/test_server__server.py:462:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/5ba5f4ad02e95752dca6b9c297be31f952dcbf0f/securedrop/tests/test_alembic.py#L135'>securedrop/tests/test_alembic.py:135:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/freedomofpress/securedrop/blob/5ba5f4ad02e95752dca6b9c297be31f952dcbf0f/securedrop/tests/test_journalist_api2.py#L326'>securedrop/tests/test_journalist_api2.py:326:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/freedomofpress/securedrop/blob/5ba5f4ad02e95752dca6b9c297be31f952dcbf0f/securedrop/tests/test_source_utils.py#L66'>securedrop/tests/test_source_utils.py:66:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+72 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/6a416c618655102847a59cae280348c0712cf45c/libs/core/tests/unit_tests/language_models/chat_models/test_base.py#L381'>libs/core/tests/unit_tests/language_models/chat_models/test_base.py:381:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/6a416c618655102847a59cae280348c0712cf45c/libs/core/tests/unit_tests/language_models/chat_models/test_base.py#L406'>libs/core/tests/unit_tests/language_models/chat_models/test_base.py:406:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/6a416c618655102847a59cae280348c0712cf45c/libs/core/tests/unit_tests/language_models/chat_models/test_base.py#L440'>libs/core/tests/unit_tests/language_models/chat_models/test_base.py:440:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/6a416c618655102847a59cae280348c0712cf45c/libs/core/tests/unit_tests/language_models/chat_models/test_base.py#L455'>libs/core/tests/unit_tests/language_models/chat_models/test_base.py:455:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/6a416c618655102847a59cae280348c0712cf45c/libs/core/tests/unit_tests/output_parsers/test_openai_tools.py#L392'>libs/core/tests/unit_tests/output_parsers/test_openai_tools.py:392:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/6a416c618655102847a59cae280348c0712cf45c/libs/core/tests/unit_tests/output_parsers/test_openai_tools.py#L404'>libs/core/tests/unit_tests/output_parsers/test_openai_tools.py:404:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/6a416c618655102847a59cae280348c0712cf45c/libs/core/tests/unit_tests/output_parsers/test_openai_tools.py#L416'>libs/core/tests/unit_tests/output_parsers/test_openai_tools.py:416:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/6a416c618655102847a59cae280348c0712cf45c/libs/core/tests/unit_tests/output_parsers/test_openai_tools.py#L435'>libs/core/tests/unit_tests/output_parsers/test_openai_tools.py:435:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/6a416c618655102847a59cae280348c0712cf45c/libs/core/tests/unit_tests/output_parsers/test_openai_tools.py#L445'>libs/core/tests/unit_tests/output_parsers/test_openai_tools.py:445:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/6a416c618655102847a59cae280348c0712cf45c/libs/core/tests/unit_tests/output_parsers/test_openai_tools.py#L456'>libs/core/tests/unit_tests/output_parsers/test_openai_tools.py:456:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
... 62 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+937 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_frame_apply.py#L1612'>pandas/tests/apply/test_frame_apply.py:1612:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_frame_apply.py#L418'>pandas/tests/apply/test_frame_apply.py:418:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_frame_apply.py#L75'>pandas/tests/apply/test_frame_apply.py:75:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_frame_apply.py#L76'>pandas/tests/apply/test_frame_apply.py:76:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_frame_apply.py#L789'>pandas/tests/apply/test_frame_apply.py:789:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_frame_apply.py#L882'>pandas/tests/apply/test_frame_apply.py:882:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_frame_transform.py#L133'>pandas/tests/apply/test_frame_transform.py:133:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_frame_transform.py#L238'>pandas/tests/apply/test_frame_transform.py:238:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_invalid_arg.py#L65'>pandas/tests/apply/test_invalid_arg.py:65:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_series_apply.py#L493'>pandas/tests/apply/test_series_apply.py:493:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_series_apply.py#L505'>pandas/tests/apply/test_series_apply.py:505:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/arithmetic/test_datetime64.py#L1453'>pandas/tests/arithmetic/test_datetime64.py:1453:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/arithmetic/test_datetime64.py#L178'>pandas/tests/arithmetic/test_datetime64.py:178:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/arithmetic/test_datetime64.py#L180'>pandas/tests/arithmetic/test_datetime64.py:180:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/arithmetic/test_datetime64.py#L366'>pandas/tests/arithmetic/test_datetime64.py:366:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/arithmetic/test_interval.py#L160'>pandas/tests/arithmetic/test_interval.py:160:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
... 921 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT101 | 2906 | 2906 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2906 -0 violations, +0 -0 fixes in 19 projects; 36 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+14 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/tests/interactions/test_base.py#L100'>tests/interactions/test_base.py:100:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/tests/interactions/test_base.py#L34'>tests/interactions/test_base.py:34:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/tests/test_abc.py#L145'>tests/test_abc.py:145:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/tests/test_http.py#L8'>tests/test_http.py:8:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/tests/test_permissions.py#L34'>tests/test_permissions.py:34:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/tests/test_permissions.py#L52'>tests/test_permissions.py:52:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/tests/test_permissions.py#L70'>tests/test_permissions.py:70:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/tests/test_permissions.py#L83'>tests/test_permissions.py:83:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/tests/test_utils.py#L1008'>tests/test_utils.py:1008:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/tests/test_utils.py#L331'>tests/test_utils.py:331:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+16 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/analysis/test_fit_functions.py#L108'>tests/analysis/test_fit_functions.py:108:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/analysis/test_fit_functions.py#L32'>tests/analysis/test_fit_functions.py:32:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/analysis/time_series/test_conditioanl_averaging.py#L249'>tests/analysis/time_series/test_conditioanl_averaging.py:249:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/analysis/time_series/test_excess_statistics.py#L10'>tests/analysis/time_series/test_excess_statistics.py:10:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/diagnostics/test_langmuir.py#L289'>tests/diagnostics/test_langmuir.py:289:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/formulary/test_lengths.py#L150'>tests/formulary/test_lengths.py:150:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/formulary/test_speeds.py#L186'>tests/formulary/test_speeds.py:186:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/particles/test_ionization_collection.py#L868'>tests/particles/test_ionization_collection.py:868:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/particles/test_ionization_collection.py#L869'>tests/particles/test_ionization_collection.py:869:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/particles/test_ionization_collection.py#L870'>tests/particles/test_ionization_collection.py:870:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+532 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/6bce6707908c571ef22a8295bfa871d3b0c2df9e/airflow-core/tests/integration/cli/commands/test_celery_command.py#L58'>airflow-core/tests/integration/cli/commands/test_celery_command.py:58:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/airflow/blob/6bce6707908c571ef22a8295bfa871d3b0c2df9e/airflow-core/tests/unit/api_fastapi/auth/managers/simple/test_simple_auth_manager.py#L143'>airflow-core/tests/unit/api_fastapi/auth/managers/simple/test_simple_auth_manager.py:143:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/airflow/blob/6bce6707908c571ef22a8295bfa871d3b0c2df9e/airflow-core/tests/unit/api_fastapi/auth/managers/simple/test_simple_auth_manager.py#L172'>airflow-core/tests/unit/api_fastapi/auth/managers/simple/test_simple_auth_manager.py:172:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/airflow/blob/6bce6707908c571ef22a8295bfa871d3b0c2df9e/airflow-core/tests/unit/api_fastapi/auth/managers/simple/test_simple_auth_manager.py#L199'>airflow-core/tests/unit/api_fastapi/auth/managers/simple/test_simple_auth_manager.py:199:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/airflow/blob/6bce6707908c571ef22a8295bfa871d3b0c2df9e/airflow-core/tests/unit/api_fastapi/auth/managers/simple/test_simple_auth_manager.py#L218'>airflow-core/tests/unit/api_fastapi/auth/managers/simple/test_simple_auth_manager.py:218:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/airflow/blob/6bce6707908c571ef22a8295bfa871d3b0c2df9e/airflow-core/tests/unit/api_fastapi/auth/managers/simple/test_simple_auth_manager.py#L244'>airflow-core/tests/unit/api_fastapi/auth/managers/simple/test_simple_auth_manager.py:244:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/airflow/blob/6bce6707908c571ef22a8295bfa871d3b0c2df9e/airflow-core/tests/unit/api_fastapi/auth/managers/test_base_auth_manager.py#L254'>airflow-core/tests/unit/api_fastapi/auth/managers/test_base_auth_manager.py:254:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/airflow/blob/6bce6707908c571ef22a8295bfa871d3b0c2df9e/airflow-core/tests/unit/api_fastapi/auth/managers/test_base_auth_manager.py#L276'>airflow-core/tests/unit/api_fastapi/auth/managers/test_base_auth_manager.py:276:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/airflow/blob/6bce6707908c571ef22a8295bfa871d3b0c2df9e/airflow-core/tests/unit/api_fastapi/auth/managers/test_base_auth_manager.py#L296'>airflow-core/tests/unit/api_fastapi/auth/managers/test_base_auth_manager.py:296:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/airflow/blob/6bce6707908c571ef22a8295bfa871d3b0c2df9e/airflow-core/tests/unit/api_fastapi/auth/managers/test_base_auth_manager.py#L316'>airflow-core/tests/unit/api_fastapi/auth/managers/test_base_auth_manager.py:316:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
... 522 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+50 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/c0bcf28947577c0fb4f3599baa98c05d2046fe47/superset-extensions-cli/tests/test_cli_validate.py#L109'>superset-extensions-cli/tests/test_cli_validate.py:109:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/superset/blob/c0bcf28947577c0fb4f3599baa98c05d2046fe47/superset-extensions-cli/tests/test_templates.py#L89'>superset-extensions-cli/tests/test_templates.py:89:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/superset/blob/c0bcf28947577c0fb4f3599baa98c05d2046fe47/tests/integration_tests/access_tests.py#L121'>tests/integration_tests/access_tests.py:121:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/superset/blob/c0bcf28947577c0fb4f3599baa98c05d2046fe47/tests/integration_tests/security/analytics_db_safety_tests.py#L26'>tests/integration_tests/security/analytics_db_safety_tests.py:26:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/superset/blob/c0bcf28947577c0fb4f3599baa98c05d2046fe47/tests/integration_tests/sqla_models_tests.py#L1031'>tests/integration_tests/sqla_models_tests.py:1031:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/superset/blob/c0bcf28947577c0fb4f3599baa98c05d2046fe47/tests/integration_tests/sqla_models_tests.py#L794'>tests/integration_tests/sqla_models_tests.py:794:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/superset/blob/c0bcf28947577c0fb4f3599baa98c05d2046fe47/tests/integration_tests/sqla_models_tests.py#L887'>tests/integration_tests/sqla_models_tests.py:887:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/superset/blob/c0bcf28947577c0fb4f3599baa98c05d2046fe47/tests/integration_tests/sqla_models_tests.py#L933'>tests/integration_tests/sqla_models_tests.py:933:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/superset/blob/c0bcf28947577c0fb4f3599baa98c05d2046fe47/tests/unit_tests/connectors/sqla/models_test.py#L610'>tests/unit_tests/connectors/sqla/models_test.py:610:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/apache/superset/blob/c0bcf28947577c0fb4f3599baa98c05d2046fe47/tests/unit_tests/datasets/commands/importers/v1/import_test.py#L747'>tests/unit_tests/datasets/commands/importers/v1/import_test.py:747:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
... 40 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_checkbox_group.py#L49'>tests/integration/widgets/test_checkbox_group.py:49:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_radio_group.py#L49'>tests/integration/widgets/test_radio_group.py:49:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/core/property/test_validation__property.py#L303'>tests/unit/bokeh/core/property/test_validation__property.py:303:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/embed/test_util__embed.py#L135'>tests/unit/bokeh/embed/test_util__embed.py:135:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test__legends.py#L101'>tests/unit/bokeh/plotting/test__legends.py:101:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test__legends.py#L136'>tests/unit/bokeh/plotting/test__legends.py:136:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test__legends.py#L76'>tests/unit/bokeh/plotting/test__legends.py:76:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test_contour.py#L118'>tests/unit/bokeh/plotting/test_contour.py:118:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test_contour.py#L82'>tests/unit/bokeh/plotting/test_contour.py:82:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/server/test_server__server.py#L462'>tests/unit/bokeh/server/test_server__server.py:462:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/5ba5f4ad02e95752dca6b9c297be31f952dcbf0f/securedrop/tests/test_alembic.py#L135'>securedrop/tests/test_alembic.py:135:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/freedomofpress/securedrop/blob/5ba5f4ad02e95752dca6b9c297be31f952dcbf0f/securedrop/tests/test_journalist_api2.py#L326'>securedrop/tests/test_journalist_api2.py:326:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/freedomofpress/securedrop/blob/5ba5f4ad02e95752dca6b9c297be31f952dcbf0f/securedrop/tests/test_source_utils.py#L66'>securedrop/tests/test_source_utils.py:66:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+72 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/6a416c618655102847a59cae280348c0712cf45c/libs/core/tests/unit_tests/language_models/chat_models/test_base.py#L381'>libs/core/tests/unit_tests/language_models/chat_models/test_base.py:381:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/6a416c618655102847a59cae280348c0712cf45c/libs/core/tests/unit_tests/language_models/chat_models/test_base.py#L406'>libs/core/tests/unit_tests/language_models/chat_models/test_base.py:406:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/6a416c618655102847a59cae280348c0712cf45c/libs/core/tests/unit_tests/language_models/chat_models/test_base.py#L440'>libs/core/tests/unit_tests/language_models/chat_models/test_base.py:440:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/6a416c618655102847a59cae280348c0712cf45c/libs/core/tests/unit_tests/language_models/chat_models/test_base.py#L455'>libs/core/tests/unit_tests/language_models/chat_models/test_base.py:455:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/6a416c618655102847a59cae280348c0712cf45c/libs/core/tests/unit_tests/output_parsers/test_openai_tools.py#L392'>libs/core/tests/unit_tests/output_parsers/test_openai_tools.py:392:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/6a416c618655102847a59cae280348c0712cf45c/libs/core/tests/unit_tests/output_parsers/test_openai_tools.py#L404'>libs/core/tests/unit_tests/output_parsers/test_openai_tools.py:404:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/6a416c618655102847a59cae280348c0712cf45c/libs/core/tests/unit_tests/output_parsers/test_openai_tools.py#L416'>libs/core/tests/unit_tests/output_parsers/test_openai_tools.py:416:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/6a416c618655102847a59cae280348c0712cf45c/libs/core/tests/unit_tests/output_parsers/test_openai_tools.py#L435'>libs/core/tests/unit_tests/output_parsers/test_openai_tools.py:435:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/6a416c618655102847a59cae280348c0712cf45c/libs/core/tests/unit_tests/output_parsers/test_openai_tools.py#L445'>libs/core/tests/unit_tests/output_parsers/test_openai_tools.py:445:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/langchain-ai/langchain/blob/6a416c618655102847a59cae280348c0712cf45c/libs/core/tests/unit_tests/output_parsers/test_openai_tools.py#L456'>libs/core/tests/unit_tests/output_parsers/test_openai_tools.py:456:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
... 62 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+937 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_frame_apply.py#L1612'>pandas/tests/apply/test_frame_apply.py:1612:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_frame_apply.py#L418'>pandas/tests/apply/test_frame_apply.py:418:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_frame_apply.py#L75'>pandas/tests/apply/test_frame_apply.py:75:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_frame_apply.py#L76'>pandas/tests/apply/test_frame_apply.py:76:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_frame_apply.py#L789'>pandas/tests/apply/test_frame_apply.py:789:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_frame_apply.py#L882'>pandas/tests/apply/test_frame_apply.py:882:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_frame_transform.py#L133'>pandas/tests/apply/test_frame_transform.py:133:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_frame_transform.py#L238'>pandas/tests/apply/test_frame_transform.py:238:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_invalid_arg.py#L65'>pandas/tests/apply/test_invalid_arg.py:65:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_series_apply.py#L493'>pandas/tests/apply/test_series_apply.py:493:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/apply/test_series_apply.py#L505'>pandas/tests/apply/test_series_apply.py:505:2:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/arithmetic/test_datetime64.py#L1453'>pandas/tests/arithmetic/test_datetime64.py:1453:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/arithmetic/test_datetime64.py#L178'>pandas/tests/arithmetic/test_datetime64.py:178:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/arithmetic/test_datetime64.py#L180'>pandas/tests/arithmetic/test_datetime64.py:180:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/arithmetic/test_datetime64.py#L366'>pandas/tests/arithmetic/test_datetime64.py:366:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
+ <a href='https://github.com/pandas-dev/pandas/blob/249b43d9a53033ec9b1ec2f5b046b696f5687296/pandas/tests/arithmetic/test_interval.py#L160'>pandas/tests/arithmetic/test_interval.py:160:6:</a> PT101 Boolean value in `pytest.mark.parametrize` without `ids` argument
... 921 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT101 | 2906 | 2906 | 0 | 0 | 0 |

</p>
</details>





---

_Comment by @ntBre on 2025-12-22 20:06_

Thanks! I'll just put `needs-decision` on this for now to give others time to weigh in, but this rule makes sense to me.

I did find a somewhat related issue in https://github.com/astral-sh/ruff/issues/14994 that recommends using `pytest.param` instead of a separate `ids` list. What do you think about recommending that approach here?

I also wonder if this should apply to all simple types, not just booleans. From the [docs](https://docs.pytest.org/en/stable/example/parametrize.html#different-options-for-test-ids):

> Numbers, strings, booleans and None will have their usual string representation used in the test ID. For other objects, pytest will make a string based on the argument name:

It seems like numbers, strings, and optional values could have the same issue.

---

_Label `rule` added by @ntBre on 2025-12-22 20:06_

---

_Label `needs-decision` added by @ntBre on 2025-12-22 20:06_

---

_Comment by @akx on 2025-12-22 22:09_

> It seems like numbers, strings, and optional values could have the same issue.

I was thinking about that earlier! Numbers are a bit here-or-there, because you could have a test that's parametrized over `n_characters` or whatever, and it'd be obvious from context. For strings, same: `test_tiktoken_tokenizer[supercalifragilisticexpialidocious-10]` would make sense (a test case pair, something you'd indeed implement with `pytest.param`). And for other objects, well, same really - you'd want their repr to show something sensible.

Forcing `pytest.param` for all cases is a bit verbose if you ask me:

```python
@pytest.mark.parametrize("with_user", [False, True], ids=["no_user", "with_user"])
```
&
```python
@pytest.mark.parametrize("with_user", pytest.param(False, id="no_user"), pytest.param(True, id="with_user"))
```
(and more so if your linter needs to split this into multiple lines).

But booleans feel to me like the obvious case where they don't add value at all without ids, especially if you have multiple `parametrize`s:

```python
@pytest.mark.parametrize("with_user", (False, True))
@pytest.mark.parametrize("with_ice_cream", (False, True))
@pytest.mark.parametrize("with_a_hangover", (False, True))
def test_sunday_vibe_check(...):
```
would leave you with a supremely useless `test_sunday_vibe_check[False-False-True] failed` :)

---
