```yaml
number: 6160
title: Ignore end-of-line file exemption comments
type: pull_request
state: merged
author: charliermarsh
labels:
  - suppression
assignees: []
merged: true
base: main
head: charlie/eol
created_at: 2023-07-28T18:36:42Z
updated_at: 2023-07-29T01:02:32Z
url: https://github.com/astral-sh/ruff/pull/6160
synced_at: 2026-01-12T02:58:30Z
```

# Ignore end-of-line file exemption comments

---

_Pull request opened by @charliermarsh on 2023-07-28 18:36_

## Summary

This PR protects against code like:

```python
from typing import Optional

import bar  # ruff: noqa
import baz

class Foo:
    x: Optional[str] = None
```

In which the user wrote `# ruff: noqa` to ignore a specific error, not realizing that it was a file-level exemption that thus turned off all lint rules.

Specifically, if a `# ruff: noqa` directive is not at the start of a line, we now ignore it and warn, since this is almost certainly a mistake.


---

_Review requested from @zanieb by @charliermarsh on 2023-07-28 18:36_

---

_Label `noqa` added by @charliermarsh on 2023-07-28 18:36_

---

_Comment by @github-actions[bot] on 2023-07-28 18:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.0±0.16ms     4.1 MB/sec    1.01     10.1±0.21ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1968.3±34.29µs     8.5 MB/sec    1.00  1971.8±30.26µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    214.9±2.61µs    13.7 MB/sec    1.00    214.9±2.02µs    13.7 MB/sec
formatter/pydantic/types.py                1.01      4.3±0.04ms     6.0 MB/sec    1.00      4.2±0.06ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.09ms     3.1 MB/sec    1.00     13.3±0.10ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     5.0 MB/sec    1.01      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    453.2±1.03µs     6.5 MB/sec    1.01    456.7±0.82µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.07ms     4.3 MB/sec    1.01      6.0±0.08ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.04ms     5.7 MB/sec    1.01      7.2±0.04ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1488.9±38.70µs    11.2 MB/sec    1.00   1489.2±5.57µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.8±0.41µs    18.0 MB/sec    1.00    163.9±2.21µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.33      4.2±1.10ms     6.1 MB/sec    1.00      3.2±0.03ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03     11.6±0.32ms     3.5 MB/sec    1.00     11.3±0.27ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.2±0.08ms     7.4 MB/sec    1.00      2.2±0.09ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00   243.0±16.26µs    12.1 MB/sec    1.02   248.6±14.05µs    11.9 MB/sec
formatter/pydantic/types.py                1.04      4.9±0.18ms     5.2 MB/sec    1.00      4.8±0.19ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.3±0.38ms     2.7 MB/sec    1.01     15.4±0.43ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.1±0.23ms     4.0 MB/sec    1.00      4.0±0.13ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.05   503.3±24.56µs     5.9 MB/sec    1.00   480.7±19.96µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.9±0.19ms     3.7 MB/sec    1.00      6.8±0.22ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.22ms     4.8 MB/sec    1.01      8.5±0.23ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1707.8±54.16µs     9.8 MB/sec    1.00  1710.0±64.83µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.03    196.5±9.12µs    15.0 MB/sec    1.00    190.3±8.94µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.11ms     6.9 MB/sec    1.00      3.7±0.10ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-07-28 20:20_

Do we not have tests for this behavior?

---

_Review comment by @MichaReiser on `crates/ruff/src/noqa.rs`:253 on 2023-07-28 21:11_

Could we use indentation_at_offset here? It is okay if it is Some, None means the directive is not an own line comment. Or would that be confusing

Nit: it's a bit unfortunate that line_start first does a backwards scan finding the start of the line, just to do a forward scan right after

---

_Review comment by @MichaReiser on `crates/ruff/src/noqa.rs`:263 on 2023-07-28 21:12_

We should at an explanation why it is unexpected 

---

_@MichaReiser approved on 2023-07-28 21:14_

Nice quality of life improvement 

---

_Comment by @charliermarsh on 2023-07-29 00:08_

> Do we not have tests for this behavior?

We have tests for `# ruff: noqa` more broadly, but I doubt we have one that puts a `# ruff: noqa` at the end of a line, since that's the mistake we're trying to catch here :) I will add another test for that case.

---

_Merged by @charliermarsh on 2023-07-29 00:40_

---

_Closed by @charliermarsh on 2023-07-29 00:40_

---

_Branch deleted on 2023-07-29 00:40_

---
