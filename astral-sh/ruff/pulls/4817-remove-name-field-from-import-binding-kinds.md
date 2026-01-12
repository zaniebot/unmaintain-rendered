```yaml
number: 4817
title: "Remove `name` field from import binding kinds"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/name
created_at: 2023-06-02T19:08:20Z
updated_at: 2023-06-03T03:22:45Z
url: https://github.com/astral-sh/ruff/pull/4817
synced_at: 2026-01-12T03:50:03Z
```

# Remove `name` field from import binding kinds

---

_Pull request opened by @charliermarsh on 2023-06-02 19:08_

## Summary

We already store the name as the key in the symbol table, so there's no need to store it as both a key _and_ a value. This reduces the size of `BindingKind` from 48 to 32 bytes.


---

_Renamed from "Rename `name` field from import binding kinds" to "Remove `name` field from import binding kinds" by @charliermarsh on 2023-06-02 19:26_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-06-02 19:29_

---

_Comment by @github-actions[bot] on 2023-06-02 19:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.9±0.04ms     2.7 MB/sec    1.01     15.1±0.06ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    371.9±1.55µs     7.9 MB/sec    1.00    373.2±0.72µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.02ms     4.1 MB/sec    1.00      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.01ms     5.5 MB/sec    1.00      7.4±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1574.7±3.66µs    10.6 MB/sec    1.00   1579.3±4.23µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.4±0.23µs    17.2 MB/sec    1.01    173.1±0.24µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.6 MB/sec    1.00      3.4±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.01      5.8±0.01ms     7.0 MB/sec    1.00      5.7±0.03ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.01   1133.2±0.66µs    14.7 MB/sec    1.00   1121.1±2.89µs    14.9 MB/sec
parser/numpy/globals.py                    1.01    116.5±0.49µs    25.3 MB/sec    1.00    115.6±0.57µs    25.5 MB/sec
parser/pydantic/types.py                   1.01      2.5±0.01ms    10.3 MB/sec    1.00      2.5±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.2±0.13ms     2.4 MB/sec    1.00     17.0±0.18ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.4±0.03ms     3.8 MB/sec    1.00      4.3±0.03ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.02    452.2±8.93µs     6.5 MB/sec    1.00    443.7±4.76µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.04ms     3.5 MB/sec    1.00      7.3±0.04ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.6±0.04ms     4.8 MB/sec    1.00      8.5±0.04ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1798.7±11.83µs     9.3 MB/sec    1.00  1803.6±10.72µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.3±2.09µs    15.1 MB/sec    1.00    195.1±4.63µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.04ms     6.5 MB/sec    1.00      3.9±0.10ms     6.5 MB/sec
parser/large/dataset.py                    1.09      7.3±0.03ms     5.6 MB/sec    1.00      6.7±0.02ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.07   1359.1±8.60µs    12.3 MB/sec    1.00  1268.3±12.23µs    13.1 MB/sec
parser/numpy/globals.py                    1.04    138.6±0.94µs    21.3 MB/sec    1.00    132.7±1.08µs    22.2 MB/sec
parser/pydantic/types.py                   1.08      3.1±0.02ms     8.3 MB/sec    1.00      2.8±0.01ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-06-02 21:47_

---

_Comment by @konstin on 2023-06-02 21:47_

is the ecosystem ci change related?

---

_Merged by @charliermarsh on 2023-06-03 03:02_

---

_Closed by @charliermarsh on 2023-06-03 03:02_

---

_Branch deleted on 2023-06-03 03:02_

---
