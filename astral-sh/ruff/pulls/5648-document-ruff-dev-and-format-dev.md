```yaml
number: 5648
title: Document ruff_dev and format_dev
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: docs
created_at: 2023-07-10T13:40:56Z
updated_at: 2023-07-12T14:18:24Z
url: https://github.com/astral-sh/ruff/pull/5648
synced_at: 2026-01-12T15:55:19Z
```

# Document ruff_dev and format_dev

---

_@konstin_

## Summary

Document all `ruff_dev` subcommands and document the `format_dev` flags in the formatter readme.

CC @zanieb please flag everything that isn't clear or missing

## Test Plan

n/a


---

_Review requested from @zanieb by @konstin on 2023-07-10 13:40_

---

_@charliermarsh reviewed on 2023-07-10 13:51_

---

_Review comment by @charliermarsh on `crates/ruff_dev/src/generate_all.rs`:24 on 2023-07-10 13:51_

Nit: don't kill me but trailing periods on all user-facing docs please (here and crates/ruff_dev/src/generate_options.rs and crates/ruff_dev/src/generate_rules_table.rs) 😅 

---

_Comment by @charliermarsh on 2023-07-10 13:51_

Nice, thank you for doing this.

---

_@konstin reviewed on 2023-07-10 13:54_

---

_Review comment by @konstin on `crates/ruff_dev/src/generate_all.rs`:24 on 2023-07-10 13:54_

good to know, will do

---

_Comment by @github-actions[bot] on 2023-07-10 14:08_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.0±0.04ms     5.1 MB/sec    1.00      7.9±0.02ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.01   1871.2±9.03µs     8.9 MB/sec    1.00   1855.9±2.74µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    208.6±0.48µs    14.1 MB/sec    1.00    208.1±0.36µs    14.2 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.01ms     6.4 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.06ms     3.0 MB/sec    1.00     13.5±0.06ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    432.4±2.24µs     6.8 MB/sec    1.00    431.1±2.04µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.3 MB/sec    1.00      6.0±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.1 MB/sec    1.00      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1468.2±3.72µs    11.3 MB/sec    1.00   1470.5±3.00µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    170.0±0.87µs    17.4 MB/sec    1.00    168.5±0.20µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.7±0.71ms     3.2 MB/sec    1.03     13.0±0.65ms     3.1 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.8±0.11ms     5.9 MB/sec    1.00      2.8±0.17ms     5.9 MB/sec
formatter/numpy/globals.py                 1.00   322.4±24.09µs     9.2 MB/sec    1.03   330.8±24.40µs     8.9 MB/sec
formatter/pydantic/types.py                1.00      6.1±0.30ms     4.2 MB/sec    1.02      6.3±0.27ms     4.1 MB/sec
linter/all-rules/large/dataset.py          1.05     22.5±0.79ms  1855.2 KB/sec    1.00     21.5±0.66ms  1940.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.7±0.35ms     2.9 MB/sec    1.01      5.7±0.31ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   664.6±34.59µs     4.4 MB/sec    1.02   675.7±67.84µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.5±0.49ms     2.7 MB/sec    1.05     10.0±0.49ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.02     11.1±0.40ms     3.7 MB/sec    1.00     10.9±0.48ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.10ms     7.2 MB/sec    1.02      2.4±0.10ms     7.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   274.3±18.80µs    10.8 MB/sec    1.02   280.1±16.97µs    10.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.25ms     5.3 MB/sec    1.01      4.9±0.35ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-07-10 14:36_

Thank you so much!

---

_Comment by @konstin on 2023-07-12 13:33_

Current dependencies on/for this PR:
* main
  * **PR #5648** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/5648" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #5688** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/5688" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #5710** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/5710" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/5648?utm_source=stack-comment).

---

_Merged by @konstin on 2023-07-12 14:18_

---

_Closed by @konstin on 2023-07-12 14:18_

---

_Branch deleted on 2023-07-12 14:18_

---
