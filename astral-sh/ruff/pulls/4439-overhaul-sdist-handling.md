```yaml
number: 4439
title: Overhaul sdist handling
type: pull_request
state: merged
author: konstin
labels:
  - release
assignees: []
merged: true
base: main
head: reduce_sdist_size
created_at: 2023-05-15T13:23:45Z
updated_at: 2023-05-18T17:02:24Z
url: https://github.com/astral-sh/ruff/pull/4439
synced_at: 2026-01-12T03:50:03Z
```

# Overhaul sdist handling

---

_Pull request opened by @konstin on 2023-05-15 13:23_

This does three tings:

 * Fix out sdist by updating maturin (https://github.com/PyO3/maturin/issues/1609)
 * Add a test on release that ensures that the sdist keeps working
 * reduce the size of the sdist by excluding test fixtures:
   `maturin sdist && du -sh target/wheels/ruff-0.0.267.tar.gz`:
   Before: 1,1M
   After: 668K

Closes #4436.

---

_Renamed from "Reduce sdist size" to "Reduce sdist size and test sdist before release" by @konstin on 2023-05-15 13:30_

---

_Comment by @konstin on 2023-05-15 13:32_

I added a source distribution test in the release step. I think building the sdist on every CI run is overblown, but this will stop the release from going to pypi if the sdist is broken. We should do a beta release to test that the workflow actually passes (i could test in the PR test but it doesn't help too much if it doesn't upload binaries)

---

_Comment by @github-actions[bot] on 2023-05-15 13:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.9±0.55ms     2.7 MB/sec    1.10     16.4±0.34ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.15ms     4.7 MB/sec    1.08      3.8±0.16ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   449.1±22.64µs     6.6 MB/sec    1.00   447.2±17.74µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.22ms     4.1 MB/sec    1.04      6.4±0.24ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.25ms     5.6 MB/sec    1.00      7.2±0.19ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1547.9±47.87µs    10.8 MB/sec    1.00  1554.2±62.60µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.04    184.0±5.25µs    16.0 MB/sec    1.00    176.6±7.01µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.09ms     7.8 MB/sec    1.00      3.3±0.10ms     7.8 MB/sec
parser/large/dataset.py                    1.00      5.8±0.17ms     7.1 MB/sec    1.07      6.2±0.09ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.00  1097.1±59.78µs    15.2 MB/sec    1.11  1221.3±13.73µs    13.6 MB/sec
parser/numpy/globals.py                    1.00    116.2±4.27µs    25.4 MB/sec    1.07    124.6±1.42µs    23.7 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.11ms    10.9 MB/sec    1.13      2.6±0.03ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     23.3±1.29ms  1786.8 KB/sec    1.00     22.7±0.87ms  1832.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.07      6.0±0.34ms     2.8 MB/sec    1.00      5.6±0.35ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   671.7±32.44µs     4.4 MB/sec    1.02   687.2±47.27µs     4.3 MB/sec
linter/all-rules/pydantic/types.py         1.03     10.0±0.43ms     2.5 MB/sec    1.00      9.8±0.41ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.00     11.3±0.42ms     3.6 MB/sec    1.12     12.6±1.01ms     3.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.4±0.13ms     6.9 MB/sec    1.04      2.5±0.11ms     6.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   266.7±14.77µs    11.1 MB/sec    1.09   291.1±18.73µs    10.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.0±0.22ms     5.1 MB/sec    1.07      5.4±0.26ms     4.8 MB/sec
parser/large/dataset.py                    1.00      9.1±0.40ms     4.5 MB/sec    1.03      9.3±0.34ms     4.4 MB/sec
parser/numpy/ctypeslib.py                  1.00  1744.5±80.92µs     9.5 MB/sec    1.04  1815.3±76.16µs     9.2 MB/sec
parser/numpy/globals.py                    1.00    177.9±9.02µs    16.6 MB/sec    1.00   177.6±11.05µs    16.6 MB/sec
parser/pydantic/types.py                   1.00      3.8±0.14ms     6.6 MB/sec    1.02      3.9±0.27ms     6.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Reduce sdist size and test sdist before release" to "Overhaul sdist handling" by @konstin on 2023-05-17 20:09_

---

_Marked ready for review by @konstin on 2023-05-17 20:17_

---

_Comment by @konstin on 2023-05-17 20:19_

Manually triggered release run to test the CI changes: https://github.com/charliermarsh/ruff/actions/runs/5008234293

---

_@charliermarsh reviewed on 2023-05-17 20:46_

---

_Review comment by @charliermarsh on `pyproject.toml`:55 on 2023-05-17 20:46_

Nit: remove?

---

_@charliermarsh approved on 2023-05-17 20:46_

---

_Label `release` added by @charliermarsh on 2023-05-17 20:46_

---

_@konstin reviewed on 2023-05-17 21:10_

---

_Review comment by @konstin on `pyproject.toml`:55 on 2023-05-17 21:10_

oops i didn't realize i committed the comments

---

_Comment by @charliermarsh on 2023-05-18 00:51_

@konstin - I linked this PR to #4436 --  it closes that issue, right?

---

_Merged by @konstin on 2023-05-18 17:02_

---

_Closed by @konstin on 2023-05-18 17:02_

---

_Branch deleted on 2023-05-18 17:02_

---
