```yaml
number: 3776
title: "Remove star-import handling from `sys-exit-alias`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/star
created_at: 2023-03-28T16:11:38Z
updated_at: 2023-03-29T21:54:38Z
url: https://github.com/astral-sh/ruff/pull/3776
synced_at: 2026-01-12T04:39:45Z
```

# Remove star-import handling from `sys-exit-alias`

---

_Pull request opened by @charliermarsh on 2023-03-28 16:11_

## Summary

Right now, if you do:

```py
from sys import *

exit(1)
```

We'll avoid telling you to use sys.exit, since we hard-code handling of this specific case. But our star import handling is not very good -- and, for example, this will raise a lint error:

```py
from sys import *
from os import *

exit(1)  # PLR1722 [*] Use `sys.exit()` instead of `exit`
```

...for reasons that relate to implementation details.

I want to remove this handling, since so much stuff doesn't work properly with star imports anyway, and I want to instead revisit holistically rather than try to handle this one case.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-28 16:11_

---

_Comment by @github-actions[bot] on 2023-03-28 16:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     16.9±0.74ms     2.4 MB/sec    1.00     16.5±0.54ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      4.4±0.26ms     3.8 MB/sec    1.00      4.1±0.19ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.02   596.9±30.65µs     4.9 MB/sec    1.00   585.6±29.35µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.29ms     3.4 MB/sec    1.01      7.4±0.36ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.29ms     4.7 MB/sec    1.00      8.6±0.34ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1949.2±72.03µs     8.5 MB/sec    1.00  1875.8±86.15µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.02   234.8±13.36µs    12.6 MB/sec    1.00   230.8±11.69µs    12.8 MB/sec
linter/default-rules/pydantic/types.py     1.05      4.1±0.13ms     6.3 MB/sec    1.00      3.9±0.14ms     6.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.3±0.11ms     2.7 MB/sec    1.00     15.4±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.05ms     4.1 MB/sec    1.00      4.0±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    498.3±9.49µs     5.9 MB/sec    1.00    497.9±7.73µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.06ms     3.9 MB/sec    1.00      6.6±0.06ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.07ms     5.0 MB/sec    1.00      8.1±0.05ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1763.7±15.63µs     9.4 MB/sec    1.00  1770.4±15.97µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    193.3±2.45µs    15.3 MB/sec    1.01    195.0±4.52µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.00      3.7±0.04ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser reviewed on 2023-03-29 07:02_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/sys_exit_alias.rs`:80 on 2023-03-29 07:02_

Removing the special handling trades a false-negative to a false-positive. This is, in my view, the worse trade. Can we exit early if we see any star import? This has the advantage that we avoid the false-negative without introducing new false-positives. 

---

_@charliermarsh reviewed on 2023-03-29 13:56_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/sys_exit_alias.rs`:80 on 2023-03-29 13:56_

Yeah fair enough. The existing behavior is "better" and we can retain it to avoiding adding false positives here. My feeling was that it's somewhat random because so much of Ruff doesn't work if you use star imports.

For example, if you do:

```py
from sys import *

exit(1)
```

We do currently (prior to this PR) avoid raising `sys-exit-alias`, but in other rules, we consider `exit` to be the builtin `exit` and not `sys.exit` (i.e., it's special-cased here).

If you do:

```py
from sys import *

path
```

You get an undefined reference error, even though `sys.path` is a real export, since we don't know what those exports are.

If you do:

```py
from sys import *
from os import *

exit(1)
```

Then suddenly we flag `sys-exit-alias` again because of limitations in the star-import model.

So, I was left feeling like we need to redo the star handling more holistically, and that this felt like a strange thing to special-case given how much else is likely going wrong. But, it's not hard to retain this.


---

_@MichaReiser approved on 2023-03-29 15:20_

---

_Merged by @charliermarsh on 2023-03-29 21:33_

---

_Closed by @charliermarsh on 2023-03-29 21:33_

---

_Branch deleted on 2023-03-29 21:33_

---
