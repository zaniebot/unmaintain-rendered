```yaml
number: 4399
title: Workaround for maturin bug
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: workaround_maturin_bug
created_at: 2023-05-12T18:50:20Z
updated_at: 2023-05-12T19:27:26Z
url: https://github.com/astral-sh/ruff/pull/4399
synced_at: 2026-01-12T03:50:02Z
```

# Workaround for maturin bug

---

_Pull request opened by @konstin on 2023-05-12 18:50_

I consider this a maturin bug, this is a quick fix for now, i'll debug into maturin later

---

_Review requested from @charliermarsh by @konstin on 2023-05-12 18:50_

---

_Merged by @charliermarsh on 2023-05-12 18:55_

---

_Closed by @charliermarsh on 2023-05-12 18:55_

---

_Branch deleted on 2023-05-12 18:55_

---

_Comment by @github-actions[bot] on 2023-05-12 19:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.4±0.07ms     2.5 MB/sec    1.01     16.5±0.30ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.01ms     4.1 MB/sec    1.00      4.0±0.01ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    488.2±1.32µs     6.0 MB/sec    1.00    489.8±1.08µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.02ms     3.7 MB/sec    1.00      6.8±0.02ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.02ms     5.1 MB/sec    1.16      9.2±3.61ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1728.2±5.55µs     9.6 MB/sec    1.33      2.3±0.60ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    205.9±8.53µs    14.3 MB/sec    1.01   206.9±16.45µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.7±0.11ms     6.9 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
parser/large/dataset.py                    1.00      6.4±0.01ms     6.4 MB/sec    1.02      6.5±0.01ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00   1245.7±2.24µs    13.4 MB/sec    1.01   1261.0±3.10µs    13.2 MB/sec
parser/numpy/globals.py                    1.00    126.7±0.30µs    23.3 MB/sec    1.06    134.4±6.39µs    22.0 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.01ms     9.4 MB/sec    1.02      2.8±0.01ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.4±0.14ms     2.5 MB/sec    1.02     16.7±0.10ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.04ms     3.9 MB/sec    1.01      4.3±0.03ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    435.7±4.18µs     6.8 MB/sec    1.01    438.7±5.63µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.06ms     3.6 MB/sec    1.02      7.1±0.06ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.04ms     4.9 MB/sec    1.02      8.4±0.08ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1753.9±24.34µs     9.5 MB/sec    1.02  1789.1±19.04µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    186.5±1.66µs    15.8 MB/sec    1.03    192.0±4.56µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.02ms     6.8 MB/sec    1.01      3.8±0.03ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.9±0.02ms     5.9 MB/sec    1.00      6.9±0.03ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.01   1309.2±6.80µs    12.7 MB/sec    1.00  1300.2±10.47µs    12.8 MB/sec
parser/numpy/globals.py                    1.00    134.8±1.09µs    21.9 MB/sec    1.00    134.7±0.91µs    21.9 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.03ms     8.7 MB/sec    1.00      2.9±0.02ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
