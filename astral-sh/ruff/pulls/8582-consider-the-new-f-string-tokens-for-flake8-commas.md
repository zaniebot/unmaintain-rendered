```yaml
number: 8582
title: "Consider the new f-string tokens for `flake8-commas`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/fstring-comma
created_at: 2023-11-09T11:44:17Z
updated_at: 2023-11-10T04:19:15Z
url: https://github.com/astral-sh/ruff/pull/8582
synced_at: 2026-01-12T15:55:26Z
```

# Consider the new f-string tokens for `flake8-commas`

---

_@dhruvmanila_

## Summary

This fixes the bug where the `flake8-commas` rules weren't taking the new f-string tokens into account.

## Test Plan

Add new test cases around f-strings for all of `flake8-commas`'s rules.

fixes: #8556


---

_Comment by @dhruvmanila on 2023-11-09 11:44_

Current dependencies on/for this PR:
* main
  * **PR #8582** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8582" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8582?utm_source=stack-comment).

---

_Review requested from @charliermarsh by @dhruvmanila on 2023-11-09 11:44_

---

_Label `bug` added by @dhruvmanila on 2023-11-09 11:44_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_commas/rules/trailing_commas.rs`:35 on 2023-11-09 11:45_

The raw token was never used, so let's simplify this a bit.

I've also renamed `type_` to `r#type`. This is just a personal preference and I can revert if the former is better.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_commas/rules/trailing_commas.rs`:271 on 2023-11-09 11:53_

This is the main change.

---

_@dhruvmanila reviewed on 2023-11-09 11:53_

---

_Comment by @github-actions[bot] on 2023-11-09 12:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/sphinx/writers/texinfo.py#L433'>sphinx/writers/texinfo.py:433:68:</a> COM819 [*] Trailing comma prohibited
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| COM819 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/sphinx/writers/texinfo.py#L433'>sphinx/writers/texinfo.py:433:68:</a> COM819 [*] Trailing comma prohibited
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| COM819 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_@dhruvmanila reviewed on 2023-11-09 12:17_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_commas/rules/trailing_commas.rs`:259 on 2023-11-09 12:17_

We're getting the range on `FStringEnd`, so the `None` case isn't possible.

---

_Comment by @dhruvmanila on 2023-11-09 12:18_

The ecosystem change is the fact that we've stopped checking for trailing comma within the expression part of f-strings.

---

_@charliermarsh approved on 2023-11-10 01:27_

Great, thanks!

---

_Merged by @dhruvmanila on 2023-11-10 04:19_

---

_Closed by @dhruvmanila on 2023-11-10 04:19_

---

_Branch deleted on 2023-11-10 04:19_

---
