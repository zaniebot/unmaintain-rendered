```yaml
number: 4906
title: Avoid no-op fix for nested with expressions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/parens
created_at: 2023-06-06T20:06:59Z
updated_at: 2023-06-06T20:37:52Z
url: https://github.com/astral-sh/ruff/pull/4906
synced_at: 2026-01-12T03:43:29Z
```

# Avoid no-op fix for nested with expressions

---

_Pull request opened by @charliermarsh on 2023-06-06 20:06_

## Summary

Modifies our `with foo as bar:` fix to remove from the "end of the first token before the `as`", instead of the "end of the expression", which avoids an erroneous fix for `with (foo) as bar:`.

Closes #4901.


---

_Label `bug` added by @charliermarsh on 2023-06-06 20:07_

---

_Marked ready for review by @charliermarsh on 2023-06-06 20:07_

---

_Merged by @charliermarsh on 2023-06-06 20:15_

---

_Closed by @charliermarsh on 2023-06-06 20:15_

---

_Branch deleted on 2023-06-06 20:15_

---

_Comment by @github-actions[bot] on 2023-06-06 20:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      6.3±0.02ms     6.5 MB/sec    1.00      6.1±0.01ms     6.6 MB/sec
formatter/numpy/ctypeslib.py               1.02   1275.8±2.02µs    13.1 MB/sec    1.00   1251.8±2.34µs    13.3 MB/sec
formatter/numpy/globals.py                 1.03    147.7±0.39µs    20.0 MB/sec    1.00    143.5±0.21µs    20.6 MB/sec
formatter/pydantic/types.py                1.01      2.7±0.01ms     9.3 MB/sec    1.00      2.7±0.01ms     9.4 MB/sec
linter/all-rules/large/dataset.py          1.00     14.9±0.08ms     2.7 MB/sec    1.01     15.0±0.06ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.6 MB/sec    1.01      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    363.1±1.53µs     8.1 MB/sec    1.00    362.9±1.28µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.01      6.2±0.07ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.06ms     5.5 MB/sec    1.01      7.4±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1525.6±4.55µs    10.9 MB/sec    1.01   1542.1±7.68µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.6±0.27µs    18.1 MB/sec    1.02    165.1±8.01µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.8 MB/sec    1.01      3.3±0.00ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.2±0.19ms     4.9 MB/sec    1.04      8.6±0.19ms     4.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1709.2±91.33µs     9.7 MB/sec    1.03  1763.1±46.67µs     9.4 MB/sec
formatter/numpy/globals.py                 1.00    196.2±8.68µs    15.0 MB/sec    1.04   204.8±10.85µs    14.4 MB/sec
formatter/pydantic/types.py                1.00      3.7±0.11ms     7.0 MB/sec    1.06      3.9±0.21ms     6.6 MB/sec
linter/all-rules/large/dataset.py          1.00     18.9±0.58ms     2.2 MB/sec    1.08     20.3±0.47ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.15ms     3.5 MB/sec    1.10      5.2±0.19ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   560.7±21.91µs     5.3 MB/sec    1.10   614.2±31.99µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.33ms     3.2 MB/sec    1.12      8.9±0.40ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00      9.4±0.27ms     4.3 MB/sec    1.13     10.6±0.77ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.06ms     8.3 MB/sec    1.15      2.3±0.15ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   242.7±10.31µs    12.2 MB/sec    1.05   254.8±16.44µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.14ms     5.9 MB/sec    1.16      5.0±0.16ms     5.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
