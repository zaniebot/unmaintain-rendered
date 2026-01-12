```yaml
number: 4347
title: Enforce max-doc-length for multi-line docstrings
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/multi-line-docstring
created_at: 2023-05-10T14:51:02Z
updated_at: 2023-05-10T15:24:52Z
url: https://github.com/astral-sh/ruff/pull/4347
synced_at: 2026-01-12T03:56:39Z
```

# Enforce max-doc-length for multi-line docstrings

---

_Pull request opened by @charliermarsh on 2023-05-10 14:51_

## Summary

Our `doc_lines` vector is intended to contain the start locations of all doc lines. However, for multi-line docstrings, we're only including the start of the _first_ line, so we're missing overlong lines in multi-line docstrings that occur beyond the first line in the docstring.

This PR modifies the collection code to include the start of all lines in the docstring.

Closes #4344.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-10 14:51_

---

_Label `bug` added by @charliermarsh on 2023-05-10 14:51_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/physical_lines.rs`:121 on 2023-05-10 14:51_

This was getting stuck on empty-lines-in-docstrings. I think `contains_inclusive` is correct here?

---

_@charliermarsh reviewed on 2023-05-10 14:51_

---

_@MichaReiser approved on 2023-05-10 14:57_

---

_Comment by @github-actions[bot] on 2023-05-10 15:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.10     15.5±0.09ms     2.6 MB/sec    1.00     14.2±0.15ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.5±0.01ms     4.7 MB/sec    1.00      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.02    427.9±0.88µs     6.9 MB/sec    1.00    420.7±0.98µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.06      6.2±0.03ms     4.1 MB/sec    1.00      5.9±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.13      7.8±0.03ms     5.2 MB/sec    1.00      6.9±0.02ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.10   1631.0±2.17µs    10.2 MB/sec    1.00   1482.7±4.23µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.06    173.8±0.26µs    17.0 MB/sec    1.00    164.5±1.54µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.10      3.4±0.01ms     7.5 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.5±0.00ms     7.5 MB/sec    1.00      5.5±0.01ms     7.4 MB/sec
parser/numpy/ctypeslib.py                  1.00   1066.5±3.01µs    15.6 MB/sec    1.00   1062.9±1.45µs    15.7 MB/sec
parser/numpy/globals.py                    1.00    108.3±0.24µs    27.2 MB/sec    1.00    108.4±0.49µs    27.2 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.01ms    11.0 MB/sec    1.00      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.1±0.27ms     2.9 MB/sec    1.00     14.0±0.18ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.03ms     4.9 MB/sec    1.01      3.4±0.07ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    355.2±4.71µs     8.3 MB/sec    1.00    354.2±5.39µs     8.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.17ms     4.4 MB/sec    1.00      5.8±0.16ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.06ms     5.8 MB/sec    1.04      7.3±0.10ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1407.1±9.03µs    11.8 MB/sec    1.04   1462.4±9.10µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    152.9±1.82µs    19.3 MB/sec    1.02    155.5±2.89µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.4 MB/sec    1.03      3.2±0.02ms     8.1 MB/sec
parser/large/dataset.py                    1.03      5.9±0.06ms     6.9 MB/sec    1.00      5.7±0.02ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.02   1089.0±7.67µs    15.3 MB/sec    1.00   1063.4±7.93µs    15.7 MB/sec
parser/numpy/globals.py                    1.02    107.6±0.85µs    27.4 MB/sec    1.00    105.9±0.87µs    27.9 MB/sec
parser/pydantic/types.py                   1.02      2.4±0.02ms    10.5 MB/sec    1.00      2.4±0.01ms    10.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-10 15:06_

---

_Closed by @charliermarsh on 2023-05-10 15:06_

---

_Branch deleted on 2023-05-10 15:06_

---
