```yaml
number: 16678
title: "[`flake8-pytest-style`] Allow for loops with empty bodies (`PT012`, `PT031`)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: micha/pytest-empty-for-loop
created_at: 2025-03-12T14:15:06Z
updated_at: 2025-03-13T07:42:52Z
url: https://github.com/astral-sh/ruff/pull/16678
synced_at: 2026-01-12T15:55:55Z
```

# [`flake8-pytest-style`] Allow for loops with empty bodies (`PT012`, `PT031`)

---

_@MichaReiser_

## Summary

This PR stabilizes the behavior change introduced in https://github.com/astral-sh/ruff/pull/15542 to allow 
for statements with an empty body in `pytest.raises` and `pytest.warns` with statements.

This raised an error before but is now allowed:

```py
with pytest.raises(KeyError, match='unknown'):
    async for _ in gpt.generate(gpt_request):
        pass
```

The same applies to 

```py
with pytest.raises(KeyError, match='unknown'):
    async for _ in gpt.generate(gpt_request):
        ...
```


There have been now new issues or PRs related to PT012 or PT031 since this behavior change was introduced in ruff 0.9.3 (January 23rd). 




---

_Added to milestone `v0.10` by @MichaReiser on 2025-03-12 14:15_

---

_Label `rule` added by @MichaReiser on 2025-03-12 14:15_

---

_Comment by @codspeed-hq[bot] on 2025-03-12 14:20_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fpytest-empty-for-loop)

### Merging #16678 will **degrade performances by 10.5%**

<sub>Comparing <code>micha/pytest-empty-for-loop</code> (ec38e73) with <code>micha/ruff-0.10</code> (e9bfdfd)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Fpytest-empty-for-loop)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 4.9 ms | 5.5 ms | -10.5% |


---

_Comment by @github-actions[bot] on 2025-03-12 14:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @MichaReiser on 2025-03-12 16:58_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/warns.rs`:16 on 2025-03-12 18:10_

Nice, I think it made sense to move these up to `## What it does` too.

---

_@ntBre approved on 2025-03-12 18:10_

---

_Merged by @MichaReiser on 2025-03-13 07:42_

---

_Closed by @MichaReiser on 2025-03-13 07:42_

---

_Branch deleted on 2025-03-13 07:42_

---
