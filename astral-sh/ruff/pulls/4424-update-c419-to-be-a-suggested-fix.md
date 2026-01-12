```yaml
number: 4424
title: Update C419 to be a suggested fix
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: fix/4320
created_at: 2023-05-14T01:33:38Z
updated_at: 2023-05-15T08:30:40Z
url: https://github.com/astral-sh/ruff/pull/4424
synced_at: 2026-01-12T15:55:15Z
```

# Update C419 to be a suggested fix

---

_@zanieb_

Per discussion in https://github.com/charliermarsh/ruff/issues/4320
Part of https://github.com/charliermarsh/ruff/issues/4184

---

_@zanieb reviewed on 2023-05-14 01:34_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:1315 on 2023-05-14 01:34_

It's not entirely clear to me if the applicability should be decided here in `fixes.rs` or `rules/...`

---

_Comment by @github-actions[bot] on 2023-05-14 01:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.6±0.06ms     2.8 MB/sec    1.00     14.6±0.04ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.00ms     4.6 MB/sec    1.00      3.6±0.00ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    365.2±1.16µs     8.1 MB/sec    1.00    362.7±1.54µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.02ms     5.6 MB/sec    1.00      7.2±0.02ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1508.3±8.55µs    11.0 MB/sec    1.00   1500.5±4.09µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.4±0.29µs    18.2 MB/sec    1.01    163.7±0.37µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     7.9 MB/sec    1.00      3.2±0.01ms     7.9 MB/sec
parser/large/dataset.py                    1.01      5.8±0.01ms     7.0 MB/sec    1.00      5.7±0.01ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1125.9±2.34µs    14.8 MB/sec    1.00   1121.8±2.63µs    14.8 MB/sec
parser/numpy/globals.py                    1.00    115.3±0.29µs    25.6 MB/sec    1.00    115.1±1.00µs    25.6 MB/sec
parser/pydantic/types.py                   1.01      2.5±0.01ms    10.4 MB/sec    1.00      2.4±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     16.4±0.19ms     2.5 MB/sec    1.00     16.1±0.18ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.06ms     4.0 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    482.3±5.85µs     6.1 MB/sec    1.00    480.1±5.60µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.11ms     3.8 MB/sec    1.00      6.8±0.09ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.01      8.0±0.07ms     5.1 MB/sec    1.00      7.9±0.05ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1695.9±32.59µs     9.8 MB/sec    1.00  1665.9±17.83µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    187.2±2.24µs    15.8 MB/sec    1.02    190.1±5.45µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.2 MB/sec    1.01      3.6±0.09ms     7.1 MB/sec
parser/large/dataset.py                    1.00      6.6±0.05ms     6.2 MB/sec    1.00      6.6±0.04ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1252.8±14.35µs    13.3 MB/sec    1.00  1254.4±12.71µs    13.3 MB/sec
parser/numpy/globals.py                    1.00    128.7±1.55µs    22.9 MB/sec    1.00    128.9±1.60µs    22.9 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.1 MB/sec    1.00      2.8±0.05ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:1315 on 2023-05-15 07:46_

I think either is fine since the method is only used in `unnecessary_comprehension_any_all` (that's why I personally would move the function and co-locate it with the rule). 

Another reasoning: If we always know that a certain `Edit` is potentially unsafe, then it's okay to decide in the helper method. 

---

_@MichaReiser approved on 2023-05-15 07:48_

The code itself looks good to me but I don't have enough python knowledge to assess the situations in which rewriting `[i for i in a]` into `i for i in a` is semantically different. @madkinsz can you explain your reasoning or @konstin @charliermarsh can you review the categorisation?

---

_Comment by @konstin on 2023-05-15 08:25_

Technically, we're changing eager to lazy, e.g.:

```python
from typing import Generator


def count() -> Generator[int, None, None]:
    for i in range(4):
        print(i)
        yield i


if __name__ == "__main__":
    print("pre fix")
    any([i > 0 for i in count()])
    print("after fix")
    any(i > 0 for i in count())
```

```
pre fix
0
1
2
3
after fix
0
1
```

I expect that this being a problem is rare, it would mean that your code relies on a generator being used to completion. A more common problem could be that we don't resolve the call path yet and sometimes people rebind `any`/`all`:

https://github.com/charliermarsh/ruff/blob/c10a4535b9549d8dab6dec024ce3231b74bf5e18/crates/ruff/src/rules/flake8_comprehensions/rules/unnecessary_comprehension_any_all.rs#L72 

I think suggested is the correct category.

---

_@konstin approved on 2023-05-15 08:26_

---

_Comment by @MichaReiser on 2023-05-15 08:30_

Thx

---

_Merged by @MichaReiser on 2023-05-15 08:30_

---

_Closed by @MichaReiser on 2023-05-15 08:30_

---
