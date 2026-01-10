```yaml
number: 20178
title: "[`flake8-bultins`] Detect class-scope builtin shadowing in decorators, default args, and attribute initializers (`A003`)"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-20171
created_at: 2025-08-31T15:33:14Z
updated_at: 2025-09-22T22:22:20Z
url: https://github.com/astral-sh/ruff/pull/20178
synced_at: 2026-01-10T17:40:28Z
```

# [`flake8-bultins`] Detect class-scope builtin shadowing in decorators, default args, and attribute initializers (`A003`)

---

_Pull request opened by @danparizher on 2025-08-31 15:33_

## Summary
Fix #20171


---

_Comment by @github-actions[bot] on 2025-08-31 15:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/b3dad0969e112a55be5f842c6bf21334fd5f2d35/providers/fab/src/airflow/providers/fab/auth_manager/models/__init__.py#L238'>providers/fab/src/airflow/providers/fab/auth_manager/models/__init__.py:238:22:</a> A003 Python builtin is shadowed by class attribute `id` from line 209
+ <a href='https://github.com/apache/airflow/blob/b3dad0969e112a55be5f842c6bf21334fd5f2d35/providers/fab/src/airflow/providers/fab/auth_manager/models/__init__.py#L245'>providers/fab/src/airflow/providers/fab/auth_manager/models/__init__.py:245:22:</a> A003 Python builtin is shadowed by class attribute `id` from line 209
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| A003 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @danparizher on 2025-09-01 16:07_

We can have this PR expanded to cover #20179 too if it makes sense, since they are in the same scope. Let me know!

---

_@ntBre reviewed on 2025-09-11 14:39_

Thanks, this looks right to me!

Would you mind gating this behind preview? I thought it was a bug fix when I labeled the issue, but it is likely to cause new diagnostics, as the ecosystem check shows, and I'm a bit wary of reintroducing the issues fixed in https://github.com/astral-sh/ruff/pull/9462, when the `first_non_type_parent_scope_id` check was added.

The structure of the A001 check looks a bit different to me, so let's leave that for a follow-up PR.

---

_Review requested from @ntBre by @danparizher on 2025-09-11 16:15_

---

_Label `bug` added by @ntBre on 2025-09-19 19:11_

---

_Label `rule` added by @ntBre on 2025-09-19 19:11_

---

_Label `bug` removed by @ntBre on 2025-09-19 19:13_

---

_Label `preview` added by @ntBre on 2025-09-19 19:13_

---

_@ntBre reviewed on 2025-09-19 19:16_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_builtins/rules/builtin_attribute_shadowing.rs`:133 on 2025-09-19 19:16_

I think this is the part that needs to be preview-gated right? The check currently inside of the `preview` check is the code we had before.

There shouldn't be any stable changes in the ecosystem report. We may also need to add a preview test runner.

---

_Review requested from @ntBre by @danparizher on 2025-09-21 22:44_

---

_@ntBre approved on 2025-09-22 22:01_

Thank you! I pushed one commit running the new tests in `preview`, but the results look good to me.

---

_Merged by @ntBre on 2025-09-22 22:12_

---

_Closed by @ntBre on 2025-09-22 22:12_

---

_Branch deleted on 2025-09-22 22:22_

---
