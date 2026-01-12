```yaml
number: 5663
title: "[`flake8-self`] Ignore `_name_` and `_value_`"
type: pull_request
state: merged
author: monosans
labels: []
assignees: []
merged: true
base: main
head: patch-01
created_at: 2023-07-10T18:29:46Z
updated_at: 2023-07-10T19:05:51Z
url: https://github.com/astral-sh/ruff/pull/5663
synced_at: 2026-01-12T15:55:19Z
```

# [`flake8-self`] Ignore `_name_` and `_value_`

---

_@monosans_

## Summary

`Enum._name_` and `Enum._value_` are so named to prevent conflicts. See <https://docs.python.org/3/library/enum.html#supported-sunder-names>.

## Test Plan

Tests for `ignore-names` already exist.

---

_Comment by @github-actions[bot] on 2023-07-10 18:41_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.03ms     5.1 MB/sec    1.00      8.0±0.01ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1759.5±2.67µs     9.5 MB/sec    1.00   1763.1±2.38µs     9.4 MB/sec
formatter/numpy/globals.py                 1.00    197.5±1.33µs    14.9 MB/sec    1.01    199.0±5.33µs    14.8 MB/sec
formatter/pydantic/types.py                1.01      3.9±0.01ms     6.5 MB/sec    1.00      3.9±0.01ms     6.6 MB/sec
linter/all-rules/large/dataset.py          1.01     13.6±0.11ms     3.0 MB/sec    1.00     13.5±0.08ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.02ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    440.7±0.72µs     6.7 MB/sec    1.00    435.3±0.44µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.03ms     4.2 MB/sec    1.01      6.1±0.06ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.0 MB/sec    1.00      6.7±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1485.8±3.93µs    11.2 MB/sec    1.00   1488.4±4.12µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.2±1.82µs    17.2 MB/sec    1.00    170.5±0.31µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     14.1±1.14ms     2.9 MB/sec    1.00     13.8±1.24ms     3.0 MB/sec
formatter/numpy/ctypeslib.py               1.00      3.0±0.26ms     5.6 MB/sec    1.01      3.0±0.35ms     5.6 MB/sec
formatter/numpy/globals.py                 1.00   338.9±34.75µs     8.7 MB/sec    1.03   349.0±42.49µs     8.5 MB/sec
formatter/pydantic/types.py                1.00      6.5±0.58ms     3.9 MB/sec    1.00      6.6±0.61ms     3.9 MB/sec
linter/all-rules/large/dataset.py          1.00     23.6±1.23ms  1762.1 KB/sec    1.00     23.7±1.44ms  1758.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      6.0±0.38ms     2.8 MB/sec    1.02      6.2±0.62ms     2.7 MB/sec
linter/all-rules/numpy/globals.py          1.05   752.4±90.39µs     3.9 MB/sec    1.00   716.5±89.85µs     4.1 MB/sec
linter/all-rules/pydantic/types.py         1.01     10.7±0.70ms     2.4 MB/sec    1.00     10.6±0.84ms     2.4 MB/sec
linter/default-rules/large/dataset.py      1.00     12.0±0.86ms     3.4 MB/sec    1.01     12.1±0.76ms     3.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.5±0.23ms     6.7 MB/sec    1.01      2.5±0.16ms     6.7 MB/sec
linter/default-rules/numpy/globals.py      1.01   308.9±37.49µs     9.6 MB/sec    1.00   305.1±34.36µs     9.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.1±0.33ms     5.0 MB/sec    1.06      5.4±0.48ms     4.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-07-10 18:43_

Dumb question, what is this used for? How are the contents different from `Enum.value` and `Enum.name`?

---

_Comment by @monosans on 2023-07-10 18:50_

They are identical. Sunder names exist in case a person wants to create an Enum member with the name `name` or `value`.

---

_Merged by @charliermarsh on 2023-07-10 18:53_

---

_Closed by @charliermarsh on 2023-07-10 18:53_

---
