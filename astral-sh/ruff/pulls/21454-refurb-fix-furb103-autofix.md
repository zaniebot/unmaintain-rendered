```yaml
number: 21454
title: "[`refurb`] Fix `FURB103` autofix"
type: pull_request
state: merged
author: chirizxc
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: FURB103
created_at: 2025-11-14T14:10:11Z
updated_at: 2025-11-16T09:43:20Z
url: https://github.com/astral-sh/ruff/pull/21454
synced_at: 2026-01-12T15:57:24Z
```

# [`refurb`] Fix `FURB103` autofix

---

_@chirizxc_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
Fixes #21381
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
`cargo nextest run furb103`
<!-- How was it tested? -->


---

_Comment by @astral-sh-bot[bot] on 2025-11-14 14:21_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/af5561ba730f3fcf1290531ac986edd4b631be13/astropy/io/fits/tests/test_hdulist.py#L1130'>astropy/io/fits/tests/test_hdulist.py:1130:14:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename)....`
- <a href='https://github.com/astropy/astropy/blob/af5561ba730f3fcf1290531ac986edd4b631be13/astropy/io/fits/tests/test_hdulist.py#L1130'>astropy/io/fits/tests/test_hdulist.py:1130:14:</a> FURB103 [*] `open` and `write` should be replaced by `Path(filename).write_text("Ceçi ne marche pas", encoding="utf=8")`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB103 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>





---

_Label `fixes` added by @amyreese on 2025-11-14 19:00_

---

_Label `bug` added by @amyreese on 2025-11-14 19:01_

---

_@amyreese approved on 2025-11-14 19:04_

---

_Review requested from @ntBre by @amyreese on 2025-11-14 19:04_

---

_@MichaReiser approved on 2025-11-16 08:32_

---

_Merged by @MichaReiser on 2025-11-16 08:32_

---

_Closed by @MichaReiser on 2025-11-16 08:32_

---

_Branch deleted on 2025-11-16 09:43_

---
