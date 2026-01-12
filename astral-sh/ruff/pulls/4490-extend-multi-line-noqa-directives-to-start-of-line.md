```yaml
number: 4490
title: Extend multi-line noqa directives to start-of-line
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/noqa
created_at: 2023-05-18T12:59:11Z
updated_at: 2023-05-18T13:26:55Z
url: https://github.com/astral-sh/ruff/pull/4490
synced_at: 2026-01-12T03:50:03Z
```

# Extend multi-line noqa directives to start-of-line

---

_Pull request opened by @charliermarsh on 2023-05-18 12:59_

Closes #4484.

---

_Label `bug` added by @charliermarsh on 2023-05-18 12:59_

---

_Merged by @charliermarsh on 2023-05-18 13:05_

---

_Closed by @charliermarsh on 2023-05-18 13:05_

---

_Branch deleted on 2023-05-18 13:05_

---

_Comment by @github-actions[bot] on 2023-05-18 13:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.8±0.40ms     2.6 MB/sec    1.04     16.4±0.52ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.10ms     4.3 MB/sec    1.03      3.9±0.14ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   483.6±18.69µs     6.1 MB/sec    1.01   489.8±27.19µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.21ms     3.9 MB/sec    1.01      6.6±0.17ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.20ms     5.5 MB/sec    1.00      7.4±0.29ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1587.9±39.44µs    10.5 MB/sec    1.03  1627.9±69.05µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.1±8.40µs    15.6 MB/sec    1.02    193.1±7.72µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.11ms     7.6 MB/sec    1.01      3.4±0.10ms     7.5 MB/sec
parser/large/dataset.py                    1.00      5.8±0.11ms     7.0 MB/sec    1.01      5.9±0.15ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1173.4±32.44µs    14.2 MB/sec    1.00  1172.7±40.10µs    14.2 MB/sec
parser/numpy/globals.py                    1.00    119.6±5.03µs    24.7 MB/sec    1.03   122.7±12.29µs    24.0 MB/sec
parser/pydantic/types.py                   1.01      2.6±0.08ms     9.9 MB/sec    1.00      2.5±0.07ms    10.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.2±0.33ms     2.4 MB/sec    1.03     17.7±0.23ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.12ms     3.9 MB/sec    1.01      4.4±0.06ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   509.4±15.29µs     5.8 MB/sec    1.00    509.3±9.30µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.14ms     3.6 MB/sec    1.03      7.4±0.12ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.8±0.22ms     4.6 MB/sec    1.02      8.9±0.10ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1785.3±22.47µs     9.3 MB/sec    1.04  1861.5±24.13µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.2±4.41µs    14.8 MB/sec    1.04    208.1±8.32µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.05ms     6.7 MB/sec    1.04      4.0±0.05ms     6.4 MB/sec
parser/large/dataset.py                    1.00      6.7±0.06ms     6.0 MB/sec    1.00      6.7±0.05ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.01  1287.6±16.89µs    12.9 MB/sec    1.00  1280.0±12.54µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    132.5±2.66µs    22.3 MB/sec    1.00    132.5±3.60µs    22.3 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.03ms     8.9 MB/sec    1.00      2.9±0.03ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
