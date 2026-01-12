```yaml
number: 4739
title: "Refactor `flake8-type-checking` rules to take `Checker`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/type-check-diagnostics
created_at: 2023-05-30T22:42:40Z
updated_at: 2023-05-30T23:08:30Z
url: https://github.com/astral-sh/ruff/pull/4739
synced_at: 2026-01-12T15:55:16Z
```

# Refactor `flake8-type-checking` rules to take `Checker`

---

_@charliermarsh_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

No functional changes, but doing this refactor separately to simplify some of the changes to-come (which will add autofix behavior).


---

_Renamed from "Refactor flake8-type-checking rules to take Checker" to "Refactor `flake8-type-checking` rules to take `Checker`" by @charliermarsh on 2023-05-30 22:42_

---

_Label `internal` added by @charliermarsh on 2023-05-30 22:42_

---

_Merged by @charliermarsh on 2023-05-30 22:51_

---

_Closed by @charliermarsh on 2023-05-30 22:51_

---

_Branch deleted on 2023-05-30 22:51_

---

_Comment by @github-actions[bot] on 2023-05-30 22:54_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.1±0.03ms     2.9 MB/sec    1.00     14.0±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    425.5±0.41µs     6.9 MB/sec    1.00    425.2±0.49µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.8±0.02ms     5.9 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1513.6±1.98µs    11.0 MB/sec    1.00   1507.5±4.33µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.3±0.54µs    17.0 MB/sec    1.00    173.4±0.90µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.02ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.01      5.2±0.01ms     7.8 MB/sec    1.00      5.2±0.00ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1023.3±0.82µs    16.3 MB/sec    1.00   1020.6±1.11µs    16.3 MB/sec
parser/numpy/globals.py                    1.00    105.3±0.20µs    28.0 MB/sec    1.00    105.2±0.27µs    28.1 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.00      2.2±0.00ms    11.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     18.8±1.59ms     2.2 MB/sec    1.00     18.4±1.60ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.3±0.18ms     3.9 MB/sec    1.00      4.2±0.10ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.03   522.2±35.13µs     5.7 MB/sec    1.00   505.0±26.24µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.0±0.68ms     3.2 MB/sec    1.00      7.9±0.52ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.01      9.5±0.84ms     4.3 MB/sec    1.00      9.4±0.70ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1799.6±67.12µs     9.3 MB/sec    1.00  1773.5±51.57µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.1±6.32µs    14.8 MB/sec    1.01    201.4±9.04µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.25ms     6.4 MB/sec    1.06      4.2±0.18ms     6.1 MB/sec
parser/large/dataset.py                    1.00      6.8±0.33ms     6.0 MB/sec    1.02      7.0±0.44ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.01  1260.0±42.06µs    13.2 MB/sec    1.00  1250.9±39.39µs    13.3 MB/sec
parser/numpy/globals.py                    1.00    127.9±4.61µs    23.1 MB/sec    1.01    128.9±7.36µs    22.9 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.11ms     8.7 MB/sec    1.00      2.9±0.13ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
