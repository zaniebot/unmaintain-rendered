```yaml
number: 5964
title: "Avoid collapsing `elif` and `else` branches during import sorting"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/elif
created_at: 2023-07-22T02:12:08Z
updated_at: 2023-07-22T02:57:27Z
url: https://github.com/astral-sh/ruff/pull/5964
synced_at: 2026-01-12T03:30:22Z
```

# Avoid collapsing `elif` and `else` branches during import sorting

---

_Pull request opened by @charliermarsh on 2023-07-22 02:12_

## Summary

I ran into this in the wild. It looks like Ruff will collapse the `else` and `elif` branches here (i.e., it doesn't recognize that they're too independent import blocks):

```python
if "sdist" in cmds:
    _sdist = cmds["sdist"]
elif "setuptools" in sys.modules:
    from setuptools.command.sdist import sdist as _sdist
else:
    from setuptools.command.sdist import sdist as _sdist
    from distutils.command.sdist import sdist as _sdist
```

Likely fallout from the `elif_else_branches` refactor.


---

_Marked ready for review by @charliermarsh on 2023-07-22 02:12_

---

_Renamed from "Avoid collapsing elif and else branches during import sorting" to "Avoid collapsing `elif` and `else` branches during import sorting" by @charliermarsh on 2023-07-22 02:12_

---

_Label `bug` added by @charliermarsh on 2023-07-22 02:12_

---

_Comment by @charliermarsh on 2023-07-22 02:13_

Probably gonna cut another release to get this out.

---

_Merged by @charliermarsh on 2023-07-22 02:18_

---

_Closed by @charliermarsh on 2023-07-22 02:18_

---

_Branch deleted on 2023-07-22 02:18_

---

_Comment by @github-actions[bot] on 2023-07-22 02:21_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.03ms     4.4 MB/sec    1.00      9.3±0.03ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.01   1857.7±1.93µs     9.0 MB/sec    1.00   1842.8±3.25µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    201.7±0.41µs    14.6 MB/sec    1.00    201.1±0.24µs    14.7 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.00ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     12.9±0.03ms     3.1 MB/sec    1.00     12.9±0.05ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    436.4±0.63µs     6.8 MB/sec    1.00    436.7±0.51µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.03ms     4.3 MB/sec    1.00      5.9±0.04ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.01ms     6.2 MB/sec    1.00      6.6±0.07ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1420.5±3.48µs    11.7 MB/sec    1.00   1422.7±2.65µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    157.9±0.25µs    18.7 MB/sec    1.00    157.7±0.26µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.00ms     8.5 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.3±0.14ms     3.6 MB/sec    1.01     11.5±0.16ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.04ms     7.6 MB/sec    1.00      2.2±0.04ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00    244.5±8.69µs    12.1 MB/sec    1.02   249.9±14.01µs    11.8 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.07ms     5.3 MB/sec    1.01      4.9±0.07ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.00     16.1±0.15ms     2.5 MB/sec    1.00     16.1±0.13ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.07ms     4.0 MB/sec    1.00      4.2±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   501.1±11.44µs     5.9 MB/sec    1.00    498.7±7.60µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.10ms     3.5 MB/sec    1.00      7.3±0.11ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.07ms     4.9 MB/sec    1.01      8.4±0.09ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1727.4±23.61µs     9.6 MB/sec    1.00  1720.1±21.03µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.3±3.85µs    15.2 MB/sec    1.00    193.6±3.35µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.04ms     6.9 MB/sec    1.00      3.7±0.03ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
