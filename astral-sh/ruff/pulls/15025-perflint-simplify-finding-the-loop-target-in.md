```yaml
number: 15025
title: "[`perflint`] Simplify finding the loop target in `PERF401`"
type: pull_request
state: merged
author: w0nder1ng
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: perf401_panic_del
created_at: 2024-12-16T20:54:49Z
updated_at: 2024-12-17T07:30:36Z
url: https://github.com/astral-sh/ruff/pull/15025
synced_at: 2026-01-10T20:42:27Z
```

# [`perflint`] Simplify finding the loop target in `PERF401`

---

_Pull request opened by @w0nder1ng on 2024-12-16 20:54_

Fixes #15012.

```python
def f():
    # panics when the code can't find the loop variable
    values = [1, 2, 3]
    result = []
    for i in values:
        result.append(i + 1)
    del i
```

I'm not sure exactly why this test case panics, but I suspect the `del i` removes the binding from the semantic model's symbols.

I changed the code to search for the correct binding by directly iterating through the bindings. Since we know exactly which binding we want, this should find the loop variable without any complications.

---

_Comment by @github-actions[bot] on 2024-12-16 21:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/console/commands/test_show.py#L127'>tests/console/commands/test_show.py:127:5:</a> B018 Found useless expression. Either assign it to a variable or remove it.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B018 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dylwil3 on 2024-12-16 23:38_

> I'm not sure exactly why this test case panics, but I suspect the del i removes the binding from the semantic model's symbols.

Yep. This is the cause of a few cases of strange behavior... When the semantic model is called "as it's built" then this behavior is correct (because at the program point of interest, the delete has not yet ocurred), but it can be counter-intuitive when called (like in the current situation) on deferred scopes/for-loops/etc.

---

_Renamed from "[`perflint`] Simplify finding the loop target in `perf401`" to "[`perflint`] Simplify finding the loop target in `PERF401`" by @dylwil3 on 2024-12-17 03:59_

---

_Label `bug` added by @dylwil3 on 2024-12-17 04:00_

---

_Label `rule` added by @dylwil3 on 2024-12-17 04:00_

---

_@dhruvmanila reviewed on 2024-12-17 04:28_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:245 on 2024-12-17 04:28_

Is this `unwrap` safe? If so, we should use `expect` but I'd possibly just do an early return if this is `None`.

---

_Label `rule` removed by @dhruvmanila on 2024-12-17 04:29_

---

_@MichaReiser reviewed on 2024-12-17 07:30_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs`:245 on 2024-12-17 07:30_

It should be fine. The rule only applies to for-loop targets that are names; a binding must exist. 

---

_@MichaReiser approved on 2024-12-17 07:30_

---

_Label `preview` added by @MichaReiser on 2024-12-17 07:30_

---

_Merged by @MichaReiser on 2024-12-17 07:30_

---

_Closed by @MichaReiser on 2024-12-17 07:30_

---
