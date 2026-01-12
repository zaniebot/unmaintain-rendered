```yaml
number: 16680
title: "[`flake8-bandit`] Deprecate `suspicious-xmle-tree-usage` (`S320`)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - rule
  - breaking
assignees: []
merged: true
base: micha/ruff-0.10
head: micha/deprecate-s320
created_at: 2025-03-12T14:51:43Z
updated_at: 2025-03-13T07:46:26Z
url: https://github.com/astral-sh/ruff/pull/16680
synced_at: 2026-01-12T15:55:55Z
```

# [`flake8-bandit`] Deprecate `suspicious-xmle-tree-usage` (`S320`)

---

_@MichaReiser_

## Summary
Deprecate `S320` because defusedxml has deprecated there `lxml` module and `lxml` has been hardened since. 

flake8-bandit has removed their implementation as well (https://github.com/PyCQA/bandit/pull/1212).

Addresses https://github.com/astral-sh/ruff/issues/13707


## Test Plan

I verified that selecting `S320` prints a warning and fails if the preview mode is enabled. 


---

_Label `breaking` added by @MichaReiser on 2025-03-12 14:52_

---

_Label `rule` added by @MichaReiser on 2025-03-12 14:52_

---

_Added to milestone `v0.10` by @MichaReiser on 2025-03-12 14:52_

---

_Review requested from @ntBre by @MichaReiser on 2025-03-12 14:53_

---

_@MichaReiser reviewed on 2025-03-12 14:53_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:29 on 2025-03-12 14:53_

This is an unrelated run-by fix. It still referenced the old option name. 

---

_Comment by @codspeed-hq[bot] on 2025-03-12 14:57_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fdeprecate-s320)

### Merging #16680 will **degrade performances by 4.61%**

<sub>Comparing <code>micha/deprecate-s320</code> (98d1767) with <code>micha/ruff-0.10</code> (464ea4a)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Fdeprecate-s320)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 5.2 ms | 5.5 ms | -4.61% |


---

_Comment by @github-actions[bot] on 2025-03-12 14:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/950107feb11365686f997128fa71360b88fca8b8/providers/amazon/src/airflow/providers/amazon/aws/hooks/base_aws.py#L395'>providers/amazon/src/airflow/providers/amazon/aws/hooks/base_aws.py:395:15:</a> S320 Using `lxml` to parse untrusted data is known to be vulnerable to XML attacks
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S320 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/950107feb11365686f997128fa71360b88fca8b8/providers/amazon/src/airflow/providers/amazon/aws/hooks/base_aws.py#L395'>providers/amazon/src/airflow/providers/amazon/aws/hooks/base_aws.py:395:15:</a> S320 Using `lxml` to parse untrusted data is known to be vulnerable to XML attacks
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S320 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_@ntBre reviewed on 2025-03-12 16:12_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:29 on 2025-03-12 16:12_

Nice catch

---

_@ntBre approved on 2025-03-12 16:14_

---

_Merged by @MichaReiser on 2025-03-13 07:46_

---

_Closed by @MichaReiser on 2025-03-13 07:46_

---

_Branch deleted on 2025-03-13 07:46_

---
