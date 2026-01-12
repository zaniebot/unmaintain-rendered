```yaml
number: 3768
title: "[isort]: support submodules in known_(first|third)_party config options"
type: pull_request
state: merged
author: astaric
labels: []
assignees: []
merged: true
base: main
head: sorting-subpackage-modules
created_at: 2023-03-28T12:41:14Z
updated_at: 2023-03-29T04:14:09Z
url: https://github.com/astral-sh/ruff/pull/3768
synced_at: 2026-01-12T15:55:13Z
```

# [isort]: support submodules in known_(first|third)_party config options

---

_@astaric_

This is another attempt at #3059. Passes the tests introduced in #3064

I merged the known_first_party and known_third_party members settings into KnownModule so I could add a "shortcut" for all the repositories that do not need this functionality.

Closes https://github.com/charliermarsh/ruff/issues/3059.

---

_Comment by @github-actions[bot] on 2023-03-28 12:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.6±0.02ms     2.8 MB/sec    1.00     14.6±0.02ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.01ms     4.4 MB/sec    1.01      3.8±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    412.9±2.70µs     7.1 MB/sec    1.00    411.3±1.23µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.0 MB/sec    1.00      6.3±0.02ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.02ms     5.3 MB/sec    1.00      7.8±0.01ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1705.0±2.42µs     9.8 MB/sec    1.01   1723.7±5.08µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.7±0.42µs    16.3 MB/sec    1.01    181.7±1.28µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.00ms     7.1 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.8±0.11ms     2.6 MB/sec    1.03     16.3±0.46ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.04ms     4.0 MB/sec    1.01      4.2±0.10ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   442.9±10.45µs     6.7 MB/sec    1.00    443.1±5.52µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.14ms     3.7 MB/sec    1.00      6.9±0.09ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.07ms     4.9 MB/sec    1.00      8.3±0.03ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1802.0±30.38µs     9.2 MB/sec    1.00  1798.9±12.53µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.0±2.22µs    15.7 MB/sec    1.01    189.5±5.71µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.7 MB/sec    1.00      3.8±0.03ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-03-29 03:48_

Thanks! Nice idea. I went ahead and extended it to the other user-provided modules (`known-local-folder` and `extra-standard-library`).

---

_Merged by @charliermarsh on 2023-03-29 03:53_

---

_Closed by @charliermarsh on 2023-03-29 03:53_

---
