```yaml
number: 5540
title: "Use Insiders version of `mkdocs-material`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/insiders
created_at: 2023-07-05T19:41:44Z
updated_at: 2023-07-05T20:58:19Z
url: https://github.com/astral-sh/ruff/pull/5540
synced_at: 2026-01-12T15:55:18Z
```

# Use Insiders version of `mkdocs-material`

---

_@charliermarsh_

## Summary

This PR migrates our `mkdocs-material` version to [Insiders](https://squidfunk.github.io/mkdocs-material/insiders/), which we can access now that we're sponsors.

We can't allow public access to the Insiders version, so we instead have a private fork, which contains a deploy key that I've added as a read-only Actions secret in this repo. (That is: the deploy key only lets you read that one repo, and do nothing else.)

In general, non-Astral contributors can use the non-insiders version, and everything is expected to "work", but without the insiders features (they're intended to be ignored). See: https://squidfunk.github.io/mkdocs-material/insiders/#compatibility.


---

_Label `documentation` added by @charliermarsh on 2023-07-05 19:42_

---

_Comment by @github-actions[bot] on 2023-07-05 20:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.5±0.29ms     3.9 MB/sec    1.00     10.5±0.35ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.09ms     7.3 MB/sec    1.07      2.4±0.18ms     6.9 MB/sec
formatter/numpy/globals.py                 1.00   261.8±14.99µs    11.3 MB/sec    1.00   261.1±10.33µs    11.3 MB/sec
formatter/pydantic/types.py                1.00      5.0±0.17ms     5.1 MB/sec    1.03      5.1±0.18ms     5.0 MB/sec
linter/all-rules/large/dataset.py          1.01     18.8±0.66ms     2.2 MB/sec    1.00     18.5±0.58ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.14ms     3.7 MB/sec    1.01      4.5±0.17ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   574.8±24.08µs     5.1 MB/sec    1.03   593.4±20.32µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.1±0.22ms     3.2 MB/sec    1.00      8.1±0.32ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      9.0±0.30ms     4.5 MB/sec    1.02      9.2±0.24ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1926.4±50.46µs     8.6 MB/sec    1.02  1969.5±57.71µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    224.3±9.33µs    13.2 MB/sec    1.05   235.1±13.08µs    12.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.12ms     6.4 MB/sec    1.04      4.1±0.21ms     6.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.8±0.32ms     3.8 MB/sec    1.00     10.7±0.61ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.10ms     7.1 MB/sec    1.02      2.4±0.27ms     6.9 MB/sec
formatter/numpy/globals.py                 1.00   266.1±17.56µs    11.1 MB/sec    1.04   275.8±18.39µs    10.7 MB/sec
formatter/pydantic/types.py                1.01      5.2±0.24ms     4.9 MB/sec    1.00      5.1±0.66ms     5.0 MB/sec
linter/all-rules/large/dataset.py          1.01     18.7±0.51ms     2.2 MB/sec    1.00     18.6±0.68ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.0±0.18ms     3.3 MB/sec    1.00      4.9±0.17ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   581.2±30.87µs     5.1 MB/sec    1.09   633.4±42.07µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.5±0.25ms     3.0 MB/sec    1.00      8.4±0.28ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.2±0.52ms     4.4 MB/sec    1.03      9.5±0.48ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.0±0.07ms     8.2 MB/sec    1.00  1993.7±134.57µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.06   238.2±11.57µs    12.4 MB/sec    1.00   224.1±14.70µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.05      4.3±0.24ms     5.9 MB/sec    1.00      4.1±0.23ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Add insiders version of MkDocs" to "Use Insiders version of `mkdocs-material`" by @charliermarsh on 2023-07-05 20:31_

---

_Merged by @charliermarsh on 2023-07-05 20:36_

---

_Closed by @charliermarsh on 2023-07-05 20:36_

---

_Branch deleted on 2023-07-05 20:36_

---
