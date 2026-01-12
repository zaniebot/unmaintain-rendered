```yaml
number: 7613
title: "Update return type for `PT022` autofix"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/pt022-autofix
created_at: 2023-09-23T05:21:36Z
updated_at: 2023-09-24T06:46:08Z
url: https://github.com/astral-sh/ruff/pull/7613
synced_at: 2026-01-12T02:39:10Z
```

# Update return type for `PT022` autofix

---

_Pull request opened by @dhruvmanila on 2023-09-23 05:21_

## Summary

This PR fixes the autofix behavior for `PT022` to create an additional edit for the return type if it's present. The edit will update the return type from `Generator[T, ...]` to `T`. As per the [official documentation](https://docs.python.org/3/library/typing.html?highlight=typing%20generator#typing.Generator), the first position is the yield type, so we can ignore other positions.

```python
typing.Generator[YieldType, SendType, ReturnType]
```

## Test Plan

Add new test cases, `cargo test` and review the snapshots.

fixes: #7610 


---

_Label `bug` added by @dhruvmanila on 2023-09-23 05:21_

---

_Review requested from @charliermarsh by @dhruvmanila on 2023-09-23 05:22_

---

_Comment by @github-actions[bot] on 2023-09-23 05:38_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@charliermarsh reviewed on 2023-09-23 16:39_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:837 on 2023-09-23 16:39_

Since we're in a method that returns an `Option`, this could probably be `slice.as_tuple_expr()?`, and the above could probably be `returns.as_subscript_expr()?`.

---

_@charliermarsh reviewed on 2023-09-23 16:39_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:844 on 2023-09-23 16:39_

Unrelated, but I feel like these should be suggested?

---

_@charliermarsh approved on 2023-09-23 16:39_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:837 on 2023-09-24 06:27_

Right, thanks!

---

_@dhruvmanila reviewed on 2023-09-24 06:27_

---

_@dhruvmanila reviewed on 2023-09-24 06:31_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:844 on 2023-09-24 06:31_

I'm not sure, it was automatic before so I've kept it that way. Plus I think they're safe fixes. That said, we can obviously update it in the future.

---

_Merged by @dhruvmanila on 2023-09-24 06:39_

---

_Closed by @dhruvmanila on 2023-09-24 06:39_

---

_Branch deleted on 2023-09-24 06:39_

---

_Comment by @codspeed-hq[bot] on 2023-09-24 06:42_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/pt022-autofix)

### Merging #7613 will **improve performances by 2.35%**

<sub>Comparing <code>dhruv/pt022-autofix</code> (46f33b4) with <code>main</code> (604cf52)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/pt022-autofix` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[large/dataset.py]` | 94.1 ms | 92 ms | +2.35% |


---
