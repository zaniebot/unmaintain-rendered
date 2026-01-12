```yaml
number: 5718
title: Run release testing on PR, not push
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/push
created_at: 2023-07-12T17:56:50Z
updated_at: 2023-07-12T18:25:39Z
url: https://github.com/astral-sh/ruff/pull/5718
synced_at: 2026-01-12T03:36:55Z
```

# Run release testing on PR, not push

---

_Pull request opened by @charliermarsh on 2023-07-12 17:56_

## Summary

This job runs whenever I put up a PR to bump the version, which is really useful. But then it also runs again when I merge, and then _that_ job tends to get cancelled immediately, because I run the _actual_ release job, which triggers the cancel-concurrent-runs flow. (See, e.g., https://github.com/astral-sh/ruff/actions/runs/5534191373.)

I think it makes sense to run these on PR (when editing `pyproject.toml` and friends), but not again on merge.


---

_Review requested from @konstin by @charliermarsh on 2023-07-12 17:56_

---

_Comment by @github-actions[bot] on 2023-07-12 18:08_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.07ms     4.4 MB/sec    1.04      9.7±0.32ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.04      2.3±0.08ms     7.2 MB/sec    1.00      2.2±0.01ms     7.5 MB/sec
formatter/numpy/globals.py                 1.01   267.2±16.48µs    11.0 MB/sec    1.00   265.5±35.88µs    11.1 MB/sec
formatter/pydantic/types.py                1.01      4.8±0.03ms     5.3 MB/sec    1.00      4.8±0.01ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.02     16.3±0.34ms     2.5 MB/sec    1.00     16.0±0.06ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.02ms     4.1 MB/sec    1.01      4.1±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    512.1±1.69µs     5.8 MB/sec    1.00    513.0±1.20µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.03ms     3.6 MB/sec    1.01      7.2±0.04ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.02      8.1±0.27ms     5.0 MB/sec    1.00      8.0±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1719.3±17.50µs     9.7 MB/sec    1.01   1742.2±3.76µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.0±0.97µs    15.2 MB/sec    1.06    204.9±7.69µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.1 MB/sec    1.00      3.6±0.02ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.10ms     4.3 MB/sec    1.06      9.9±0.35ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.03ms     7.8 MB/sec    1.06      2.3±0.11ms     7.3 MB/sec
formatter/numpy/globals.py                 1.00    240.9±9.24µs    12.2 MB/sec    1.07   257.2±19.29µs    11.5 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.06ms     5.5 MB/sec    1.06      5.0±0.25ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.18ms     2.6 MB/sec    1.00     15.7±0.14ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.06ms     4.1 MB/sec    1.00      4.1±0.06ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    498.2±8.35µs     5.9 MB/sec    1.01   502.7±19.12µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.07ms     3.6 MB/sec    1.00      7.0±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.05ms     5.1 MB/sec    1.05      8.4±0.27ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1698.7±23.82µs     9.8 MB/sec    1.07  1818.3±76.97µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.5±4.72µs    14.7 MB/sec    1.04   207.7±12.45µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.1 MB/sec    1.07      3.8±0.11ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-07-12 18:15_

---

_Merged by @charliermarsh on 2023-07-12 18:22_

---

_Closed by @charliermarsh on 2023-07-12 18:22_

---

_Branch deleted on 2023-07-12 18:22_

---
