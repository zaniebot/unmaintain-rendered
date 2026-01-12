```yaml
number: 4494
title: Fix scoping of comprehensions within classes
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/comprehension
created_at: 2023-05-18T14:13:52Z
updated_at: 2023-05-18T14:53:09Z
url: https://github.com/astral-sh/ruff/pull/4494
synced_at: 2026-01-12T03:50:03Z
```

# Fix scoping of comprehensions within classes

---

_Pull request opened by @charliermarsh on 2023-05-18 14:13_

## Summary

See the lengthy inline comment. It turns out that we're missing an edge case in how comprehensions-within-classes are handled. Pyflakes has the same behavior.

For more, see the [PEP 709](https://discuss.python.org/t/pep-709-one-behavior-change-that-was-missed-in-the-pep/26691) follow-up discussion, or the linked issue (#4486).

Closes #4486.


---

_Label `bug` added by @charliermarsh on 2023-05-18 14:13_

---

_Merged by @charliermarsh on 2023-05-18 14:30_

---

_Closed by @charliermarsh on 2023-05-18 14:30_

---

_Branch deleted on 2023-05-18 14:30_

---

_Comment by @github-actions[bot] on 2023-05-18 14:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.0±0.05ms     2.9 MB/sec    1.00     14.0±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.3±0.44µs     7.0 MB/sec    1.01    426.7±1.15µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1466.7±2.36µs    11.4 MB/sec    1.00   1469.9±2.34µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.2±0.94µs    17.9 MB/sec    1.00    165.3±0.68µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.01      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.4±0.01ms     7.6 MB/sec    1.00      5.4±0.00ms     7.6 MB/sec
parser/numpy/ctypeslib.py                  1.00   1059.0±0.52µs    15.7 MB/sec    1.00   1054.0±0.68µs    15.8 MB/sec
parser/numpy/globals.py                    1.01    109.2±4.74µs    27.0 MB/sec    1.00    108.3±0.38µs    27.2 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.1 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.6±0.20ms     2.3 MB/sec    1.00     17.5±0.32ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.4±0.09ms     3.8 MB/sec    1.00      4.3±0.09ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    507.5±8.51µs     5.8 MB/sec    1.01    513.0±8.25µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.2±0.12ms     3.5 MB/sec    1.00      7.1±0.14ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.13ms     4.8 MB/sec    1.01      8.5±0.11ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1734.0±28.64µs     9.6 MB/sec    1.04  1807.4±28.45µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.2±3.36µs    15.0 MB/sec    1.05    206.7±5.36µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.8 MB/sec    1.04      3.9±0.48ms     6.5 MB/sec
parser/large/dataset.py                    1.00      6.7±0.07ms     6.0 MB/sec    1.01      6.8±0.07ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.01  1292.7±27.21µs    12.9 MB/sec    1.00  1285.1±22.98µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    131.9±2.01µs    22.4 MB/sec    1.00    132.6±3.01µs    22.3 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.04ms     8.9 MB/sec    1.00      2.9±0.04ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-05-18 14:36_

Woof this one just bit me last week — additional context in https://github.com/python/cpython/issues/99295 too

I had to change something like

```python
class Foo:
    a = 1
    b = [x for x in range(10) if x != a]   # a is not defined
```

to

```python
class Foo:
    a = 1
    b = [x for x in set(range(10)) - {a}]
```

Might be a helpful case for ruff to detect and suggest! It would have saved me a lot of time to have a ref with details.



---
