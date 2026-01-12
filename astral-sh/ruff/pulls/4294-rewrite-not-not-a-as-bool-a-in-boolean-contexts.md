```yaml
number: 4294
title: "Rewrite `not not a` as `bool(a)` in boolean contexts"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/not-not
created_at: 2023-05-08T23:28:00Z
updated_at: 2023-05-09T06:02:39Z
url: https://github.com/astral-sh/ruff/pull/4294
synced_at: 2026-01-12T03:56:39Z
```

# Rewrite `not not a` as `bool(a)` in boolean contexts

---

_Pull request opened by @charliermarsh on 2023-05-08 23:28_

Closes #3350.

---

_Marked ready for review by @charliermarsh on 2023-05-08 23:28_

---

_Merged by @charliermarsh on 2023-05-08 23:38_

---

_Closed by @charliermarsh on 2023-05-08 23:38_

---

_Branch deleted on 2023-05-08 23:38_

---

_Comment by @github-actions[bot] on 2023-05-08 23:45_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.0±0.10ms     2.9 MB/sec    1.00     13.8±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    411.5±0.88µs     7.2 MB/sec    1.00    412.2±0.87µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.02ms     4.4 MB/sec    1.00      5.7±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.02ms     5.9 MB/sec    1.00      6.8±0.02ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1477.6±2.99µs    11.3 MB/sec    1.00   1468.6±4.52µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.2±0.50µs    18.2 MB/sec    1.00    162.4±1.14µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.00ms     8.3 MB/sec    1.00      3.1±0.02ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.4±0.01ms     7.5 MB/sec    1.19      6.5±0.00ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00   1057.0±0.69µs    15.8 MB/sec    1.15   1220.6±2.03µs    13.6 MB/sec
parser/numpy/globals.py                    1.00    107.2±0.27µs    27.5 MB/sec    1.12    119.9±1.24µs    24.6 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.01ms    11.0 MB/sec    1.16      2.7±0.01ms     9.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.2±0.19ms     2.5 MB/sec    1.04     16.9±0.17ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.07ms     4.1 MB/sec    1.02      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    485.9±8.48µs     6.1 MB/sec    1.02   494.6±10.29µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.14ms     3.7 MB/sec    1.03      7.1±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.07ms     5.0 MB/sec    1.08      8.8±0.09ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1737.1±23.30µs     9.6 MB/sec    1.07  1864.3±20.58µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    193.3±3.30µs    15.3 MB/sec    1.07    207.1±6.77µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     7.0 MB/sec    1.07      3.9±0.05ms     6.5 MB/sec
parser/large/dataset.py                    1.00      6.7±0.05ms     6.1 MB/sec    1.01      6.7±0.04ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1277.2±16.83µs    13.0 MB/sec    1.01  1288.8±13.81µs    12.9 MB/sec
parser/numpy/globals.py                    1.00    131.6±4.56µs    22.4 MB/sec    1.00    131.9±2.56µs    22.4 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.0 MB/sec    1.00      2.9±0.04ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser reviewed on 2023-05-09 06:02_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/snapshots/ruff__rules__flake8_simplify__tests__SIM208_SIM208.py.snap`:89 on 2023-05-09 06:02_

The message and autofix title now no longer match what the fix is doing (I have a task to move the autofix title to `Fix`)

---
