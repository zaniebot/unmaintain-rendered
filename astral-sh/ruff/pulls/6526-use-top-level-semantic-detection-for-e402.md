```yaml
number: 6526
title: Use top-level semantic detection for E402
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/E402
created_at: 2023-08-12T18:41:10Z
updated_at: 2023-08-12T19:12:34Z
url: https://github.com/astral-sh/ruff/pull/6526
synced_at: 2026-01-12T15:55:21Z
```

# Use top-level semantic detection for E402

---

_@charliermarsh_

## Summary

Noticed in https://github.com/astral-sh/ruff/pull/6378. Given `import h; import i`, we don't consider `import i` to be a "top-level" import for E402 purposes, which is wrong. Similarly, we _do_ consider `import k` to be a "top-level" import in:

```python
if __name__ == "__main__":
    import j; \
import k
```

Using the semantic detection, rather than relying on newline position, fixes both cases.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2023-08-12 18:41_

---

_Merged by @charliermarsh on 2023-08-12 18:52_

---

_Closed by @charliermarsh on 2023-08-12 18:52_

---

_Branch deleted on 2023-08-12 18:52_

---

_Comment by @github-actions[bot] on 2023-08-12 18:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.03ms     4.9 MB/sec    1.00      8.3±0.07ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.02  1684.8±48.03µs     9.9 MB/sec    1.00  1655.2±34.05µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00    190.0±7.43µs    15.5 MB/sec    1.00    190.0±7.03µs    15.5 MB/sec
formatter/pydantic/types.py                1.02      3.5±0.14ms     7.2 MB/sec    1.00      3.4±0.04ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.02     10.7±0.04ms     3.8 MB/sec    1.00     10.5±0.05ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.8±0.01ms     5.9 MB/sec    1.00      2.8±0.01ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    398.9±4.24µs     7.4 MB/sec    1.00    398.0±3.60µs     7.4 MB/sec
linter/all-rules/pydantic/types.py         1.02      5.6±0.10ms     4.6 MB/sec    1.00      5.5±0.05ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.04      5.7±0.03ms     7.2 MB/sec    1.00      5.5±0.03ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1231.5±4.80µs    13.5 MB/sec    1.00   1203.4±8.49µs    13.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    143.2±2.43µs    20.6 MB/sec    1.00    142.2±1.37µs    20.7 MB/sec
linter/default-rules/pydantic/types.py     1.03      2.5±0.01ms    10.0 MB/sec    1.00      2.5±0.03ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.1±0.12ms     4.0 MB/sec    1.01     10.2±0.13ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1942.6±24.72µs     8.6 MB/sec    1.01  1961.2±44.77µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    217.1±4.43µs    13.6 MB/sec    1.01    218.6±8.45µs    13.5 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.05ms     6.1 MB/sec    1.01      4.2±0.08ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.13ms     3.2 MB/sec    1.02     12.9±0.13ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.03ms     4.9 MB/sec    1.03      3.5±0.04ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    427.4±5.52µs     6.9 MB/sec    1.04    443.0±7.27µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.11ms     3.9 MB/sec    1.02      6.6±0.07ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.09ms     5.9 MB/sec    1.01      6.9±0.05ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1439.9±16.99µs    11.6 MB/sec    1.03  1483.6±28.29µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.0±2.80µs    17.4 MB/sec    1.04    176.0±2.67µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.05ms     8.3 MB/sec    1.02      3.1±0.06ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
