```yaml
number: 4957
title: "Add Pylint rule `comparison-with-itself` (`R0124`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
assignees: []
merged: true
base: main
head: comparison-with-itself
created_at: 2023-06-08T11:21:03Z
updated_at: 2023-07-10T09:55:29Z
url: https://github.com/astral-sh/ruff/pull/4957
synced_at: 2026-01-12T03:36:54Z
```

# Add Pylint rule `comparison-with-itself` (`R0124`)

---

_Pull request opened by @tjkuson on 2023-06-08 11:21_

## Summary

Implements Pylint rule `comparison-with-itself` (`R0124`) as `comparison-with-itself` (`PLR0124`). Related to #970. Includes documentation.

Moves comparison logic to `rules::pylint::helpers` to facilitate reuse.

## Test Plan

`cargo test`

---

_Marked ready for review by @tjkuson on 2023-06-08 11:27_

---

_Comment by @github-actions[bot] on 2023-06-08 11:30_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+3, -0, 0 error(s))

<details><summary>airflow (+3, -0)</summary>
<p>

```diff
+ tests/models/test_dag.py:1594:16: PLR0124 Name compared with itself, consider replacing `dag == dag`
+ tests/utils/test_operator_resources.py:27:16: PLR0124 Name compared with itself, consider replacing `r == r`
+ tests/utils/test_sqlalchemy.py:330:20: PLR0124 Name compared with itself, consider replacing `a == a`
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLR0124 | 3 | 3 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.13      6.8±0.01ms     5.9 MB/sec    1.00      6.1±0.03ms     6.7 MB/sec
formatter/numpy/ctypeslib.py               1.01   1200.1±3.55µs    13.9 MB/sec    1.00   1182.6±3.29µs    14.1 MB/sec
formatter/numpy/globals.py                 1.00    134.1±0.36µs    22.0 MB/sec    1.00    133.5±0.23µs    22.1 MB/sec
formatter/pydantic/types.py                1.05      2.8±0.01ms     9.3 MB/sec    1.00      2.6±0.00ms     9.7 MB/sec
linter/all-rules/large/dataset.py          1.00     14.8±0.07ms     2.8 MB/sec    1.00     14.8±0.02ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.04ms     4.6 MB/sec    1.00      3.6±0.00ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    367.9±1.21µs     8.0 MB/sec    1.00    366.4±1.36µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.01ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.4±0.01ms     5.5 MB/sec    1.00      7.3±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1537.3±4.04µs    10.8 MB/sec    1.00   1528.6±2.38µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.0±0.28µs    17.9 MB/sec    1.00    164.2±0.51µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.00ms     7.8 MB/sec    1.00      3.3±0.00ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.16      8.0±0.15ms     5.1 MB/sec    1.00      6.9±0.06ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.09  1424.8±55.23µs    11.7 MB/sec    1.00  1311.5±10.34µs    12.7 MB/sec
formatter/numpy/globals.py                 1.06    156.7±5.41µs    18.8 MB/sec    1.00    148.4±2.37µs    19.9 MB/sec
formatter/pydantic/types.py                1.08      3.2±0.06ms     7.9 MB/sec    1.00      3.0±0.04ms     8.5 MB/sec
linter/all-rules/large/dataset.py          1.00     17.1±0.35ms     2.4 MB/sec    1.02     17.5±0.34ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.02ms     3.9 MB/sec    1.07      4.6±0.09ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    438.7±6.68µs     6.7 MB/sec    1.02   446.8±30.16µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.14ms     3.5 MB/sec    1.04      7.6±0.14ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.14ms     4.8 MB/sec    1.02      8.8±0.13ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1787.9±36.76µs     9.3 MB/sec    1.02  1815.3±30.19µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.9±3.29µs    15.6 MB/sec    1.01    190.6±1.33µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.05ms     6.6 MB/sec    1.03      4.0±0.05ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/helpers.rs`:96 on 2023-06-08 11:39_

Do you need the `ViolationsCmpop` only to implement `Display`? An alternative design that uses fewer code is to wrap the `Cmpop` instead
```suggestion
pub(crate) struct ViolationsCmpop(Cmpop);

impl fmt::Display for ViolationsCmpop {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        let representation = match self.0 {
            Self::Eq => "==",
            Self::NotEq => "!=",
            Self::Lt => "<",
            Self::LtE => "<=",
            Self::Gt => ">",
            Self::GtE => ">=",
            Self::Is => "is",
            Self::IsNot => "is not",
            Self::In => "in",
            Self::NotIn => "not in",
        };
        write!(f, "{representation}")
    }
}
```

---

_@MichaReiser reviewed on 2023-06-08 11:40_

---

_@tjkuson reviewed on 2023-06-08 12:45_

---

_Review comment by @tjkuson on `crates/ruff/src/rules/pylint/helpers.rs`:96 on 2023-06-08 12:45_

Wouldn't we still need to implement `From<&Cmpop>` etc. for the `comparison-of-constants` and `comparison-with-itself` rules which are passed `&[Cmpop]`? The CI complained about conversions not being implemented.

---

_Review requested from @charliermarsh by @charliermarsh on 2023-06-08 19:58_

---

_Label `rule` added by @charliermarsh on 2023-06-09 00:48_

---

_Merged by @charliermarsh on 2023-06-09 00:57_

---

_Closed by @charliermarsh on 2023-06-09 00:57_

---

_Branch deleted on 2023-07-10 09:55_

---
