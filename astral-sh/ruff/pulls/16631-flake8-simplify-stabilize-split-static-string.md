```yaml
number: 16631
title: "[`flake8-simplify`] Stabilize `split-static-string` (`SIM905`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: brent/sim905-0.10
created_at: 2025-03-11T16:01:45Z
updated_at: 2025-03-11T17:53:35Z
url: https://github.com/astral-sh/ruff/pull/16631
synced_at: 2026-01-10T19:49:01Z
```

# [`flake8-simplify`] Stabilize `split-static-string` (`SIM905`)

---

_Pull request opened by @ntBre on 2025-03-11 16:01_

Summary
--

Stabilizes SIM905 and adds a small addition to the docs. The test was already in the right place.

Test Plan
--

No issues except 2 recent, general issues about whitespace normalization.

---

_Label `rule` added by @ntBre on 2025-03-11 16:01_

---

_Added to milestone `v0.10` by @ntBre on 2025-03-11 16:01_

---

_Comment by @codspeed-hq[bot] on 2025-03-11 16:06_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fsim905-0.10)

### Merging #16631 will **degrade performances by 12.87%**

<sub>Comparing <code>brent/sim905-0.10</code> (540697e) with <code>micha/ruff-0.10</code> (303b061)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/brent%2Fsim905-0.10)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 4.8 ms | 5.5 ms | -12.87% |


---

