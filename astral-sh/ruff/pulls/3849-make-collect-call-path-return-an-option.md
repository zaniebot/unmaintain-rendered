```yaml
number: 3849
title: "Make `collect_call_path` return an `Option`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/call-path-option
created_at: 2023-04-01T15:51:02Z
updated_at: 2023-04-02T02:29:34Z
url: https://github.com/astral-sh/ruff/pull/3849
synced_at: 2026-01-12T15:55:13Z
```

# Make `collect_call_path` return an `Option`

---

_@charliermarsh_

## Summary

This probably should've always returned an `Option`. I suspect that right now, if you ran `collect_call_path` over `foo.bar()`, we'd return `["foo"]`, which is incorrect.


---

_Comment by @github-actions[bot] on 2023-04-01 16:17_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.15ms     2.4 MB/sec    1.02     17.1±0.13ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.03ms     3.9 MB/sec    1.01      4.3±0.02ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    543.0±1.22µs     5.4 MB/sec    1.01    546.0±1.93µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.03ms     3.6 MB/sec    1.02      7.3±0.04ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.8±0.05ms     4.6 MB/sec    1.00      8.7±0.04ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1977.6±8.08µs     8.4 MB/sec    1.00  1957.2±10.87µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    219.2±0.68µs    13.5 MB/sec    1.00    217.2±1.15µs    13.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.02ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     21.9±1.25ms  1905.3 KB/sec    1.02     22.2±1.32ms  1873.3 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.3±0.30ms     3.1 MB/sec    1.00      5.3±0.25ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.03   643.2±44.19µs     4.6 MB/sec    1.00   625.5±28.52µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.03      9.2±0.62ms     2.8 MB/sec    1.00      9.0±0.62ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.06     11.6±0.63ms     3.5 MB/sec    1.00     11.0±0.77ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.09      2.5±0.13ms     6.5 MB/sec    1.00      2.3±0.16ms     7.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   275.1±22.79µs    10.7 MB/sec    1.03   283.7±28.23µs    10.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.28ms     5.3 MB/sec    1.04      5.0±0.28ms     5.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-02 02:29_

---

_Closed by @charliermarsh on 2023-04-02 02:29_

---

_Branch deleted on 2023-04-02 02:29_

---
