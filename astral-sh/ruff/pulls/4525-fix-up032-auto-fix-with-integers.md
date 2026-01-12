```yaml
number: 4525
title: Fix UP032 auto-fix with integers
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: fix-up032-auto-fix-with-integers
created_at: 2023-05-19T10:43:00Z
updated_at: 2023-05-19T19:08:19Z
url: https://github.com/astral-sh/ruff/pull/4525
synced_at: 2026-01-12T03:50:03Z
```

# Fix UP032 auto-fix with integers

---

_Pull request opened by @JonathanPlasse on 2023-05-19 10:43_

- Related to #4523

---

_Comment by @github-actions[bot] on 2023-05-19 10:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.4±0.23ms     2.8 MB/sec    1.03     14.9±0.13ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.03      3.5±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    421.2±0.57µs     7.0 MB/sec    1.01    426.0±0.67µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.04ms     4.3 MB/sec    1.03      6.1±0.05ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     6.0 MB/sec    1.08      7.3±0.05ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1462.0±2.00µs    11.4 MB/sec    1.05   1541.3±3.99µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.2±0.31µs    18.2 MB/sec    1.03    167.5±0.24µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.06      3.2±0.01ms     7.9 MB/sec
parser/large/dataset.py                    1.02      5.5±0.01ms     7.4 MB/sec    1.00      5.4±0.02ms     7.6 MB/sec
parser/numpy/ctypeslib.py                  1.02   1071.7±0.84µs    15.5 MB/sec    1.00   1055.6±3.29µs    15.8 MB/sec
parser/numpy/globals.py                    1.02    110.5±0.15µs    26.7 MB/sec    1.00    108.5±0.43µs    27.2 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.4±0.12ms     2.5 MB/sec    1.00     16.4±0.11ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    492.9±6.39µs     6.0 MB/sec    1.01    495.5±9.28µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.07ms     3.7 MB/sec    1.00      6.9±0.05ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.10ms     4.9 MB/sec    1.00      8.2±0.05ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1736.6±19.01µs     9.6 MB/sec    1.01  1747.4±16.70µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.4±2.37µs    15.0 MB/sec    1.01    198.2±6.39µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.00      3.7±0.04ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.6±0.04ms     6.1 MB/sec    1.16      7.7±0.04ms     5.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1268.3±24.94µs    13.1 MB/sec    1.13  1438.7±18.13µs    11.6 MB/sec
parser/numpy/globals.py                    1.00    130.9±2.29µs    22.5 MB/sec    1.10    144.5±2.59µs    20.4 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.0 MB/sec    1.14      3.2±0.04ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/snapshots/ruff__rules__pyupgrade__tests__UP032_2.py.snap`:9 on 2023-05-19 12:40_

Happens happens with floats? E.g. `.format(1.0)`?

---

_@charliermarsh reviewed on 2023-05-19 12:40_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pyupgrade/snapshots/ruff__rules__pyupgrade__tests__UP032_2.py.snap`:9 on 2023-05-19 12:43_

Tested it before.
The problem is only present with integers. floats and complex numbers work.
```python
f"{0.real}"  # SyntaxError
f"{0.0.real}"  # OK
f"{0j.real}"  # OK
```

---

_@JonathanPlasse reviewed on 2023-05-19 12:43_

---

_@charliermarsh reviewed on 2023-05-19 12:45_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/snapshots/ruff__rules__pyupgrade__tests__UP032_2.py.snap`:9 on 2023-05-19 12:45_

Great, thank you 🙏 

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/snapshots/ruff__rules__pyupgrade__tests__UP032_2.py.snap`:9 on 2023-05-19 12:47_

It looks like these work too?

```py
>>> f"{0b101.real}"
'5'
>>> f"{0o101.real}"
'65'
```

---

_@charliermarsh reviewed on 2023-05-19 12:47_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/pyupgrade/UP032_2.py`:4 on 2023-05-19 12:47_

Can you add some more test cases here, for floats, octal integers, complex, etc.?

---

_@charliermarsh reviewed on 2023-05-19 12:47_

---

_@JonathanPlasse reviewed on 2023-05-19 12:47_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pyupgrade/snapshots/ruff__rules__pyupgrade__tests__UP032_2.py.snap`:9 on 2023-05-19 12:47_

Indeed

---

_Merged by @charliermarsh on 2023-05-19 18:53_

---

_Closed by @charliermarsh on 2023-05-19 18:53_

---

_Branch deleted on 2023-05-19 19:04_

---
