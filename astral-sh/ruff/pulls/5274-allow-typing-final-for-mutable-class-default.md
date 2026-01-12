```yaml
number: 5274
title: "Allow `typing.Final` for `mutable-class-default annotations` (`RUF012`)"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/final
created_at: 2023-06-21T23:54:16Z
updated_at: 2023-06-22T01:25:19Z
url: https://github.com/astral-sh/ruff/pull/5274
synced_at: 2026-01-12T03:43:30Z
```

# Allow `typing.Final` for `mutable-class-default annotations` (`RUF012`)

---

_Pull request opened by @charliermarsh on 2023-06-21 23:54_

## Summary

See: https://github.com/astral-sh/ruff/issues/5243.


---

_Comment by @github-actions[bot] on 2023-06-22 00:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.9±0.01ms     5.9 MB/sec    1.00      6.9±0.04ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1422.9±4.62µs    11.7 MB/sec    1.01   1430.6±3.04µs    11.6 MB/sec
formatter/numpy/globals.py                 1.00    133.2±0.22µs    22.2 MB/sec    1.00    133.4±0.65µs    22.1 MB/sec
formatter/pydantic/types.py                1.00      2.9±0.01ms     8.9 MB/sec    1.00      2.9±0.01ms     8.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.06ms     3.0 MB/sec    1.01     13.7±0.06ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.01      3.5±0.00ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    362.8±5.82µs     8.1 MB/sec    1.00    362.5±1.34µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.02ms     4.2 MB/sec    1.00      6.1±0.03ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.02ms     5.8 MB/sec    1.00      7.1±0.07ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1483.4±12.75µs    11.2 MB/sec    1.01   1493.9±2.15µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.1±0.18µs    18.7 MB/sec    1.00    158.8±0.13µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     7.9 MB/sec    1.00      3.2±0.01ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.08      8.3±0.07ms     4.9 MB/sec    1.00      7.7±0.06ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.06  1730.3±19.65µs     9.6 MB/sec    1.00  1636.7±19.16µs    10.2 MB/sec
formatter/numpy/globals.py                 1.03    164.0±2.47µs    18.0 MB/sec    1.00   159.7±11.17µs    18.5 MB/sec
formatter/pydantic/types.py                1.07      3.5±0.05ms     7.4 MB/sec    1.00      3.2±0.03ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.00     15.0±0.09ms     2.7 MB/sec    1.00     14.9±0.08ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.05ms     4.2 MB/sec    1.00      4.0±0.03ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    484.6±5.36µs     6.1 MB/sec    1.00    483.9±8.98µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.7±0.30ms     3.8 MB/sec    1.00      6.6±0.05ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.02      7.9±0.07ms     5.1 MB/sec    1.00      7.7±0.05ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1680.3±13.55µs     9.9 MB/sec    1.00  1658.7±16.21µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    192.2±2.98µs    15.3 MB/sec    1.00    191.1±3.75µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.6±0.07ms     7.2 MB/sec    1.00      3.5±0.03ms     7.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-22 00:24_

---

_Closed by @charliermarsh on 2023-06-22 00:24_

---

_Branch deleted on 2023-06-22 00:24_

---

_Comment by @andersk on 2023-06-22 00:55_

`Final` doesn’t mean immutable; `Final[list[int]]` is just as mutable as `list[int]`. So I don’t see why it should get a special exemption here. 

```python
from typing import Final

class Foo:
    var: Final[list[int]] = []

a = Foo()
a.var.extend([1, 2, 3])
b = Foo()
print(b.var)  # [1, 2, 3]
```

([mypy playground](https://mypy-play.net/?mypy=latest&python=3.11&gist=7179d63de1694aacb0c35b9b87caea30))

`var: Final[list[int]]` protects against reassignment like `a.var = [1, 2, 3]`, but that has nothing to do with `list`’s mutability: `var: Final[int]` and `a.var = 2` is equally problematic.

---

_Comment by @charliermarsh on 2023-06-22 00:59_

Do you think `ClassVar` should have a special exemption here?

---

_Comment by @charliermarsh on 2023-06-22 01:02_

The reason I'm asking is that [PEP 591](https://peps.python.org/pep-0591/) says:

> Type checkers should infer a final attribute that is initialized in a class body as being a class variable. Variables should not be annotated with both ClassVar and Final.

That is: if a variable is annotated with `Final` and initialized in the class body, it implies `ClassVar`, which we _are_ exempting.


---

_Comment by @andersk on 2023-06-22 01:25_

Good question. By my above logic, `ClassVar` should not be exempted, but maybe the idea is that we only want to protect against the *simultaneous* possibility of both mutation and reassignment, in which case `ClassVar` and `Final` are fine? I withdraw this criticism for now.

---
