```yaml
number: 6283
title: "Avoid `PTH206` with `maxsplit`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/split
created_at: 2023-08-02T18:07:10Z
updated_at: 2023-08-02T18:46:23Z
url: https://github.com/astral-sh/ruff/pull/6283
synced_at: 2026-01-12T02:58:30Z
```

# Avoid `PTH206` with `maxsplit`

---

_Pull request opened by @charliermarsh on 2023-08-02 18:07_

## Summary

Avoid suggesting `Path.parts` when a `maxsplit` is specified, since these behavior differently.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2023-08-02 18:07_

---

_Merged by @charliermarsh on 2023-08-02 18:16_

---

_Closed by @charliermarsh on 2023-08-02 18:16_

---

_Branch deleted on 2023-08-02 18:16_

---

_Comment by @github-actions[bot] on 2023-08-02 18:21_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -1, 0 error(s))

<details><summary>bokeh (+0, -1)</summary>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/38d05d037223234aedeb25e35502abf3332bf0b4/src/bokeh/server/views/multi_root_static_handler.py#L58'>src/bokeh/server/views/multi_root_static_handler.py:58:40:</a> PTH206 Replace `.split(os.sep)` with `Path.parts`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PTH206 | 1 | 0 | 1 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     10.1±0.06ms     4.0 MB/sec    1.00      9.9±0.14ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.02  1988.2±40.47µs     8.4 MB/sec    1.00  1946.5±19.42µs     8.6 MB/sec
formatter/numpy/globals.py                 1.01    222.5±6.59µs    13.3 MB/sec    1.00    221.1±5.97µs    13.3 MB/sec
formatter/pydantic/types.py                1.02      4.2±0.07ms     6.0 MB/sec    1.00      4.2±0.05ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.09ms     3.0 MB/sec    1.01     13.4±0.11ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     5.0 MB/sec    1.00      3.4±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    457.0±2.02µs     6.5 MB/sec    1.01    460.8±1.15µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.03ms     4.3 MB/sec    1.01      5.9±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.07ms     5.9 MB/sec    1.02      7.1±0.02ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1442.4±7.90µs    11.5 MB/sec    1.02   1477.8±5.90µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.1±0.57µs    18.2 MB/sec    1.01    164.5±0.43µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.3 MB/sec    1.01      3.1±0.01ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.04ms     4.1 MB/sec    1.00      9.8±0.04ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1905.6±12.99µs     8.7 MB/sec    1.00  1903.4±18.15µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    200.3±2.42µs    14.7 MB/sec    1.00    199.5±5.57µs    14.8 MB/sec
formatter/pydantic/types.py                1.02      4.3±0.03ms     5.9 MB/sec    1.00      4.2±0.05ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.01     13.2±0.08ms     3.1 MB/sec    1.00     13.1±0.04ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    361.8±5.39µs     8.2 MB/sec    1.00    360.5±5.71µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.0±0.09ms     4.2 MB/sec    1.00      5.9±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.02ms     5.6 MB/sec    1.00      7.2±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1450.9±10.46µs    11.5 MB/sec    1.00  1433.1±11.49µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    145.3±1.67µs    20.3 MB/sec    1.01   146.9±14.89µs    20.1 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.2±0.02ms     7.9 MB/sec    1.00      3.2±0.02ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
