```yaml
number: 3698
title: "Avoid parsing `ForwardRef` contents as type references"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/forward
created_at: 2023-03-23T22:23:57Z
updated_at: 2023-03-23T22:56:34Z
url: https://github.com/astral-sh/ruff/pull/3698
synced_at: 2026-01-12T04:39:45Z
```

# Avoid parsing `ForwardRef` contents as type references

---

_Pull request opened by @charliermarsh on 2023-03-23 22:23_

## Summary

Today, we parse the inner contents of `ForwardRef` (e.g., the `"List[int]"` in `ForwardRef("List[int]")`) as a forward annotation. However, no other tools treat them like that. E.g., if you run this code through Flake8, or Mypy, or Pyright:

```py
from typing import TypeVar

X = TypeVar("X", "List[int]", int)
X = ForwardRef("List[int]")
```

...none of them flag the second `List[int]` as an undefined reference to `List`.

Closes #3678 although there's another fix coming to avoid a panic there.

---

_Label `bug` added by @charliermarsh on 2023-03-23 22:29_

---

_Comment by @github-actions[bot] on 2023-03-23 22:38_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.5±0.04ms     3.0 MB/sec    1.00     13.5±0.05ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.01      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    485.9±0.75µs     6.1 MB/sec    1.03    499.1±5.00µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.01      6.0±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.7 MB/sec    1.01      7.2±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1608.3±2.31µs    10.4 MB/sec    1.00   1602.9±2.38µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.0±0.61µs    16.5 MB/sec    1.02    181.8±9.41µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.02ms     7.6 MB/sec    1.00      3.4±0.03ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     15.3±0.14ms     2.7 MB/sec    1.00     15.1±0.07ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.03ms     4.0 MB/sec    1.00      4.1±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    456.1±4.20µs     6.5 MB/sec    1.00    455.5±4.47µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.05ms     3.8 MB/sec    1.00      6.8±0.05ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.06ms     5.0 MB/sec    1.00      8.2±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1780.1±11.06µs     9.4 MB/sec    1.00  1763.9±10.79µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.9±1.76µs    16.2 MB/sec    1.01   184.4±29.31µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.03ms     6.6 MB/sec    1.00      3.8±0.02ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-03-23 22:44_

---

_Closed by @charliermarsh on 2023-03-23 22:44_

---

_Branch deleted on 2023-03-23 22:44_

---
