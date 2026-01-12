```yaml
number: 5889
title: Add missing backtick to B034 documentation
type: pull_request
state: merged
author: DavidCain
labels:
  - documentation
assignees: []
merged: true
base: main
head: dcain-B034-docs
created_at: 2023-07-19T17:17:40Z
updated_at: 2023-07-19T17:44:44Z
url: https://github.com/astral-sh/ruff/pull/5889
synced_at: 2026-01-12T15:55:19Z
```

# Add missing backtick to B034 documentation

---

_@DavidCain_

This is a great rule, but the documentation page shows some wonky formatting due to a missing backtick. Fix a typo too.

Should fix display on https://beta.ruff.rs/docs/rules/re-sub-positional-args/

<img width="1160" alt="image" src="https://github.com/astral-sh/ruff/assets/901169/44bd76ec-9eb9-4290-ba7a-7691a7ea21d4">



---

_Comment by @charliermarsh on 2023-07-19 17:22_

Thanks!

---

_Label `documentation` added by @charliermarsh on 2023-07-19 17:22_

---

_Merged by @charliermarsh on 2023-07-19 17:25_

---

_Closed by @charliermarsh on 2023-07-19 17:25_

---

_Branch deleted on 2023-07-19 17:25_

---

_Comment by @github-actions[bot] on 2023-07-19 17:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.7±0.24ms     3.8 MB/sec    1.02     10.9±0.33ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.2±0.13ms     7.4 MB/sec    1.00      2.2±0.04ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00   237.1±11.11µs    12.4 MB/sec    1.06   251.2±14.71µs    11.7 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.01ms     5.3 MB/sec    1.07      5.1±0.25ms     5.0 MB/sec
linter/all-rules/large/dataset.py          1.06     16.1±0.72ms     2.5 MB/sec    1.00     15.2±0.47ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.0±0.10ms     4.1 MB/sec    1.00      4.0±0.03ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   542.8±34.18µs     5.4 MB/sec    1.04   565.9±46.19µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.36ms     3.5 MB/sec    1.01      7.4±0.27ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.24ms     5.1 MB/sec    1.01      8.0±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1650.0±55.81µs    10.1 MB/sec    1.03   1694.6±5.78µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.8±2.30µs    15.9 MB/sec    1.04   193.4±11.33µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.17ms     7.1 MB/sec    1.00      3.6±0.02ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.1±0.06ms     3.7 MB/sec    1.01     11.2±0.08ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.01ms     7.8 MB/sec    1.00      2.1±0.02ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    233.2±5.73µs    12.7 MB/sec    1.00    233.5±6.20µs    12.6 MB/sec
formatter/pydantic/types.py                1.01      4.8±0.06ms     5.3 MB/sec    1.00      4.8±0.03ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.09ms     2.6 MB/sec    1.00     15.4±0.10ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.03ms     4.1 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    422.5±6.58µs     7.0 MB/sec    1.00    421.2±6.63µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.07ms     3.6 MB/sec    1.00      7.0±0.06ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.03ms     5.0 MB/sec    1.00      8.1±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1657.3±19.34µs    10.0 MB/sec    1.00  1654.7±13.49µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.4±1.48µs    17.0 MB/sec    1.00    173.4±1.73µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.0 MB/sec    1.00      3.6±0.02ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
