```yaml
number: 4380
title: "Remove special-casing for `flake8-builtins` rules"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/builtins
created_at: 2023-05-11T20:29:22Z
updated_at: 2023-05-11T20:56:47Z
url: https://github.com/astral-sh/ruff/pull/4380
synced_at: 2026-01-12T03:56:39Z
```

# Remove special-casing for `flake8-builtins` rules

---

_Pull request opened by @charliermarsh on 2023-05-11 20:29_

## Summary

This PR modifies the `flake8-builtins` rules to follow some of our more conventional patterns (namely, by removing special-purpose methods on `Checker`). The updated patterns aren't necessarily _better_, but they're more consistent and will be useful if we migrate to a more declarative visitor-based approach.

---

_Comment by @github-actions[bot] on 2023-05-11 20:38_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.5±0.05ms     2.8 MB/sec    1.01     14.6±0.10ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.7 MB/sec    1.00      3.6±0.00ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    361.2±0.87µs     8.2 MB/sec    1.00    362.6±1.70µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.01ms     5.6 MB/sec    1.00      7.3±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1522.3±3.34µs    10.9 MB/sec    1.00   1513.9±1.82µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.2±0.21µs    18.1 MB/sec    1.01    164.5±4.91µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.8 MB/sec    1.00      3.3±0.01ms     7.8 MB/sec
parser/large/dataset.py                    1.01      5.9±0.00ms     6.9 MB/sec    1.00      5.8±0.01ms     7.0 MB/sec
parser/numpy/ctypeslib.py                  1.02   1146.3±1.80µs    14.5 MB/sec    1.00   1126.0±0.99µs    14.8 MB/sec
parser/numpy/globals.py                    1.02    117.5±0.32µs    25.1 MB/sec    1.00    115.6±0.38µs    25.5 MB/sec
parser/pydantic/types.py                   1.01      2.5±0.00ms    10.3 MB/sec    1.00      2.5±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.2±0.05ms     2.5 MB/sec    1.01     16.4±0.10ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.03ms     3.9 MB/sec    1.00      4.3±0.02ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    433.7±4.21µs     6.8 MB/sec    1.01    436.5±7.28µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.03ms     3.7 MB/sec    1.01      7.0±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.02ms     4.9 MB/sec    1.01      8.3±0.08ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1728.3±8.75µs     9.6 MB/sec    1.01   1747.1±9.10µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    186.2±1.27µs    15.8 MB/sec    1.02    189.9±5.73µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     6.9 MB/sec    1.01      3.8±0.02ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.8±0.02ms     6.0 MB/sec    1.02      6.9±0.02ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1290.3±5.24µs    12.9 MB/sec    1.01   1304.1±6.99µs    12.8 MB/sec
parser/numpy/globals.py                    1.00    132.6±0.79µs    22.3 MB/sec    1.01    133.3±1.51µs    22.1 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.02ms     8.9 MB/sec    1.01      2.9±0.01ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-11 20:39_

---

_Closed by @charliermarsh on 2023-05-11 20:39_

---

_Branch deleted on 2023-05-11 20:39_

---
