```yaml
number: 3966
title: "Move user-defined section validation into `Settings`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/isort-custom-sections
created_at: 2023-04-13T22:04:48Z
updated_at: 2023-04-13T22:54:32Z
url: https://github.com/astral-sh/ruff/pull/3966
synced_at: 2026-01-12T15:55:14Z
```

# Move user-defined section validation into `Settings`

---

_@charliermarsh_

## Summary

We generally prefer `Settings` to represent the resolved configuration, rather than the user-provided configuration, so it makes sense to me that we'd do this validation and "clean up" the settings in `from(options: Options)`, rather than in the `isort` module. It also means we do the validation once, rather than once per file.

I've also removed some clones by removing the requirement that we store the user-provided modules on `KnownModules` in perpetuity. They're only needed for converting from `Settings` back to `Options`, which IIRC only occurs in the playground -- it's a minority use-case, so I'd rather _that_ operation be expensive than the other direction.

---

_Merged by @charliermarsh on 2023-04-13 22:40_

---

_Closed by @charliermarsh on 2023-04-13 22:40_

---

_Branch deleted on 2023-04-13 22:40_

---

_Comment by @github-actions[bot] on 2023-04-13 22:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.2±0.02ms     2.7 MB/sec    1.00     15.1±0.02ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.8±0.01ms     4.3 MB/sec    1.00      3.8±0.00ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    412.9±1.43µs     7.1 MB/sec    1.00    414.6±0.97µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.01ms     3.9 MB/sec    1.00      6.5±0.01ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.02      7.9±0.02ms     5.1 MB/sec    1.00      7.8±0.01ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1736.0±3.38µs     9.6 MB/sec    1.00   1730.3±4.71µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.0±0.38µs    16.3 MB/sec    1.00    181.6±0.42µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.00ms     7.0 MB/sec    1.00      3.6±0.02ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     20.0±0.60ms     2.0 MB/sec    1.00     19.6±0.46ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.19ms     3.2 MB/sec    1.00      5.1±0.27ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.01   622.7±20.25µs     4.7 MB/sec    1.00   614.1±32.23µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.5±0.22ms     3.0 MB/sec    1.00      8.4±0.34ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.8±0.20ms     4.1 MB/sec    1.01      9.9±0.36ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.07ms     7.8 MB/sec    1.03      2.2±0.06ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   253.2±12.18µs    11.7 MB/sec    1.02   259.3±11.03µs    11.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.17ms     5.6 MB/sec    1.00      4.5±0.14ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
