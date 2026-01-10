```yaml
number: 16648
title: "[`ruff`] Stabilize `unnecessary-nested-literal` (`RUF041`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: brent/ruf041-0.10
created_at: 2025-03-11T21:05:23Z
updated_at: 2025-03-12T18:12:29Z
url: https://github.com/astral-sh/ruff/pull/16648
synced_at: 2026-01-10T19:49:02Z
```

# [`ruff`] Stabilize `unnecessary-nested-literal` (`RUF041`)

---

_Pull request opened by @ntBre on 2025-03-11 21:05_

Summary
--

Stabilizes RUF041. The tests are already in the right place, and the docs look good.

Test Plan
--

0 issues, 1 [PR] fixing nested literals and unions the day after the rule was added. No changes since then

I wonder if the fix in that PR could be relevant for https://github.com/astral-sh/ruff/pull/16639, where I noticed a potential issue with `Union`. It could be unrelated, though.

[PR]: https://github.com/astral-sh/ruff/pull/14641


---

_Label `rule` added by @ntBre on 2025-03-11 21:05_

---

_Added to milestone `v0.10` by @ntBre on 2025-03-11 21:05_

---

_Comment by @codspeed-hq[bot] on 2025-03-11 21:10_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fruf041-0.10)

### Merging #16648 will **degrade performances by 4.6%**

<sub>Comparing <code>brent/ruf041-0.10</code> (4ddef74) with <code>micha/ruff-0.10</code> (7bc8bb2)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/brent%2Fruf041-0.10)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 5.2 ms | 5.5 ms | -4.6% |


---

_Comment by @github-actions[bot] on 2025-03-11 21:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/tests/test_utils.py#L747'>tests/test_utils.py:747:18:</a> RUF041 [*] Unnecessary nested `Literal`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/tests/test_utils.py#L775'>tests/test_utils.py:775:10:</a> RUF041 [*] Unnecessary nested `Literal`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF041 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-03-11 22:00_

These look like true positives to me. Interestingly, it looks like disnake is testing this flattening behavior, so they will probably want to `noqa` or otherwise disable this rule, though.

---

_@MichaReiser approved on 2025-03-12 08:55_

---

_Merged by @ntBre on 2025-03-12 18:12_

---

_Closed by @ntBre on 2025-03-12 18:12_

---

_Branch deleted on 2025-03-12 18:12_

---
