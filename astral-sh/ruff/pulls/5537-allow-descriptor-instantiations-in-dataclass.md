```yaml
number: 5537
title: Allow descriptor instantiations in dataclass fields
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/descriptor
created_at: 2023-07-05T19:05:02Z
updated_at: 2023-07-05T19:35:20Z
url: https://github.com/astral-sh/ruff/pull/5537
synced_at: 2026-01-12T03:36:55Z
```

# Allow descriptor instantiations in dataclass fields

---

_Pull request opened by @charliermarsh on 2023-07-05 19:05_

## Summary

Per the Python documentation, dataclasses are allowed to instantiate descriptors, like so:

```python
class IntConversionDescriptor:
  def __init__(self, *, default):
    self._default = default

  def __set_name__(self, owner, name):
    self._name = "_" + name

  def __get__(self, obj, type):
    if obj is None:
      return self._default

    return getattr(obj, self._name, self._default)

  def __set__(self, obj, value):
    setattr(obj, self._name, int(value))

@dataclass
class InventoryItem:
  quantity_on_hand: IntConversionDescriptor = IntConversionDescriptor(default=100)
```

Closes https://github.com/astral-sh/ruff/issues/4451.


---

_Comment by @github-actions[bot] on 2023-07-05 19:15_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.03ms     4.9 MB/sec    1.01      8.4±0.03ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1780.5±3.72µs     9.4 MB/sec    1.00   1783.4±4.82µs     9.3 MB/sec
formatter/numpy/globals.py                 1.00    195.2±1.34µs    15.1 MB/sec    1.00    195.6±1.03µs    15.1 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.01ms     6.4 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     14.0±0.03ms     2.9 MB/sec    1.00     13.9±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    367.9±1.22µs     8.0 MB/sec    1.00    365.2±1.47µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.1 MB/sec    1.00      6.1±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.7 MB/sec    1.00      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1467.8±2.27µs    11.3 MB/sec    1.00   1461.5±2.49µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    157.3±0.26µs    18.8 MB/sec    1.00    155.8±0.22µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.13ms     4.4 MB/sec    1.00      9.3±0.09ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1978.1±21.43µs     8.4 MB/sec    1.00  1984.5±32.01µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    217.6±4.27µs    13.6 MB/sec    1.00    217.3±4.91µs    13.6 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.06ms     5.7 MB/sec    1.00      4.5±0.06ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.01     15.7±0.13ms     2.6 MB/sec    1.00     15.6±0.14ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.07ms     4.0 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    436.3±7.80µs     6.8 MB/sec    1.00    432.9±9.20µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.07ms     3.6 MB/sec    1.00      7.0±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.06ms     5.0 MB/sec    1.00      8.1±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1684.1±18.38µs     9.9 MB/sec    1.01  1697.4±181.82µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.9±2.40µs    16.2 MB/sec    1.00    181.8±3.82µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     7.0 MB/sec    1.00      3.7±0.06ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-05 19:19_

---

_Closed by @charliermarsh on 2023-07-05 19:19_

---

_Branch deleted on 2023-07-05 19:19_

---
