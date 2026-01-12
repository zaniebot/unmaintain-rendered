```yaml
number: 3911
title: Remove old documentation
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: docs
created_at: 2023-04-08T00:43:47Z
updated_at: 2023-04-08T02:51:25Z
url: https://github.com/astral-sh/ruff/pull/3911
synced_at: 2026-01-12T15:55:14Z
```

# Remove old documentation

---

_@evanrittenhouse_

Found this while reading through `CONTRIBUTION.md` tonight - seems like it's a bit outdated

---

_Comment by @github-actions[bot] on 2023-04-08 00:54_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -11, 0 error(s))

<details><summary>airflow (+0, -9)</summary>
<p>

```diff
- dev/breeze/src/airflow_breeze/params/build_prod_params.py:47:27: RUF009 Do not perform function call `get_airflow_extras` in dataclass defaults
- dev/breeze/src/airflow_breeze/params/common_build_params.py:42:27: RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- dev/breeze/src/airflow_breeze/params/common_build_params.py:43:39: RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- dev/breeze/src/airflow_breeze/params/common_build_params.py:54:27: RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- dev/breeze/src/airflow_breeze/params/common_build_params.py:56:25: RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- dev/breeze/src/airflow_breeze/params/shell_params.py:70:27: RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- dev/breeze/src/airflow_breeze/params/shell_params.py:71:39: RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- dev/breeze/src/airflow_breeze/params/shell_params.py:87:27: RUF009 Do not perform function call `os.environ.get` in dataclass defaults
- dev/breeze/src/airflow_breeze/params/shell_params.py:89:25: RUF009 Do not perform function call `os.environ.get` in dataclass defaults
```

</p>
</details>
<details><summary>scikit-build-core (+0, -2)</summary>
<p>

```diff
- src/scikit_build_core/file_api/model/toolchains.py:41:27: RUF009 Do not perform function call `APIVersion` in dataclass defaults
- tests/test_settings.py:23:17: RUF009 Do not perform function call `Path` in dataclass defaults
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.9±0.05ms     2.7 MB/sec    1.01     15.0±0.08ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.01ms     4.4 MB/sec    1.00      3.8±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.01    414.8±1.20µs     7.1 MB/sec    1.00    410.9±1.89µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.01ms     4.0 MB/sec    1.00      6.4±0.02ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.01      7.8±0.03ms     5.2 MB/sec    1.00      7.7±0.02ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1739.9±3.56µs     9.6 MB/sec    1.00   1718.3±3.59µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.4±0.40µs    16.4 MB/sec    1.01    181.6±0.42µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.02ms     7.1 MB/sec    1.00      3.6±0.00ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.0±0.10ms     2.5 MB/sec    1.00     16.0±0.09ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.01ms     4.0 MB/sec    1.00      4.2±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    436.6±4.32µs     6.8 MB/sec    1.00    431.9±7.35µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.03ms     3.7 MB/sec    1.00      6.8±0.02ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.04ms     5.0 MB/sec    1.00      8.2±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1783.5±9.22µs     9.3 MB/sec    1.02  1814.7±143.65µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    186.9±3.08µs    15.8 MB/sec    1.00    186.7±3.92µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.03ms     6.8 MB/sec    1.00      3.7±0.03ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-08 02:51_

---

_Closed by @charliermarsh on 2023-04-08 02:51_

---

_Comment by @charliermarsh on 2023-04-08 02:51_

Thanks!

---
