```yaml
number: 4928
title: Use taiki-e/install-action to install cargo fuzz
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: cargo_fuzz_install
created_at: 2023-06-07T13:58:12Z
updated_at: 2023-06-07T16:18:15Z
url: https://github.com/astral-sh/ruff/pull/4928
synced_at: 2026-01-12T03:43:29Z
```

# Use taiki-e/install-action to install cargo fuzz

---

_Pull request opened by @konstin on 2023-06-07 13:58_

The cargo fuzz ci run seems to sometimes fail for unclear reasons (https://github.com/charliermarsh/ruff/actions/runs/5200348677/jobs/9379742606?pr=4900). I hope that this might fix it. I'll push more commits to this PR to check the caching behaviour.

---

_@MichaReiser approved on 2023-06-07 14:06_

---

_Comment by @github-actions[bot] on 2023-06-07 14:09_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04      6.3±0.01ms     6.5 MB/sec    1.00      6.0±0.01ms     6.7 MB/sec
formatter/numpy/ctypeslib.py               1.04   1223.1±1.25µs    13.6 MB/sec    1.00   1177.3±3.51µs    14.1 MB/sec
formatter/numpy/globals.py                 1.03    136.0±0.50µs    21.7 MB/sec    1.00    132.0±0.72µs    22.3 MB/sec
formatter/pydantic/types.py                1.03      2.7±0.01ms     9.5 MB/sec    1.00      2.6±0.02ms     9.8 MB/sec
linter/all-rules/large/dataset.py          1.00     15.0±0.06ms     2.7 MB/sec    1.00     15.1±0.04ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    366.5±0.77µs     8.1 MB/sec    1.00    366.8±1.86µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.01ms     4.1 MB/sec    1.00      6.2±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.4±0.01ms     5.5 MB/sec    1.00      7.4±0.02ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1540.5±4.17µs    10.8 MB/sec    1.00   1531.3±5.32µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.6±0.28µs    17.8 MB/sec    1.00    164.9±0.28µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.00ms     7.7 MB/sec    1.00      3.3±0.00ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03      7.2±0.04ms     5.7 MB/sec    1.00      7.0±0.04ms     5.8 MB/sec
formatter/numpy/ctypeslib.py               1.02   1367.0±8.98µs    12.2 MB/sec    1.00  1338.8±11.93µs    12.4 MB/sec
formatter/numpy/globals.py                 1.03    153.6±4.08µs    19.2 MB/sec    1.00    149.8±3.50µs    19.7 MB/sec
formatter/pydantic/types.py                1.02      3.1±0.03ms     8.3 MB/sec    1.00      3.0±0.02ms     8.5 MB/sec
linter/all-rules/large/dataset.py          1.00     16.8±0.11ms     2.4 MB/sec    1.02     17.1±0.12ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.02ms     3.9 MB/sec    1.01      4.3±0.03ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    433.0±6.20µs     6.8 MB/sec    1.00    435.0±5.23µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.05ms     3.6 MB/sec    1.02      7.3±0.09ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.03ms     4.9 MB/sec    1.01      8.4±0.04ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1743.2±13.79µs     9.6 MB/sec    1.01  1753.5±12.11µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    187.6±1.66µs    15.7 MB/sec    1.00    187.3±1.68µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.02ms     6.7 MB/sec    1.00      3.8±0.03ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @konstin on 2023-06-07 15:01_

The cache action didn't actually cache the cargo build previously since it cached the wrong target directory (fuzz has its own)

---

_Comment by @konstin on 2023-06-07 15:16_

@addisoncrump do you know why https://github.com/charliermarsh/ruff/actions/runs/5201694282/jobs/9382234535?pr=4928 is failing? I unfortunately don't understand the submodule structure, i can only see in the github ui that https://github.com/eurecom-s3/qsym/commit/d17a39d40dc3ea1d17262dd52607f8a6527dde10 says this commit doesn't belong to any branch

---

_Comment by @addisoncrump on 2023-06-07 15:18_

Yuck. Okay, just remove the `libafl` items for now, I'll just do this locally. I'm afraid this is a case of "works on my machine" (I've been using it all day) and it's not worthwhile to diagnose when it doesn't affect downstream users.

---

_Comment by @addisoncrump on 2023-06-07 15:30_

If preferred I can make a PR to this PR for reverting the libafl dependencies. They are not necessary for the fuzzer to operate correctly, it simply specifies an alternative fuzzer engine to use. It will likely break in the future since it's a git dependency anyways :sweat_smile: 

---

_Comment by @konstin on 2023-06-07 15:47_

i'll merge this since it unblocks other PRs, for libafl it's @MichaReiser's call since he has the context from #4822.

---

_Merged by @konstin on 2023-06-07 15:57_

---

_Closed by @konstin on 2023-06-07 15:57_

---

_Branch deleted on 2023-06-07 15:57_

---