_Comment by @github-actions[bot] on 2025-03-11 16:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+36 -0 violations, +0 -0 fixes in 7 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/5dae3b5a43565e5bec43c600e008491374e8c887/airflow/example_dags/example_params_ui_tutorial.py#L168'>airflow/example_dags/example_params_ui_tutorial.py:168:26:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/apache/airflow/blob/5dae3b5a43565e5bec43c600e008491374e8c887/scripts/ci/pre_commit/check_min_python_version.py#L29'>scripts/ci/pre_commit/check_min_python_version.py:29:35:</a> SIM905 [*] Consider using a list literal instead of `str.split`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/utils/date_parser.py#L695'>superset/utils/date_parser.py:695:9:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/tests/integration_tests/db_engine_specs/hive_tests.py#L33'>tests/integration_tests/db_engine_specs/hive_tests.py:33:11:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/tests/integration_tests/db_engine_specs/hive_tests.py#L41'>tests/integration_tests/db_engine_specs/hive_tests.py:41:11:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/tests/integration_tests/db_engine_specs/hive_tests.py#L48'>tests/integration_tests/db_engine_specs/hive_tests.py:48:11:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/tests/integration_tests/db_engine_specs/hive_tests.py#L56'>tests/integration_tests/db_engine_specs/hive_tests.py:56:11:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/tests/integration_tests/db_engine_specs/hive_tests.py#L65'>tests/integration_tests/db_engine_specs/hive_tests.py:65:11:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/tests/integration_tests/db_engine_specs/hive_tests.py#L75'>tests/integration_tests/db_engine_specs/hive_tests.py:75:11:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/tests/integration_tests/db_engine_specs/hive_tests.py#L86'>tests/integration_tests/db_engine_specs/hive_tests.py:86:11:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/tests/integration_tests/db_engine_specs/hive_tests.py#L99'>tests/integration_tests/db_engine_specs/hive_tests.py:99:11:</a> SIM905 [*] Consider using a list literal instead of `str.split`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/request_llms/edge_gpt_free.py#L1048'>request_llms/edge_gpt_free.py:1048:46:</a> SIM905 [*] Consider using a list literal instead of `str.split`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/install.py#L5'>scripts/hooks/install.py:5:20:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/uninstall.py#L5'>scripts/hooks/uninstall.py:5:20:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/sri.py#L20'>scripts/sri.py:20:11:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/resources.py#L674'>src/bokeh/resources.py:674:11:</a> SIM905 [*] Consider using a list literal instead of `str.split`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/0d95492048842ec4011727b36a0f4bbecef22ea2/admin/tests/test_integration.py#L542'>admin/tests/test_integration.py:542:27:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/0d95492048842ec4011727b36a0f4bbecef22ea2/admin/tests/test_integration.py#L547'>admin/tests/test_integration.py:547:27:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/0d95492048842ec4011727b36a0f4bbecef22ea2/admin/tests/test_integration.py#L642'>admin/tests/test_integration.py:642:27:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/0d95492048842ec4011727b36a0f4bbecef22ea2/admin/tests/test_integration.py#L643'>admin/tests/test_integration.py:643:27:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/0d95492048842ec4011727b36a0f4bbecef22ea2/admin/tests/test_integration.py#L646'>admin/tests/test_integration.py:646:27:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/0d95492048842ec4011727b36a0f4bbecef22ea2/admin/tests/test_integration.py#L675'>admin/tests/test_integration.py:675:27:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/0d95492048842ec4011727b36a0f4bbecef22ea2/securedrop/pretty_bad_protocol/_parsers.py#L1194'>securedrop/pretty_bad_protocol/_parsers.py:1194:16:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/0d95492048842ec4011727b36a0f4bbecef22ea2/securedrop/pretty_bad_protocol/_parsers.py#L1233'>securedrop/pretty_bad_protocol/_parsers.py:1233:16:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/0d95492048842ec4011727b36a0f4bbecef22ea2/securedrop/pretty_bad_protocol/_parsers.py#L1292'>securedrop/pretty_bad_protocol/_parsers.py:1292:24:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/0d95492048842ec4011727b36a0f4bbecef22ea2/securedrop/pretty_bad_protocol/_parsers.py#L1400'>securedrop/pretty_bad_protocol/_parsers.py:1400:24:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/0d95492048842ec4011727b36a0f4bbecef22ea2/securedrop/pretty_bad_protocol/_parsers.py#L671'>securedrop/pretty_bad_protocol/_parsers.py:671:30:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/0d95492048842ec4011727b36a0f4bbecef22ea2/securedrop/pretty_bad_protocol/gnupg.py#L565'>securedrop/pretty_bad_protocol/gnupg.py:565:26:</a> SIM905 [*] Consider using a list literal instead of `str.split`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/d1aaf2dbd3ad07cbe2f07b91f7653b2446546ea3/tests/test_skbuild_settings.py#L395'>tests/test_skbuild_settings.py:395:12:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/d1aaf2dbd3ad07cbe2f07b91f7653b2446546ea3/tests/test_skbuild_settings.py#L435'>tests/test_skbuild_settings.py:435:12:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/d1aaf2dbd3ad07cbe2f07b91f7653b2446546ea3/tests/test_skbuild_settings.py#L747'>tests/test_skbuild_settings.py:747:12:</a> SIM905 [*] Consider using a list literal instead of `str.split`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/7547cfca16582a47497016b873fa8a14b46b9cf6/astropy/coordinates/tests/test_polarization.py#L39'>astropy/coordinates/tests/test_polarization.py:39:29:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/astropy/astropy/blob/7547cfca16582a47497016b873fa8a14b46b9cf6/astropy/io/ascii/tests/test_ecsv.py#L953'>astropy/io/ascii/tests/test_ecsv.py:953:16:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/astropy/astropy/blob/7547cfca16582a47497016b873fa8a14b46b9cf6/astropy/table/tests/test_info.py#L57'>astropy/table/tests/test_info.py:57:36:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/astropy/astropy/blob/7547cfca16582a47497016b873fa8a14b46b9cf6/astropy/units/tests/test_quantity_erfa_ufuncs.py#L519'>astropy/units/tests/test_quantity_erfa_ufuncs.py:519:25:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/astropy/astropy/blob/7547cfca16582a47497016b873fa8a14b46b9cf6/astropy/units/tests/test_quantity_erfa_ufuncs.py#L523'>astropy/units/tests/test_quantity_erfa_ufuncs.py:523:25:</a> SIM905 [*] Consider using a list literal instead of `str.split`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM905 | 36 | 36 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-03-11 16:20_

All of the ecosystem checks look like true positives. I think the codspeed results should be unrelated to these changes?

---

_Comment by @MichaReiser on 2025-03-11 17:52_

> All of the ecosystem checks look like true positives. I think the codspeed results should be unrelated to these changes?

Yeah, codspeed is probably confused about the base. We might need to rebase the base branch. But let's wait until we merged almost all outstanding PRs

---

_@MichaReiser approved on 2025-03-11 17:52_

---

_Merged by @ntBre on 2025-03-11 17:53_

---

_Closed by @ntBre on 2025-03-11 17:53_

---

_Branch deleted on 2025-03-11 17:53_

---
