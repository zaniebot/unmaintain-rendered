```yaml
number: 15444
title: "[`flake8-pytest-style`] Implement pytest.warns diagnostics (`PT029`, `PT030`, `PT031`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: pytest-raises
created_at: 2025-01-12T20:33:24Z
updated_at: 2025-01-13T23:40:37Z
url: https://github.com/astral-sh/ruff/pull/15444
synced_at: 2026-01-12T15:55:51Z
```

# [`flake8-pytest-style`] Implement pytest.warns diagnostics (`PT029`, `PT030`, `PT031`)

---

_@tjkuson_

## Summary

Implements upstream diagnostics `PT029`, `PT030`, `PT031` that function as pytest.warns corollaries of `PT010`, `PT011`, `PT012` respectively. Most of the implementation and documentation is designed to mirror those existing diagnostics.

Closes #14239

## Test Plan

Tests for `PT029`, `PT030`, `PT031` largely copied from `PT010`, `PT011`, `PT012` respectively.

`cargo nextest run`


---

_Comment by @github-actions[bot] on 2025-01-12 20:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+182 -0 violations, +0 -0 fixes in 8 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/d847f505441d7ccf1ec4b63645ba8eae2cdc8882/tests/ext/commands/test_params.py#L119'>tests/ext/commands/test_params.py:119:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/DisnakeDev/disnake/blob/d847f505441d7ccf1ec4b63645ba8eae2cdc8882/tests/test_embeds.py#L473'>tests/test_embeds.py:473:23:</a> PT030 `pytest.warns(DeprecationWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/DisnakeDev/disnake/blob/d847f505441d7ccf1ec4b63645ba8eae2cdc8882/tests/test_embeds.py#L475'>tests/test_embeds.py:475:23:</a> PT030 `pytest.warns(DeprecationWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/DisnakeDev/disnake/blob/d847f505441d7ccf1ec4b63645ba8eae2cdc8882/tests/test_embeds.py#L477'>tests/test_embeds.py:477:23:</a> PT030 `pytest.warns(DeprecationWarning)` is too broad, set the `match` parameter or use a more specific warning
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+38 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/fcb706cca4ff224daf70a92a6ea11fc7cb1736bd/tests/analysis/swept_langmuir/test_floating_potential.py#L213'>tests/analysis/swept_langmuir/test_floating_potential.py:213:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/fcb706cca4ff224daf70a92a6ea11fc7cb1736bd/tests/analysis/test_fit_functions.py#L530'>tests/analysis/test_fit_functions.py:530:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/fcb706cca4ff224daf70a92a6ea11fc7cb1736bd/tests/diagnostics/charged_particle_radiography/test_synthetic_radiography.py#L372'>tests/diagnostics/charged_particle_radiography/test_synthetic_radiography.py:372:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/fcb706cca4ff224daf70a92a6ea11fc7cb1736bd/tests/formulary/collisions/test_collisions_dimensionless.py#L133'>tests/formulary/collisions/test_collisions_dimensionless.py:133:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/fcb706cca4ff224daf70a92a6ea11fc7cb1736bd/tests/formulary/collisions/test_collisions_frequencies.py#L727'>tests/formulary/collisions/test_collisions_frequencies.py:727:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/fcb706cca4ff224daf70a92a6ea11fc7cb1736bd/tests/formulary/collisions/test_collisions_lengths.py#L207'>tests/formulary/collisions/test_collisions_lengths.py:207:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/fcb706cca4ff224daf70a92a6ea11fc7cb1736bd/tests/formulary/collisions/test_collisions_misc.py#L152'>tests/formulary/collisions/test_collisions_misc.py:152:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/fcb706cca4ff224daf70a92a6ea11fc7cb1736bd/tests/formulary/collisions/test_coulomb.py#L134'>tests/formulary/collisions/test_coulomb.py:134:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/fcb706cca4ff224daf70a92a6ea11fc7cb1736bd/tests/formulary/collisions/test_coulomb.py#L160'>tests/formulary/collisions/test_coulomb.py:160:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/fcb706cca4ff224daf70a92a6ea11fc7cb1736bd/tests/formulary/collisions/test_coulomb.py#L97'>tests/formulary/collisions/test_coulomb.py:97:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
... 28 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+37 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/amazon/aws/utils/test_connection_wrapper.py#L121'>providers/tests/amazon/aws/utils/test_connection_wrapper.py:121:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/celery/executors/test_celery_executor.py#L259'>providers/tests/celery/executors/test_celery_executor.py:259:31:</a> PT030 `pytest.warns(DeprecationWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/cncf/kubernetes/executors/test_kubernetes_executor.py#L1269'>providers/tests/cncf/kubernetes/executors/test_kubernetes_executor.py:1269:27:</a> PT030 `pytest.warns(DeprecationWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/google/cloud/hooks/vertex_ai/test_generative_model.py#L153'>providers/tests/google/cloud/hooks/vertex_ai/test_generative_model.py:153:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/google/cloud/hooks/vertex_ai/test_generative_model.py#L168'>providers/tests/google/cloud/hooks/vertex_ai/test_generative_model.py:168:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/google/cloud/hooks/vertex_ai/test_generative_model.py#L179'>providers/tests/google/cloud/hooks/vertex_ai/test_generative_model.py:179:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/google/cloud/hooks/vertex_ai/test_generative_model.py#L193'>providers/tests/google/cloud/hooks/vertex_ai/test_generative_model.py:193:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/providers/tests/google/cloud/hooks/vertex_ai/test_generative_model.py#L208'>providers/tests/google/cloud/hooks/vertex_ai/test_generative_model.py:208:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
... 24 additional changes omitted for rule PT031
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/task_sdk/tests/defintions/test_asset.py#L117'>task_sdk/tests/defintions/test_asset.py:117:23:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/apache/airflow/blob/ead9386a68bb104e5afafca3c5d768afa27dc89d/task_sdk/tests/defintions/test_baseoperator.py#L242'>task_sdk/tests/defintions/test_baseoperator.py:242:27:</a> PT030 `pytest.warns(DeprecationWarning)` is too broad, set the `match` parameter or use a more specific warning
... 27 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+20 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_formatters.py#L35'>tests/unit/bokeh/models/test_formatters.py:35:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_plots.py#L81'>tests/unit/bokeh/models/test_plots.py:81:27:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_plots.py#L81'>tests/unit/bokeh/models/test_plots.py:81:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_sources.py#L283'>tests/unit/bokeh/models/test_sources.py:283:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_sources.py#L765'>tests/unit/bokeh/models/test_sources.py:765:27:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_sources.py#L771'>tests/unit/bokeh/models/test_sources.py:771:27:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_sources.py#L777'>tests/unit/bokeh/models/test_sources.py:777:27:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_sources.py#L783'>tests/unit/bokeh/models/test_sources.py:783:27:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test_graph.py#L122'>tests/unit/bokeh/plotting/test_graph.py:122:23:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
... 6 additional changes omitted for rule PT030
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test_graph.py#L122'>tests/unit/bokeh/plotting/test_graph.py:122:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
... 10 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/57d248978a4168aa73871a62ab79c47dc2977bb0/pandas/tests/copy_view/test_chained_assignment_deprecation.py#L20'>pandas/tests/copy_view/test_chained_assignment_deprecation.py:20:10:</a> PT029 Set the expected warning in `pytest.warns()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/setuptools/blob/fb7f3d3cab98f2c00cbf18fe887ce73007f49e18/pkg_resources/tests/test_resources.py#L864'>pkg_resources/tests/test_resources.py:864:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/pypa/setuptools/blob/fb7f3d3cab98f2c00cbf18fe887ce73007f49e18/setuptools/tests/config/test_pyprojecttoml_dynamic_deps.py#L106'>setuptools/tests/config/test_pyprojecttoml_dynamic_deps.py:106:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/pypa/setuptools/blob/fb7f3d3cab98f2c00cbf18fe887ce73007f49e18/setuptools/tests/test_build_py.py#L168'>setuptools/tests/test_build_py.py:168:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-trio/trio/blob/b41bb13f8a7b0a8792493b0e8ad9d50c82945c3e/src/trio/_core/_tests/test_asyncgen.py#L45'>src/trio/_core/_tests/test_asyncgen.py:45:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/python-trio/trio/blob/b41bb13f8a7b0a8792493b0e8ad9d50c82945c3e/src/trio/_core/_tests/test_guest_mode.py#L421'>src/trio/_core/_tests/test_guest_mode.py:421:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/python-trio/trio/blob/b41bb13f8a7b0a8792493b0e8ad9d50c82945c3e/src/trio/_tests/test_deprecate.py#L273'>src/trio/_tests/test_deprecate.py:273:23:</a> PT030 `pytest.warns(DeprecationWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/python-trio/trio/blob/b41bb13f8a7b0a8792493b0e8ad9d50c82945c3e/src/trio/_tests/test_dtls.py#L752'>src/trio/_tests/test_dtls.py:752:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/python-trio/trio/blob/b41bb13f8a7b0a8792493b0e8ad9d50c82945c3e/src/trio/_tests/test_dtls.py#L768'>src/trio/_tests/test_dtls.py:768:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/python-trio/trio/blob/b41bb13f8a7b0a8792493b0e8ad9d50c82945c3e/src/trio/_tests/test_dtls.py#L791'>src/trio/_tests/test_dtls.py:791:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/python-trio/trio/blob/b41bb13f8a7b0a8792493b0e8ad9d50c82945c3e/src/trio/_tests/test_dtls.py#L808'>src/trio/_tests/test_dtls.py:808:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
... 3 additional changes omitted for rule PT031
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+70 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/e6957e7a1e14bd35dadcd742501d3fba40122652/astropy/config/tests/test_configs.py#L500'>astropy/config/tests/test_configs.py:500:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/e6957e7a1e14bd35dadcd742501d3fba40122652/astropy/config/tests/test_configs.py#L540'>astropy/config/tests/test_configs.py:540:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/e6957e7a1e14bd35dadcd742501d3fba40122652/astropy/convolution/tests/test_convolve.py#L1259'>astropy/convolution/tests/test_convolve.py:1259:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/e6957e7a1e14bd35dadcd742501d3fba40122652/astropy/convolution/tests/test_convolve_fft.py#L812'>astropy/convolution/tests/test_convolve_fft.py:812:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/e6957e7a1e14bd35dadcd742501d3fba40122652/astropy/coordinates/tests/test_sky_coord.py#L1860'>astropy/coordinates/tests/test_sky_coord.py:1860:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/e6957e7a1e14bd35dadcd742501d3fba40122652/astropy/coordinates/tests/test_velocity_corrs.py#L293'>astropy/coordinates/tests/test_velocity_corrs.py:293:23:</a> PT030 `pytest.warns(Warning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/astropy/astropy/blob/e6957e7a1e14bd35dadcd742501d3fba40122652/astropy/cosmology/funcs/tests/test_funcs.py#L270'>astropy/cosmology/funcs/tests/test_funcs.py:270:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/e6957e7a1e14bd35dadcd742501d3fba40122652/astropy/io/ascii/tests/test_c_reader.py#L1175'>astropy/io/ascii/tests/test_c_reader.py:1175:15:</a> PT029 Set the expected warning in `pytest.warns()`
+ <a href='https://github.com/astropy/astropy/blob/e6957e7a1e14bd35dadcd742501d3fba40122652/astropy/io/ascii/tests/test_c_reader.py#L1281'>astropy/io/ascii/tests/test_c_reader.py:1281:15:</a> PT029 Set the expected warning in `pytest.warns()`
+ <a href='https://github.com/astropy/astropy/blob/e6957e7a1e14bd35dadcd742501d3fba40122652/astropy/io/ascii/tests/test_c_reader.py#L1288'>astropy/io/ascii/tests/test_c_reader.py:1288:19:</a> PT029 Set the expected warning in `pytest.warns()`
+ <a href='https://github.com/astropy/astropy/blob/e6957e7a1e14bd35dadcd742501d3fba40122652/astropy/io/ascii/tests/test_c_reader.py#L1352'>astropy/io/ascii/tests/test_c_reader.py:1352:10:</a> PT029 Set the expected warning in `pytest.warns()`
+ <a href='https://github.com/astropy/astropy/blob/e6957e7a1e14bd35dadcd742501d3fba40122652/astropy/io/ascii/tests/test_c_reader.py#L1363'>astropy/io/ascii/tests/test_c_reader.py:1363:10:</a> PT029 Set the expected warning in `pytest.warns()`
+ <a href='https://github.com/astropy/astropy/blob/e6957e7a1e14bd35dadcd742501d3fba40122652/astropy/io/ascii/tests/test_ipac_definitions.py#L101'>astropy/io/ascii/tests/test_ipac_definitions.py:101:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
... 50 additional changes omitted for rule PT031
+ <a href='https://github.com/astropy/astropy/blob/e6957e7a1e14bd35dadcd742501d3fba40122652/astropy/io/ascii/tests/test_qdp.py#L222'>astropy/io/ascii/tests/test_qdp.py:222:23:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/astropy/astropy/blob/e6957e7a1e14bd35dadcd742501d3fba40122652/astropy/io/fits/hdu/compressed/tests/test_compressed.py#L501'>astropy/io/fits/hdu/compressed/tests/test_compressed.py:501:31:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/astropy/astropy/blob/e6957e7a1e14bd35dadcd742501d3fba40122652/astropy/io/fits/hdu/compressed/tests/test_compressed.py#L513'>astropy/io/fits/hdu/compressed/tests/test_compressed.py:513:18:</a> PT029 Set the expected warning in `pytest.warns()`
+ <a href='https://github.com/astropy/astropy/blob/e6957e7a1e14bd35dadcd742501d3fba40122652/astropy/io/fits/tests/test_header.py#L195'>astropy/io/fits/tests/test_header.py:195:22:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/astropy/astropy/blob/e6957e7a1e14bd35dadcd742501d3fba40122652/astropy/table/tests/test_table.py#L2325'>astropy/table/tests/test_table.py:2325:27:</a> PT030 `pytest.warns(UserWarning)` is too broad, set the `match` parameter or use a more specific warning
+ <a href='https://github.com/astropy/astropy/blob/e6957e7a1e14bd35dadcd742501d3fba40122652/astropy/wcs/tests/test_profiling.py#L72'>astropy/wcs/tests/test_profiling.py:72:10:</a> PT029 Set the expected warning in `pytest.warns()`
... 2 additional changes omitted for rule PT029
... 51 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT031 | 144 | 144 | 0 | 0 | 0 |
| PT030 | 28 | 28 | 0 | 0 | 0 |
| PT029 | 10 | 10 | 0 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @tjkuson on 2025-01-12 20:58_

