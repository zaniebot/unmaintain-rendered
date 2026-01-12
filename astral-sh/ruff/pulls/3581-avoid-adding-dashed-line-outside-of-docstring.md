```yaml
number: 3581
title: Avoid adding dashed line outside of docstring
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/underline
created_at: 2023-03-17T18:07:36Z
updated_at: 2023-03-17T19:01:23Z
url: https://github.com/astral-sh/ruff/pull/3581
synced_at: 2026-01-12T15:55:13Z
```

# Avoid adding dashed line outside of docstring

---

_@charliermarsh_

If you have a docstring like:

```py
def no_underline_and_no_newline():  # noqa: D416
    """Toggle the gizmo.
    Returns"""
```

Then right now, when we inject the dashed underline under `Returns`, we end up putting it _outside_ of the docstring 🤦 This PR adds handling for this (uncommon) "no following lines" case.

Closes #3534.


---

_Merged by @charliermarsh on 2023-03-17 18:40_

---

_Closed by @charliermarsh on 2023-03-17 18:40_

---

_Branch deleted on 2023-03-17 18:40_

---

_Comment by @github-actions[bot] on 2023-03-17 18:48_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.6±0.07ms     3.0 MB/sec  
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec  
linter/all-rules/numpy/globals.py          1.00    486.3±1.22µs     6.1 MB/sec  
linter/all-rules/pydantic/types.py         1.00      6.0±0.02ms     4.3 MB/sec  
linter/default-rules/large/dataset.py      1.00      7.6±0.02ms     5.4 MB/sec  
linter/default-rules/numpy/ctypeslib.py    1.00   1677.8±4.37µs     9.9 MB/sec  
linter/default-rules/numpy/globals.py      1.00    189.7±0.40µs    15.6 MB/sec  
linter/default-rules/pydantic/types.py     1.00      3.5±0.01ms     7.2 MB/sec  
linter/large/dataset.py                                                           1.00      7.6±0.03ms     5.3 MB/sec
linter/numpy/ctypeslib.py                                                         1.00      2.3±0.00ms   145.4 MB/sec
linter/numpy/globals.py                                                           1.00   1208.6±4.71µs   147.5 MB/sec
linter/pydantic/types.py                                                          1.00      3.6±0.01ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.9±0.23ms     2.6 MB/sec  
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.07ms     4.0 MB/sec  
linter/all-rules/numpy/globals.py          1.00   529.5±11.83µs     5.6 MB/sec  
linter/all-rules/pydantic/types.py         1.00      7.0±0.09ms     3.6 MB/sec  
linter/default-rules/large/dataset.py      1.00      8.8±0.13ms     4.6 MB/sec  
linter/default-rules/numpy/ctypeslib.py    1.00  1917.3±31.84µs     8.7 MB/sec  
linter/default-rules/numpy/globals.py      1.00    208.7±2.80µs    14.1 MB/sec  
linter/default-rules/pydantic/types.py     1.00      4.0±0.05ms     6.3 MB/sec  
linter/large/dataset.py                                                           1.00      8.9±0.25ms     4.6 MB/sec
linter/numpy/ctypeslib.py                                                         1.00      2.9±0.02ms   116.6 MB/sec
linter/numpy/globals.py                                                           1.00  1523.6±39.78µs   117.0 MB/sec
linter/pydantic/types.py                                                          1.00      4.1±0.09ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
