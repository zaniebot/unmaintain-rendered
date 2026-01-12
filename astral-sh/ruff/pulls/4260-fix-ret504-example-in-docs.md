```yaml
number: 4260
title: "Fix `RET504` example in docs"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/ret504-docs
created_at: 2023-05-06T20:21:26Z
updated_at: 2023-05-06T21:00:33Z
url: https://github.com/astral-sh/ruff/pull/4260
synced_at: 2026-01-12T03:56:39Z
```

# Fix `RET504` example in docs

---

_Pull request opened by @charliermarsh on 2023-05-06 20:21_

Closes #4240.

---

_Label `documentation` added by @charliermarsh on 2023-05-06 20:21_

---

_Comment by @github-actions[bot] on 2023-05-06 20:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.3±0.24ms     2.9 MB/sec    1.00     14.1±0.24ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.03ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    416.5±0.90µs     7.1 MB/sec    1.00    418.5±1.77µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.04ms     4.4 MB/sec    1.01      5.9±0.07ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      7.0±0.05ms     5.8 MB/sec    1.00      6.9±0.07ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1484.1±4.47µs    11.2 MB/sec    1.00   1475.3±1.67µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.7±0.27µs    18.3 MB/sec    1.00    161.6±0.69µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.02ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.4±0.01ms     7.5 MB/sec    1.00      5.4±0.01ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.01   1061.0±0.80µs    15.7 MB/sec    1.00   1053.9±0.77µs    15.8 MB/sec
parser/numpy/globals.py                    1.01    107.6±0.19µs    27.4 MB/sec    1.00    107.0±0.19µs    27.6 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.15ms     2.5 MB/sec    1.02     16.8±0.26ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.03ms     3.9 MB/sec    1.00      4.3±0.04ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    431.0±6.12µs     6.8 MB/sec    1.00    432.6±5.68µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.11ms     3.6 MB/sec    1.00      7.1±0.14ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.06ms     4.9 MB/sec    1.00      8.3±0.05ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1750.7±14.71µs     9.5 MB/sec    1.01  1764.2±43.28µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    187.4±1.80µs    15.7 MB/sec    1.01    189.3±6.06µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.02ms     6.8 MB/sec    1.01      3.8±0.04ms     6.7 MB/sec
parser/large/dataset.py                    1.02      7.0±0.03ms     5.8 MB/sec    1.00      6.9±0.04ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.01  1322.3±11.04µs    12.6 MB/sec    1.00  1315.1±11.20µs    12.7 MB/sec
parser/numpy/globals.py                    1.00    135.1±1.04µs    21.8 MB/sec    1.00    135.4±1.76µs    21.8 MB/sec
parser/pydantic/types.py                   1.01      3.0±0.02ms     8.6 MB/sec    1.00      2.9±0.02ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-06 20:56_

---

_Closed by @charliermarsh on 2023-05-06 20:56_

---

_Branch deleted on 2023-05-06 20:56_

---

_Comment by @calumy on 2023-05-06 21:00_

Appologise for this one. I should have checked that the flake8-return example for this rule worked in ruff too. If I contribute any more docs I'll be sure to double check the examples 

---

_Comment by @charliermarsh on 2023-05-06 21:00_

No worries at all, don't sweat it!

---
