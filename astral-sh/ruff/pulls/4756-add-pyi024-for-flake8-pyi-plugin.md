```yaml
number: 4756
title: "Add PYI024 for `flake8-pyi` plugin"
type: pull_request
state: merged
author: qdegraaf
labels:
  - rule
assignees: []
merged: true
base: main
head: feature/PYI024
created_at: 2023-05-31T14:08:38Z
updated_at: 2023-05-31T16:32:34Z
url: https://github.com/astral-sh/ruff/pull/4756
synced_at: 2026-01-12T03:50:03Z
```

# Add PYI024 for `flake8-pyi` plugin

---

_Pull request opened by @qdegraaf on 2023-05-31 14:08_

## Summary

Implements [Y024](https://github.com/PyCQA/flake8-pyi/blob/ceab86d16b822d12abae1d8087850d0bc66d2f52/pyi.py#L2062) from `flake8-pyi` with autofix

Implemented as `flake8-pyi` implemented it by checking the import. Does not catch e.g.:

```python
import collections

some_named_tuple = collections.namedtuple("foo", ["x", "y"])
```

Can change the implementation to check for calls to `collections.namedtuple` instead of import if we prefer to deviate from upstream implementation.

For autofix, went with `Fix::suggested` as typing import may already exist and any calls to `collections.namedtuple` need to be fixed as well. If manual (or no autofix) is better in this scenario let me know and I will change/remove.

## Test Plan

Added stub and non-stub files with the offending import

## Issues

Refers: https://github.com/charliermarsh/ruff/issues/848 

---

_@qdegraaf reviewed on 2023-05-31 14:11_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_pyi/rules/incorrect_named_tuple.rs`:58 on 2023-05-31 14:11_

Tried adding a check here similar to  `is_builtin` to check if we have the core python module and not some local import. Could not get it to work (found `resolve_imported_module_path` and tried something similar to `/// PLW0406` but no success). 

Any input on this is more than welcome.



---

_Comment by @github-actions[bot] on 2023-05-31 14:49_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.07ms     2.9 MB/sec    1.01     14.4±0.18ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.4±0.63µs     7.0 MB/sec    1.00    426.1±0.69µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.03ms     4.3 MB/sec    1.00      5.9±0.05ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      6.9±0.04ms     5.9 MB/sec    1.00      6.8±0.02ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1508.2±5.55µs    11.0 MB/sec    1.00   1502.4±3.97µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.9±0.68µs    17.3 MB/sec    1.00    171.5±1.20µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.2±0.01ms     7.8 MB/sec    1.07      5.6±0.01ms     7.3 MB/sec
parser/numpy/ctypeslib.py                  1.00   1020.7±0.44µs    16.3 MB/sec    1.05   1071.8±0.61µs    15.5 MB/sec
parser/numpy/globals.py                    1.00    106.1±0.39µs    27.8 MB/sec    1.03    109.8±1.76µs    26.9 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.06      2.4±0.00ms    10.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.7±0.90ms     2.1 MB/sec    1.00     19.7±0.69ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.28ms     3.3 MB/sec    1.01      5.2±0.35ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.03   613.1±32.03µs     4.8 MB/sec    1.00   595.4±29.22µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.4±0.35ms     3.0 MB/sec    1.00      8.3±0.38ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.9±0.38ms     4.1 MB/sec    1.00     10.0±0.43ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.1±0.10ms     8.0 MB/sec    1.00      2.1±0.09ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.01   256.4±11.29µs    11.5 MB/sec    1.00   253.7±18.69µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.04      4.7±0.21ms     5.5 MB/sec    1.00      4.5±0.21ms     5.7 MB/sec
parser/large/dataset.py                    1.01      7.8±0.29ms     5.2 MB/sec    1.00      7.8±0.23ms     5.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1511.9±74.95µs    11.0 MB/sec    1.01  1521.4±63.97µs    10.9 MB/sec
parser/numpy/globals.py                    1.04    160.0±9.68µs    18.4 MB/sec    1.00    153.9±9.47µs    19.2 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.28ms     7.5 MB/sec    1.00      3.4±0.15ms     7.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `rule` added by @charliermarsh on 2023-05-31 15:43_

---

_Merged by @charliermarsh on 2023-05-31 16:07_

---

_Closed by @charliermarsh on 2023-05-31 16:07_

---

_Branch deleted on 2023-05-31 16:32_

---
