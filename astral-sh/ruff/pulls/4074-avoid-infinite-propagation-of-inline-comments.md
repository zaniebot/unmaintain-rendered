```yaml
number: 4074
title: Avoid infinite-propagation of inline comments when force-splitting imports
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - isort
assignees: []
merged: true
base: main
head: charlie/force-single
created_at: 2023-04-24T04:20:47Z
updated_at: 2023-04-24T04:59:48Z
url: https://github.com/astral-sh/ruff/pull/4074
synced_at: 2026-01-12T15:55:14Z
```

# Avoid infinite-propagation of inline comments when force-splitting imports

---

_@charliermarsh_

## Summary

In #3521, we modified our `force-single-line = true` behavior to preserve inline comments on imports when splitting multi-member imports into multiple single-member imports. This is useful, since those inline comments are often suppressions that we want to propagate and preserve. For example given:

```py
from foo import (  # noqa: F401
  bar,
  baz,
)
```

After #3521, we reformatted that (with `force-single-line = true`) to:

```py
from foo import bar  # noqa: F401
from foo import baz  # noqa: F401
```

This is all well and good, but #4062 pointed out a case in which this comment propagate caused us to infinitely duplicate those inline comments in certain cases, leading to an exponential blowup.

Given:

```py
from mypackage.subpackage import (  # long comment that seems to be a problem
    a_long_variable_name_that_causes_problems,
    items,
)
```

After one round of fixing, we'd produce:

```py
from mypackage.subpackage import (  # long comment that seems to be a problem
    a_long_variable_name_that_causes_problems,
)
from mypackage.subpackage import items  # long comment that seems to be a problem
```

On the next pass, we'd recombine these imports, and notice that `from mypackage.subpackage import items` is missing the inline comment `# long comment that seems to be a problem`, which is instead attached to the _member import_ `items`. When we then force-split these imports, we'd duplicate the comment:

```py
from mypackage.subpackage import (  # long comment that seems to be a problem
    a_long_variable_name_that_causes_problems,
)
from mypackage.subpackage import (  # long comment that seems to be a problem
    items,  # long comment that seems to be a problem
)
```

On the next pass, we'd find _two_ inline comments, and we'd duplicate them on each import statement.

Anyway, the fix here is to avoid ever combining separate import statements if they're going to eventually be split. Previously, we'd combine those statements during normalization, then split them up at a later pass via a dedicated method. Instead, if we avoid combining imports and handle the splitting during normalization itself, we avoid all of these issues.

Closes #4062.


---

_Label `bug` added by @charliermarsh on 2023-04-24 04:20_

---

_Label `isort` added by @charliermarsh on 2023-04-24 04:20_

---

_Comment by @github-actions[bot] on 2023-04-24 04:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.03ms     2.9 MB/sec    1.00     14.2±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    455.0±0.96µs     6.5 MB/sec    1.00    456.8±0.86µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.02ms     4.2 MB/sec    1.00      6.0±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.01ms     5.7 MB/sec    1.00      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1623.5±3.51µs    10.3 MB/sec    1.00   1614.1±8.19µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    180.9±0.74µs    16.3 MB/sec    1.00    178.5±0.57µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.00ms     7.7 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
parser/large/dataset.py                    1.00      5.9±0.00ms     6.9 MB/sec    1.09      6.4±0.01ms     6.4 MB/sec
parser/numpy/ctypeslib.py                  1.00   1155.3±0.89µs    14.4 MB/sec    1.07   1234.7±0.64µs    13.5 MB/sec
parser/numpy/globals.py                    1.00    114.8±0.30µs    25.7 MB/sec    1.04    119.9±0.15µs    24.6 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.1 MB/sec    1.08      2.7±0.00ms     9.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     21.1±0.52ms  1969.8 KB/sec    1.00     21.2±0.45ms  1967.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.4±0.16ms     3.1 MB/sec    1.00      5.4±0.15ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   658.7±20.12µs     4.5 MB/sec    1.00   661.3±21.26µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.0±0.26ms     2.8 MB/sec    1.00      9.0±0.21ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.7±0.29ms     3.8 MB/sec    1.00     10.7±0.21ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.4±0.11ms     7.0 MB/sec    1.00      2.3±0.06ms     7.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    265.2±8.68µs    11.1 MB/sec    1.00   262.9±13.14µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.12ms     5.3 MB/sec    1.02      4.9±0.15ms     5.2 MB/sec
parser/large/dataset.py                    1.00      8.4±0.13ms     4.8 MB/sec    1.01      8.4±0.14ms     4.8 MB/sec
parser/numpy/ctypeslib.py                  1.01  1608.0±42.15µs    10.4 MB/sec    1.00  1594.5±35.24µs    10.4 MB/sec
parser/numpy/globals.py                    1.00    160.5±5.16µs    18.4 MB/sec    1.00    160.5±8.00µs    18.4 MB/sec
parser/pydantic/types.py                   1.01      3.7±0.09ms     7.0 MB/sec    1.00      3.6±0.07ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-24 04:39_

---

_Closed by @charliermarsh on 2023-04-24 04:39_

---

_Branch deleted on 2023-04-24 04:39_

---
