```yaml
number: 4797
title: "Ignore error calls with `exc_info` in TRY400"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/exc_info
created_at: 2023-06-02T04:51:25Z
updated_at: 2023-06-02T05:25:12Z
url: https://github.com/astral-sh/ruff/pull/4797
synced_at: 2026-01-12T03:50:03Z
```

# Ignore error calls with `exc_info` in TRY400

---

_Pull request opened by @charliermarsh on 2023-06-02 04:51_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

TRY400 is focused on flagging `logging.error` calls that fail to capture a traceback. But adding `exc_info` manually does "solve" that problem. We have a separate rule (`G201`) to flag usages of `logging.error` with `exc_info` that can be rewritten as `logging.exception`, so refining TRY400 to allow `exc_info=True` helps avoid any overlapping responsibilities.

Closes #4603.


---

_Renamed from "Ignore error calls with exc_info in TRY400" to "Ignore error calls with `exc_info` in TRY400" by @charliermarsh on 2023-06-02 04:51_

---

_Label `rule` added by @charliermarsh on 2023-06-02 04:51_

---

_Merged by @charliermarsh on 2023-06-02 04:59_

---

_Closed by @charliermarsh on 2023-06-02 04:59_

---

_Branch deleted on 2023-06-02 04:59_

---

_Comment by @github-actions[bot] on 2023-06-02 05:02_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -9, 0 error(s))

<details><summary>airflow (+0, -1)</summary>
<p>

```diff
- airflow/executors/kubernetes_executor.py:725:21: TRY400 Use `logging.exception` instead of `logging.error`
```

</p>
</details>
<details><summary>bokeh (+0, -8)</summary>
<p>

```diff
- src/bokeh/client/connection.py:358:17: TRY400 Use `logging.exception` instead of `logging.error`
- src/bokeh/server/contexts.py:191:13: TRY400 Use `logging.exception` instead of `logging.error`
- src/bokeh/server/contexts.py:197:13: TRY400 Use `logging.exception` instead of `logging.error`
- src/bokeh/server/contexts.py:238:17: TRY400 Use `logging.exception` instead of `logging.error`
- src/bokeh/server/contexts.py:297:17: TRY400 Use `logging.exception` instead of `logging.error`
- src/bokeh/server/protocol_handler.py:99:13: TRY400 Use `logging.exception` instead of `logging.error`
- src/bokeh/server/views/ws.py:247:13: TRY400 Use `logging.exception` instead of `logging.error`
- src/bokeh/server/views/ws.py:259:13: TRY400 Use `logging.exception` instead of `logging.error`
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| TRY400 | 9 | 0 | 9 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.9±0.02ms     2.7 MB/sec    1.01     15.1±0.11ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.01    375.9±1.06µs     7.9 MB/sec    1.00    373.7±1.60µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.01ms     5.4 MB/sec    1.01      7.6±0.02ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1603.1±2.06µs    10.4 MB/sec    1.00   1608.3±2.13µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    176.6±0.48µs    16.7 MB/sec    1.00    177.3±0.90µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.5 MB/sec    1.00      3.4±0.01ms     7.5 MB/sec
parser/large/dataset.py                    1.00      5.7±0.00ms     7.1 MB/sec    1.00      5.7±0.02ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1124.9±13.61µs    14.8 MB/sec    1.00   1120.0±0.94µs    14.9 MB/sec
parser/numpy/globals.py                    1.00    115.9±0.31µs    25.5 MB/sec    1.00    116.0±0.74µs    25.4 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.01ms    10.4 MB/sec    1.00      2.5±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.01     16.9±0.18ms     2.4 MB/sec     1.00     16.7±0.16ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.04ms     3.9 MB/sec     1.00      4.2±0.05ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    506.5±8.24µs     5.8 MB/sec     1.00    504.3±8.91µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.12ms     3.6 MB/sec     1.00      7.1±0.11ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.3±0.11ms     4.9 MB/sec     1.00      8.3±0.08ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1766.1±30.73µs     9.4 MB/sec     1.01  1783.9±27.16µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.7±8.35µs    14.5 MB/sec     1.00    204.3±5.82µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.8 MB/sec     1.01      3.8±0.06ms     6.7 MB/sec
parser/large/dataset.py                    1.03      6.6±0.33ms     6.2 MB/sec     1.00      6.4±0.04ms     6.4 MB/sec
parser/numpy/ctypeslib.py                  1.03  1229.0±120.74µs    13.5 MB/sec    1.00  1195.9±14.19µs    13.9 MB/sec
parser/numpy/globals.py                    1.00    123.8±2.12µs    23.8 MB/sec     1.00    123.7±2.26µs    23.9 MB/sec
parser/pydantic/types.py                   1.01      2.7±0.03ms     9.3 MB/sec     1.00      2.7±0.03ms     9.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
