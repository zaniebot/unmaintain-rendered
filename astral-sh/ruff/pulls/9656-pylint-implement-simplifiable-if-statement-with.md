```yaml
number: 9656
title: "[`pylint`] - implement `simplifiable-if-statement` with fix (`PLR1703`)"
type: pull_request
state: closed
author: diceroll123
labels:
  - accepted
assignees: []
base: main
head: add-PLR1703
created_at: 2024-01-27T07:08:06Z
updated_at: 2024-04-07T03:00:25Z
url: https://github.com/astral-sh/ruff/pull/9656
synced_at: 2026-01-12T15:55:29Z
```

# [`pylint`] - implement `simplifiable-if-statement` with fix (`PLR1703`)

---

_@diceroll123_

## Summary

Add rule + fix for [`simplifiable-if-statement` (`PLR1703`)](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/simplifiable-if-statement.html)

See: #970 

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2024-01-27 07:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+31 -28 violations, +0 -0 fixes in 3 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/7eab3b8d3efa03c893e4dcf548584d5e3b5d2f0a/securedrop/journalist_app/admin.py#L49'>securedrop/journalist_app/admin.py:49:9:</a> PLR1703 [*] Simplifiable if-statement
+ <a href='https://github.com/freedomofpress/securedrop/blob/7eab3b8d3efa03c893e4dcf548584d5e3b5d2f0a/securedrop/models.py#L148'>securedrop/models.py:148:9:</a> PLR1703 [*] Simplifiable if-statement
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+28 -28 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/core/reshape/concat.py#L400'>pandas/core/reshape/concat.py:400:9:</a> PLR0917 Too many positional arguments (10/5)
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/core/reshape/concat.py#L403'>pandas/core/reshape/concat.py:403:9:</a> PLR0917 Too many positional arguments (10/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/core/reshape/concat.py#L448'>pandas/core/reshape/concat.py:448:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/core/reshape/concat.py#L451'>pandas/core/reshape/concat.py:451:13:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/core/reshape/concat.py#L475'>pandas/core/reshape/concat.py:475:9:</a> PLR6301 Method `_get_ndims` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/core/reshape/concat.py#L478'>pandas/core/reshape/concat.py:478:9:</a> PLR6301 Method `_get_ndims` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/core/reshape/concat.py#L489'>pandas/core/reshape/concat.py:489:9:</a> PLR6301 Method `_clean_keys_and_objs` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/core/reshape/concat.py#L492'>pandas/core/reshape/concat.py:492:9:</a> PLR6301 Method `_clean_keys_and_objs` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/core/reshape/concat.py#L574'>pandas/core/reshape/concat.py:574:9:</a> PLR6301 Method `_sanitize_mixed_ndim` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/core/reshape/concat.py#L577'>pandas/core/reshape/concat.py:577:9:</a> PLR6301 Method `_sanitize_mixed_ndim` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/core/reshape/concat.py#L617'>pandas/core/reshape/concat.py:617:9:</a> PLR0914 Too many local variables (17/15)
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/core/reshape/concat.py#L620'>pandas/core/reshape/concat.py:620:9:</a> PLR0914 Too many local variables (17/15)
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/core/reshape/concat.py#L776'>pandas/core/reshape/concat.py:776:5:</a> PLR0914 Too many local variables (21/15)
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/core/reshape/concat.py#L779'>pandas/core/reshape/concat.py:779:5:</a> PLR0914 Too many local variables (21/15)
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1208'>pandas/plotting/_matplotlib/core.py:1208:9:</a> PLR6301 Method `_get_subplots` could be a function, class method, or static method
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1210'>pandas/plotting/_matplotlib/core.py:1210:13:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1212'>pandas/plotting/_matplotlib/core.py:1212:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1213'>pandas/plotting/_matplotlib/core.py:1213:9:</a> PLR6301 Method `_get_subplots` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1215'>pandas/plotting/_matplotlib/core.py:1215:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1217'>pandas/plotting/_matplotlib/core.py:1217:13:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1253'>pandas/plotting/_matplotlib/core.py:1253:9:</a> PLR6301 Method `_get_nseries` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1258'>pandas/plotting/_matplotlib/core.py:1258:9:</a> PLR6301 Method `_get_nseries` could be a function, class method, or static method
... 7 additional changes omitted for rule PLR6301
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1320'>pandas/plotting/_matplotlib/core.py:1320:9:</a> PLR0914 Too many local variables (20/15)
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1325'>pandas/plotting/_matplotlib/core.py:1325:9:</a> PLR0914 Too many local variables (20/15)
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1402'>pandas/plotting/_matplotlib/core.py:1402:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1407'>pandas/plotting/_matplotlib/core.py:1407:13:</a> PLC0415 `import` should be at the top-level of a file
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1477'>pandas/plotting/_matplotlib/core.py:1477:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1482'>pandas/plotting/_matplotlib/core.py:1482:9:</a> PLC0415 `import` should be at the top-level of a file
... 5 additional changes omitted for rule PLC0415
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1495'>pandas/plotting/_matplotlib/core.py:1495:9:</a> PLR0914 Too many local variables (16/15)
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1500'>pandas/plotting/_matplotlib/core.py:1500:9:</a> PLR0914 Too many local variables (16/15)
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1559'>pandas/plotting/_matplotlib/core.py:1559:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1564'>pandas/plotting/_matplotlib/core.py:1564:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1734'>pandas/plotting/_matplotlib/core.py:1734:9:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1739'>pandas/plotting/_matplotlib/core.py:1739:9:</a> PLR0917 Too many positional arguments (7/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1862'>pandas/plotting/_matplotlib/core.py:1862:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L1867'>pandas/plotting/_matplotlib/core.py:1867:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L2002'>pandas/plotting/_matplotlib/core.py:2002:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L2007'>pandas/plotting/_matplotlib/core.py:2007:9:</a> PLR0917 Too many positional arguments (6/5)
... 1 additional changes omitted for rule PLR0917
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L435'>pandas/plotting/_matplotlib/core.py:435:77:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L440'>pandas/plotting/_matplotlib/core.py:440:77:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L653'>pandas/plotting/_matplotlib/core.py:653:28:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L658'>pandas/plotting/_matplotlib/core.py:658:28:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L921'>pandas/plotting/_matplotlib/core.py:921:46:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/plotting/_matplotlib/core.py#L926'>pandas/plotting/_matplotlib/core.py:926:46:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/tests/groupby/aggregate/test_cython.py#L345'>pandas/tests/groupby/aggregate/test_cython.py:345:19:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/pandas-dev/pandas/blob/9008ee5810c09bc907b5fdc36fc3c1dff4a50c55/pandas/tests/groupby/aggregate/test_cython.py#L345'>pandas/tests/groupby/aggregate/test_cython.py:345:38:</a> PLR6201 Use a `set` literal when testing for membership
... 10 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/af6a30db7e5d566a2ba817738b7febc70393d4a3/zproject/computed_settings.py#L97'>zproject/computed_settings.py:97:1:</a> PLR1703 [*] Simplifiable if-statement
</pre>

</p>
</details>
<details><summary>Changes by rule (6 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR6301 | 16 | 8 | 8 | 0 | 0 |
| PLC0415 | 14 | 7 | 7 | 0 | 0 |
| PLR0917 | 10 | 5 | 5 | 0 | 0 |
| PLR0914 | 8 | 4 | 4 | 0 | 0 |
| PLR6201 | 8 | 4 | 4 | 0 | 0 |
| PLR1703 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @Skylion007 on 2024-01-27 16:12_

I think this is actually a duplicate of SIM210 which is already implemented @diceroll123 https://docs.astral.sh/ruff/rules/if-expr-with-true-false/ 

---

_Label `accepted` added by @MichaReiser on 2024-04-05 10:18_

---

_Comment by @charliermarsh on 2024-04-07 03:00_

Unfortunately I think this is a duplicate of [`if-else-block-instead-of-if-exp`](https://docs.astral.sh/ruff/rules/if-else-block-instead-of-if-exp/) based on playing around in the playground and looking through the examples, so I'm going to close for now. Sorry about that.

---

_Closed by @charliermarsh on 2024-04-07 03:00_

---
