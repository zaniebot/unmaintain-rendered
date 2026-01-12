```yaml
number: 4378
title: Avoid re-using imports beyond current edit site
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/import
created_at: 2023-05-11T17:59:08Z
updated_at: 2023-05-11T18:47:23Z
url: https://github.com/astral-sh/ruff/pull/4378
synced_at: 2026-01-12T03:56:39Z
```

# Avoid re-using imports beyond current edit site

---

_Pull request opened by @charliermarsh on 2023-05-11 17:59_

Given code like:

```py
def a():
    try:
        pass
    except OSError:
        pass


import contextlib
```

Our current edit actions will attempt to form an edit to (1) use `contextlib.suppress` instead of `try`, and (2) retain the `import contextlib` at the bottom of the file. This leads to an out-of-order edit.

One fix here would be to simply reorder the edits. That would _often_ be valid, but it could lead to confusing bugs if the function is called between the definition site and the import site. Another would be to add an import to the top of the file instead of re-using. This would also _often_ be valid, but runs the risk that the import is shadowed between the top-of-file and any call sites. We don't currently have access to that kind of tracing. So, for now, I've opted to just avoid applying a fix in these cases.

Closes #4374.


---

_Label `bug` added by @charliermarsh on 2023-05-11 17:59_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-11 17:59_

---

_Review requested from @konstin by @charliermarsh on 2023-05-11 17:59_

---

_Comment by @charliermarsh on 2023-05-11 17:59_

Quite torn on this. Maybe using the import is actually a better choice (and just sorting the edits).

---

_Comment by @charliermarsh on 2023-05-11 18:00_

Hmm, actually, in the reported case, re-using the import would have led to broken code.

---

_Comment by @charliermarsh on 2023-05-11 18:01_

I think the "right" fix is probably to add an import at top-of-file, if we can ensure that it's not shadowed later on.

---

_Comment by @github-actions[bot] on 2023-05-11 18:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.06     14.7±0.09ms     2.8 MB/sec    1.00     13.8±0.13ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.5±0.03ms     4.8 MB/sec    1.00      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.02    418.8±2.53µs     7.0 MB/sec    1.00    412.0±1.83µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.04      6.1±0.07ms     4.2 MB/sec    1.00      5.8±0.07ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.14      7.7±0.02ms     5.3 MB/sec    1.00      6.7±0.03ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.10   1583.7±1.87µs    10.5 MB/sec    1.00   1441.7±6.07µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.08    172.1±0.49µs    17.1 MB/sec    1.00    159.8±1.06µs    18.5 MB/sec
linter/default-rules/pydantic/types.py     1.11      3.4±0.01ms     7.6 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
parser/large/dataset.py                    1.01      5.4±0.03ms     7.5 MB/sec    1.00      5.3±0.00ms     7.6 MB/sec
parser/numpy/ctypeslib.py                  1.01   1045.6±0.73µs    15.9 MB/sec    1.00   1038.9±2.27µs    16.0 MB/sec
parser/numpy/globals.py                    1.01    106.1±0.19µs    27.8 MB/sec    1.00    105.1±0.22µs    28.1 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.00ms    11.2 MB/sec    1.00      2.3±0.00ms    11.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     22.4±0.90ms  1862.6 KB/sec    1.00     21.7±0.85ms  1917.6 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.6±0.24ms     2.9 MB/sec    1.00      5.6±0.29ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.08   717.1±88.39µs     4.1 MB/sec    1.00   663.0±84.64µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.03      9.4±0.39ms     2.7 MB/sec    1.00      9.2±0.42ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.01     11.5±0.46ms     3.6 MB/sec    1.00     11.4±0.43ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.4±0.15ms     7.0 MB/sec    1.00      2.4±0.11ms     7.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   273.4±15.87µs    10.8 MB/sec    1.00   272.1±18.35µs    10.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.1±0.28ms     5.0 MB/sec    1.00      5.1±0.23ms     5.0 MB/sec
parser/large/dataset.py                    1.01      9.0±0.34ms     4.5 MB/sec    1.00      8.9±0.36ms     4.5 MB/sec
parser/numpy/ctypeslib.py                  1.04  1761.0±84.21µs     9.5 MB/sec    1.00  1697.6±91.47µs     9.8 MB/sec
parser/numpy/globals.py                    1.00   178.1±10.22µs    16.6 MB/sec    1.00   178.2±14.75µs    16.6 MB/sec
parser/pydantic/types.py                   1.01      3.8±0.16ms     6.7 MB/sec    1.00      3.8±0.16ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-05-11 18:13_

---

_Merged by @charliermarsh on 2023-05-11 18:47_

---

_Closed by @charliermarsh on 2023-05-11 18:47_

---

_Branch deleted on 2023-05-11 18:47_

---

_Renamed from "Avoid re-using imports beyond current edit size" to "Avoid re-using imports beyond current edit site" by @charliermarsh on 2023-05-11 18:47_

---
