```yaml
number: 5688
title: Link issue tracker in contributing docs
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: add_issue_tracker_links_to_contributing_docs
created_at: 2023-07-11T13:45:18Z
updated_at: 2023-07-13T11:06:17Z
url: https://github.com/astral-sh/ruff/pull/5688
synced_at: 2026-01-12T15:55:19Z
```

# Link issue tracker in contributing docs

---

_@konstin_

## Summary

This adds links to issue categories that are good for people looking to implement something and a link to the contributing guide feedback issue (https://github.com/astral-sh/ruff/issues/5684)

---

_Label `internal` added by @konstin on 2023-07-11 13:48_

---

_Comment by @github-actions[bot] on 2023-07-11 13:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.0±0.01ms     5.1 MB/sec    1.00      7.9±0.02ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1849.2±1.84µs     9.0 MB/sec    1.00   1856.7±6.68µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    207.5±0.69µs    14.2 MB/sec    1.01   210.1±15.26µs    14.0 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.06ms     6.4 MB/sec    1.00      4.0±0.02ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.02ms     3.0 MB/sec    1.00     13.3±0.02ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     5.0 MB/sec    1.01      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    428.5±0.42µs     6.9 MB/sec    1.01    431.7±0.87µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.3 MB/sec    1.01      6.0±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.1 MB/sec    1.01      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1448.1±2.99µs    11.5 MB/sec    1.02   1474.1±3.07µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.7±0.77µs    17.8 MB/sec    1.02    168.7±0.33µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.01      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.5±0.04ms     4.3 MB/sec    1.00      9.4±0.03ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.01ms     7.9 MB/sec    1.01      2.1±0.01ms     7.9 MB/sec
formatter/numpy/globals.py                 1.00    228.4±1.62µs    12.9 MB/sec    1.00    229.4±7.21µs    12.9 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.02ms     5.4 MB/sec    1.00      4.7±0.03ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.01     15.7±0.11ms     2.6 MB/sec    1.00     15.6±0.06ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.0 MB/sec    1.00      4.1±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    426.7±4.90µs     6.9 MB/sec    1.00    423.8±4.74µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.05ms     3.6 MB/sec    1.00      7.1±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.05ms     5.0 MB/sec    1.00      8.0±0.04ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1653.2±9.62µs    10.1 MB/sec    1.00  1647.3±25.61µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    180.1±1.17µs    16.4 MB/sec    1.00    177.7±1.86µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.02ms     7.0 MB/sec    1.00      3.6±0.02ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb reviewed on 2023-07-11 23:51_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:28 on 2023-07-11 23:51_

```suggestion
[improvements that are ready for contributions](https://github.com/astral-sh/ruff/issues?q=is%3Aissue+is%3Aopen+label%3Aaccepted).
```

---

_@zanieb reviewed on 2023-07-11 23:51_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:39 on 2023-07-11 23:51_

Moved to a discussion right?

---

_Comment by @konstin on 2023-07-12 13:33_

Current dependencies on/for this PR:
* main
  * **PR #5648** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/5648" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #5688** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/5688" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈
      * **PR #5710** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/5710" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/5688?utm_source=stack-comment).

---

_@konstin reviewed on 2023-07-12 14:20_

---

_Review comment by @konstin on `CONTRIBUTING.md`:39 on 2023-07-12 14:20_

yep, updated

---

_Comment by @charliermarsh on 2023-07-12 20:22_

(Disregard deleted comment, I can work around for now.)

---

_@charliermarsh approved on 2023-07-12 20:48_

---

_@charliermarsh reviewed on 2023-07-12 20:49_

---

_Review comment by @charliermarsh on `CONTRIBUTING.md`:39 on 2023-07-12 20:49_

I'd probably write something corny like: "If you have suggestions on how we might improve the contributing documentation, [let us know](https://github.com/astral-sh/ruff/discussions/5693)!"

---

_@charliermarsh reviewed on 2023-07-12 20:49_

LGTM, thanks.

---

_@konstin reviewed on 2023-07-13 10:35_

---

_Review comment by @konstin on `CONTRIBUTING.md`:39 on 2023-07-13 10:35_

smooth

---

_Merged by @konstin on 2023-07-13 10:42_

---

_Closed by @konstin on 2023-07-13 10:42_

---

_Branch deleted on 2023-07-13 10:42_

---
