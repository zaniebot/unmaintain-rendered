```yaml
number: 5730
title: "Formatter: Better f-string dummy"
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: Formatter_Better_f-string_dummy
created_at: 2023-07-13T09:18:00Z
updated_at: 2023-07-13T09:44:33Z
url: https://github.com/astral-sh/ruff/pull/5730
synced_at: 2026-01-12T03:36:55Z
```

# Formatter: Better f-string dummy

---

_Pull request opened by @konstin on 2023-07-13 09:18_

## Summary

The previous dummy was causing instabilities since it turned a string into a variable.

E.g.
```python
            script_header_dict[
                "slurm_partition_line"
            ] = f"#SBATCH --partition {resources.queue_name}"
```
has an instability as
```python
-            script_header_dict["slurm_partition_line"] = (
-                NOT_YET_IMPLEMENTED_ExprJoinedStr
-            )
+            script_header_dict[
+                "slurm_partition_line"
+            ] = NOT_YET_IMPLEMENTED_ExprJoinedStr
```

## Test Plan

The instability is gone, otherwise it's still a dummy


---

_Label `formatter` added by @konstin on 2023-07-13 09:18_

---

_Merged by @konstin on 2023-07-13 09:27_

---

_Closed by @konstin on 2023-07-13 09:27_

---

_Branch deleted on 2023-07-13 09:27_

---

_Comment by @github-actions[bot] on 2023-07-13 09:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      9.5±0.30ms     4.3 MB/sec    1.00      9.3±0.34ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.05      2.2±0.14ms     7.4 MB/sec    1.00      2.1±0.08ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00   259.5±11.99µs    11.4 MB/sec    1.00   258.2±22.87µs    11.4 MB/sec
formatter/pydantic/types.py                1.02      4.8±0.20ms     5.3 MB/sec    1.00      4.7±0.26ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     16.6±0.64ms     2.5 MB/sec    1.02     17.0±0.72ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.15ms     4.0 MB/sec    1.01      4.2±0.20ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.03   537.7±25.89µs     5.5 MB/sec    1.00   520.6±23.42µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.24ms     3.5 MB/sec    1.03      7.6±0.27ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.25ms     4.9 MB/sec    1.01      8.3±0.39ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1775.5±67.60µs     9.4 MB/sec    1.01  1788.6±67.60µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.05   213.8±10.40µs    13.8 MB/sec    1.00    203.9±8.67µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.10ms     7.0 MB/sec    1.03      3.7±0.19ms     6.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.07     10.3±0.14ms     4.0 MB/sec    1.00      9.6±0.10ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.05      2.2±0.04ms     7.4 MB/sec    1.00      2.1±0.03ms     7.8 MB/sec
formatter/numpy/globals.py                 1.01    238.8±6.35µs    12.4 MB/sec    1.00   236.9±10.39µs    12.5 MB/sec
formatter/pydantic/types.py                1.06      5.1±0.06ms     5.0 MB/sec    1.00      4.8±0.04ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     16.1±0.28ms     2.5 MB/sec    1.02     16.4±0.31ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.04ms     4.0 MB/sec    1.02      4.2±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    443.0±8.64µs     6.7 MB/sec    1.00    441.9±8.04µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.18ms     3.5 MB/sec    1.01      7.4±0.13ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.12ms     4.9 MB/sec    1.00      8.4±0.10ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1700.7±25.80µs     9.8 MB/sec    1.01  1709.9±17.83µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    183.4±2.32µs    16.1 MB/sec    1.00    184.3±3.68µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.01      3.7±0.04ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
