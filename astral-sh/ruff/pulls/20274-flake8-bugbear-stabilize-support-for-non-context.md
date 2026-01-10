```yaml
number: 20274
title: "[`flake8-bugbear`] Stabilize support for non-context-manager calls in `assert-raises-exception` (`B017`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: brent/0.13.0
head: dylan/stabilize-assert-raises
created_at: 2025-09-05T19:33:06Z
updated_at: 2025-09-08T13:58:47Z
url: https://github.com/astral-sh/ruff/pull/20274
synced_at: 2026-01-10T17:46:21Z
```

# [`flake8-bugbear`] Stabilize support for non-context-manager calls in `assert-raises-exception` (`B017`)

---

_Pull request opened by @dylwil3 on 2025-09-05 19:33_

Introduced in #19063. Removed gating, updated tests. Not documented so docs are the same.


---

_Comment by @github-actions[bot] on 2025-09-05 19:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/45b35ebc26d82e9bd3890430376e47884cc20062/testing/code/test_code.py#L91'>testing/code/test_code.py:91:15:</a> B017 Do not assert blind exception: `Exception`
+ <a href='https://github.com/pytest-dev/pytest/blob/45b35ebc26d82e9bd3890430376e47884cc20062/testing/example_scripts/fixtures/fill_fixtures/test_conftest_funcargs_only_available_in_subdir/sub2/conftest.py#L9'>testing/example_scripts/fixtures/fill_fixtures/test_conftest_funcargs_only_available_in_subdir/sub2/conftest.py:9:5:</a> B017 Do not assert blind exception: `Exception`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B017 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-09-05 21:05_

---

_Merged by @dylwil3 on 2025-09-08 13:58_

---

_Closed by @dylwil3 on 2025-09-08 13:58_

---

_Branch deleted on 2025-09-08 13:58_

---

_Label `rule` added by @dylwil3 on 2025-09-08 13:58_

---
