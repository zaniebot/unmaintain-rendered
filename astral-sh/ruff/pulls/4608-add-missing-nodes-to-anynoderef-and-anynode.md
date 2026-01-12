```yaml
number: 4608
title: "Add missing nodes to `AnyNodeRef` and `AnyNode`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: add-missing-nodes
created_at: 2023-05-23T15:40:52Z
updated_at: 2023-05-23T16:30:32Z
url: https://github.com/astral-sh/ruff/pull/4608
synced_at: 2026-01-12T15:55:15Z
```

# Add missing nodes to `AnyNodeRef` and `AnyNode`

---

_@MichaReiser_

I missed the nodes that aren't part of any enum. This PR adds the missing nodes to `AnyNodeRef` and `AnyNode`.

It also adds a `NodeKind` enum. It can be useful for testing when you only want to test a path, but not match against the specific nodes.

---

_@charliermarsh approved on 2023-05-23 15:47_

---

_Comment by @github-actions[bot] on 2023-05-23 15:56_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.9±0.10ms     2.9 MB/sec    1.00     13.9±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.00ms     5.0 MB/sec    1.01      3.4±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    410.3±2.33µs     7.2 MB/sec    1.01    415.1±1.13µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.7±0.02ms     4.5 MB/sec    1.01      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.01ms     6.2 MB/sec    1.01      6.6±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1425.6±4.64µs    11.7 MB/sec    1.01   1434.6±1.81µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.4±0.23µs    18.6 MB/sec    1.00    157.6±0.28µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.00ms     8.6 MB/sec    1.01      3.0±0.00ms     8.6 MB/sec
parser/large/dataset.py                    1.00      5.2±0.00ms     7.9 MB/sec    1.00      5.2±0.00ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1019.2±6.65µs    16.3 MB/sec    1.00   1022.2±0.48µs    16.3 MB/sec
parser/numpy/globals.py                    1.00    104.0±0.16µs    28.4 MB/sec    1.02    105.9±0.19µs    27.9 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.00      2.2±0.00ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.05     22.6±1.21ms  1845.0 KB/sec    1.00     21.6±1.05ms  1929.4 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.5±0.26ms     3.0 MB/sec    1.00      5.5±0.19ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   627.2±24.57µs     4.7 MB/sec    1.03   648.6±35.45µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.0±0.36ms     2.8 MB/sec    1.02      9.2±0.51ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.4±0.41ms     3.9 MB/sec    1.00     10.4±0.45ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.08ms     7.6 MB/sec    1.03      2.2±0.10ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   253.3±13.95µs    11.7 MB/sec    1.06   268.6±14.91µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.18ms     5.4 MB/sec    1.00      4.7±0.18ms     5.4 MB/sec
parser/large/dataset.py                    1.01      8.7±0.25ms     4.7 MB/sec    1.00      8.6±0.23ms     4.7 MB/sec
parser/numpy/ctypeslib.py                  1.02  1676.7±64.47µs     9.9 MB/sec    1.00  1651.3±67.26µs    10.1 MB/sec
parser/numpy/globals.py                    1.02   173.2±10.47µs    17.0 MB/sec    1.00    169.4±8.20µs    17.4 MB/sec
parser/pydantic/types.py                   1.02      3.8±0.12ms     6.7 MB/sec    1.00      3.8±0.15ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @MichaReiser on 2023-05-23 16:30_

---

_Closed by @MichaReiser on 2023-05-23 16:30_

---

_Branch deleted on 2023-05-23 16:30_

---

_Label `internal` added by @MichaReiser on 2023-05-23 16:30_

---
