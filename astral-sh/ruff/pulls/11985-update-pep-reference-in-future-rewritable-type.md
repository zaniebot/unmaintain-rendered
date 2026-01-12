```yaml
number: 11985
title: Update PEP reference in future_rewritable_type_annotation.rs
type: pull_request
state: merged
author: ericbn
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-06-22T22:20:19Z
updated_at: 2024-06-23T01:15:56Z
url: https://github.com/astral-sh/ruff/pull/11985
synced_at: 2026-01-12T15:55:40Z
```

# Update PEP reference in future_rewritable_type_annotation.rs

---

_@ericbn_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Documentation mentions:

> PEP 563 enabled the use of a number of convenient type annotations, such as `list[str]` instead of `List[str]`

but it meant [PEP 585](https://peps.python.org/pep-0585/) instead.

[PEP 563](https://peps.python.org/pep-0563/) is the one defining `from __future__ import annotations`.

## Test Plan

No automated test required, just verify that https://peps.python.org/pep-0585/ is the correct reference.


---

_Comment by @codspeed-hq[bot] on 2024-06-22 22:25_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ericbn:patch-1)

### Merging #11985 will **degrade performances by 4.81%**

<sub>Comparing <code>ericbn:patch-1</code> (4d78032) with <code>main</code> (519a278)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ericbn:patch-1)._

### Benchmarks breakdown

|     | Benchmark | `main` | `ericbn:patch-1` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[pydantic/types.py]` | 1.8 ms | 1.9 ms | -4.81% |


---

_@VascoSch92 reviewed on 2024-06-22 22:37_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/flake8_future_annotations/rules/future_rewritable_type_annotation.rs`:12 on 2024-06-22 22:37_

this should also been changed or?

---

_Review comment by @ericbn on `crates/ruff_linter/src/rules/flake8_future_annotations/rules/future_rewritable_type_annotation.rs`:12 on 2024-06-22 22:55_

No, this reference is correct.

 Lots of PEP involved here.  :- )

---

_@ericbn reviewed on 2024-06-22 22:55_

---

_@zanieb approved on 2024-06-23 01:14_

Thanks!

---

_Label `documentation` added by @zanieb on 2024-06-23 01:15_

---

_Merged by @zanieb on 2024-06-23 01:15_

---

_Closed by @zanieb on 2024-06-23 01:15_

---

_Branch deleted on 2024-06-23 01:15_

---
