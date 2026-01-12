```yaml
number: 6220
title: Avoid PERF401 false positive on list access in loop
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/PERF401
created_at: 2023-08-01T03:48:46Z
updated_at: 2023-08-01T04:22:14Z
url: https://github.com/astral-sh/ruff/pull/6220
synced_at: 2026-01-12T15:55:20Z
```

# Avoid PERF401 false positive on list access in loop

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/6210.

---

_Label `bug` added by @charliermarsh on 2023-08-01 03:48_

---

_Merged by @charliermarsh on 2023-08-01 03:56_

---

_Closed by @charliermarsh on 2023-08-01 03:56_

---

_Branch deleted on 2023-08-01 03:56_

---

_Comment by @github-actions[bot] on 2023-08-01 04:05_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -1, 0 error(s))

<details><summary>zulip (+0, -1)</summary>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/ff409342e15624dd6f44c40fcc21a238396ed81e/analytics/lib/fixtures.py#L68'>analytics/lib/fixtures.py:68:9:</a> PERF401 Use a list comprehension to create a transformed list
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PERF401 | 1 | 0 | 1 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.4±0.37ms     3.9 MB/sec    1.00     10.3±0.41ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.04      2.1±0.09ms     8.0 MB/sec    1.00  1988.6±105.54µs     8.4 MB/sec
formatter/numpy/globals.py                 1.03   229.5±13.80µs    12.9 MB/sec    1.00   222.3±14.97µs    13.3 MB/sec
formatter/pydantic/types.py                1.08      4.5±0.20ms     5.7 MB/sec    1.00      4.2±0.18ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.01     15.0±0.55ms     2.7 MB/sec    1.00     14.8±0.45ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.10ms     4.6 MB/sec    1.04      3.7±0.13ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   494.9±15.46µs     6.0 MB/sec    1.03   508.9±20.21µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.17ms     4.0 MB/sec    1.02      6.5±0.24ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.05      8.0±0.20ms     5.1 MB/sec    1.00      7.6±0.30ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1576.4±56.40µs    10.6 MB/sec    1.04  1640.8±49.29µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.7±6.41µs    16.2 MB/sec    1.03    186.3±6.87µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.04      3.5±0.11ms     7.3 MB/sec    1.00      3.3±0.15ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.1±0.14ms     4.0 MB/sec    1.05     10.6±0.09ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1947.9±31.67µs     8.5 MB/sec    1.05      2.0±0.04ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00    213.3±4.97µs    13.8 MB/sec    1.04    221.0±5.46µs    13.4 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.08ms     6.0 MB/sec    1.05      4.5±0.13ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.12ms     3.0 MB/sec    1.00     13.5±0.12ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.04ms     4.7 MB/sec    1.00      3.6±0.05ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   437.8±10.11µs     6.7 MB/sec    1.00    436.9±6.38µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.08ms     4.2 MB/sec    1.00      6.1±0.08ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.06ms     5.4 MB/sec    1.00      7.5±0.09ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1516.2±19.26µs    11.0 MB/sec    1.00  1507.7±15.30µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    168.1±3.10µs    17.6 MB/sec    1.00    168.8±2.65µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.04ms     7.8 MB/sec    1.00      3.3±0.03ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
