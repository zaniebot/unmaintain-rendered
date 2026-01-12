```yaml
number: 16669
title: "Consider all `TYPE_CHECKING` symbols for type-checking blocks"
type: pull_request
state: merged
author: MichaReiser
labels:
  - breaking
assignees: []
merged: true
base: micha/ruff-0.10
head: micha/non-typing-type-checking
created_at: 2025-03-12T11:50:07Z
updated_at: 2025-03-13T07:44:06Z
url: https://github.com/astral-sh/ruff/pull/16669
synced_at: 2026-01-12T15:55:55Z
```

# Consider all `TYPE_CHECKING` symbols for type-checking blocks

---

_@MichaReiser_

## Summary

This PR stabilizes the preview behavior introduced in https://github.com/astral-sh/ruff/pull/15719 to recognize all symbols named `TYPE_CHECKING` as type-checking
checks in `if TYPE_CHECKING` conditions. This ensures compatibility with mypy and pyright. 

This PR also stabilizes the new behavior that removes `if 0:` and `if False` to be no longer considered type checking blocks. 
Since then, this syntax has been removed from the typing spec and was only used for Python modules that don't have a `typing` module ([comment](https://github.com/astral-sh/ruff/pull/15719#issuecomment-2612787793)).

The preview behavior was first released with Ruff 0.9.5 (6th of February), which was about a month ago. There are no open issues or PRs for the changed behavior


## Test Plan

The snapshots for `SIM108` change because `SIM108` ignored type checking blocks but it can no
simplify `if 0` or `if False` blocks again because they're no longer considered type checking blocks. 

The changes in the `TC005` snapshot or only due to that `if 0` and `if False` are no longer recognized as type checking blocks

<!-- How was it tested? -->


---

_Label `breaking` added by @MichaReiser on 2025-03-12 11:50_

---

_Comment by @codspeed-hq[bot] on 2025-03-12 11:57_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fnon-typing-type-checking)

### Merging #16669 will **degrade performances by 10.62%**

<sub>Comparing <code>micha/non-typing-type-checking</code> (233ddc1) with <code>micha/ruff-0.10</code> (948435d)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Fnon-typing-type-checking)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 4.9 ms | 5.5 ms | -10.62% |


---

_Comment by @github-actions[bot] on 2025-03-12 11:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/d1aaf2dbd3ad07cbe2f07b91f7653b2446546ea3/src/scikit_build_core/resources/_editable_redirect.py#L11'>src/scikit_build_core/resources/_editable_redirect.py:11:12:</a> TC004 Move import `importlib.machinery` out of type-checking block. Import is used for more than type hinting.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TC004 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-03-12 12:01_

The ecosystem change is a false positive but this is an issue with the corresponding rules and not specific to the change itself. See https://github.com/astral-sh/ruff/pull/15719#issuecomment-2612876491 for an in-depth explanation. There's also an open PR to fix this, but I haven't found the time yet to review it.

---

_Added to milestone `v0.10` by @MichaReiser on 2025-03-12 12:48_

---

_Review requested from @ntBre by @MichaReiser on 2025-03-12 16:58_

---

_@ntBre approved on 2025-03-12 18:00_

---

_Merged by @MichaReiser on 2025-03-13 07:44_

---

_Closed by @MichaReiser on 2025-03-13 07:44_

---

_Branch deleted on 2025-03-13 07:44_

---
