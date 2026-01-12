```yaml
number: 4263
title: "Box `BindingKind` enum variants"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/binding-size
created_at: 2023-05-07T03:52:25Z
updated_at: 2023-05-08T11:45:46Z
url: https://github.com/astral-sh/ruff/pull/4263
synced_at: 2026-01-12T15:55:15Z
```

# Box `BindingKind` enum variants

---

_@charliermarsh_

## Summary

Boxing the variants reduces the enum size from 48 bytes to 8 bytes. Need to do some deeper profiling to understand whether the tradeoffs here are favorable. 


---

_Comment by @charliermarsh on 2023-05-07 04:28_

I thought this would have a bigger impact. Best I can tell, it's _maybe_ a hair faster but not a significant difference.

---

_Comment by @github-actions[bot] on 2023-05-07 04:29_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.0±0.14ms     2.9 MB/sec    1.00     13.9±0.08ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    412.6±0.71µs     7.2 MB/sec    1.00    411.4±0.65µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.02ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.02ms     5.9 MB/sec    1.01      6.9±0.03ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1477.0±2.65µs    11.3 MB/sec    1.00   1483.6±1.94µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.5±0.28µs    18.3 MB/sec    1.01    163.8±0.62µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.01      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.4±0.00ms     7.5 MB/sec    1.01      5.5±0.01ms     7.4 MB/sec
parser/numpy/ctypeslib.py                  1.00   1053.3±0.69µs    15.8 MB/sec    1.01   1063.5±1.46µs    15.7 MB/sec
parser/numpy/globals.py                    1.00    106.5±0.31µs    27.7 MB/sec    1.01    107.6±0.18µs    27.4 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.5±0.20ms     2.5 MB/sec    1.00     16.3±0.19ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.04ms     4.0 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    488.6±9.35µs     6.0 MB/sec    1.00    488.9±6.26µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.10ms     3.7 MB/sec    1.00      6.9±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.06ms     4.9 MB/sec    1.01      8.3±0.05ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1735.9±14.14µs     9.6 MB/sec    1.02  1768.1±21.52µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.7±2.91µs    15.3 MB/sec    1.03   198.7±10.25µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.01      3.7±0.08ms     6.8 MB/sec
parser/large/dataset.py                    1.02      6.8±0.04ms     6.0 MB/sec    1.00      6.7±0.03ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.01  1292.6±14.22µs    12.9 MB/sec    1.00  1276.5±15.02µs    13.0 MB/sec
parser/numpy/globals.py                    1.01    132.3±2.19µs    22.3 MB/sec    1.00    131.6±1.94µs    22.4 MB/sec
parser/pydantic/types.py                   1.02      2.9±0.05ms     8.8 MB/sec    1.00      2.8±0.04ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Closed by @charliermarsh on 2023-05-07 04:31_

---

_Comment by @charliermarsh on 2023-05-07 04:31_

\cc @MichaReiser - you may find these results interesting.

---

_Comment by @MichaReiser on 2023-05-08 11:45_

> \cc @MichaReiser - you may find these results interesting.

I don't find the results very unexpected. The main issue is that the change exchanges the cost of writing and reading larger data structures with the cost of heap allocating, dereferencing, and decreased cache-locality. 

The change also only reduces the size from 112 bytes to 82 bytes, which is still more than a single cache line. 

Are there any usage patterns that we can make use of? For example, do we write the binding more often than read the `FromImportation`? If so, could we only store the start position (`TextSize`) and use a binary search to find it by traversing the AST or list of statements? 

Or could we store the `StmtId` instead? 

Other mechanisms that we could try is to store some of the optional metadata outside of the `Binding` (`runtime_usage`, `typing_usage`, `synthetic_usage`, `Exceptions`), especially if most `Binding`s have all these values set to `None`. 

---
