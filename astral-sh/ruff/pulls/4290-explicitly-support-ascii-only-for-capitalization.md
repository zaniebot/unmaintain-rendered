```yaml
number: 4290
title: Explicitly support ASCII-only for capitalization checks
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ascii-d403
created_at: 2023-05-08T19:34:43Z
updated_at: 2023-05-08T20:08:18Z
url: https://github.com/astral-sh/ruff/pull/4290
synced_at: 2026-01-12T03:56:39Z
```

# Explicitly support ASCII-only for capitalization checks

---

_Pull request opened by @charliermarsh on 2023-05-08 19:34_

Apparently (like pydocstyle), the capitalization rule only runs on pure-ASCII words. This PR documents that limitation, and removes some guardrails that only apply to non-ASCII cases.


---

_@charliermarsh reviewed on 2023-05-08 19:35_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pydocstyle/rules/capitalized.rs`:49 on 2023-05-08 19:35_

I think our `uppercase_first_char` check will always be true here anyway, so this seems unnecessary? And it requires an allocation.

---

_Marked ready for review by @charliermarsh on 2023-05-08 19:37_

---

_Merged by @charliermarsh on 2023-05-08 19:41_

---

_Closed by @charliermarsh on 2023-05-08 19:41_

---

_Branch deleted on 2023-05-08 19:41_

---

_Comment by @github-actions[bot] on 2023-05-08 19:49_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.5±0.12ms     2.8 MB/sec    1.03     14.9±0.10ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.03ms     4.9 MB/sec    1.02      3.5±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    415.0±0.92µs     7.1 MB/sec    1.01    421.2±1.38µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.06ms     4.3 MB/sec    1.02      6.1±0.03ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.05ms     5.8 MB/sec    1.06      7.5±0.04ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1489.0±2.40µs    11.2 MB/sec    1.05  1563.9±10.72µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.5±0.24µs    18.0 MB/sec    1.03    168.6±0.20µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.06      3.3±0.02ms     7.8 MB/sec
parser/large/dataset.py                    1.01      5.5±0.02ms     7.4 MB/sec    1.00      5.4±0.01ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.01   1061.4±0.74µs    15.7 MB/sec    1.00   1054.0±1.05µs    15.8 MB/sec
parser/numpy/globals.py                    1.01    107.5±0.17µs    27.4 MB/sec    1.00    106.9±0.34µs    27.6 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.4±0.12ms     2.5 MB/sec    1.00     16.5±0.24ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.02ms     4.0 MB/sec    1.01      4.3±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    428.3±5.76µs     6.9 MB/sec    1.00    429.9±7.59µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.03ms     3.6 MB/sec    1.01      7.0±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.05ms     4.8 MB/sec    1.00      8.4±0.02ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1754.1±9.15µs     9.5 MB/sec    1.01  1769.4±14.15µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    187.8±1.53µs    15.7 MB/sec    1.01    189.9±5.12µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.01ms     6.8 MB/sec    1.01      3.8±0.02ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.8±0.03ms     6.0 MB/sec    1.03      6.9±0.02ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1289.1±10.19µs    12.9 MB/sec    1.02   1318.5±8.79µs    12.6 MB/sec
parser/numpy/globals.py                    1.00    133.0±0.71µs    22.2 MB/sec    1.02    135.2±0.84µs    21.8 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.02ms     8.9 MB/sec    1.02      2.9±0.03ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