---

_@charliermarsh approved on 2025-01-13 01:35_

This looks great, thanks.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-13 01:35_

---

_Label `rule` added by @charliermarsh on 2025-01-13 01:35_

---

_Label `preview` added by @charliermarsh on 2025-01-13 01:35_

---

_@charliermarsh reviewed on 2025-01-13 01:43_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/warns.rs`:32 on 2025-01-13 01:43_

I couldn't figure out from the docs... Is this true? Does the block stop as soon as the warning is triggered? Or is this a copy-paste error from the `raises` variant.

---

_Merged by @charliermarsh on 2025-01-13 01:46_

---

_Closed by @charliermarsh on 2025-01-13 01:46_

---

_@tjkuson reviewed on 2025-01-13 01:49_

---

_Review comment by @tjkuson on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/warns.rs`:32 on 2025-01-13 01:49_

That comment was a copy error, apologies!

---

_Branch deleted on 2025-01-13 01:49_

---

_@charliermarsh reviewed on 2025-01-13 15:51_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/warns.rs`:32 on 2025-01-13 15:51_

Are you able to PR a revision to the docs?

---

_@tjkuson reviewed on 2025-01-13 23:40_

---

_Review comment by @tjkuson on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/warns.rs`:32 on 2025-01-13 23:40_

On it!

---
