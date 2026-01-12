```yaml
number: 6487
title: "[`pylint`] Implement `subprocess-run-check` (`W1510`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
assignees: []
merged: true
base: main
head: PLW1510
created_at: 2023-08-10T21:45:31Z
updated_at: 2023-08-11T07:27:24Z
url: https://github.com/astral-sh/ruff/pull/6487
synced_at: 2026-01-12T15:55:21Z
```

# [`pylint`] Implement `subprocess-run-check` (`W1510`)

---

_@tjkuson_

## Summary

Implements  [`subprocess-run-check` (`W1510`)](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/subprocess-run-check.html) as  `subprocess-run-without-check` (`PLW1510`). Includes documentation.

Related to #970.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-08-10 21:57_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+37, -0, 0 error(s))

<details><summary>airflow (+10, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/7d87d71e053712a96c5a14da38b310a69667de7d/airflow/providers/google/cloud/hooks/dataflow.py#L1013'>airflow/providers/google/cloud/hooks/dataflow.py:1013:20:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/airflow/blob/7d87d71e053712a96c5a14da38b310a69667de7d/dev/breeze/src/airflow_breeze/utils/publish_docs_builder.py#L162'>dev/breeze/src/airflow_breeze/utils/publish_docs_builder.py:162:30:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/airflow/blob/7d87d71e053712a96c5a14da38b310a69667de7d/dev/breeze/src/airflow_breeze/utils/publish_docs_builder.py#L239'>dev/breeze/src/airflow_breeze/utils/publish_docs_builder.py:239:30:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/airflow/blob/7d87d71e053712a96c5a14da38b310a69667de7d/docs/exts/docs_build/docs_builder.py#L163'>docs/exts/docs_build/docs_builder.py:163:30:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/airflow/blob/7d87d71e053712a96c5a14da38b310a69667de7d/docs/exts/docs_build/docs_builder.py#L240'>docs/exts/docs_build/docs_builder.py:240:30:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/airflow/blob/7d87d71e053712a96c5a14da38b310a69667de7d/scripts/ci/pre_commit/pre_commit_update_common_sql_api_stubs.py#L335'>scripts/ci/pre_commit/pre_commit_update_common_sql_api_stubs.py:335:5:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/airflow/blob/7d87d71e053712a96c5a14da38b310a69667de7d/scripts/in_container/remove_arm_packages.py#L47'>scripts/in_container/remove_arm_packages.py:47:5:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/airflow/blob/7d87d71e053712a96c5a14da38b310a69667de7d/scripts/tools/initialize_virtualenv.py#L172'>scripts/tools/initialize_virtualenv.py:172:5:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/airflow/blob/7d87d71e053712a96c5a14da38b310a69667de7d/scripts/tools/initialize_virtualenv.py#L181'>scripts/tools/initialize_virtualenv.py:181:5:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/apache/airflow/blob/7d87d71e053712a96c5a14da38b310a69667de7d/scripts/tools/initialize_virtualenv.py#L97'>scripts/tools/initialize_virtualenv.py:97:9:</a> PLW1510 `subprocess.run` without explicit `check` argument
</pre>

</p>
</details>
<details><summary>bokeh (+18, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/df464aff6334cbf2f11a844768476b3b5e49db9f/release/system.py#L43'>release/system.py:43:18:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/df464aff6334cbf2f11a844768476b3b5e49db9f/scripts/hooks/install.py#L5'>scripts/hooks/install.py:5:5:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/df464aff6334cbf2f11a844768476b3b5e49db9f/scripts/hooks/protect_branches.py#L10'>scripts/hooks/protect_branches.py:10:22:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/df464aff6334cbf2f11a844768476b3b5e49db9f/scripts/hooks/uninstall.py#L5'>scripts/hooks/uninstall.py:5:5:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/df464aff6334cbf2f11a844768476b3b5e49db9f/setup.py#L52'>setup.py:52:16:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/df464aff6334cbf2f11a844768476b3b5e49db9f/tests/codebase/test_eslint.py#L37'>tests/codebase/test_eslint.py:37:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/df464aff6334cbf2f11a844768476b3b5e49db9f/tests/codebase/test_isort.py#L58'>tests/codebase/test_isort.py:58:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/df464aff6334cbf2f11a844768476b3b5e49db9f/tests/codebase/test_js_license_set.py#L50'>tests/codebase/test_js_license_set.py:50:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/df464aff6334cbf2f11a844768476b3b5e49db9f/tests/codebase/test_license.py#L40'>tests/codebase/test_license.py:40:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/df464aff6334cbf2f11a844768476b3b5e49db9f/tests/codebase/test_no_client_server_common.py#L48'>tests/codebase/test_no_client_server_common.py:48:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/df464aff6334cbf2f11a844768476b3b5e49db9f/tests/codebase/test_no_client_server_common.py#L57'>tests/codebase/test_no_client_server_common.py:57:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/df464aff6334cbf2f11a844768476b3b5e49db9f/tests/codebase/test_no_ipython_common.py#L51'>tests/codebase/test_no_ipython_common.py:51:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/df464aff6334cbf2f11a844768476b3b5e49db9f/tests/codebase/test_no_pandas_common.py#L53'>tests/codebase/test_no_pandas_common.py:53:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/df464aff6334cbf2f11a844768476b3b5e49db9f/tests/codebase/test_no_selenium_common.py#L52'>tests/codebase/test_no_selenium_common.py:52:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/df464aff6334cbf2f11a844768476b3b5e49db9f/tests/codebase/test_no_tornado_common.py#L55'>tests/codebase/test_no_tornado_common.py:55:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/df464aff6334cbf2f11a844768476b3b5e49db9f/tests/codebase/test_no_typing_extensions_common.py#L49'>tests/codebase/test_no_typing_extensions_common.py:49:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/df464aff6334cbf2f11a844768476b3b5e49db9f/tests/codebase/test_ruff.py#L33'>tests/codebase/test_ruff.py:33:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/bokeh/bokeh/blob/df464aff6334cbf2f11a844768476b3b5e49db9f/tests/support/util/project.py#L46'>tests/support/util/project.py:46:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
</pre>

</p>
</details>
<details><summary>cibuildwheel (+6, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/c655dbdaab67bc855f9b6a421342d4a3495a02a6/bin/bump_version.py#L140'>bin/bump_version.py:140:5:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/pypa/cibuildwheel/blob/c655dbdaab67bc855f9b6a421342d4a3495a02a6/bin/bump_version.py#L63'>bin/bump_version.py:63:26:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/pypa/cibuildwheel/blob/c655dbdaab67bc855f9b6a421342d4a3495a02a6/bin/make_dependency_update_pr.py#L16'>bin/make_dependency_update_pr.py:16:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/pypa/cibuildwheel/blob/c655dbdaab67bc855f9b6a421342d4a3495a02a6/bin/run_example_ci_configs.py#L20'>bin/run_example_ci_configs.py:20:12:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/pypa/cibuildwheel/blob/c655dbdaab67bc855f9b6a421342d4a3495a02a6/bin/sample_build.py#L27'>bin/sample_build.py:27:14:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/pypa/cibuildwheel/blob/c655dbdaab67bc855f9b6a421342d4a3495a02a6/bin/update_how_it_works_image.py#L22'>bin/update_how_it_works_image.py:22:5:</a> PLW1510 `subprocess.run` without explicit `check` argument
</pre>

</p>
</details>
<details><summary>scikit-build (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build/blob/676e110315a971abb856edbd6df0c74293e5ba2d/tests/__init__.py#L185'>tests/__init__.py:185:13:</a> PLW1510 `subprocess.run` without explicit `check` argument
</pre>

</p>
</details>
<details><summary>zulip (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/27b5b0dc267a05cdc47cccd48ec1b3555e6955aa/tools/lib/pretty_print.py#L183'>tools/lib/pretty_print.py:183:9:</a> PLW1510 `subprocess.run` without explicit `check` argument
+ <a href='https://github.com/zulip/zulip/blob/27b5b0dc267a05cdc47cccd48ec1b3555e6955aa/zerver/tests/test_email_mirror.py#L1489'>zerver/tests/test_email_mirror.py:1489:13:</a> PLW1510 `subprocess.run` without explicit `check` argument
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLW1510 | 37 | 37 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.01ms     4.9 MB/sec    1.01      8.4±0.02ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1623.4±2.82µs    10.3 MB/sec    1.01  1631.9±23.11µs    10.2 MB/sec
formatter/numpy/globals.py                 1.00    172.8±0.18µs    17.1 MB/sec    1.01    174.5±2.88µs    16.9 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.01ms     7.2 MB/sec    1.00      3.6±0.01ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.00     10.4±0.01ms     3.9 MB/sec    1.01     10.6±0.01ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.00ms     5.8 MB/sec    1.01      2.9±0.01ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    322.8±0.90µs     9.1 MB/sec    1.00    320.5±1.18µs     9.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.01ms     4.7 MB/sec    1.01      5.5±0.02ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.01ms     7.5 MB/sec    1.01      5.5±0.01ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1139.5±2.88µs    14.6 MB/sec    1.00   1144.5±3.76µs    14.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    118.9±0.48µs    24.8 MB/sec    1.00    118.8±0.65µs    24.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.00ms    10.3 MB/sec    1.00      2.5±0.01ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.4±0.41ms     3.3 MB/sec    1.09     13.5±0.41ms     3.0 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.18ms     6.8 MB/sec    1.05      2.6±0.10ms     6.5 MB/sec
formatter/numpy/globals.py                 1.00   272.6±16.11µs    10.8 MB/sec    1.04   283.4±29.72µs    10.4 MB/sec
formatter/pydantic/types.py                1.00      5.3±0.22ms     4.8 MB/sec    1.09      5.8±0.25ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.00     16.2±0.63ms     2.5 MB/sec    1.09     17.7±0.64ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.18ms     3.8 MB/sec    1.09      4.8±0.25ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   549.6±27.48µs     5.4 MB/sec    1.08   593.7±18.18µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.2±0.27ms     3.1 MB/sec    1.11      9.1±0.26ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.22ms     4.7 MB/sec    1.10      9.6±0.45ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1817.0±77.34µs     9.2 MB/sec    1.10  1999.2±53.42µs     8.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    223.1±9.21µs    13.2 MB/sec    1.08    241.1±9.32µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.13ms     6.7 MB/sec    1.12      4.3±0.16ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh approved on 2023-08-11 00:54_

---

_Label `rule` added by @charliermarsh on 2023-08-11 00:54_

---

_Merged by @charliermarsh on 2023-08-11 00:54_

---

_Closed by @charliermarsh on 2023-08-11 00:54_

---

_Comment by @charliermarsh on 2023-08-11 00:54_

Looks great, thanks.

---

_Branch deleted on 2023-08-11 07:27_

---
