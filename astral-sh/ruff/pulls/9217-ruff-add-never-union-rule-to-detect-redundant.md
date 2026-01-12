```yaml
number: 9217
title: "[`ruff`] Add `never-union` rule to detect redundant `typing.NoReturn` and `typing.Never`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: charlie/union
created_at: 2023-12-20T18:35:25Z
updated_at: 2023-12-21T21:03:38Z
url: https://github.com/astral-sh/ruff/pull/9217
synced_at: 2026-01-12T15:55:28Z
```

# [`ruff`] Add `never-union` rule to detect redundant `typing.NoReturn` and `typing.Never`

---

_@charliermarsh_

## Summary

Adds a rule to detect unions that include `typing.NoReturn` or `typing.Never`. In such cases, the use of the bottom type is redundant.

Closes https://github.com/astral-sh/ruff/issues/9113.

## Test Plan

`cargo test`

---

_Label `rule` added by @charliermarsh on 2023-12-20 18:35_

---

_Label `preview` added by @charliermarsh on 2023-12-20 18:35_

---

_Comment by @charliermarsh on 2023-12-20 18:35_

This should be fixable but isn't trivial, working on it separately.

---

_Review requested from @zanieb by @charliermarsh on 2023-12-20 18:50_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-12-20 18:50_

---

_Comment by @github-actions[bot] on 2023-12-20 19:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 1 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/support/plugins/selenium.py#L118'>tests/support/plugins/selenium.py:118:88:</a> RUF020 `NoReturn | T` is equivalent to `T`
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/support/plugins/selenium.py#L132'>tests/support/plugins/selenium.py:132:47:</a> RUF020 `NoReturn | T` is equivalent to `T`
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/support/plugins/selenium.py#L146'>tests/support/plugins/selenium.py:146:47:</a> RUF020 `NoReturn | T` is equivalent to `T`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF020 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/never_union.rs`:111 on 2023-12-21 00:05_

Nit: I struggled with the name because I incorrectly assumed it represents `int | str`. Maybe `TypingUnion`? or `Typing` and `BinOp`?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/never_union.rs`:62 on 2023-12-21 00:07_

Unrelated to this PR: Hmm, one downside of quoting runtime only type annotations is that it makes all lint-rules useless because we don't parse string annotations...

---

_@MichaReiser reviewed on 2023-12-21 00:10_

---

_@charliermarsh reviewed on 2023-12-21 02:40_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/never_union.rs`:62 on 2023-12-21 02:40_

We actually do!

---

_@zanieb approved on 2023-12-21 04:18_

Cool!

---

_Comment by @zanieb on 2023-12-21 04:19_

Interesting notes in the ecosystem e.g. 

> XXX: no return should be needed with NoReturn type (type-checker bug?) [ref](https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/support/plugins/selenium.py#L142-L143)

---

_Merged by @charliermarsh on 2023-12-21 20:53_

---

_Closed by @charliermarsh on 2023-12-21 20:53_

---

_Branch deleted on 2023-12-21 20:53_

---
