```yaml
number: 5936
title: "[`flake8-use-pathlib`] Implement `os-sep-split` (`PTH206`)"
type: pull_request
state: merged
author: sbrugman
labels: []
assignees: []
merged: true
base: main
head: os-sep-split-pth206
created_at: 2023-07-21T02:05:13Z
updated_at: 2023-07-23T12:31:18Z
url: https://github.com/astral-sh/ruff/pull/5936
synced_at: 2026-01-12T15:55:20Z
```

# [`flake8-use-pathlib`] Implement `os-sep-split` (`PTH206`)

---

_@sbrugman_

Implements https://github.com/astral-sh/ruff/issues/5905#issuecomment-1644822548

---

_Comment by @github-actions[bot] on 2023-07-21 02:25_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+2, -0, 0 error(s))

<details><summary>airflow (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4c878798ef88a1fa45956163630d71b6fc4f401f/airflow/task/task_runner/cgroup_task_runner.py#L86'>airflow/task/task_runner/cgroup_task_runner.py:86:27:</a> PTH206 Replace `.split(os.sep)` with `Path.parts`
+ <a href='https://github.com/apache/airflow/blob/4c878798ef88a1fa45956163630d71b6fc4f401f/dev/breeze/src/airflow_breeze/commands/setup_commands.py#L112'>dev/breeze/src/airflow_breeze/commands/setup_commands.py:112:73:</a> PTH206 Replace `.split(os.sep)` with `Path.parts`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PTH206 | 2 | 2 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.03ms     4.1 MB/sec    1.00      9.8±0.03ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1902.6±6.16µs     8.8 MB/sec    1.00  1905.1±11.16µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    206.4±0.85µs    14.3 MB/sec    1.01    207.8±0.25µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.0 MB/sec    1.00      4.2±0.02ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.04ms     3.0 MB/sec    1.00     13.6±0.09ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.01ms     4.8 MB/sec    1.00      3.4±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    375.2±1.39µs     7.9 MB/sec    1.00    373.1±1.20µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.04ms     4.1 MB/sec    1.00      6.1±0.03ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.1±0.02ms     5.7 MB/sec    1.00      7.0±0.01ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1433.6±3.99µs    11.6 MB/sec    1.00   1429.7±6.51µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    150.5±0.38µs    19.6 MB/sec    1.00    149.9±0.30µs    19.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.01ms     8.1 MB/sec    1.00      3.1±0.01ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.12ms     3.7 MB/sec    1.07     11.7±0.13ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.03ms     7.8 MB/sec    1.06      2.3±0.05ms     7.4 MB/sec
formatter/numpy/globals.py                 1.00    242.2±7.31µs    12.2 MB/sec    1.05   255.0±16.61µs    11.6 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.07ms     5.4 MB/sec    1.06      5.0±0.08ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.00     15.5±0.13ms     2.6 MB/sec    1.00     15.4±0.21ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.28      5.2±3.76ms     3.2 MB/sec    1.00      4.0±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    475.8±6.56µs     6.2 MB/sec    1.02    486.4±6.22µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.07ms     3.7 MB/sec    1.01      7.0±0.09ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.16ms     5.1 MB/sec    1.00      8.0±0.07ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1658.1±20.38µs    10.0 MB/sec    1.01  1672.0±19.36µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    184.6±3.09µs    16.0 MB/sec    1.03    190.5±3.25µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.03ms     7.2 MB/sec    1.01      3.6±0.08ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @sbrugman on 2023-07-21 02:26_

Detected violations in airflow by the ecosystem are correct

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_use_pathlib/rules/os_sep_split.rs`:71 on 2023-07-21 11:06_

```suggestion
        // `.split(os.sep)`
        let [arg] = args else {
```

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_use_pathlib/rules/os_sep_split.rs`:77 on 2023-07-21 11:07_

```suggestion
        // `.split(sep=os.sep)`
        let Some(keyword) = find_keyword(keywords, "sep") else {
```

---

_@konstin approved on 2023-07-21 11:37_

---

_Comment by @konstin on 2023-07-23 10:22_

thanks

---

_Merged by @konstin on 2023-07-23 10:22_

---

_Closed by @konstin on 2023-07-23 10:22_

---

_Branch deleted on 2023-07-23 12:31_

---
