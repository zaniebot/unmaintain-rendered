```yaml
number: 9510
title: "[`flake8-bugbear`] Add fix for `duplicate-value` (`B033`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: add-B033-autofix
created_at: 2024-01-14T04:24:33Z
updated_at: 2024-01-14T23:27:44Z
url: https://github.com/astral-sh/ruff/pull/9510
synced_at: 2026-01-12T15:55:29Z
```

# [`flake8-bugbear`] Add fix for `duplicate-value` (`B033`)

---

_@diceroll123_

## Summary

Adds autofix for [B033](https://docs.astral.sh/ruff/rules/duplicate-value/)

## Test Plan

`cargo test`

---

_Comment by @codspeed-hq[bot] on 2024-01-14 04:37_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/diceroll123:add-B033-autofix)

### Merging #9510 will **not alter performance**

<sub>Comparing <code>diceroll123:add-B033-autofix</code> (a88fd83) with <code>main</code> (953d48b)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-01-14 04:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+32 -0 violations, +2 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/4883c38f3b2e55c6e2acbc0ed690907d7b725e77/tests/providers/weaviate/hooks/test_weaviate.py#L734'>tests/providers/weaviate/hooks/test_weaviate.py:734:39:</a> B033 Sets should not contain duplicate item `"uuid2"`
+ <a href='https://github.com/apache/airflow/blob/4883c38f3b2e55c6e2acbc0ed690907d7b725e77/tests/providers/weaviate/hooks/test_weaviate.py#L734'>tests/providers/weaviate/hooks/test_weaviate.py:734:39:</a> B033 [*] Sets should not contain duplicate item `"uuid2"`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+32 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L1226'>pandas/core/groupby/groupby.py:1226:1:</a> PLR0904 Too many public methods (41 > 20)
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L1298'>pandas/core/groupby/groupby.py:1298:9:</a> PLR0917 Too many positional arguments (13/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L1449'>pandas/core/groupby/groupby.py:1449:9:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L1635'>pandas/core/groupby/groupby.py:1635:9:</a> F841 Local variable `ids` is assigned to but never used
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L1852'>pandas/core/groupby/groupby.py:1852:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L1949'>pandas/core/groupby/groupby.py:1949:12:</a> E721 Use `is` and `is not` for type comparisons, or `isinstance()` for isinstance checks
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L1988'>pandas/core/groupby/groupby.py:1988:27:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L1990'>pandas/core/groupby/groupby.py:1990:44:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L2001'>pandas/core/groupby/groupby.py:2001:19:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L2044'>pandas/core/groupby/groupby.py:2044:28:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L2444'>pandas/core/groupby/groupby.py:2444:13:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L2630'>pandas/core/groupby/groupby.py:2630:13:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L2739'>pandas/core/groupby/groupby.py:2739:13:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L2757'>pandas/core/groupby/groupby.py:2757:9:</a> PLR0914 Too many local variables (27/15)
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L2757'>pandas/core/groupby/groupby.py:2757:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L3134'>pandas/core/groupby/groupby.py:3134:13:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L3253'>pandas/core/groupby/groupby.py:3253:13:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L3321'>pandas/core/groupby/groupby.py:3321:13:</a> PLC0415 `import` should be at the top-level of a file
... 8 additional changes omitted for rule PLC0415
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L4280'>pandas/core/groupby/groupby.py:4280:26:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L4692'>pandas/core/groupby/groupby.py:4692:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L5071'>pandas/core/groupby/groupby.py:5071:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L5332'>pandas/core/groupby/groupby.py:5332:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L5387'>pandas/core/groupby/groupby.py:5387:31:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/pandas-dev/pandas/blob/73bc5f45a15969888edfb0e3c0ca45fdca2553e0/pandas/core/groupby/groupby.py#L5648'>pandas/core/groupby/groupby.py:5648:9:</a> PLR0917 Too many positional arguments (6/5)
... 2 additional changes omitted for rule PLR0917
</pre>

</p>
</details>
<details><summary>Changes by rule (8 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC0415 | 14 | 14 | 0 | 0 | 0 |
| PLR0917 | 8 | 8 | 0 | 0 | 0 |
| PLR6201 | 6 | 6 | 0 | 0 | 0 |
| B033 | 2 | 0 | 0 | 2 | 0 |
| PLR0904 | 1 | 1 | 0 | 0 | 0 |
| F841 | 1 | 1 | 0 | 0 | 0 |
| E721 | 1 | 1 | 0 | 0 | 0 |
| PLR0914 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_@charliermarsh reviewed on 2024-01-14 15:12_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/duplicate_value.rs`:79 on 2024-01-14 15:12_

I would prefer to do something similar to `remove_argument`, if possible, since the generator-based approach will lose comments. Are you open to trying that?

---

_@diceroll123 reviewed on 2024-01-14 17:27_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/duplicate_value.rs`:79 on 2024-01-14 17:27_

Seems like `remove_argument` is strictly for `ExprCall`s _and associated `Arguments`_, unsure if it'd work to cast everything over.

All usages of `remove_argument` come from actual `ExprCall`s, and we're working with a vector of `Expr` here from a set.

Please advise! 🙏🏽 

---

_@charliermarsh reviewed on 2024-01-14 17:31_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/duplicate_value.rs`:79 on 2024-01-14 17:31_

Oh sorry, I meant, like, writing a new method that removes an element from a comma-separated collections, but based on the token stream, similar to `remove_argument`.

---

_@diceroll123 reviewed on 2024-01-14 17:32_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/duplicate_value.rs`:79 on 2024-01-14 17:32_

Ahh. I'll see what I can do!

---

_Comment by @diceroll123 on 2024-01-14 18:38_

Tweaked accordingly. The new function is not special to this set implementation, and could be placed into the `fix::edits` crate for reuse, but that's up to you!

I feel like this rule could also be expanded to work on `Name`.

---

_@charliermarsh approved on 2024-01-14 23:09_

Thanks!

---

_Renamed from "add autofix for `B033`" to "[`flake8-bugbear`] Add fix for `duplicate-value` (`B033`)" by @charliermarsh on 2024-01-14 23:09_

---

_Comment by @diceroll123 on 2024-01-14 23:11_

Woop, left dead code in there *facepalm*

Thanks Charlie!

---

_Label `preview` added by @charliermarsh on 2024-01-14 23:14_

---

_Label `autofix` added by @charliermarsh on 2024-01-14 23:14_

---

_Comment by @charliermarsh on 2024-01-14 23:14_

No it's all good, I just realized that with the revised fix implementation, we could revert back to the previous structure!

---

_Comment by @charliermarsh on 2024-01-14 23:14_

Had to move this under preview since we're not supposed to add safe fixes outside of preview (this is gonna change in the next minor release though, and we'll elevate this out of preview).

---

_Merged by @charliermarsh on 2024-01-14 23:20_

---

_Closed by @charliermarsh on 2024-01-14 23:20_

---
