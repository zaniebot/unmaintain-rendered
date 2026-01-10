```yaml
number: 12866
title: "[ruff 0.6] Improve the error message for PLW0642"
type: pull_request
state: merged
author: AlexWaygood
labels: []
assignees: []
merged: true
base: ruff-0.6
head: self-assignment-message
created_at: 2024-08-13T16:05:40Z
updated_at: 2024-08-13T16:19:24Z
url: https://github.com/astral-sh/ruff/pull/12866
synced_at: 2026-01-10T21:38:32Z
```

# [ruff 0.6] Improve the error message for PLW0642

---

_Pull request opened by @AlexWaygood on 2024-08-13 16:05_

A followup to #12857, as suggested by @dhruvmanila in https://github.com/astral-sh/ruff/pull/12857#discussion_r1715313031

---

_@MichaReiser approved on 2024-08-13 16:15_

---

_Merged by @AlexWaygood on 2024-08-13 16:16_

---

_Closed by @AlexWaygood on 2024-08-13 16:16_

---

_Branch deleted on 2024-08-13 16:16_

---

_Comment by @github-actions[bot] on 2024-08-13 16:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+407 -653 violations, +0 -0 fixes in 9 projects; 45 projects unchanged)

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
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+71 -89 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/f5d614d80d560adacb35f171568115dfd082098c/tests/integration_tests/annotation_layers/fixtures.py#L69'>tests/integration_tests/annotation_layers/fixtures.py:69:1:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
- <a href='https://github.com/apache/superset/blob/f5d614d80d560adacb35f171568115dfd082098c/tests/integration_tests/cachekeys/api_tests.py#L37'>tests/integration_tests/cachekeys/api_tests.py:37:1:</a> PT001 [*] Use `@pytest.fixture()` over `@pytest.fixture`
+ <a href='https://github.com/apache/superset/blob/f5d614d80d560adacb35f171568115dfd082098c/tests/integration_tests/charts/api_tests.py#L118'>tests/integration_tests/charts/api_tests.py:118:5:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
+ <a href='https://github.com/apache/superset/blob/f5d614d80d560adacb35f171568115dfd082098c/tests/integration_tests/charts/api_tests.py#L1273'>tests/integration_tests/charts/api_tests.py:1273:5:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
+ <a href='https://github.com/apache/superset/blob/f5d614d80d560adacb35f171568115dfd082098c/tests/integration_tests/charts/api_tests.py#L131'>tests/integration_tests/charts/api_tests.py:131:5:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
+ <a href='https://github.com/apache/superset/blob/f5d614d80d560adacb35f171568115dfd082098c/tests/integration_tests/charts/api_tests.py#L154'>tests/integration_tests/charts/api_tests.py:154:5:</a> PT001 [*] Use `@pytest.fixture` over `@pytest.fixture()`
... 151 additional changes omitted for rule PT001
- <a href='https://github.com/apache/superset/blob/f5d614d80d560adacb35f171568115dfd082098c/tests/integration_tests/charts/data/api_tests.py#L142'>tests/integration_tests/charts/data/api_tests.py:142:1:</a> PT023 [*] Use `@pytest.mark.chart_data_flow()` over `@pytest.mark.chart_data_flow`
- <a href='https://github.com/apache/superset/blob/f5d614d80d560adacb35f171568115dfd082098c/tests/integration_tests/charts/data/api_tests.py#L992'>tests/integration_tests/charts/data/api_tests.py:992:1:</a> PT023 [*] Use `@pytest.mark.chart_data_flow()` over `@pytest.mark.chart_data_flow`
- <a href='https://github.com/apache/superset/blob/f5d614d80d560adacb35f171568115dfd082098c/tests/integration_tests/import_export_tests.py#L523'>tests/integration_tests/import_export_tests.py:523:5:</a> PT023 [*] Use `@pytest.mark.skip()` over `@pytest.mark.skip`
- <a href='https://github.com/apache/superset/blob/f5d614d80d560adacb35f171568115dfd082098c/tests/integration_tests/sqllab_tests.py#L70'>tests/integration_tests/sqllab_tests.py:70:1:</a> PT023 [*] Use `@pytest.mark.sql_json_flow()` over `@pytest.mark.sql_json_flow`
... 150 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -143 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/events.py#L182'>src/bokeh/events.py:182:9:</a> PLW0642 Confusing assignment to `cls` argument in class method
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/events.py#L182'>src/bokeh/events.py:182:9:</a> PLW0642 Reassigned `cls` variable in class method
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/embed/test_json_item.py#L57'>tests/integration/embed/test_json_item.py:57:1:</a> PT023 [*] Use `@pytest.mark.selenium()` over `@pytest.mark.selenium`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/models/test_datarange1d.py#L53'>tests/integration/models/test_datarange1d.py:53:1:</a> PT023 [*] Use `@pytest.mark.selenium()` over `@pytest.mark.selenium`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/models/test_plot.py#L57'>tests/integration/models/test_plot.py:57:1:</a> PT023 [*] Use `@pytest.mark.selenium()` over `@pytest.mark.selenium`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/models/test_sources.py#L51'>tests/integration/models/test_sources.py:51:1:</a> PT023 [*] Use `@pytest.mark.selenium()` over `@pytest.mark.selenium`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/test_regressions.py#L32'>tests/integration/test_regressions.py:32:1:</a> PT023 [*] Use `@pytest.mark.selenium()` over `@pytest.mark.selenium`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/tools/test_box_edit_tool.py#L81'>tests/integration/tools/test_box_edit_tool.py:81:1:</a> PT023 [*] Use `@pytest.mark.selenium()` over `@pytest.mark.selenium`
... 114 additional changes omitted for rule PT023
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/ipython.py#L44'>tests/support/plugins/ipython.py:44:1:</a> PT001 [*] Use `@pytest.fixture()` over `@pytest.fixture`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/managed_server_loop.py#L59'>tests/support/plugins/managed_server_loop.py:59:1:</a> PT001 [*] Use `@pytest.fixture()` over `@pytest.fixture`
... 137 additional changes omitted for project
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
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+51 -51 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/categorical.py#L572'>pandas/core/arrays/categorical.py:572:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/categorical.py#L572'>pandas/core/arrays/categorical.py:572:13:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1065'>pandas/core/arrays/datetimelike.py:1065:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1065'>pandas/core/arrays/datetimelike.py:1065:9:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1080'>pandas/core/arrays/datetimelike.py:1080:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1080'>pandas/core/arrays/datetimelike.py:1080:9:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1081'>pandas/core/arrays/datetimelike.py:1081:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1081'>pandas/core/arrays/datetimelike.py:1081:9:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1109'>pandas/core/arrays/datetimelike.py:1109:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1109'>pandas/core/arrays/datetimelike.py:1109:9:</a> PLW0642 Reassigned `self` variable in instance method
... 92 additional changes omitted for project
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
- <a href='https://github.com/rotki/rotki/blob/9c0676903f55e0a5deabeaebbf45f8120186347f/rotkehlchen/tests/api/blockchain/test_substrate.py#L102'>rotkehlchen/tests/api/blockchain/test_substrate.py:102:1:</a> PT023 [*] Use `@pytest.mark.vcr()` over `@pytest.mark.vcr`
- <a href='https://github.com/rotki/rotki/blob/9c0676903f55e0a5deabeaebbf45f8120186347f/rotkehlchen/tests/api/blockchain/test_substrate.py#L51'>rotkehlchen/tests/api/blockchain/test_substrate.py:51:1:</a> PT023 [*] Use `@pytest.mark.vcr()` over `@pytest.mark.vcr`
- <a href='https://github.com/rotki/rotki/blob/9c0676903f55e0a5deabeaebbf45f8120186347f/rotkehlchen/tests/api/test_caching.py#L116'>rotkehlchen/tests/api/test_caching.py:116:1:</a> PT023 [*] Use `@pytest.mark.vcr()` over `@pytest.mark.vcr`
- <a href='https://github.com/rotki/rotki/blob/9c0676903f55e0a5deabeaebbf45f8120186347f/rotkehlchen/tests/api/test_current_assets_price.py#L311'>rotkehlchen/tests/api/test_current_assets_price.py:311:1:</a> PT023 [*] Use `@pytest.mark.vcr()` over `@pytest.mark.vcr`
- <a href='https://github.com/rotki/rotki/blob/9c0676903f55e0a5deabeaebbf45f8120186347f/rotkehlchen/tests/api/test_current_assets_price.py#L345'>rotkehlchen/tests/api/test_current_assets_price.py:345:1:</a> PT023 [*] Use `@pytest.mark.vcr()` over `@pytest.mark.vcr`
- <a href='https://github.com/rotki/rotki/blob/9c0676903f55e0a5deabeaebbf45f8120186347f/rotkehlchen/tests/api/test_history_events_export.py#L83'>rotkehlchen/tests/api/test_history_events_export.py:83:1:</a> PT023 [*] Use `@pytest.mark.vcr()` over `@pytest.mark.vcr`
- <a href='https://github.com/rotki/rotki/blob/9c0676903f55e0a5deabeaebbf45f8120186347f/rotkehlchen/tests/api/test_makerdao_vaults.py#L890'>rotkehlchen/tests/api/test_makerdao_vaults.py:890:1:</a> PT023 [*] Use `@pytest.mark.vcr()` over `@pytest.mark.vcr`
- <a href='https://github.com/rotki/rotki/blob/9c0676903f55e0a5deabeaebbf45f8120186347f/rotkehlchen/tests/api/test_metadata.py#L14'>rotkehlchen/tests/api/test_metadata.py:14:1:</a> PT023 [*] Use `@pytest.mark.vcr()` over `@pytest.mark.vcr`
- <a href='https://github.com/rotki/rotki/blob/9c0676903f55e0a5deabeaebbf45f8120186347f/rotkehlchen/tests/api/test_nfts.py#L135'>rotkehlchen/tests/api/test_nfts.py:135:1:</a> PT023 [*] Use `@pytest.mark.vcr()` over `@pytest.mark.vcr`
... 334 additional changes omitted for rule PT023
- <a href='https://github.com/rotki/rotki/blob/9c0676903f55e0a5deabeaebbf45f8120186347f/rotkehlchen/tests/conftest.py#L107'>rotkehlchen/tests/conftest.py:107:5:</a> PT001 [*] Use `@pytest.fixture()` over `@pytest.fixture`
- <a href='https://github.com/rotki/rotki/blob/9c0676903f55e0a5deabeaebbf45f8120186347f/rotkehlchen/tests/fixtures/accounting.py#L73'>rotkehlchen/tests/fixtures/accounting.py:73:1:</a> PT001 [*] Use `@pytest.fixture()` over `@pytest.fixture`
- <a href='https://github.com/rotki/rotki/blob/9c0676903f55e0a5deabeaebbf45f8120186347f/rotkehlchen/tests/fixtures/accounting.py#L85'>rotkehlchen/tests/fixtures/accounting.py:85:1:</a> PT001 [*] Use `@pytest.fixture()` over `@pytest.fixture`
- <a href='https://github.com/rotki/rotki/blob/9c0676903f55e0a5deabeaebbf45f8120186347f/rotkehlchen/tests/fixtures/db.py#L125'>rotkehlchen/tests/fixtures/db.py:125:1:</a> PT001 [*] Use `@pytest.fixture()` over `@pytest.fixture`
- <a href='https://github.com/rotki/rotki/blob/9c0676903f55e0a5deabeaebbf45f8120186347f/rotkehlchen/tests/fixtures/exchanges/binance.py#L12'>rotkehlchen/tests/fixtures/exchanges/binance.py:12:1:</a> PT001 [*] Use `@pytest.fixture()` over `@pytest.fixture`
- <a href='https://github.com/rotki/rotki/blob/9c0676903f55e0a5deabeaebbf45f8120186347f/rotkehlchen/tests/fixtures/exchanges/bitcoinde.py#L6'>rotkehlchen/tests/fixtures/exchanges/bitcoinde.py:6:1:</a> PT001 [*] Use `@pytest.fixture()` over `@pytest.fixture`
- <a href='https://github.com/rotki/rotki/blob/9c0676903f55e0a5deabeaebbf45f8120186347f/rotkehlchen/tests/fixtures/exchanges/bitfinex.py#L17'>rotkehlchen/tests/fixtures/exchanges/bitfinex.py:17:1:</a> PT001 [*] Use `@pytest.fixture()` over `@pytest.fixture`
- <a href='https://github.com/rotki/rotki/blob/9c0676903f55e0a5deabeaebbf45f8120186347f/rotkehlchen/tests/fixtures/exchanges/bitmex.py#L11'>rotkehlchen/tests/fixtures/exchanges/bitmex.py:11:1:</a> PT001 [*] Use `@pytest.fixture()` over `@pytest.fixture`
... 353 additional changes omitted for project
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

