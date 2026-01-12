```yaml
number: 3988
title: "Remove autofix behavior for uncapitalized-environment-variables (`SIM112`)"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/up
created_at: 2023-04-16T23:01:44Z
updated_at: 2023-04-16T23:35:16Z
url: https://github.com/astral-sh/ruff/pull/3988
synced_at: 2026-01-12T15:55:14Z
```

# Remove autofix behavior for uncapitalized-environment-variables (`SIM112`)

---

_@charliermarsh_

This is a dangerous fix, since it's not purely contained within the code. That is: if you change the environment variable here, you're effectively guaranteed to break code if the user doesn't notice and propagate the change to their environment.

We can revisit when we support autofix severities or a review mode of some sort, but for now, it seems too dangerous.

Closes #3987.


---

_Renamed from "Remove autofix behavior for uncapitalized-environment-variables (SIM112)" to "Remove autofix behavior for uncapitalized-environment-variables (`SIM112`)" by @charliermarsh on 2023-04-16 23:01_

---

_Merged by @charliermarsh on 2023-04-16 23:19_

---

_Closed by @charliermarsh on 2023-04-16 23:19_

---

_Branch deleted on 2023-04-16 23:19_

---

_Comment by @github-actions[bot] on 2023-04-16 23:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.6±0.17ms     2.8 MB/sec    1.00     14.5±0.12ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.03ms     4.6 MB/sec    1.00      3.6±0.03ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.01    463.1±1.09µs     6.4 MB/sec    1.00    459.2±1.24µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.08ms     4.1 MB/sec    1.00      6.2±0.04ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.03ms     5.6 MB/sec    1.01      7.3±0.05ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1623.1±2.55µs    10.3 MB/sec    1.01   1639.1±2.91µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.4±3.25µs    16.3 MB/sec    1.00    181.4±0.51µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.6 MB/sec    1.00      3.3±0.01ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.4±0.10ms     3.0 MB/sec    1.00     13.4±0.05ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.01      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    347.4±2.62µs     8.5 MB/sec    1.02   352.8±13.60µs     8.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.6±0.02ms     4.6 MB/sec    1.02      5.7±0.03ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.02ms     5.9 MB/sec    1.01      7.0±0.02ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1439.1±6.74µs    11.6 MB/sec    1.02   1470.0±5.89µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    150.7±2.10µs    19.6 MB/sec    1.01    152.1±2.13µs    19.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.02      3.1±0.01ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
