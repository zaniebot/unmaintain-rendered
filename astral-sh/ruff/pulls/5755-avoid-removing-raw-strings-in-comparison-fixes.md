```yaml
number: 5755
title: Avoid removing raw strings in comparison fixes
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/raw
created_at: 2023-07-14T03:06:38Z
updated_at: 2023-07-14T04:43:56Z
url: https://github.com/astral-sh/ruff/pull/5755
synced_at: 2026-01-12T03:30:21Z
```

# Avoid removing raw strings in comparison fixes

---

_Pull request opened by @charliermarsh on 2023-07-14 03:06_

## Summary

Use `Locator`-based verbatim fix rather than a `Generator`-based fix, which loses trivia (and raw strings).

Closes https://github.com/astral-sh/ruff/issues/4130.


---

_Label `bug` added by @charliermarsh on 2023-07-14 03:06_

---

_Comment by @github-actions[bot] on 2023-07-14 03:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.0±0.02ms     5.1 MB/sec    1.00      8.0±0.05ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.01   1855.6±1.45µs     9.0 MB/sec    1.00   1841.8±2.21µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    206.9±0.32µs    14.3 MB/sec    1.00    206.9±2.04µs    14.3 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.00ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.01     13.7±0.09ms     3.0 MB/sec    1.00     13.5±0.05ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    451.9±1.44µs     6.5 MB/sec    1.00    452.5±0.49µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.06ms     4.2 MB/sec    1.00      6.0±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.1 MB/sec    1.00      6.7±0.03ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1472.4±2.82µs    11.3 MB/sec    1.00   1471.2±1.62µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.4±0.25µs    17.3 MB/sec    1.00    169.7±0.67µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     11.4±0.26ms     3.6 MB/sec    1.00     11.2±0.17ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.5±0.04ms     6.5 MB/sec    1.00      2.5±0.04ms     6.5 MB/sec
formatter/numpy/globals.py                 1.00    288.8±7.22µs    10.2 MB/sec    1.02    293.8±9.09µs    10.0 MB/sec
formatter/pydantic/types.py                1.00      5.5±0.08ms     4.6 MB/sec    1.00      5.6±0.08ms     4.6 MB/sec
linter/all-rules/large/dataset.py          1.00     19.1±0.19ms     2.1 MB/sec    1.00     19.1±0.18ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.0±0.10ms     3.3 MB/sec    1.00      4.9±0.08ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   592.5±12.68µs     5.0 MB/sec    1.00   595.3±10.46µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.5±0.11ms     3.0 MB/sec    1.00      8.5±0.12ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.10ms     4.2 MB/sec    1.00      9.6±0.10ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.02ms     8.1 MB/sec    1.00      2.1±0.03ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    242.0±6.17µs    12.2 MB/sec    1.00    241.1±7.67µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.07ms     5.9 MB/sec    1.00      4.3±0.06ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-14 04:27_

---

_Closed by @charliermarsh on 2023-07-14 04:27_

---

_Branch deleted on 2023-07-14 04:27_

---
