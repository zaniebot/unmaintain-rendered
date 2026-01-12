```yaml
number: 18569
title: "Stabilize `pytest-warns-with-multiple-statements` (`PT031`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-pt031
created_at: 2025-06-08T19:23:20Z
updated_at: 2025-06-09T19:09:38Z
url: https://github.com/astral-sh/ruff/pull/18569
synced_at: 2026-01-12T15:56:21Z
```

# Stabilize `pytest-warns-with-multiple-statements` (`PT031`)

---

_@dylwil3_

_No description provided._

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-08 19:23_

---

_Label `rule` added by @dylwil3 on 2025-06-08 19:23_

---

_Comment by @github-actions[bot] on 2025-06-08 19:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+159 -8 violations, +0 -0 fixes in 7 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/tests/ext/commands/test_params.py#L119'>tests/ext/commands/test_params.py:119:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+38 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/70387b845abfd92e12d722d5a7c7f28a75a6cd4d/tests/analysis/swept_langmuir/test_floating_potential.py#L213'>tests/analysis/swept_langmuir/test_floating_potential.py:213:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/70387b845abfd92e12d722d5a7c7f28a75a6cd4d/tests/analysis/test_fit_functions.py#L530'>tests/analysis/test_fit_functions.py:530:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/70387b845abfd92e12d722d5a7c7f28a75a6cd4d/tests/diagnostics/charged_particle_radiography/test_synthetic_radiography.py#L377'>tests/diagnostics/charged_particle_radiography/test_synthetic_radiography.py:377:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/70387b845abfd92e12d722d5a7c7f28a75a6cd4d/tests/formulary/collisions/test_collisions_dimensionless.py#L133'>tests/formulary/collisions/test_collisions_dimensionless.py:133:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/70387b845abfd92e12d722d5a7c7f28a75a6cd4d/tests/formulary/collisions/test_collisions_frequencies.py#L727'>tests/formulary/collisions/test_collisions_frequencies.py:727:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/70387b845abfd92e12d722d5a7c7f28a75a6cd4d/tests/formulary/collisions/test_collisions_lengths.py#L207'>tests/formulary/collisions/test_collisions_lengths.py:207:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/70387b845abfd92e12d722d5a7c7f28a75a6cd4d/tests/formulary/collisions/test_collisions_misc.py#L152'>tests/formulary/collisions/test_collisions_misc.py:152:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/70387b845abfd92e12d722d5a7c7f28a75a6cd4d/tests/formulary/collisions/test_coulomb.py#L134'>tests/formulary/collisions/test_coulomb.py:134:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/70387b845abfd92e12d722d5a7c7f28a75a6cd4d/tests/formulary/collisions/test_coulomb.py#L160'>tests/formulary/collisions/test_coulomb.py:160:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/70387b845abfd92e12d722d5a7c7f28a75a6cd4d/tests/formulary/collisions/test_coulomb.py#L97'>tests/formulary/collisions/test_coulomb.py:97:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/70387b845abfd92e12d722d5a7c7f28a75a6cd4d/tests/formulary/test_lengths.py#L333'>tests/formulary/test_lengths.py:333:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
... 27 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+40 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/always/test_providers_manager.py#L75'>airflow-core/tests/unit/always/test_providers_manager.py:75:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/core/test_configuration.py#L1112'>airflow-core/tests/unit/core/test_configuration.py:1112:17:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/core/test_configuration.py#L1136'>airflow-core/tests/unit/core/test_configuration.py:1136:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/core/test_configuration.py#L1163'>airflow-core/tests/unit/core/test_configuration.py:1163:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/core/test_configuration.py#L1203'>airflow-core/tests/unit/core/test_configuration.py:1203:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/datasets/test_dataset.py#L76'>airflow-core/tests/unit/datasets/test_dataset.py:76:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/utils/test_warnings.py#L41'>airflow-core/tests/unit/utils/test_warnings.py:41:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/amazon/tests/unit/amazon/aws/utils/test_connection_wrapper.py#L121'>providers/amazon/tests/unit/amazon/aws/utils/test_connection_wrapper.py:121:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/google/tests/unit/google/cloud/hooks/test_bigquery.py#L783'>providers/google/tests/unit/google/cloud/hooks/test_bigquery.py:783:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/google/tests/unit/google/cloud/hooks/test_bigquery.py#L828'>providers/google/tests/unit/google/cloud/hooks/test_bigquery.py:828:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/google/tests/unit/google/cloud/hooks/test_bigquery.py#L874'>providers/google/tests/unit/google/cloud/hooks/test_bigquery.py:874:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
... 29 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_formatters.py#L35'>tests/unit/bokeh/models/test_formatters.py:35:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_plots.py#L81'>tests/unit/bokeh/models/test_plots.py:81:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/models/test_sources.py#L283'>tests/unit/bokeh/models/test_sources.py:283:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test_graph.py#L122'>tests/unit/bokeh/plotting/test_graph.py:122:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test_graph.py#L131'>tests/unit/bokeh/plotting/test_graph.py:131:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test_graph.py#L141'>tests/unit/bokeh/plotting/test_graph.py:141:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/plotting/test_graph.py#L175'>tests/unit/bokeh/plotting/test_graph.py:175:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/util/test_token.py#L217'>tests/unit/bokeh/util/test_token.py:217:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/util/test_token.py#L232'>tests/unit/bokeh/util/test_token.py:232:13:</a> PT031 `pytest.warns()` block should contain a single simple statement
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pypa/setuptools/blob/c3f486f0f7ebf8fa141dfd7314cbcaba7370db0b/pkg_resources/tests/test_resources.py#L864'>pkg_resources/tests/test_resources.py:864:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/pypa/setuptools/blob/c3f486f0f7ebf8fa141dfd7314cbcaba7370db0b/setuptools/tests/config/test_pyprojecttoml_dynamic_deps.py#L106'>setuptools/tests/config/test_pyprojecttoml_dynamic_deps.py:106:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/pypa/setuptools/blob/c3f486f0f7ebf8fa141dfd7314cbcaba7370db0b/setuptools/tests/test_build_py.py#L168'>setuptools/tests/test_build_py.py:168:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+0 -8 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/python-trio/trio/blob/8771618c56cab079f7acd80aafe89255e8164408/src/trio/_core/_tests/test_asyncgen.py#L46'>src/trio/_core/_tests/test_asyncgen.py:46:29:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PT031`)
- <a href='https://github.com/python-trio/trio/blob/8771618c56cab079f7acd80aafe89255e8164408/src/trio/_core/_tests/test_guest_mode.py#L441'>src/trio/_core/_tests/test_guest_mode.py:441:25:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PT031`)
- <a href='https://github.com/python-trio/trio/blob/8771618c56cab079f7acd80aafe89255e8164408/src/trio/_tests/test_dtls.py#L796'>src/trio/_tests/test_dtls.py:796:42:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PT031`)
- <a href='https://github.com/python-trio/trio/blob/8771618c56cab079f7acd80aafe89255e8164408/src/trio/_tests/test_dtls.py#L812'>src/trio/_tests/test_dtls.py:812:42:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PT031`)
- <a href='https://github.com/python-trio/trio/blob/8771618c56cab079f7acd80aafe89255e8164408/src/trio/_tests/test_dtls.py#L835'>src/trio/_tests/test_dtls.py:835:42:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PT031`)
- <a href='https://github.com/python-trio/trio/blob/8771618c56cab079f7acd80aafe89255e8164408/src/trio/_tests/test_dtls.py#L852'>src/trio/_tests/test_dtls.py:852:42:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PT031`)
- <a href='https://github.com/python-trio/trio/blob/8771618c56cab079f7acd80aafe89255e8164408/src/trio/_tests/test_subprocess.py#L678'>src/trio/_tests/test_subprocess.py:678:61:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PT031`)
- <a href='https://github.com/python-trio/trio/blob/8771618c56cab079f7acd80aafe89255e8164408/src/trio/_tests/test_subprocess.py#L692'>src/trio/_tests/test_subprocess.py:692:70:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PT031`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+68 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/config/tests/test_configs.py#L524'>astropy/config/tests/test_configs.py:524:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/config/tests/test_configs.py#L564'>astropy/config/tests/test_configs.py:564:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/convolution/tests/test_convolve.py#L1231'>astropy/convolution/tests/test_convolve.py:1231:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/convolution/tests/test_convolve_fft.py#L825'>astropy/convolution/tests/test_convolve_fft.py:825:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/coordinates/tests/test_regression.py#L255'>astropy/coordinates/tests/test_regression.py:255:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/coordinates/tests/test_sky_coord.py#L1859'>astropy/coordinates/tests/test_sky_coord.py:1859:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/cosmology/_src/tests/funcs/test_funcs.py#L279'>astropy/cosmology/_src/tests/funcs/test_funcs.py:279:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/ascii/tests/test_ipac_definitions.py#L101'>astropy/io/ascii/tests/test_ipac_definitions.py:101:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/ascii/tests/test_tdat.py#L125'>astropy/io/ascii/tests/test_tdat.py:125:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/ascii/tests/test_tdat.py#L136'>astropy/io/ascii/tests/test_tdat.py:136:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/ascii/tests/test_tdat.py#L156'>astropy/io/ascii/tests/test_tdat.py:156:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/ascii/tests/test_tdat.py#L404'>astropy/io/ascii/tests/test_tdat.py:404:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/ascii/tests/test_tdat.py#L545'>astropy/io/ascii/tests/test_tdat.py:545:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/ascii/tests/test_tdat.py#L682'>astropy/io/ascii/tests/test_tdat.py:682:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/ascii/tests/test_tdat.py#L683'>astropy/io/ascii/tests/test_tdat.py:683:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/ascii/tests/test_tdat.py#L732'>astropy/io/ascii/tests/test_tdat.py:732:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/ascii/tests/test_tdat.py#L746'>astropy/io/ascii/tests/test_tdat.py:746:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/ascii/tests/test_tdat.py#L798'>astropy/io/ascii/tests/test_tdat.py:798:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/fits/hdu/compressed/tests/test_tiled_compression.py#L165'>astropy/io/fits/hdu/compressed/tests/test_tiled_compression.py:165:5:</a> PT031 `pytest.warns()` block should contain a single simple statement
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/fits/tests/test_header.py#L1956'>astropy/io/fits/tests/test_header.py:1956:9:</a> PT031 `pytest.warns()` block should contain a single simple statement
... 48 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT031 | 159 | 159 | 0 | 0 | 0 |
| RUF100 | 8 | 0 | 8 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @ntBre on 2025-06-08 19:55_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-08 20:26_

---

_Merged by @dylwil3 on 2025-06-09 19:09_

---

_Closed by @dylwil3 on 2025-06-09 19:09_

---

_Branch deleted on 2025-06-09 19:09_

---
