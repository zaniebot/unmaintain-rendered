```yaml
number: 4706
title: "Add Pylint rule `C0208` (`use-sequence-for-iteration`) as `PLC0208` (`iteration-over-set`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
assignees: []
merged: true
base: main
head: iterate-over-set
created_at: 2023-05-29T15:33:28Z
updated_at: 2023-07-10T09:55:30Z
url: https://github.com/astral-sh/ruff/pull/4706
synced_at: 2026-01-12T15:55:16Z
```

# Add Pylint rule `C0208` (`use-sequence-for-iteration`) as `PLC0208` (`iteration-over-set`)

---

_@tjkuson_

## Summary

Implement the Pylint [C0208 rule](https://pylint.readthedocs.io/en/latest/user_guide/messages/convention/use-sequence-for-iteration.html) that checks for `set` iterators. Related to https://github.com/charliermarsh/ruff/issues/970. Includes documentation.

Currently, it checks for iterating over in-place `set` but not `set` names. For example, this triggers the rule:

```python
for item in {1, 2, 3}:
    ...
```

But this does not:

```python
items = {1, 2, 3}
for item in items:
    ...
```

I am curious whether this should be the case. On the one hand, using a `set` via a name incurs the same performance impact as using a `set` literal. However, it is unclear if using a named `set` invites the same recommendation to replace it with a sequence (there could be a conversion cost, and a `set` might be more efficient elsewhere). *This is also how Pylint currently implements the rule.*

## Test Plan

Added test fixtures.


---

_Comment by @github-actions[bot] on 2023-05-29 15:57_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+7, -0, 0 error(s))

<details><summary>airflow (+1, -0)</summary>
<p>

```diff
+ airflow/serialization/serialized_objects.py:1071:19: PLC0208 Use a sequence type instead of a `set` when iterating over values
```

</p>
</details>
<details><summary>cibuildwheel (+1, -0)</summary>
<p>

```diff
+ unit_test/option_prepare_test.py:154:42: PLC0208 Use a sequence type instead of a `set` when iterating over values
```

</p>
</details>
<details><summary>zulip (+5, -0)</summary>
<p>

