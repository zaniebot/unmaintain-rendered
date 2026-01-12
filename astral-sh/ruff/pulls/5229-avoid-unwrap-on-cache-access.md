```yaml
number: 5229
title: "Avoid `.unwrap()` on cache access"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/cache
created_at: 2023-06-20T22:51:04Z
updated_at: 2023-06-21T00:56:51Z
url: https://github.com/astral-sh/ruff/pull/5229
synced_at: 2026-01-12T15:55:18Z
```

# Avoid `.unwrap()` on cache access

---

_@charliermarsh_

## Summary

I haven't been able to determine why / when this is happening, but in some cases, users are reporting that this `unwrap()` is causing a panic. It's fine to just return `None` here and fallback to "No cache", certainly better than panicking (while we figure out the edge case).

Closes #5225.

Closes #5228.


---

_Renamed from "Avoid `.unwrap()`  on cache access" to "Avoid `.unwrap()` on cache access" by @charliermarsh on 2023-06-20 22:51_

---

_Review requested from @Thomasdezeeuw by @charliermarsh on 2023-06-20 22:57_

---

_Merged by @charliermarsh on 2023-06-20 23:01_

---

_Closed by @charliermarsh on 2023-06-20 23:01_

---

_Branch deleted on 2023-06-20 23:01_

---

_Comment by @tlambert03 on 2023-06-20 23:07_

just here to say you're amazing :)

was coming to see what I could learn about this and it's already closed!

---

_Comment by @charliermarsh on 2023-06-20 23:09_

Haha, thank you, sorry for the churn. If you have a publicly available repro, it'd actually be helpful for me! This will fix the crash, but I can't quite understand why it was happening in the first place.

---

_Comment by @tlambert03 on 2023-06-20 23:10_

I do, but it's a rather convoluted usage (involving subprocesses) :joy:  lemme see if I can make a reproducible example.

---

_Comment by @github-actions[bot] on 2023-06-20 23:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     6.0 MB/sec    1.02      6.9±0.04ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1367.3±4.53µs    12.2 MB/sec    1.01   1383.9±3.44µs    12.0 MB/sec
formatter/numpy/globals.py                 1.00    132.1±0.49µs    22.3 MB/sec    1.02    134.6±0.30µs    21.9 MB/sec
formatter/pydantic/types.py                1.00      2.7±0.01ms     9.3 MB/sec    1.02      2.8±0.03ms     9.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.05ms     3.0 MB/sec    1.00     13.6±0.07ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.5±0.01ms     4.8 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    365.8±1.15µs     8.1 MB/sec    1.00    362.7±1.67µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.0±0.01ms     4.2 MB/sec    1.00      6.0±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.01ms     5.8 MB/sec    1.00      7.0±0.02ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1471.7±2.76µs    11.3 MB/sec    1.00   1460.4±7.81µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    156.2±0.31µs    18.9 MB/sec    1.00    155.3±0.19µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.01ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.03ms     5.1 MB/sec    1.04      8.2±0.04ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1567.7±9.80µs    10.6 MB/sec    1.03  1620.1±10.44µs    10.3 MB/sec
formatter/numpy/globals.py                 1.00    154.1±1.95µs    19.1 MB/sec    1.01    155.1±3.27µs    19.0 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.03ms     7.9 MB/sec    1.03      3.3±0.05ms     7.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.0±0.05ms     2.7 MB/sec    1.01     15.2±0.09ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.02ms     4.1 MB/sec    1.00      4.0±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    407.9±4.56µs     7.2 MB/sec    1.05    428.9±5.95µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.03ms     3.8 MB/sec    1.01      6.8±0.03ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.02ms     5.1 MB/sec    1.01      8.0±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1631.3±11.43µs    10.2 MB/sec    1.02  1669.3±12.20µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.5±1.18µs    17.2 MB/sec    1.05    180.6±1.83µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.1 MB/sec    1.02      3.6±0.01ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @tlambert03 on 2023-06-20 23:28_

alright, this is super far from a minimal example (sorry) ... but I can at least reproduce it with this:

```bash
# somewhere, in a clean environment ...
git clone --depth 1 --branch ruff-panic https://github.com/tlambert03/ome-types.git
cd ome-types
pip install ruff==0.0.273
ruff src/ome_types/model/
```


---

_Comment by @charliermarsh on 2023-06-21 00:56_

That's perfect, thank you!

---
