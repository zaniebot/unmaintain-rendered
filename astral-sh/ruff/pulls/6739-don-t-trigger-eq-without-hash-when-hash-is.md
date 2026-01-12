```yaml
number: 6739
title: "Don't trigger `eq-without-hash` when `__hash__` is explicitly set to `None`"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-PLW1641
created_at: 2023-08-21T20:41:35Z
updated_at: 2023-08-25T20:45:27Z
url: https://github.com/astral-sh/ruff/pull/6739
synced_at: 2026-01-12T02:45:38Z
```

# Don't trigger `eq-without-hash` when `__hash__` is explicitly set to `None`

---

_Pull request opened by @LaBatata101 on 2023-08-21 20:41_

## Summary
Closes #6701 

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-08-21 20:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.8±0.08ms    10.6 MB/sec    1.03      3.9±0.05ms    10.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   816.7±13.68µs    20.4 MB/sec    1.02   830.7±10.04µs    20.0 MB/sec
formatter/numpy/globals.py                 1.00     88.4±2.47µs    33.4 MB/sec    1.01     89.2±1.13µs    33.1 MB/sec
formatter/pydantic/types.py                1.00  1581.2±29.75µs    16.1 MB/sec    1.02  1617.8±25.86µs    15.8 MB/sec
linter/all-rules/large/dataset.py          1.01     11.8±0.21ms     3.4 MB/sec    1.00     11.8±0.13ms     3.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.04ms     5.2 MB/sec    1.00      3.2±0.03ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    445.2±6.68µs     6.6 MB/sec    1.02    453.0±6.17µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.10ms     4.2 MB/sec    1.01      6.1±0.08ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.3±0.10ms     6.5 MB/sec    1.01      6.3±0.06ms     6.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1384.5±19.39µs    12.0 MB/sec    1.01  1396.5±18.96µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.6±2.84µs    18.3 MB/sec    1.03    166.9±1.87µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.04ms     9.0 MB/sec    1.01      2.9±0.04ms     8.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.8±0.06ms    10.7 MB/sec    1.00      3.8±0.08ms    10.7 MB/sec
formatter/numpy/ctypeslib.py               1.00   775.5±11.87µs    21.5 MB/sec    1.00   777.3±21.25µs    21.4 MB/sec
formatter/numpy/globals.py                 1.00     81.8±1.91µs    36.1 MB/sec    1.00     81.7±1.72µs    36.1 MB/sec
formatter/pydantic/types.py                1.00  1559.0±36.91µs    16.4 MB/sec    1.00  1566.1±32.72µs    16.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.0±0.22ms     3.1 MB/sec    1.01     13.2±0.25ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.08ms     4.7 MB/sec    1.00      3.5±0.07ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    444.6±7.64µs     6.6 MB/sec    1.01    447.3±9.42µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.15ms     3.7 MB/sec    1.02      7.0±0.17ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.11ms     5.6 MB/sec    1.00      7.2±0.11ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1534.2±23.75µs    10.9 MB/sec    1.00  1522.9±21.49µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.02    178.9±7.60µs    16.5 MB/sec    1.00    174.8±3.96µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.05ms     7.8 MB/sec    1.00      3.2±0.06ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `crates/ruff/src/rules/pylint/rules/eq_without_hash.rs`:82 on 2023-08-21 20:53_

Does this need to be `None` or would assignment work?

---

_@konstin approved on 2023-08-21 20:53_

---

_@LaBatata101 reviewed on 2023-08-21 21:33_

---

_Review comment by @LaBatata101 on `crates/ruff/src/rules/pylint/rules/eq_without_hash.rs`:82 on 2023-08-21 21:33_

> While the purpose of PLW1641 is exactly to check for when `__eq__` is defined and `__hash__` is None (the default when `__eq__` is defined but `__hash__` is not), I think that if the code explicitly sets `__hash__ = None` then the lint should not trigger since the programmer explicitly asks for it.

It needs to be `None` according to the issue description.

---

_@zanieb reviewed on 2023-08-21 22:20_

---

_Review comment by @zanieb on `crates/ruff/src/rules/pylint/rules/eq_without_hash.rs`:82 on 2023-08-21 22:20_

👍 

---

_Label `bug` added by @charliermarsh on 2023-08-21 23:43_

---

_Merged by @charliermarsh on 2023-08-21 23:51_

---

_Closed by @charliermarsh on 2023-08-21 23:51_

---

_Comment by @charliermarsh on 2023-08-22 00:28_

Thanks @LaBatata101!

---

_Branch deleted on 2023-08-25 20:45_

---
