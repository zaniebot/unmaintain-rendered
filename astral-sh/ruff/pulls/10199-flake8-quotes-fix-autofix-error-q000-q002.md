```yaml
number: 10199
title: "[`flake8-quotes`] Fix Autofix Error (`Q000, Q002`)"
type: pull_request
state: merged
author: robincaloudis
labels: []
assignees: []
merged: true
base: main
head: rc-fix-q000-and-q002
created_at: 2024-03-02T17:10:10Z
updated_at: 2024-03-18T01:37:12Z
url: https://github.com/astral-sh/ruff/pull/10199
synced_at: 2026-01-10T22:47:01Z
```

# [`flake8-quotes`] Fix Autofix Error (`Q000, Q002`)

---

_Pull request opened by @robincaloudis on 2024-03-02 17:10_

## Summary
In issue https://github.com/astral-sh/ruff/issues/6785 it is reported that a docstring in the form of `''"assert" ' SAM macro definitions '''` is autocorrected to `"""assert" ' SAM macro definitions '''` (note the triple quotes one only one side), which breaks the python program due `undetermined string lateral`.

* `Q002`: Not only would docstrings in the form of `''"assert" ' SAM macro definitions '''` (single quotes) be autofixed wrongly, but also e.g. `""'assert' ' SAM macro definitions '''` (double quotes). The bug is present for docstrings in all scopes (e.g. module docstrings, class docstrings, function docstrings)

* `Q000`: The autofix error is not only present for `Q002` (docstrings), but also for inline strings (`Q000`). Therefore `s = ''"assert" ' SAM macro definitions '''` will also be wrongly autofixed.

Note that situation in which the first string is non-empty can be fixed, e.g. `'123'"assert" ' SAM macro definitions '''` -> `"123""assert" ' SAM macro definitions '''` is valid. 

## What
* Change FixAvailability of `Q000` `Q002` to `Sometimes`
* Changed both rules such that docstrings/inline strings that cannot be fixed are still reported as bad quotes via diagnostics, but no fix is provided

## Test Plan
* For `Q000`: Add docstrings in different scopes that (partially) would have been autofixed wrongly
* For `Q002`: Add inline strings that (partially) would have been autofixed wrongly

Closes https://github.com/astral-sh/ruff/issues/6785

---

_Comment by @github-actions[bot] on 2024-03-02 17:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -37 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -37 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L1085'>pandas/io/stata.py:1085:9:</a> PLR0917 Too many positional arguments (11/5)
- <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L1236'>pandas/io/stata.py:1236:40:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L1395'>pandas/io/stata.py:1395:40:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L1615'>pandas/io/stata.py:1615:9:</a> PLR0914 Too many local variables (20/15)
- <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L1615'>pandas/io/stata.py:1615:9:</a> PLR0917 Too many positional arguments (8/5)
- <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L1720'>pandas/io/stata.py:1720:29:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L1742'>pandas/io/stata.py:1742:29:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L1745'>pandas/io/stata.py:1745:31:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L1791'>pandas/io/stata.py:1791:33:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L2058'>pandas/io/stata.py:2058:30:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L2060'>pandas/io/stata.py:2060:32:</a> PLR6201 Use a `set` literal when testing for membership
... 18 additional changes omitted for rule PLR6201
- <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L2281'>pandas/io/stata.py:2281:9:</a> PLR0917 Too many positional arguments (10/5)
- <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L2426'>pandas/io/stata.py:2426:9:</a> PLR6301 Method `_validate_variable_name` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L2448'>pandas/io/stata.py:2448:17:</a> PLR0916 Too many Boolean expressions (7 > 5)
- <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L2866'>pandas/io/stata.py:2866:9:</a> PLR6301 Method `_convert_strls` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L3244'>pandas/io/stata.py:3244:9:</a> PLR0917 Too many positional arguments (11/5)
- <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L3637'>pandas/io/stata.py:3637:9:</a> PLR0917 Too many positional arguments (12/5)
- <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L3681'>pandas/io/stata.py:3681:9:</a> PLR6301 Method `_validate_variable_name` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L3704'>pandas/io/stata.py:3704:17:</a> PLR0916 Too many Boolean expressions (10 > 5)
- <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L787'>pandas/io/stata.py:787:7:</a> PLW1641 Object does not implement `__hash__` method
+ <a href='https://github.com/pandas-dev/pandas/blob/89898a689c8309ee8c06796b215d606234aad69f/pandas/io/stata.py#L864'>pandas/io/stata.py:864:16:</a> E999 SyntaxError: Unexpected token Newline
</pre>

</p>
</details>
<details><summary>Changes by rule (7 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR6201 | 25 | 0 | 25 | 0 | 0 |
| PLR0917 | 5 | 0 | 5 | 0 | 0 |
| PLR6301 | 3 | 0 | 3 | 0 | 0 |
| PLR0916 | 2 | 0 | 2 | 0 | 0 |
| E999 | 1 | 1 | 0 | 0 | 0 |
| PLR0914 | 1 | 0 | 1 | 0 | 0 |
| PLW1641 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Comment by @codspeed-hq[bot] on 2024-03-03 12:41_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/robincaloudis:rc-fix-q000-and-q002)

### Merging #10199 will **not alter performance**

<sub>Comparing <code>robincaloudis:rc-fix-q000-and-q002</code> (f4da30c) with <code>main</code> (fd26b29)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @robincaloudis on 2024-03-16 16:57_

@charliermarsh, do you mind to review this fix? Thanks!

---

_Renamed from "WIP: [`flake8-quotes`] Fix Autofix Error (`Q000, Q002`)" to "[`flake8-quotes`] Fix Autofix Error (`Q000, Q002`)" by @robincaloudis on 2024-03-17 14:24_

---

_Assigned to @zanieb by @zanieb on 2024-03-17 20:49_

---

_@charliermarsh approved on 2024-03-18 01:12_

This looks good, thanks! Appreciate all the additional tests. Sorry it took me so long to get to it.

---

_Merged by @charliermarsh on 2024-03-18 01:31_

---

_Closed by @charliermarsh on 2024-03-18 01:31_

---