```diff
+ zerver/management/commands/add_users_to_streams.py:28:28: PLC0208 Use a sequence type instead of a `set` when iterating over values
+ zerver/management/commands/create_default_stream_groups.py:43:28: PLC0208 Use a sequence type instead of a `set` when iterating over values
+ zerver/tests/test_auth_backends.py:1591:28: PLC0208 Use a sequence type instead of a `set` when iterating over values
+ zerver/tests/test_auth_backends.py:4602:28: PLC0208 Use a sequence type instead of a `set` when iterating over values
+ zproject/backends.py:870:69: PLC0208 Use a sequence type instead of a `set` when iterating over values
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLC0208 | 7 | 7 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     16.9±0.38ms     2.4 MB/sec    1.00     16.6±0.07ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.03ms     4.1 MB/sec    1.00      4.0±0.02ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    499.3±5.11µs     5.9 MB/sec    1.00    497.5±3.65µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.07ms     3.6 MB/sec    1.00      6.9±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.06ms     5.1 MB/sec    1.03      8.2±0.02ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1780.5±20.24µs     9.4 MB/sec    1.01   1803.7±2.13µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.9±3.63µs    14.6 MB/sec    1.09    220.3±6.76µs    13.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     6.9 MB/sec    1.01      3.7±0.02ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.2±0.02ms     6.6 MB/sec    1.01      6.2±0.02ms     6.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1209.9±4.79µs    13.8 MB/sec    1.01   1218.6±6.19µs    13.7 MB/sec
parser/numpy/globals.py                    1.00    125.4±0.34µs    23.5 MB/sec    1.00    125.9±0.81µs    23.4 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.01ms     9.6 MB/sec    1.01      2.7±0.01ms     9.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     21.8±0.75ms  1912.1 KB/sec    1.00     21.5±0.86ms  1940.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.5±0.28ms     3.0 MB/sec    1.00      5.5±0.31ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   666.9±55.35µs     4.4 MB/sec    1.00   660.4±70.99µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.02      9.4±0.61ms     2.7 MB/sec    1.00      9.2±0.51ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.7±0.44ms     3.8 MB/sec    1.02     10.9±0.65ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.3±0.11ms     7.3 MB/sec    1.00      2.3±0.12ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   264.3±22.87µs    11.2 MB/sec    1.00   264.8±19.98µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.8±0.30ms     5.3 MB/sec    1.00      4.8±0.23ms     5.4 MB/sec
parser/large/dataset.py                    1.00      8.3±0.38ms     4.9 MB/sec    1.02      8.5±0.43ms     4.8 MB/sec
parser/numpy/ctypeslib.py                  1.00  1550.6±88.89µs    10.7 MB/sec    1.04  1619.7±147.38µs    10.3 MB/sec
parser/numpy/globals.py                    1.01   160.0±14.25µs    18.4 MB/sec    1.00   159.1±10.98µs    18.5 MB/sec
parser/pydantic/types.py                   1.00      3.5±0.17ms     7.3 MB/sec    1.01      3.6±0.18ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @tjkuson on 2023-05-31 18:49_

---

_Comment by @tjkuson on 2023-05-31 18:52_

I think the rule is ready for review. To my knowledge, it is at par with Pylint C0208.

---

_Renamed from "Add Pylint rule use-sequence-for-iteration" to "Add Pylint rule C0208 (use-sequence-for-iteration)" by @tjkuson on 2023-05-31 18:53_

---

_Renamed from "Add Pylint rule C0208 (use-sequence-for-iteration)" to "Add Pylint rule `C0208` (`use-sequence-for-iteration`) as `PLC0208` (`iterate-over-set`)" by @tjkuson on 2023-05-31 19:12_

---

_@MichaReiser approved on 2023-06-01 06:24_

---

_Review requested from @charliermarsh by @charliermarsh on 2023-06-01 14:29_

---

_Renamed from "Add Pylint rule `C0208` (`use-sequence-for-iteration`) as `PLC0208` (`iterate-over-set`)" to "Add Pylint rule `C0208` (`use-sequence-for-iteration`) as `PLC0208` (`iteration-over-set`)" by @charliermarsh on 2023-06-01 21:17_

---

_Label `rule` added by @charliermarsh on 2023-06-01 21:17_

---

_Merged by @charliermarsh on 2023-06-01 21:26_

---

_Closed by @charliermarsh on 2023-06-01 21:26_

---

_Comment by @andersk on 2023-06-06 20:16_

Pylint does *not* flag `for i in set(a)`, and I don’t think we should either. The [point of this rule](https://pylint.readthedocs.io/en/latest/user_guide/messages/convention/use-sequence-for-iteration.html) is for efficiency:

- `for i in {1, 2, 3}` → `for i in [1, 2, 3]` is more efficient, but
- `for i in set(a)` → `for i in a` is incorrect (`set` deduplicates), and
- `for i in set(a)` → `for i in list(set(a))` is less efficient (we still iterate over the `set` to build the `list`).

---

_Comment by @charliermarsh on 2023-06-06 20:19_

Ah interesting. By that logic, we'd want to omit set-comprehensions too?

---

_Comment by @andersk on 2023-06-06 20:20_

Yes (and Pylint allows them too).

---

_Comment by @charliermarsh on 2023-06-06 20:23_

Yeah, this makes sense. I'll get this change in before cutting the next release (or feel free to PR if you'd like).

---

_Comment by @tjkuson on 2023-06-06 20:24_

Oops, my bad. I must have misunderstood the Pylint implementation. I have opened a new PR to fix this: #4907.

---

_Branch deleted on 2023-07-10 09:55_

---
