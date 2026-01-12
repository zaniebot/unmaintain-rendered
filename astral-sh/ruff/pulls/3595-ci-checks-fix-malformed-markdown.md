```yaml
number: 3595
title: "CI Checks: Fix malformed markdown"
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: ci/fix-invalid-markdown
created_at: 2023-03-18T09:50:35Z
updated_at: 2023-03-18T10:25:13Z
url: https://github.com/astral-sh/ruff/pull/3595
synced_at: 2026-01-12T04:39:45Z
```

# CI Checks: Fix malformed markdown

---

_Pull request opened by @MichaReiser on 2023-03-18 09:50_

## Summary 
The Benchmark results aren't formatted properly if the ecosystem check finds differences because the ecosystem check doesn't emit a trailing newline.

This PR enforces a new line between the ecosystem and benchmark results. 

## Test Plan

I manually triggered the [PR Comment Workflow](https://github.com/charliermarsh/ruff/actions/runs/4454806535) (with the version on this branch) for #3569. The [updated comment](https://github.com/charliermarsh/ruff/pull/3569#issuecomment-1473062469) is correctly formatted.

---

_Marked ready for review by @MichaReiser on 2023-03-18 10:03_

---

_Comment by @MichaReiser on 2023-03-18 10:04_

Going ahead with merging this because it is a trivial change

---

_Merged by @MichaReiser on 2023-03-18 10:04_

---

_Closed by @MichaReiser on 2023-03-18 10:04_

---

_Branch deleted on 2023-03-18 10:04_

---

_Comment by @github-actions[bot] on 2023-03-18 10:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     20.1±0.53ms     2.0 MB/sec    1.00     19.7±0.61ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.1±0.23ms     3.3 MB/sec    1.00      5.1±0.22ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.02   667.7±24.65µs     4.4 MB/sec    1.00   657.5±29.03µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.7±0.31ms     2.9 MB/sec    1.00      8.6±0.38ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.03     10.8±0.29ms     3.8 MB/sec    1.00     10.5±0.32ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.3±0.09ms     7.4 MB/sec    1.00      2.2±0.10ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.01   274.5±16.03µs    10.7 MB/sec    1.00   272.5±22.78µs    10.8 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.9±0.19ms     5.2 MB/sec    1.00      4.7±0.17ms     5.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     16.2±0.16ms     2.5 MB/sec    1.00     15.5±0.12ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.3±0.04ms     3.9 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    546.4±5.40µs     5.4 MB/sec    1.00   543.4±10.01µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.05      7.2±0.11ms     3.5 MB/sec    1.00      6.9±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.05      9.1±0.08ms     4.5 MB/sec    1.00      8.7±0.07ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1977.9±19.86µs     8.4 MB/sec    1.00  1896.6±44.63µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    216.0±4.55µs    13.7 MB/sec    1.00    213.1±6.26µs    13.8 MB/sec
linter/default-rules/pydantic/types.py     1.04      4.2±0.04ms     6.1 MB/sec    1.00      4.0±0.05ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
