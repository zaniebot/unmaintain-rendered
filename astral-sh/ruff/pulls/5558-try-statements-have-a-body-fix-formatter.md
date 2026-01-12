```yaml
number: 5558
title: "Try statements have a body: Fix formatter instability"
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: try-has-a-body
created_at: 2023-07-06T10:38:02Z
updated_at: 2023-07-06T14:07:49Z
url: https://github.com/astral-sh/ruff/pull/5558
synced_at: 2026-01-12T15:55:18Z
```

# Try statements have a body: Fix formatter instability

---

_@konstin_

## Summary

The following code was previously leading to unstable formatting:
```python
try:
    try:
        pass
    finally:
        print(1)  # issue7208
except A:
    pass
```
The comment would be formatted as a trailing comment of `try` which is unstable as an end-of-line comment gets two extra whitespaces.

This was originally found in https://github.com/python/cpython/blob/99b00efd5edfd5b26bf9e2a35cbfc96277fdcbb1/Lib/getpass.py#L68-L91

## Test Plan

I added a regression test


---

_Label `formatter` added by @konstin on 2023-07-06 10:38_

---

_Marked ready for review by @konstin on 2023-07-06 10:40_

---

_Review requested from @MichaReiser by @konstin on 2023-07-06 10:40_

---

_Comment by @github-actions[bot] on 2023-07-06 10:50_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.03ms     5.2 MB/sec    1.00      7.7±0.02ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1721.7±2.53µs     9.7 MB/sec    1.00   1714.9±3.14µs     9.7 MB/sec
formatter/numpy/globals.py                 1.00    191.6±0.41µs    15.4 MB/sec    1.00    191.8±2.24µs    15.4 MB/sec
formatter/pydantic/types.py                1.01      3.8±0.01ms     6.8 MB/sec    1.00      3.7±0.01ms     6.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.07ms     3.0 MB/sec    1.08     14.4±0.09ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.02      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    423.1±1.46µs     7.0 MB/sec    1.00    424.5±2.82µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.08      6.3±0.05ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.02ms     6.1 MB/sec    1.00      6.6±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1427.3±3.93µs    11.7 MB/sec    1.00   1421.6±2.14µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.01    160.2±0.40µs    18.4 MB/sec    1.00    158.9±0.52µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.00      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.06ms     4.3 MB/sec    1.01      9.4±0.06ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1986.5±21.43µs     8.4 MB/sec    1.01      2.0±0.02ms     8.3 MB/sec
formatter/numpy/globals.py                 1.00    218.8±3.70µs    13.5 MB/sec    1.01   221.3±12.39µs    13.3 MB/sec
formatter/pydantic/types.py                1.01      4.5±0.03ms     5.6 MB/sec    1.00      4.5±0.04ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.6±0.10ms     2.6 MB/sec    1.02     15.9±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.19ms     4.0 MB/sec    1.00      4.2±0.07ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    430.8±6.95µs     6.8 MB/sec    1.00    431.7±7.63µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.08ms     3.6 MB/sec    1.01      7.2±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.04ms     5.0 MB/sec    1.03      8.3±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1668.5±13.51µs    10.0 MB/sec    1.02  1705.0±16.21µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.3±1.95µs    16.6 MB/sec    1.02    181.0±1.80µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.0 MB/sec    1.02      3.7±0.03ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:338 on 2023-07-06 12:24_

Did you change any logic in `placement.rs` or is this just a refactor?

---

_@MichaReiser reviewed on 2023-07-06 12:24_

---

_@konstin reviewed on 2023-07-06 13:11_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:338 on 2023-07-06 13:11_

i think i changed something and then changed it back when i realized it wasn't the cause, this is now just a refactor; i can split it out if you want

---

_@MichaReiser reviewed on 2023-07-06 13:29_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:338 on 2023-07-06 13:29_

This won't be necessary. I'm only trying to understand the change.

---

_@MichaReiser approved on 2023-07-06 13:29_

---

_Merged by @konstin on 2023-07-06 14:07_

---

_Closed by @konstin on 2023-07-06 14:07_

---

_Branch deleted on 2023-07-06 14:07_

---
