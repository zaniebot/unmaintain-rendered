```yaml
number: 5233
title: Use package roots rather than package members for cache initialization
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/root
created_at: 2023-06-21T01:07:34Z
updated_at: 2023-06-21T03:01:53Z
url: https://github.com/astral-sh/ruff/pull/5233
synced_at: 2026-01-12T15:55:18Z
```

# Use package roots rather than package members for cache initialization

---

_@charliermarsh_

## Summary

This is a proper fix for the issue patched-over in https://github.com/astral-sh/ruff/pull/5229, thanks to an extremely helpful repro from @tlambert03 in that thread. It looks like we were using the keys of `package_roots` rather than the values to initialize the cache -- but it's a map from package to package root.

## Test Plan

Reverted #5229, then ran through the plan that @tlambert03 included in https://github.com/astral-sh/ruff/pull/5229#issuecomment-1599723226. Verified the panic before but not after this change.


---

_Review requested from @Thomasdezeeuw by @charliermarsh on 2023-06-21 01:07_

---

_Label `bug` added by @charliermarsh on 2023-06-21 01:07_

---

_Comment by @github-actions[bot] on 2023-06-21 01:17_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.4±0.01ms     6.4 MB/sec    1.00      6.3±0.02ms     6.4 MB/sec
formatter/numpy/ctypeslib.py               1.02   1351.4±2.55µs    12.3 MB/sec    1.00   1326.8±2.94µs    12.6 MB/sec
formatter/numpy/globals.py                 1.01    132.9±0.17µs    22.2 MB/sec    1.00    132.0±0.13µs    22.4 MB/sec
formatter/pydantic/types.py                1.02      2.6±0.01ms     9.6 MB/sec    1.00      2.6±0.01ms     9.8 MB/sec
linter/all-rules/large/dataset.py          1.00     13.0±0.05ms     3.1 MB/sec    1.01     13.1±0.07ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.00ms     5.1 MB/sec    1.01      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    426.3±0.56µs     6.9 MB/sec    1.00    426.4±0.57µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.01      5.8±0.03ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.01ms     6.2 MB/sec    1.01      6.6±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1454.8±2.39µs    11.4 MB/sec    1.00   1455.4±4.23µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.1±0.24µs    17.7 MB/sec    1.00    167.8±1.63µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.6±0.06ms     5.4 MB/sec    1.00      7.5±0.09ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.01  1549.4±18.13µs    10.7 MB/sec    1.00  1537.2±21.23µs    10.8 MB/sec
formatter/numpy/globals.py                 1.00    151.4±3.19µs    19.5 MB/sec    1.01    152.6±5.75µs    19.3 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.03ms     8.2 MB/sec    1.00      3.1±0.06ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.04     15.7±0.39ms     2.6 MB/sec    1.00     15.1±0.17ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.07ms     4.1 MB/sec    1.00      4.0±0.06ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    495.3±9.77µs     6.0 MB/sec    1.00    488.6±5.75µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.8±0.10ms     3.8 MB/sec    1.00      6.7±0.07ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.03      8.1±0.30ms     5.0 MB/sec    1.00      7.8±0.05ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1703.9±32.59µs     9.8 MB/sec    1.00  1672.6±19.02µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.2±3.10µs    15.4 MB/sec    1.02    194.4±4.84µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.2 MB/sec    1.00      3.6±0.05ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-21 01:21_

---

_Closed by @charliermarsh on 2023-06-21 01:21_

---

_Branch deleted on 2023-06-21 01:21_

---

_Comment by @charliermarsh on 2023-06-21 01:52_

This reduces the FastAPI cache size down significantly:

```
❯ du -sh .ruff_cache
132K	.ruff_cache
```

However, when I run with `--verbose`, 343 out of 823 files aren't hitting the cache 🤔 

---

_Comment by @charliermarsh on 2023-06-21 02:11_

Ah, I think it's because those files are all in the documentation, and don't belong to a package. So when we iterate over `package_roots`, we don't create an entry for them at all. Instead, we'd need to iterate over all files, infer its root, and use _that_ to initialize the cache.

---

_Comment by @charliermarsh on 2023-06-21 02:12_

(In general this affects a very small number of files.)

---

_@charliermarsh reviewed on 2023-06-21 02:21_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/run.rs`:86 on 2023-06-21 02:21_

Using `.dedup` here is subtly wrong because these may not be ordered, but it's fine, it's just an optimization. Will fix in a follow-up.

---

_Comment by @tlambert03 on 2023-06-21 03:01_

Thanks much! 🙏

---
