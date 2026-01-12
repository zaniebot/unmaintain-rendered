```yaml
number: 6523
title: "importer: skip whitespace between comments at start of file"
type: pull_request
state: merged
author: durumu
labels:
  - isort
assignees: []
merged: true
base: main
head: fix_i002
created_at: 2023-08-12T14:39:13Z
updated_at: 2023-08-12T20:37:56Z
url: https://github.com/astral-sh/ruff/pull/6523
synced_at: 2026-01-12T02:52:04Z
```

# importer: skip whitespace between comments at start of file

---

_Pull request opened by @durumu on 2023-08-12 14:39_

## Summary

When adding an import, such as when fixing `I002`, ruff doesn't skip whitespace between comments, but isort does. See this issue for more detail: https://github.com/astral-sh/ruff/issues/6504

This change would fix that by skipping whitespace between comments in `Insertion.start_of_file()`.

## Test Plan

I added a new test, `comments_and_newlines`, to verify this behavior. I also ran `cargo test` and no existing tests broke. That being said, this is technically a breaking change, as it's possible that someone was relying on the previous behavior.


---

_Comment by @github-actions[bot] on 2023-08-12 14:50_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.5±0.06ms     4.8 MB/sec    1.00      8.4±0.03ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.01   1637.4±2.16µs    10.2 MB/sec    1.00  1623.1±13.78µs    10.3 MB/sec
formatter/numpy/globals.py                 1.00    172.8±0.42µs    17.1 MB/sec    1.00    172.0±0.47µs    17.2 MB/sec
formatter/pydantic/types.py                1.01      3.6±0.01ms     7.1 MB/sec    1.00      3.5±0.02ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.01     10.6±0.02ms     3.8 MB/sec    1.00     10.5±0.03ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      2.9±0.01ms     5.7 MB/sec    1.00      2.9±0.00ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.05    333.2±2.21µs     8.9 MB/sec    1.00    318.1±0.91µs     9.3 MB/sec
linter/all-rules/pydantic/types.py         1.13      5.5±0.02ms     4.6 MB/sec    1.00      4.9±0.01ms     5.2 MB/sec
linter/default-rules/large/dataset.py      1.03      5.6±0.01ms     7.3 MB/sec    1.00      5.4±0.06ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07   1190.4±2.18µs    14.0 MB/sec    1.00   1116.8±4.13µs    14.9 MB/sec
linter/default-rules/numpy/globals.py      1.09    124.7±0.26µs    23.7 MB/sec    1.00    114.5±0.78µs    25.8 MB/sec
linter/default-rules/pydantic/types.py     1.05      2.5±0.02ms    10.1 MB/sec    1.00      2.4±0.00ms    10.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     10.0±0.04ms     4.1 MB/sec    1.00      9.8±0.04ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.01  1909.0±13.01µs     8.7 MB/sec    1.00  1882.1±41.19µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    196.1±2.63µs    15.0 MB/sec    1.02    199.3±7.25µs    14.8 MB/sec
formatter/pydantic/types.py                1.01      4.2±0.02ms     6.0 MB/sec    1.00      4.2±0.05ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.02     12.8±0.10ms     3.2 MB/sec    1.00     12.5±0.05ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.6±0.01ms     4.7 MB/sec    1.00      3.5±0.02ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.02    371.4±5.46µs     7.9 MB/sec    1.00    363.0±8.66µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.13      6.6±0.04ms     3.9 MB/sec    1.00      5.8±0.06ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.03      6.9±0.08ms     5.9 MB/sec    1.00      6.7±0.03ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04   1441.4±8.34µs    11.6 MB/sec    1.00   1380.9±8.20µs    12.1 MB/sec
linter/default-rules/numpy/globals.py      1.04    148.3±1.39µs    19.9 MB/sec    1.00    143.2±1.45µs    20.6 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.1±0.02ms     8.2 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "importer: skip whitespace when checking for comments at start of file" to "importer: skip whitespace between comments at start of file" by @durumu on 2023-08-12 15:41_

---

_@charliermarsh reviewed on 2023-08-12 16:34_

---

_Review comment by @charliermarsh on `crates/ruff/src/importer/insertion.rs`:76 on 2023-08-12 16:34_

Given:

```python
# This is a comment

# This is a comment about this specific import
from sys import stdout
```

I guess we'd now do:

```python
# This is a comment

# This is a comment about this specific import
from __future__ import annotations
from sys import stdout
```

I guess we arguably already had that problem, since given:
```python
# This is a comment about this specific import
from sys import stdout
```

We already do:

```python
# This is a comment about this specific import
from __future__ import annotations
from sys import stdout
```

I suppose we could modify the logic to only skip comments if the comments are followed by a blank line but that may be surprising -- I'm not sure. What do you think?


---

_@durumu reviewed on 2023-08-12 20:23_

---

_Review comment by @durumu on `crates/ruff/src/importer/insertion.rs`:76 on 2023-08-12 20:23_

That's a good point. For what it's worth, `isort -a "from __future__ import annotations"` seems to do the same thing:

```py
# This is a comment

# Comment about this import
from foo import bar
```

turns into:

```py
# This is a comment

# Comment about this import
from __future__ import annotations

from foo import bar
```

That makes me inclined to think this case isn't too valuable to support. If we do want to support it, though, I do think your proposal of skipping comments if the comments are followed by a blank line is pretty reasonable and intuitive.

---

_@charliermarsh reviewed on 2023-08-12 20:37_

---

_Review comment by @charliermarsh on `crates/ruff/src/importer/insertion.rs`:76 on 2023-08-12 20:37_

Ok, thanks for looking into that. If `isort` has this behavior, then it seems reasonable to me to proceed as-is.

---

_Label `isort` added by @charliermarsh on 2023-08-12 20:37_

---

_Merged by @charliermarsh on 2023-08-12 20:37_

---

_Closed by @charliermarsh on 2023-08-12 20:37_

---
