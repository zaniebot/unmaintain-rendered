```yaml
number: 5589
title: Fix CI by downgrading to cargo insta 1.29.0
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: fail_tests
created_at: 2023-07-07T10:06:55Z
updated_at: 2023-07-08T15:09:37Z
url: https://github.com/astral-sh/ruff/pull/5589
synced_at: 2026-01-12T03:36:55Z
```

# Fix CI by downgrading to cargo insta 1.29.0

---

_Pull request opened by @konstin on 2023-07-07 10:06_

Since the (implicit) update to cargo-insta 1.30, CI would pass even when the tests failed. This downgrades to cargo insta 1.29.0 and CI fails again when it should (which i can't show here, because CI needs to pass to merge this PR). I've improved the unreferenced snapshot handling in the process

See https://github.com/mitsuhiko/insta/issues/392


---

_Comment by @github-actions[bot] on 2023-07-07 10:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.23ms     4.4 MB/sec    1.18     11.0±0.31ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.08ms     7.9 MB/sec    1.09      2.3±0.05ms     7.2 MB/sec
formatter/numpy/globals.py                 1.00    242.3±8.64µs    12.2 MB/sec    1.07   260.0±10.26µs    11.3 MB/sec
formatter/pydantic/types.py                1.00      4.6±0.11ms     5.6 MB/sec    1.11      5.1±0.12ms     5.0 MB/sec
linter/all-rules/large/dataset.py          1.00     16.5±0.29ms     2.5 MB/sec    1.00     16.5±0.37ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.10ms     4.1 MB/sec    1.00      4.1±0.13ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   543.3±18.21µs     5.4 MB/sec    1.00   545.9±30.27µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.15ms     3.5 MB/sec    1.01      7.3±0.24ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.13ms     5.1 MB/sec    1.01      8.1±0.23ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1767.2±51.53µs     9.4 MB/sec    1.00  1774.6±54.88µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    214.5±6.97µs    13.8 MB/sec    1.00    213.8±6.02µs    13.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.07ms     6.9 MB/sec    1.00      3.7±0.07ms     6.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     13.5±0.57ms     3.0 MB/sec    1.05     14.2±0.54ms     2.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.8±0.16ms     5.9 MB/sec    1.03      2.9±0.15ms     5.8 MB/sec
formatter/numpy/globals.py                 1.00   312.9±17.14µs     9.4 MB/sec    1.02   319.4±20.53µs     9.2 MB/sec
formatter/pydantic/types.py                1.00      6.2±0.30ms     4.1 MB/sec    1.06      6.5±0.36ms     3.9 MB/sec
linter/all-rules/large/dataset.py          1.00     17.8±0.70ms     2.3 MB/sec    1.09     19.5±0.66ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.29ms     3.4 MB/sec    1.03      5.1±0.20ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   592.9±39.60µs     5.0 MB/sec    1.11   655.9±42.41µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.2±0.43ms     3.1 MB/sec    1.09      9.0±0.50ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.1±0.40ms     4.0 MB/sec    1.09     10.9±0.53ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.08ms     8.0 MB/sec    1.12      2.3±0.12ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   256.8±13.04µs    11.5 MB/sec    1.01   259.8±13.30µs    11.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.20ms     5.8 MB/sec    1.05      4.6±0.21ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Fail tests" to "Fix CI by downgrading to cargo insta 1.29.0" by @konstin on 2023-07-07 11:58_

---

_Review comment by @konstin on `.github/workflows/ci.yaml`:62 on 2023-07-07 11:59_

i've fixed the unreferenced snapshot situation while i was already at it

---

_@konstin reviewed on 2023-07-07 11:59_

---

_Review requested from @MichaReiser by @konstin on 2023-07-07 12:00_

---

_Comment by @konstin on 2023-07-07 12:16_

I've confirmed that this indeed catches CI failures again because main had an outdated schema

---

_Comment by @zanieb on 2023-07-07 13:50_

Interesting, what's different in insta 1.30?

---

_Comment by @konstin on 2023-07-07 14:02_

i don't know, and unfortunately i can't reproduce locally. I scrolled through the diff and https://github.com/mitsuhiko/insta/compare/1.29.0...1.30.0#diff-5174eb15293f46eb47c52ac6d42dce9f2f5dd7f8dca11068a2159b342e5c712cR615 is the only thing that looks possibly relevant

---

_@zanieb approved on 2023-07-07 16:56_

---

_@MichaReiser approved on 2023-07-07 17:02_

---

_Comment by @konstin on 2023-07-08 14:13_

Current dependencies on/for this PR:
* main
  * **PR #5589** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/5589" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #5612** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/5612" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/5589?utm_source=stack-comment).

---

_Label `internal` added by @konstin on 2023-07-08 14:46_

---

_Merged by @konstin on 2023-07-08 14:54_

---

_Closed by @konstin on 2023-07-08 14:54_

---

_Branch deleted on 2023-07-08 14:54_

---
