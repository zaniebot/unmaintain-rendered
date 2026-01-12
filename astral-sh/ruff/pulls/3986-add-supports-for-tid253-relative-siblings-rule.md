```yaml
number: 3986
title: Add supports for TID253/relative-siblings rule
type: pull_request
state: closed
author: noirbizarre
labels: []
assignees: []
base: main
head: feature/relative-sibling-imports
created_at: 2023-04-16T16:48:03Z
updated_at: 2023-10-19T15:04:47Z
url: https://github.com/astral-sh/ruff/pull/3986
synced_at: 2026-01-12T02:32:41Z
```

# Add supports for TID253/relative-siblings rule

---

_Pull request opened by @noirbizarre on 2023-04-16 16:48_

This PR is the `ruff` counterpart of adamchainz/flake8-tidy-imports#441.

Fixes #1014 

---

_Renamed from "feat(flake8-tidy-imports): add supports for TID253/relative-siblings …" to "Add supports for TID253/relative-siblings rule" by @noirbizarre on 2023-04-16 16:49_

---

_Comment by @github-actions[bot] on 2023-04-16 17:10_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.3±0.02ms     2.8 MB/sec    1.00     14.2±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.02    466.3±1.58µs     6.3 MB/sec    1.00    458.5±1.27µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.02ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.03      7.3±0.01ms     5.6 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1641.6±1.60µs    10.1 MB/sec    1.00   1620.0±2.54µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    181.8±1.97µs    16.2 MB/sec    1.00    179.9±0.95µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.4±0.01ms     7.6 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.24ms     2.5 MB/sec    1.00     16.6±0.19ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.06ms     4.0 MB/sec    1.00      4.2±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    498.4±7.97µs     5.9 MB/sec    1.00   496.4±11.83µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.09ms     3.7 MB/sec    1.00      6.8±0.12ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.03      8.3±0.09ms     4.9 MB/sec    1.00      8.1±0.06ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1791.4±29.84µs     9.3 MB/sec    1.00  1777.2±19.57µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    195.6±4.13µs    15.1 MB/sec    1.00    194.3±4.09µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.08ms     6.8 MB/sec    1.00      3.7±0.09ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-10-19 15:04_

Hey! Sorry you never got a reply here; this looks stalled since the upstream pull request is unmerged. We're also not sure if we want to change this behavior and since this is stale now I'm going to close it. If something changes upstream please let us know and we will reconsider.

---

_Closed by @zanieb on 2023-10-19 15:04_

---
