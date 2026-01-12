```yaml
number: 12065
title: "[`numpy`] Update `NPY201` to include exception deprecations"
type: pull_request
state: merged
author: mtsokol
labels:
  - rule
assignees: []
merged: true
base: main
head: update-numpy2-rule
created_at: 2024-06-27T08:56:08Z
updated_at: 2024-07-05T12:40:44Z
url: https://github.com/astral-sh/ruff/pull/12065
synced_at: 2026-01-12T15:55:40Z
```

# [`numpy`] Update `NPY201` to include exception deprecations

---

_@mtsokol_

Hi!

This PR updates `NPY201` rule to address https://github.com/astral-sh/ruff/issues/12034 and partially https://github.com/numpy/numpy/issues/26800. 

---

_Comment by @mtsokol on 2024-06-27 09:54_

I'm having trouble generating new snapshots. The current `NPY201.py.snap` snapshot works with `NPY201.py` file up to the line `125`. With two last lines removed in `NPY201.py` the command `cargo insta test -p ruff_linter` passes.

But when I add another function that is covered by the rule (as right now in the PR):
```python

    np.compare_chararrays

```

The above `test` command fails and doesn't generate new snap update file. How can I solve it?

I did the existing changes to the NumPy snapshot file by running the above `test` command and `cargo insta review` which worked for the first functions that I added to the `.py` file. Is there a snapshot file line count limit or some other constraint?

---

_Comment by @charliermarsh on 2024-06-27 12:48_

I'll take a look. We might be hitting the limit we set on number of iterations during testing (this is just a safeguard).

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-27 12:48_

---

_Comment by @MichaReiser on 2024-06-27 16:32_

It seems that the fixes never converge

```
Failed to converge after 10 iterations. This likely indicates a bug in the implementation of the fix. Last diagnostics:
NPY201.py:15:5: NPY201 `np.add_newdoc_ufunc` will be removed in NumPy 2.0. `add_newdoc_ufunc` is an internal function.
   |
13 |     add_newdoc
14 | 
15 |     np.add_newdoc_ufunc
   |     ^^^^^^^^^^^^^^^^^^^ NPY201
16 | 
17 |     np.asfarray([1,2,3])
```

---

_Comment by @charliermarsh on 2024-06-27 16:52_

I think more likely is that it needs more than 10 iterations due to conflicts.

---

_Comment by @MichaReiser on 2024-06-27 18:19_

It seems we're only replacing one `ExprName` at a time... 

Probably because they all overlap because each of them tries to add the `numpy` import. Although it's unclear why we still try to add the numpy import when it already exists.

---

_Comment by @charliermarsh on 2024-06-27 18:24_

They probably all touch the `numpy` import to ensure that it doesn't get removed by another rule.

---

_Comment by @MichaReiser on 2024-06-27 18:25_

Yeah, the cullprint is this. Which works as intended for removals but also means that it only applies one fix at the time. 

https://github.com/astral-sh/ruff/blob/bf5b62edaccdc007dfcf559ab9e870be66c19877/crates/ruff_linter/src/importer/mod.rs#L304-L324

An easy workaround could be to split the test into two files

---

_Comment by @charliermarsh on 2024-06-27 18:26_

Yeah, or bump the limit a little bit. Either is ok with me. (We probably want a smarter strategy for this in the future.)

---

_Comment by @charliermarsh on 2024-06-27 18:35_

I'll do it and merge this.

---

_Comment by @MichaReiser on 2024-06-27 18:53_

I'm leaning toward splitting the tests. Many iterations can make it more difficult to debug real issues.

---

_Comment by @charliermarsh on 2024-06-27 18:53_

Too late!

---

_Comment by @MichaReiser on 2024-06-27 18:54_

Fair enough

---

_Renamed from "Update `NPY201` rule" to "[`numpy`] Update `NPY201` to include exception deprecations" by @charliermarsh on 2024-06-27 18:54_

---

_Merged by @charliermarsh on 2024-06-27 18:56_

---

_Closed by @charliermarsh on 2024-06-27 18:56_

---

_Comment by @codspeed-hq[bot] on 2024-06-27 18:58_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtsokol:update-numpy2-rule)

### Merging #12065 will **degrade performances by 5.15%**

<sub>Comparing <code>mtsokol:update-numpy2-rule</code> (8726e6e) with <code>mtsokol:update-numpy2-rule</code> (38ca9b8)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/mtsokol:update-numpy2-rule)._

### Benchmarks breakdown

|     | Benchmark | `mtsokol:update-numpy2-rule` | `mtsokol:update-numpy2-rule` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[unicode/pypinyin.py]` | 2.1 ms | 2.3 ms | -5.15% |


---

_Branch deleted on 2024-06-28 07:20_

---

_Comment by @mtsokol on 2024-06-28 07:31_

Thank you for solving it! One comment - the python file test is still missing these new entries added to the rule:
```python
np.DTypePromotionError
np.ModuleDeprecationWarning
np.RankWarning
np.TooHardError
np.VisibleDeprecationWarning
np.chararray
np.format_parser
```
I didn't add them yesterday to reproduce the issue in a minimal configuration. We can still add them or leave the test as it's right now - both options are fine with me.

---

_Label `rule` added by @dhruvmanila on 2024-07-05 12:40_

---
