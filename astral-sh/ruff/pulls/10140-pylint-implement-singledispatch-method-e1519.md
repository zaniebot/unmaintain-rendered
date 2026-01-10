```yaml
number: 10140
title: "[`pylint`] Implement `singledispatch-method` (`E1519`)"
type: pull_request
state: merged
author: Glyphack
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: pytlint/singledispatch-method-E1519
created_at: 2024-02-26T20:22:10Z
updated_at: 2024-03-01T08:51:25Z
url: https://github.com/astral-sh/ruff/pull/10140
synced_at: 2026-01-10T22:47:01Z
```

# [`pylint`] Implement `singledispatch-method` (`E1519`)

---

_Pull request opened by @Glyphack on 2024-02-26 20:22_

Implementing the rule 
https://pylint.readthedocs.io/en/stable/user_guide/messages/error/singledispatch-method.html#singledispatch-method-e1519

Implementation simply checks the function type and name of the decorators.

---

_Renamed from "Implement pylint E1519 singledispatch-method #970" to "linter: Implement pylint E1519 singledispatch-method #970" by @Glyphack on 2024-02-28 19:04_

---

_Marked ready for review by @Glyphack on 2024-02-28 19:05_

---

_Comment by @github-actions[bot] on 2024-02-28 20:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -3 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/301c5c719bc13c20d44b488d987a05fa5fd605a0/pandas/_libs/tslibs/period.pyi#L57'>pandas/_libs/tslibs/period.pyi:57:5:</a> PLR0917 Too many positional arguments (9/5)
- <a href='https://github.com/pandas-dev/pandas/blob/301c5c719bc13c20d44b488d987a05fa5fd605a0/pandas/_libs/tslibs/period.pyi#L73'>pandas/_libs/tslibs/period.pyi:73:1:</a> PLR0904 Too many public methods (24 > 20)
- <a href='https://github.com/pandas-dev/pandas/blob/301c5c719bc13c20d44b488d987a05fa5fd605a0/pandas/_libs/tslibs/period.pyi#L79'>pandas/_libs/tslibs/period.pyi:79:9:</a> PLR0917 Too many positional arguments (10/5)
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0917 | 2 | 0 | 2 | 0 | 0 |
| PLR0904 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/pylint/rules/single_dispatch_method.rs`:13 on 2024-02-29 00:36_

Example code is helpful here. Take a look at other rules to see the format. The pylint docs contains examples to use. 

You can also link the Python documentation of singledispatchmethod 

---

_Review comment by @sbrugman on `crates/ruff_linter/resources/test/fixtures/pylint/E1519.py`:16 on 2024-02-29 00:38_

Is it possible to catch @funcname.register as the pylint rule does?

Even if not, it would be good to include those examples in the tests in case we can do it in the future.

---

_@sbrugman reviewed on 2024-02-29 00:38_

---

_Label `rule` added by @charliermarsh on 2024-02-29 15:51_

---

_Label `preview` added by @charliermarsh on 2024-02-29 15:51_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/pylint/E1519.py`:16 on 2024-02-29 15:52_

Probably a bit harder (but possible) to catch `@funcname.register`, but agree we should add tests for it with commentary.

---

_@charliermarsh reviewed on 2024-02-29 15:52_

---

_Renamed from "linter: Implement pylint E1519 singledispatch-method #970" to "[`pylint`] Implement `singledispatch-method` (`E1519`)" by @charliermarsh on 2024-03-01 01:09_

---

_Comment by @charliermarsh on 2024-03-01 01:09_

Gonna address feedback and merge.

---

_@charliermarsh approved on 2024-03-01 02:02_

---

_Merged by @charliermarsh on 2024-03-01 02:22_

---

_Closed by @charliermarsh on 2024-03-01 02:22_

---

_Branch deleted on 2024-03-01 08:50_

---

_Comment by @Glyphack on 2024-03-01 08:51_

Thanks for addressing the comments 😃

---
