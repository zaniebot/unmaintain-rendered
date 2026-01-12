```yaml
number: 6781
title: "Update `mutable-argument-default` (`B006`) to use `extend-immutable-calls` when determining if annotations are immutable"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: rule/b006-extend
created_at: 2023-08-22T16:45:40Z
updated_at: 2023-08-23T16:12:17Z
url: https://github.com/astral-sh/ruff/pull/6781
synced_at: 2026-01-12T02:45:38Z
```

# Update `mutable-argument-default` (`B006`) to use `extend-immutable-calls` when determining if annotations are immutable

---

_Pull request opened by @zanieb on 2023-08-22 16:45_

Part of https://github.com/astral-sh/ruff/issues/3762

---

_Renamed from "Update `B006` to respect `extend_immutable_calls` when determining if annotations are immutable" to "Update `mutable-argument-default` (`B006`) to use `extend-immutable-calls` when determining if annotations are immutable" by @zanieb on 2023-08-22 16:48_

---

_@zanieb reviewed on 2023-08-22 16:51_

---

_Review comment by @zanieb on `crates/ruff/src/rules/ruff/rules/mutable_class_default.rs`:63 on 2023-08-22 16:51_

These could be updated to use the setting as well in a follow-up

---

_Comment by @github-actions[bot] on 2023-08-22 17:38_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.08      4.2±0.15ms     9.7 MB/sec    1.00      3.9±0.20ms    10.5 MB/sec
formatter/numpy/ctypeslib.py               1.09   883.2±60.78µs    18.9 MB/sec    1.00   810.7±38.44µs    20.5 MB/sec
formatter/numpy/globals.py                 1.04     89.5±8.51µs    33.0 MB/sec    1.00     85.7±3.34µs    34.4 MB/sec
formatter/pydantic/types.py                1.06  1686.7±72.18µs    15.1 MB/sec    1.00  1587.5±69.38µs    16.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.0±0.37ms     3.1 MB/sec    1.01     13.1±0.38ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.10ms     4.9 MB/sec    1.01      3.5±0.13ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   490.0±17.03µs     6.0 MB/sec    1.01   495.1±14.29µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.23ms     3.7 MB/sec    1.00      6.9±0.26ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      7.0±0.19ms     5.8 MB/sec    1.00      6.9±0.23ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1524.6±55.47µs    10.9 MB/sec    1.00  1495.5±55.97µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    186.5±8.56µs    15.8 MB/sec    1.02    190.2±9.34µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.2±0.08ms     8.0 MB/sec    1.00      3.1±0.10ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.12      4.1±0.05ms     9.9 MB/sec    1.00      3.7±0.05ms    11.1 MB/sec
formatter/numpy/ctypeslib.py               1.08   796.9±11.03µs    20.9 MB/sec    1.00   738.2±13.77µs    22.6 MB/sec
formatter/numpy/globals.py                 1.07     81.4±2.14µs    36.2 MB/sec    1.00     76.3±3.57µs    38.7 MB/sec
formatter/pydantic/types.py                1.09  1651.1±35.15µs    15.4 MB/sec    1.00  1508.6±18.52µs    16.9 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.27ms     3.2 MB/sec    1.02     12.8±0.28ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.06ms     4.7 MB/sec    1.01      3.6±0.12ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    366.8±7.66µs     8.0 MB/sec    1.02    375.2±7.85µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.15ms     3.9 MB/sec    1.00      6.6±0.14ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.11ms     5.8 MB/sec    1.01      7.1±0.11ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1480.0±21.74µs    11.3 MB/sec    1.01  1495.7±17.43µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    150.0±3.13µs    19.7 MB/sec    1.00    150.7±3.72µs    19.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.04ms     8.0 MB/sec    1.00      3.2±0.05ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @zanieb on 2023-08-22 19:30_

---

_Comment by @zanieb on 2023-08-22 19:30_

@charliermarsh is this what you meant?

---

_@charliermarsh reviewed on 2023-08-22 20:57_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/mutable_dataclass_default.rs`:79 on 2023-08-22 20:57_

Can you just pass in `&[]`?

---

_@charliermarsh reviewed on 2023-08-22 20:58_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:58 on 2023-08-22 20:58_

Should we mention the effect of this in the rule description above?

---

_@charliermarsh approved on 2023-08-22 20:58_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:58 on 2023-08-22 21:22_

I didn't really see precedence for this elsewhere but I didn't look very hard! I'm happy to mention it

---

_@zanieb reviewed on 2023-08-22 21:22_

---

_@zanieb reviewed on 2023-08-22 21:22_

---

_Review comment by @zanieb on `crates/ruff/src/rules/ruff/rules/mutable_dataclass_default.rs`:79 on 2023-08-22 21:22_

👀 

---

_@charliermarsh reviewed on 2023-08-22 22:38_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:58 on 2023-08-22 22:38_

We do it in some places, grep for "[`flake8-", but totally up to you.

---

_Merged by @zanieb on 2023-08-23 15:44_

---

_Closed by @zanieb on 2023-08-23 15:44_

---

_Branch deleted on 2023-08-23 15:44_

---
