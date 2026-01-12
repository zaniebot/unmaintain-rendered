```yaml
number: 4929
title: "[`ruff`] Add a rule for static keys in dict comprehensions "
type: pull_request
state: merged
author: rodjunger
labels:
  - rule
assignees: []
merged: true
base: main
head: static-key
created_at: 2023-06-07T14:23:52Z
updated_at: 2023-06-09T02:26:25Z
url: https://github.com/astral-sh/ruff/pull/4929
synced_at: 2026-01-12T03:43:29Z
```

# [`ruff`] Add a rule for static keys in dict comprehensions 

---

_Pull request opened by @rodjunger on 2023-06-07 14:23_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This rule checks for static keys in dict comprehensions, fixes https://github.com/charliermarsh/ruff/issues/4857
I've named it RUF011

## Test Plan

<!-- How was it tested? -->

A fixture was added with tests for good and bad cases, including tuples which are special cases (they're not detected by is_constant_expr())

---

_Comment by @github-actions[bot] on 2023-06-07 14:55_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+1, -0, 0 error(s))

<details><summary>airflow (+1, -0)</summary>
<p>

```diff
+ tests/system/providers/amazon/aws/example_sagemaker.py:435:20: RUF011 Dictionary comprehension uses static key: `"imageDigest"`
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| RUF011 | 1 | 1 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05      6.7±0.01ms     6.1 MB/sec    1.00      6.4±0.03ms     6.4 MB/sec
formatter/numpy/ctypeslib.py               1.04   1176.0±2.81µs    14.2 MB/sec    1.00   1131.0±3.12µs    14.7 MB/sec
formatter/numpy/globals.py                 1.03    130.9±0.98µs    22.5 MB/sec    1.00    126.6±0.15µs    23.3 MB/sec
formatter/pydantic/types.py                1.04      2.7±0.01ms     9.5 MB/sec    1.00      2.6±0.01ms     9.9 MB/sec
linter/all-rules/large/dataset.py          1.00     14.0±0.03ms     2.9 MB/sec    1.01     14.2±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    425.0±0.88µs     6.9 MB/sec    1.00    424.5±0.35µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.00      5.9±0.04ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.9±0.03ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1486.4±4.16µs    11.2 MB/sec    1.00   1477.8±5.67µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.3±0.64µs    17.8 MB/sec    1.00    164.5±0.26µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      6.6±0.03ms     6.2 MB/sec    1.00      6.5±0.02ms     6.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1120.9±5.96µs    14.9 MB/sec    1.00   1122.2±8.87µs    14.8 MB/sec
formatter/numpy/globals.py                 1.01    120.5±1.01µs    24.5 MB/sec    1.00    119.4±1.99µs    24.7 MB/sec
formatter/pydantic/types.py                1.01      2.6±0.02ms     9.7 MB/sec    1.00      2.6±0.02ms     9.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.03ms     3.1 MB/sec    1.00     13.3±0.06ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    338.7±3.18µs     8.7 MB/sec    1.01    342.1±7.28µs     8.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.6±0.02ms     4.5 MB/sec    1.00      5.6±0.02ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.01      6.9±0.04ms     5.9 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1382.8±7.42µs    12.0 MB/sec    1.00   1375.3±5.78µs    12.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    149.1±1.17µs    19.8 MB/sec    1.00    148.5±1.16µs    19.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/ruff/mod.rs`:208 on 2023-06-07 18:37_

I think we can add this as a `#[test_case]` on the `rules` function above:
```rust
#[test_case(Rule::StaticKeyDictComprehension, Path::new("RUF011.py"))]
```

---

_@dhruvmanila reviewed on 2023-06-07 18:37_

---

_@rodjunger reviewed on 2023-06-07 19:31_

---

_Review comment by @rodjunger on `crates/ruff/src/rules/ruff/mod.rs`:208 on 2023-06-07 19:31_

Great catch, seems like the test for ruff_pairwise_over_zipped could use this treatment too, but that's out of scope for this PR

---

_Review requested from @charliermarsh by @charliermarsh on 2023-06-08 19:58_

---

_Label `rule` added by @charliermarsh on 2023-06-09 01:54_

---

_Merged by @charliermarsh on 2023-06-09 02:06_

---

_Closed by @charliermarsh on 2023-06-09 02:06_

---
