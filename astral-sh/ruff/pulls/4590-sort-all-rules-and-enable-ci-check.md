```yaml
number: 4590
title: Sort all rules and enable CI check
type: pull_request
state: closed
author: JonathanPlasse
labels: []
assignees: []
draft: true
base: main
head: sort-all-rules
created_at: 2023-05-22T22:25:53Z
updated_at: 2023-10-17T11:07:09Z
url: https://github.com/astral-sh/ruff/pull/4590
synced_at: 2026-01-12T15:55:15Z
```

# Sort all rules and enable CI check

---

_@JonathanPlasse_

Depends on
- #3747

The files were sorted with `./scripts/sort_rules.py`.

---

_Comment by @github-actions[bot] on 2023-05-22 22:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.4±0.11ms     2.8 MB/sec    1.02     14.6±0.12ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.02ms     4.8 MB/sec    1.01      3.5±0.05ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    428.3±1.37µs     6.9 MB/sec    1.00    426.9±1.12µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.08ms     4.3 MB/sec    1.00      6.0±0.06ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.02ms     5.9 MB/sec    1.00      6.8±0.03ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1511.0±6.03µs    11.0 MB/sec    1.00   1506.3±4.85µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.5±0.47µs    17.3 MB/sec    1.00    171.3±0.53µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.1 MB/sec    1.00      3.1±0.02ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.2±0.01ms     7.9 MB/sec    1.00      5.2±0.01ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1020.3±0.65µs    16.3 MB/sec    1.00   1020.2±8.22µs    16.3 MB/sec
parser/numpy/globals.py                    1.00    105.1±0.18µs    28.1 MB/sec    1.00    105.1±0.28µs    28.1 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.00      2.2±0.01ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.1±0.85ms     2.0 MB/sec    1.05     21.1±0.68ms  1970.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.21ms     3.3 MB/sec    1.04      5.3±0.19ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   583.6±23.85µs     5.1 MB/sec    1.05   614.0±17.85µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.24ms     3.0 MB/sec    1.05      8.8±0.35ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      9.9±0.31ms     4.1 MB/sec    1.01     10.0±0.24ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.10ms     7.7 MB/sec    1.00      2.2±0.05ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   244.2±16.02µs    12.1 MB/sec    1.03   252.4±11.25µs    11.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.21ms     5.6 MB/sec    1.00      4.5±0.14ms     5.6 MB/sec
parser/large/dataset.py                    1.00      7.7±0.20ms     5.3 MB/sec    1.05      8.1±0.28ms     5.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1452.3±62.03µs    11.5 MB/sec    1.05  1531.3±94.75µs    10.9 MB/sec
parser/numpy/globals.py                    1.03   157.4±16.35µs    18.7 MB/sec    1.00    153.5±6.55µs    19.2 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.25ms     7.5 MB/sec    1.01      3.5±0.18ms     7.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-05-23 22:57_

I'll take a look at this tonight.

---

_Review requested from @charliermarsh by @charliermarsh on 2023-05-23 22:57_

---

_Converted to draft by @JonathanPlasse on 2023-05-24 12:07_

---

_Comment by @charliermarsh on 2023-07-21 20:10_

See #3747.

---

_Closed by @charliermarsh on 2023-07-21 20:10_

---

_Branch deleted on 2023-10-17 11:07_

---
