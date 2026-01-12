```yaml
number: 3699
title: Avoid parsing f-strings in type annotations
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/f-string-annotations
created_at: 2023-03-23T22:28:30Z
updated_at: 2023-03-23T23:08:48Z
url: https://github.com/astral-sh/ruff/pull/3699
synced_at: 2026-01-12T04:39:45Z
```

# Avoid parsing f-strings in type annotations

---

_Pull request opened by @charliermarsh on 2023-03-23 22:28_

## Summary

If we see code like `x: f"List[int]"`, while that's not a valid type annotation, we _do_ attempt to parse the string "within" the f-string, which (from the AST's perspective) doesn't start with a valid start and end quote (which end up on the f-string, not the string itself). This leads to a panic in `parse_type_annotation`, where we attempt to strip the string's start and end quotes.

Instead, we should just skip these. They're invalid anyway.


---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-23 22:34_

---

_Label `bug` added by @charliermarsh on 2023-03-23 22:34_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-03-23 22:34_

---

_Merged by @charliermarsh on 2023-03-23 22:51_

---

_Closed by @charliermarsh on 2023-03-23 22:51_

---

_Branch deleted on 2023-03-23 22:51_

---

_Comment by @github-actions[bot] on 2023-03-23 22:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.8±0.15ms     2.6 MB/sec    1.01     16.0±0.13ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.04ms     4.0 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    576.7±6.49µs     5.1 MB/sec    1.00    579.1±6.96µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.06ms     3.7 MB/sec    1.01      7.0±0.06ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.06ms     4.8 MB/sec    1.01      8.5±0.05ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1904.1±16.61µs     8.7 MB/sec    1.01  1929.5±24.26µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    212.9±2.70µs    13.9 MB/sec    1.04   221.9±15.14µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.04ms     6.3 MB/sec    1.00      4.0±0.04ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.4±0.19ms     2.6 MB/sec    1.01     15.5±0.20ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.12ms     4.0 MB/sec    1.00      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    540.3±8.14µs     5.5 MB/sec    1.00    540.4±8.78µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.14ms     3.7 MB/sec    1.00      6.8±0.09ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.07ms     4.9 MB/sec    1.00      8.3±0.13ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1824.6±19.90µs     9.1 MB/sec    1.01  1838.9±20.34µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.9±2.85µs    14.5 MB/sec    1.00    202.3±5.57µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.05ms     6.6 MB/sec    1.01      3.9±0.06ms     6.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
