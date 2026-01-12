```yaml
number: 6039
title: "Move `flake8-executable` rules out of physical lines checker"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/shebang
created_at: 2023-07-24T17:58:37Z
updated_at: 2023-07-24T18:38:06Z
url: https://github.com/astral-sh/ruff/pull/6039
synced_at: 2026-01-12T15:55:20Z
```

# Move `flake8-executable` rules out of physical lines checker

---

_@charliermarsh_

## Summary

These only need the token stream, and we always prefer token-based to physical line-based rules.

There are a few other changes snuck in here:

- Renaming the rule files to match the diagnostic names (likely an error).
- The "leading whitespace before shebang" rule now works regardless of where the comment occurs (i.e., if the shebang is on the second line, and the first line is blank, we flag and remove that leading whitespace).


---

_@charliermarsh reviewed on 2023-07-24 17:58_

---

_Review comment by @charliermarsh on `crates/ruff/src/registry.rs`:292 on 2023-07-24 17:58_

(Moved the executable rules to `LintSource::Tokens`, then sorted both sections.

---

_@charliermarsh reviewed on 2023-07-24 17:59_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_executable/snapshots/ruff__rules__flake8_executable__tests__EXE005_2.py.snap`:8 on 2023-07-24 17:59_

This is intentional, I think we should include this in the diagnostic range.

---

_Label `internal` added by @charliermarsh on 2023-07-24 17:59_

---

_Comment by @github-actions[bot] on 2023-07-24 18:12_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+18, -18, 0 error(s))

<details><summary>airflow (+17, -17)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/airflow/__main__.py#L1'>airflow/__main__.py:1:1:</a> EXE001 Shebang is present but file is not executable
- <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/airflow/__main__.py#L1'>airflow/__main__.py:1:3:</a> EXE001 Shebang is present but file is not executable
+ <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/airflow/cli/cli_config.py#L1'>airflow/cli/cli_config.py:1:1:</a> EXE001 Shebang is present but file is not executable
- <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/airflow/cli/cli_config.py#L1'>airflow/cli/cli_config.py:1:3:</a> EXE001 Shebang is present but file is not executable
+ <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/airflow/cli/cli_parser.py#L1'>airflow/cli/cli_parser.py:1:1:</a> EXE001 Shebang is present but file is not executable
- <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/airflow/cli/cli_parser.py#L1'>airflow/cli/cli_parser.py:1:3:</a> EXE001 Shebang is present but file is not executable
+ <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/airflow/security/kerberos.py#L1'>airflow/security/kerberos.py:1:1:</a> EXE001 Shebang is present but file is not executable
- <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/airflow/security/kerberos.py#L1'>airflow/security/kerberos.py:1:3:</a> EXE001 Shebang is present but file is not executable
+ <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/airflow/security/utils.py#L1'>airflow/security/utils.py:1:1:</a> EXE001 Shebang is present but file is not executable
- <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/airflow/security/utils.py#L1'>airflow/security/utils.py:1:3:</a> EXE001 Shebang is present but file is not executable
+ <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/airflow/utils/dot_renderer.py#L1'>airflow/utils/dot_renderer.py:1:1:</a> EXE001 Shebang is present but file is not executable
- <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/airflow/utils/dot_renderer.py#L1'>airflow/utils/dot_renderer.py:1:3:</a> EXE001 Shebang is present but file is not executable
+ <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/airflow/www/gunicorn_config.py#L1'>airflow/www/gunicorn_config.py:1:1:</a> EXE001 Shebang is present but file is not executable
- <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/airflow/www/gunicorn_config.py#L1'>airflow/www/gunicorn_config.py:1:3:</a> EXE001 Shebang is present but file is not executable
+ <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/scripts/ci/runners/sync_authors.py#L1'>scripts/ci/runners/sync_authors.py:1:1:</a> EXE001 Shebang is present but file is not executable
- <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/scripts/ci/runners/sync_authors.py#L1'>scripts/ci/runners/sync_authors.py:1:3:</a> EXE001 Shebang is present but file is not executable
+ <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/scripts/in_container/filter_out_warnings.py#L1'>scripts/in_container/filter_out_warnings.py:1:1:</a> EXE001 Shebang is present but file is not executable
- <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/scripts/in_container/filter_out_warnings.py#L1'>scripts/in_container/filter_out_warnings.py:1:3:</a> EXE001 Shebang is present but file is not executable
+ <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/scripts/in_container/remove_arm_packages.py#L1'>scripts/in_container/remove_arm_packages.py:1:1:</a> EXE001 Shebang is present but file is not executable
- <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/scripts/in_container/remove_arm_packages.py#L1'>scripts/in_container/remove_arm_packages.py:1:3:</a> EXE001 Shebang is present but file is not executable
+ <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/scripts/in_container/run_resource_check.py#L1'>scripts/in_container/run_resource_check.py:1:1:</a> EXE001 Shebang is present but file is not executable
- <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/scripts/in_container/run_resource_check.py#L1'>scripts/in_container/run_resource_check.py:1:3:</a> EXE001 Shebang is present but file is not executable
+ <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/tests/cli/test_cli_parser.py#L1'>tests/cli/test_cli_parser.py:1:1:</a> EXE001 Shebang is present but file is not executable
- <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/tests/cli/test_cli_parser.py#L1'>tests/cli/test_cli_parser.py:1:3:</a> EXE001 Shebang is present but file is not executable
+ <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/tests/providers/google/cloud/transfers/test_gcs_to_sftp.py#L1'>tests/providers/google/cloud/transfers/test_gcs_to_sftp.py:1:1:</a> EXE001 Shebang is present but file is not executable
- <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/tests/providers/google/cloud/transfers/test_gcs_to_sftp.py#L1'>tests/providers/google/cloud/transfers/test_gcs_to_sftp.py:1:3:</a> EXE001 Shebang is present but file is not executable
+ <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/tests/providers/google/cloud/transfers/test_sftp_to_gcs.py#L1'>tests/providers/google/cloud/transfers/test_sftp_to_gcs.py:1:1:</a> EXE001 Shebang is present but file is not executable
- <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/tests/providers/google/cloud/transfers/test_sftp_to_gcs.py#L1'>tests/providers/google/cloud/transfers/test_sftp_to_gcs.py:1:3:</a> EXE001 Shebang is present but file is not executable
+ <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/tests/system/providers/google/cloud/dataproc/resources/hello_world.py#L1'>tests/system/providers/google/cloud/dataproc/resources/hello_world.py:1:1:</a> EXE001 Shebang is present but file is not executable
- <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/tests/system/providers/google/cloud/dataproc/resources/hello_world.py#L1'>tests/system/providers/google/cloud/dataproc/resources/hello_world.py:1:3:</a> EXE001 Shebang is present but file is not executable
+ <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/tests/test_utils/get_all_tests.py#L1'>tests/test_utils/get_all_tests.py:1:1:</a> EXE001 Shebang is present but file is not executable
- <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/tests/test_utils/get_all_tests.py#L1'>tests/test_utils/get_all_tests.py:1:3:</a> EXE001 Shebang is present but file is not executable
+ <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/tests/utils/test_dot_renderer.py#L1'>tests/utils/test_dot_renderer.py:1:1:</a> EXE001 Shebang is present but file is not executable
- <a href='https://github.com/apache/airflow/blob/031e3945e44030d4f085753ffdc43dc104708f91/tests/utils/test_dot_renderer.py#L1'>tests/utils/test_dot_renderer.py:1:3:</a> EXE001 Shebang is present but file is not executable
</pre>

</p>
</details>
<details><summary>bokeh (+1, -1)</summary>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/examples/output/apis/server_document/bokeh_server.py#L1'>examples/output/apis/server_document/bokeh_server.py:1:1:</a> EXE001 Shebang is present but file is not executable
- <a href='https://github.com/bokeh/bokeh/blob/5ad0156dc652746e1b971aaab98bceaa32431e3b/examples/output/apis/server_document/bokeh_server.py#L1'>examples/output/apis/server_document/bokeh_server.py:1:3:</a> EXE001 Shebang is present but file is not executable
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| EXE001 | 36 | 18 | 18 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.16ms     3.7 MB/sec    1.00     10.9±0.07ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.02ms     7.5 MB/sec    1.00      2.2±0.06ms     7.5 MB/sec
formatter/numpy/globals.py                 1.04   262.2±10.04µs    11.3 MB/sec    1.00    251.8±3.29µs    11.7 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.06ms     5.4 MB/sec    1.00      4.8±0.04ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.6±0.43ms     2.6 MB/sec    1.00     15.5±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.02ms     4.3 MB/sec    1.01      3.9±0.02ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.05   528.8±32.57µs     5.6 MB/sec    1.00    505.2±4.29µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.1±0.23ms     3.6 MB/sec    1.00      7.0±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.08ms     5.2 MB/sec    1.01      7.9±0.09ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1676.4±16.26µs     9.9 MB/sec    1.00  1667.0±17.08µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.06    195.2±8.42µs    15.1 MB/sec    1.00    184.5±1.70µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.2 MB/sec    1.01      3.6±0.02ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.06ms     3.7 MB/sec    1.01     11.0±0.17ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.01ms     7.9 MB/sec    1.00      2.1±0.02ms     7.9 MB/sec
formatter/numpy/globals.py                 1.00    231.3±3.94µs    12.8 MB/sec    1.02    234.9±6.08µs    12.6 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.02ms     5.4 MB/sec    1.00      4.7±0.04ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.11ms     2.7 MB/sec    1.00     15.2±0.13ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.03ms     4.1 MB/sec    1.00      4.0±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    408.6±5.50µs     7.2 MB/sec    1.01    412.5±6.08µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.05ms     3.7 MB/sec    1.01      7.0±0.05ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.04ms     5.0 MB/sec    1.00      8.1±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1636.4±15.80µs    10.2 MB/sec    1.00   1626.8±9.35µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.1±1.62µs    17.1 MB/sec    1.01    173.3±3.02µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.1 MB/sec    1.01      3.6±0.02ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_executable/snapshots/ruff__rules__flake8_executable__tests__EXE005_2.py.snap`:8 on 2023-07-24 18:15_

Agreed.

---

_@zanieb reviewed on 2023-07-24 18:15_

---

_Merged by @charliermarsh on 2023-07-24 18:38_

---

_Closed by @charliermarsh on 2023-07-24 18:38_

---

_Branch deleted on 2023-07-24 18:38_

---
