```yaml
number: 5766
title: "Expand scope of `quoted-annotation` rule"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/unquote
created_at: 2023-07-14T20:14:47Z
updated_at: 2023-07-15T19:37:36Z
url: https://github.com/astral-sh/ruff/pull/5766
synced_at: 2026-01-12T03:30:21Z
```

# Expand scope of `quoted-annotation` rule

---

_Pull request opened by @charliermarsh on 2023-07-14 20:14_

## Summary

Previously, the `quoted-annotation` rule only removed quotes when `from __future__ import annotations` was present. However, there are some other cases in which this is also safe -- for example:

```python
def foo():
    x: "MyClass"
```

We already model these in the semantic model, so this PR just expands the scope of the rule to handle those.


---

_Label `rule` added by @charliermarsh on 2023-07-14 20:14_

---

_Comment by @github-actions[bot] on 2023-07-14 20:49_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.02ms     4.4 MB/sec    1.00      9.3±0.02ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1852.5±1.96µs     9.0 MB/sec    1.00   1850.8±2.86µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    209.3±0.77µs    14.1 MB/sec    1.00    209.3±0.34µs    14.1 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.03ms     3.0 MB/sec    1.00     13.5±0.05ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.00ms     4.8 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    455.0±0.36µs     6.5 MB/sec    1.00    454.4±0.67µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.0±0.05ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      6.7±0.01ms     6.1 MB/sec    1.00      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1473.6±2.10µs    11.3 MB/sec    1.00   1471.6±1.92µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.1±1.03µs    17.3 MB/sec    1.00    170.1±0.67µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.4±0.31ms     3.6 MB/sec    1.00     11.3±0.30ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.06ms     7.6 MB/sec    1.00      2.2±0.04ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00    246.7±6.88µs    12.0 MB/sec    1.03    253.5±9.70µs    11.6 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.11ms     5.3 MB/sec    1.00      4.8±0.13ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.02     16.7±0.56ms     2.4 MB/sec    1.00     16.5±0.42ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.13ms     3.9 MB/sec    1.00      4.2±0.11ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   514.1±11.58µs     5.7 MB/sec    1.00    513.3±9.76µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.22ms     3.5 MB/sec    1.01      7.4±0.23ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.01      8.4±0.21ms     4.9 MB/sec    1.00      8.3±0.16ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1772.8±30.94µs     9.4 MB/sec    1.01  1788.4±59.17µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    208.7±6.12µs    14.1 MB/sec    1.00    208.3±5.28µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.07ms     6.8 MB/sec    1.00      3.7±0.09ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@dhruvmanila approved on 2023-07-15 13:04_

---

_Merged by @charliermarsh on 2023-07-15 19:37_

---

_Closed by @charliermarsh on 2023-07-15 19:37_

---

_Branch deleted on 2023-07-15 19:37_

---
