```yaml
number: 15032
title: "[`pylint`] Detect nested methods correctly (`PLW1641`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: PLW1641
created_at: 2024-12-17T09:13:51Z
updated_at: 2024-12-31T14:58:42Z
url: https://github.com/astral-sh/ruff/pull/15032
synced_at: 2026-01-12T15:55:49Z
```

# [`pylint`] Detect nested methods correctly (`PLW1641`)

---

_@InSyncWithFoo_

## Summary

Resolves #14882.

## Test Plan

`cargo nextest run` and `cargo insta test`.

---

_Comment by @github-actions[bot] on 2024-12-17 09:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/d6c92e0d76c393e38d9b5863a628dbaaf931ed5a/astropy/modeling/parameters.py#L113'>astropy/modeling/parameters.py:113:7:</a> PLW1641 Object does not implement `__hash__` method
+ <a href='https://github.com/astropy/astropy/blob/d6c92e0d76c393e38d9b5863a628dbaaf931ed5a/astropy/table/bst.py#L90'>astropy/table/bst.py:90:7:</a> PLW1641 Object does not implement `__hash__` method
+ <a href='https://github.com/astropy/astropy/blob/d6c92e0d76c393e38d9b5863a628dbaaf931ed5a/astropy/table/column.py#L1164'>astropy/table/column.py:1164:7:</a> PLW1641 Object does not implement `__hash__` method
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW1641 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @MichaReiser on 2024-12-18 07:57_

I'm a bit concerned about special casing this behavior only for `PLW1641`. Other rules that need to inspect a class's methods, like https://github.com/astral-sh/ruff/pull/14688, will now have inconsistent behavior. 

I wonder if we could make this more reusable so that all class method rules could profit from it. Although I'm not quiet sure how to do that.

For example, we could create a `any_method` function in `semantic/analyze/class`

---

_@MichaReiser approved on 2024-12-30 15:51_

I refactored your code and introduced a reusable `any_member_declaration` as discussed in the PR before. The reason why I prefer having this helper is that it now can be used for other rules as well. For example, too many public methods could make use of it, resulting in more consistent behavior across our rules.

---

_Label `rule` added by @MichaReiser on 2024-12-30 15:51_

---

_Label `preview` added by @MichaReiser on 2024-12-30 15:51_

---

_Merged by @MichaReiser on 2024-12-30 15:55_

---

_Closed by @MichaReiser on 2024-12-30 15:55_

---

_Comment by @MichaReiser on 2024-12-30 15:55_

Thank you

---

_Branch deleted on 2024-12-31 14:58_

---