<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT023 | 661 | 196 | 465 | 0 | 0 |
| PT001 | 295 | 159 | 136 | 0 | 0 |
| PLW0642 | 104 | 52 | 52 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+52 -52 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/events.py#L182'>src/bokeh/events.py:182:9:</a> PLW0642 Confusing assignment to `cls` argument in class method
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/events.py#L182'>src/bokeh/events.py:182:9:</a> PLW0642 Reassigned `cls` variable in class method
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+51 -51 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/categorical.py#L572'>pandas/core/arrays/categorical.py:572:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/categorical.py#L572'>pandas/core/arrays/categorical.py:572:13:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1065'>pandas/core/arrays/datetimelike.py:1065:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1065'>pandas/core/arrays/datetimelike.py:1065:9:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1080'>pandas/core/arrays/datetimelike.py:1080:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1080'>pandas/core/arrays/datetimelike.py:1080:9:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1081'>pandas/core/arrays/datetimelike.py:1081:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1081'>pandas/core/arrays/datetimelike.py:1081:9:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1109'>pandas/core/arrays/datetimelike.py:1109:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1109'>pandas/core/arrays/datetimelike.py:1109:9:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1118'>pandas/core/arrays/datetimelike.py:1118:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1118'>pandas/core/arrays/datetimelike.py:1118:9:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1129'>pandas/core/arrays/datetimelike.py:1129:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1129'>pandas/core/arrays/datetimelike.py:1129:9:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1131'>pandas/core/arrays/datetimelike.py:1131:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1131'>pandas/core/arrays/datetimelike.py:1131:9:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1136'>pandas/core/arrays/datetimelike.py:1136:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1136'>pandas/core/arrays/datetimelike.py:1136:9:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1185'>pandas/core/arrays/datetimelike.py:1185:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1185'>pandas/core/arrays/datetimelike.py:1185:9:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1187'>pandas/core/arrays/datetimelike.py:1187:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1187'>pandas/core/arrays/datetimelike.py:1187:9:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1203'>pandas/core/arrays/datetimelike.py:1203:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1203'>pandas/core/arrays/datetimelike.py:1203:9:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1260'>pandas/core/arrays/datetimelike.py:1260:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1260'>pandas/core/arrays/datetimelike.py:1260:13:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1274'>pandas/core/arrays/datetimelike.py:1274:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1274'>pandas/core/arrays/datetimelike.py:1274:9:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1480'>pandas/core/arrays/datetimelike.py:1480:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1480'>pandas/core/arrays/datetimelike.py:1480:13:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1699'>pandas/core/arrays/datetimelike.py:1699:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L1699'>pandas/core/arrays/datetimelike.py:1699:13:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L2131'>pandas/core/arrays/datetimelike.py:2131:17:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L2131'>pandas/core/arrays/datetimelike.py:2131:17:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L2153'>pandas/core/arrays/datetimelike.py:2153:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L2153'>pandas/core/arrays/datetimelike.py:2153:13:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L456'>pandas/core/arrays/datetimelike.py:456:17:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L456'>pandas/core/arrays/datetimelike.py:456:17:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L778'>pandas/core/arrays/datetimelike.py:778:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L778'>pandas/core/arrays/datetimelike.py:778:13:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L979'>pandas/core/arrays/datetimelike.py:979:13:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/datetimelike.py#L979'>pandas/core/arrays/datetimelike.py:979:13:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/frame.py#L7827'>pandas/core/frame.py:7827:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/frame.py#L7827'>pandas/core/frame.py:7827:9:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/frame.py#L7840'>pandas/core/frame.py:7840:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/frame.py#L7840'>pandas/core/frame.py:7840:9:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/frame.py#L8183'>pandas/core/frame.py:8183:9:</a> PLW0642 Confusing assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/frame.py#L8183'>pandas/core/frame.py:8183:9:</a> PLW0642 Reassigned `self` variable in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/frame.py#L8195'>pandas/core/frame.py:8195:21:</a> PLW0642 Confusing assignment to `self` argument in instance method
... 53 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0642 | 104 | 52 | 52 | 0 | 0 |

</p>
</details>




---
