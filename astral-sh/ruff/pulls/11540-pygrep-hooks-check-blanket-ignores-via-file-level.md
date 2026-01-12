```yaml
number: 11540
title: "[`pygrep_hooks`] Check blanket ignores via file-level pragmas (`PGH004`)"
type: pull_request
state: merged
author: ajesipow
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: pgh004-file-level
created_at: 2024-05-25T10:58:22Z
updated_at: 2024-06-04T03:52:47Z
url: https://github.com/astral-sh/ruff/pull/11540
synced_at: 2026-01-12T15:55:38Z
```

# [`pygrep_hooks`] Check blanket ignores via file-level pragmas (`PGH004`)

---

_@ajesipow_

## Summary

Should resolve https://github.com/astral-sh/ruff/issues/11454.

This is my first PR to `ruff`, so I may have missed something.

If I understood the suggestion in the issue correctly, rule `PGH004` should be set to `Preview` again.

## Test Plan

Created two fixtures derived from the issue.


---

_Comment by @github-actions[bot] on 2024-05-25 11:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+5 -0 violations, +0 -0 fixes in 4 projects; 46 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/8b99ad0fbe136371de4c303f7ffee9b5469e77e0/docs/exts/exampleinclude.py#L1'>docs/exts/exampleinclude.py:1:1:</a> PGH004 Use specific rule codes when using `ruff: noqa`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/ae28f6b55c09cf3cc1508e921977ae9f0cb46cb8/indico/legacy/pdfinterface/base.py#L8'>indico/legacy/pdfinterface/base.py:8:1:</a> PGH004 Use specific rule codes when using `ruff: noqa`
+ <a href='https://github.com/indico/indico/blob/ae28f6b55c09cf3cc1508e921977ae9f0cb46cb8/indico/legacy/pdfinterface/conference.py#L8'>indico/legacy/pdfinterface/conference.py:8:1:</a> PGH004 Use specific rule codes when using `ruff: noqa`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/7be95f9b30ffd418483bdb57112f9339465d4695/testing/code/test_source.py#L2'>testing/code/test_source.py:2:1:</a> PGH004 Use specific rule codes when using `ruff: noqa`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pdm-project/pdm">pdm-project/pdm</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pdm-project/pdm/blob/f429652f856a61f59ce5013e3195a30fbe88ce62/src/pdm/installers/__init__.py#L1'>src/pdm/installers/__init__.py:1:1:</a> PGH004 Use specific rule codes when using `ruff: noqa`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PGH004 | 5 | 5 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @JonathanPlasse on 2024-05-25 12:55_

The stable behavior should be unchanged, the added behavior should only be present in preview.
i.e. The ruff ecosystem results should not change for stable, only preview should have additional violations.
Otherwise, the users of stable will lose the PGH004 rule.

---

_Comment by @ajesipow on 2024-05-25 17:10_

Thanks for the swift feedback.

I added a check for `PreviewMode::Enabled` and added tests for the preview case.

Would you like me to also update the docs for `BlanketNOQA`?

---

_Review requested from @zanieb by @zanieb on 2024-05-25 17:35_

---

_@charliermarsh reviewed on 2024-05-27 00:39_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pygrep_hooks/rules/blanket_noqa.rs`:89 on 2024-05-27 00:39_

One downside here is that this is only going to flag the _first_ `# ruff: noqa` in the file. I assume if you add multiple such comments, only the first is flagged as a diagnostic?

---

_@charliermarsh reviewed on 2024-05-27 00:40_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pygrep_hooks/rules/blanket_noqa.rs`:89 on 2024-05-27 00:40_

We might need to do something like `NoqaDirectives`, whereby we parse all of these upfront, then use the parsed exemptions in `crates/ruff_linter/src/checkers/noqa.rs` (and here).

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-28 13:56_

---

_Label `rule` added by @charliermarsh on 2024-05-28 13:56_

---

_Label `preview` added by @charliermarsh on 2024-05-28 13:56_

---

_@charliermarsh reviewed on 2024-05-30 03:59_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pygrep_hooks/rules/blanket_noqa.rs`:89 on 2024-05-30 03:59_

@ajesipow - Do you want to give that a try?

---

_@ajesipow reviewed on 2024-05-30 07:24_

---

_Review comment by @ajesipow on `crates/ruff_linter/src/rules/pygrep_hooks/rules/blanket_noqa.rs`:89 on 2024-05-30 07:24_

Thanks for the ping @charliermarsh.
I haven't found the time yet after work this week, but today is a public holiday over here so I'll look into it today :) 

---

_@ajesipow reviewed on 2024-05-30 10:21_

---

_Review comment by @ajesipow on `crates/ruff_linter/src/noqa.rs`:285 on 2024-05-30 10:21_

I left this as a `Vec`, but we could consider using a `HashSet` instead as we're checking if certain `NoqaCode`s are contained in a few places (one being the loop on line 54 in `checkers/noqa.rs`). 

It's probably fine though as I'd expect there will usually only be very few elements here and checking the small `Vec` is likely faster compared to the overhead of hashing every time (and building the `HashSet` in the first place).
We'd also have to derive `Hash` on `NoqaCode`.

---

_Review comment by @ajesipow on `crates/ruff_linter/src/rules/pygrep_hooks/rules/blanket_noqa.rs`:89 on 2024-05-30 10:26_

I just pushed the changes and adapted the tests again accordingly.

Looking forward to your feedback again.

---

_@ajesipow reviewed on 2024-05-30 10:26_

---

_Comment by @charliermarsh on 2024-05-30 17:44_

Thanks @ajesipow, I will take a look.

---

_@charliermarsh approved on 2024-06-04 03:41_

Thank you!

---

_Renamed from "[pygrep_hooks] Check file-level pragmas (PGH004)" to "[`pygrep_hooks`] Check blanket ignores via file-level pragmas (`PGH004`)" by @charliermarsh on 2024-06-04 03:41_

---

_Merged by @charliermarsh on 2024-06-04 03:42_

---

_Closed by @charliermarsh on 2024-06-04 03:42_

---

_Comment by @codspeed-hq[bot] on 2024-06-04 03:44_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ajesipow:pgh004-file-level)

### Merging #11540 will **degrade performances by 5.63%**

<sub>Comparing <code>ajesipow:pgh004-file-level</code> (4e80490) with <code>ajesipow:pgh004-file-level</code> (8574036)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ajesipow:pgh004-file-level)._

### Benchmarks breakdown

|     | Benchmark | `ajesipow:pgh004-file-level` | `ajesipow:pgh004-file-level` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[unicode/pypinyin.py]` | 2 ms | 2.1 ms | -5.63% |


---
