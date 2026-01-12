```yaml
number: 4907
title: "Change `iteration-over-set` to flag set literals only"
type: pull_request
state: merged
author: tjkuson
labels: []
assignees: []
merged: true
base: main
head: fix-set-check
created_at: 2023-06-06T20:44:40Z
updated_at: 2023-07-10T09:55:29Z
url: https://github.com/astral-sh/ruff/pull/4907
synced_at: 2026-01-12T03:36:54Z
```

# Change `iteration-over-set` to flag set literals only

---

_Pull request opened by @tjkuson on 2023-06-06 20:44_

## Summary

Changes the logic of the `iteration-over-set` rule to match Pylint and only flag iterations over set literals.

Addresses comment by @andersk in https://github.com/charliermarsh/ruff/issues/4706#issuecomment-1579386665.

## Test Plan

`cargo test`


---

_@charliermarsh reviewed on 2023-06-06 20:45_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/iteration_over_set.rs`:41 on 2023-06-06 20:45_

Nit: can probably be `expr.is_set_expr()`.

---

_Comment by @charliermarsh on 2023-06-06 20:45_

Thanks!

---

_@tjkuson reviewed on 2023-06-06 20:51_

---

_Review comment by @tjkuson on `crates/ruff/src/rules/pylint/rules/iteration_over_set.rs`:41 on 2023-06-06 20:51_

Fixed in [89c1dc3](https://github.com/charliermarsh/ruff/pull/4907/commits/89c1dc39f7cd47e9b8139bd3afa4574c01c60277)

---

_Marked ready for review by @tjkuson on 2023-06-06 20:57_

---

_Comment by @tjkuson on 2023-06-06 20:58_

This PR is ready for review. It should resolve the issue raised in the comment I linked above!

---

_Merged by @charliermarsh on 2023-06-06 21:06_

---

_Closed by @charliermarsh on 2023-06-06 21:06_

---

_Comment by @github-actions[bot] on 2023-06-06 21:09_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -5, 0 error(s))

<details><summary>airflow (+0, -1)</summary>
<p>

```diff
- airflow/serialization/serialized_objects.py:1071:19: PLC0208 Use a sequence type instead of a `set` when iterating over values
```

</p>
</details>
<details><summary>zulip (+0, -4)</summary>
<p>

```diff
- zerver/management/commands/add_users_to_streams.py:28:28: PLC0208 Use a sequence type instead of a `set` when iterating over values
- zerver/management/commands/create_default_stream_groups.py:43:28: PLC0208 Use a sequence type instead of a `set` when iterating over values
- zerver/tests/test_auth_backends.py:1591:28: PLC0208 Use a sequence type instead of a `set` when iterating over values
- zerver/tests/test_auth_backends.py:4602:28: PLC0208 Use a sequence type instead of a `set` when iterating over values
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLC0208 | 5 | 0 | 5 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.2±0.01ms     6.5 MB/sec    1.00      6.2±0.02ms     6.6 MB/sec
formatter/numpy/ctypeslib.py               1.01  1271.3±34.18µs    13.1 MB/sec    1.00   1257.4±7.80µs    13.2 MB/sec
formatter/numpy/globals.py                 1.00    145.7±0.34µs    20.2 MB/sec    1.00    146.3±0.47µs    20.2 MB/sec
formatter/pydantic/types.py                1.00      2.7±0.00ms     9.4 MB/sec    1.00      2.7±0.01ms     9.4 MB/sec
linter/all-rules/large/dataset.py          1.01     15.1±0.07ms     2.7 MB/sec    1.00     15.0±0.08ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    365.7±3.89µs     8.1 MB/sec    1.00    364.4±1.27µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.02ms     4.1 MB/sec    1.00      6.2±0.09ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.02      7.4±0.02ms     5.5 MB/sec    1.00      7.3±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1544.1±4.16µs    10.8 MB/sec    1.00  1538.8±11.85µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.3±0.20µs    18.0 MB/sec    1.00    164.5±0.88µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.03ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.1±0.03ms     6.7 MB/sec    1.00      6.1±0.03ms     6.7 MB/sec
formatter/numpy/ctypeslib.py               1.00   1183.0±5.95µs    14.1 MB/sec    1.00  1181.5±22.07µs    14.1 MB/sec
formatter/numpy/globals.py                 1.00    131.9±1.24µs    22.4 MB/sec    1.00    132.4±6.64µs    22.3 MB/sec
formatter/pydantic/types.py                1.01      2.6±0.20ms     9.7 MB/sec    1.00      2.6±0.02ms     9.8 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.06ms     3.0 MB/sec    1.00     13.5±0.06ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.02ms     5.0 MB/sec    1.00      3.3±0.02ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    340.5±2.65µs     8.7 MB/sec    1.00    341.6±1.96µs     8.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.7±0.02ms     4.5 MB/sec    1.00      5.7±0.03ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.03ms     5.9 MB/sec    1.00      6.9±0.02ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1397.7±7.74µs    11.9 MB/sec    1.00   1391.5±6.74µs    12.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    149.7±1.05µs    19.7 MB/sec    1.00    149.2±1.27µs    19.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Branch deleted on 2023-07-10 09:55_

---
