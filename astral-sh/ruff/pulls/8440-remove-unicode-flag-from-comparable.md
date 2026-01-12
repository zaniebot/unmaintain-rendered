```yaml
number: 8440
title: Remove unicode flag from comparable
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/comparable
created_at: 2023-11-02T05:57:28Z
updated_at: 2023-11-02T07:52:53Z
url: https://github.com/astral-sh/ruff/pull/8440
synced_at: 2026-01-10T23:40:55Z
```

# Remove unicode flag from comparable

---

_Pull request opened by @dhruvmanila on 2023-11-02 05:57_

## Summary

This PR removes the `unicode` flag from the string literal in `ComparableExpr`. This flag isn't required as all strings are unicode in Python 3 so `"foo" == u"foo"`.


---

_Label `internal` added by @dhruvmanila on 2023-11-02 05:57_

---

_Review requested from @charliermarsh by @dhruvmanila on 2023-11-02 05:57_

---

_@MichaReiser approved on 2023-11-02 06:08_

---

_Comment by @MichaReiser on 2023-11-02 06:08_

Let's hear ecosytem's opinion on that

---

_Comment by @github-actions[bot] on 2023-11-02 06:15_

## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -18 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -18 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/ba4322431d1261fa7f4203aafad74621d6f8bc72/pandas/core/reshape/merge.py#L1196'>pandas/core/reshape/merge.py:1196:24:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/ba4322431d1261fa7f4203aafad74621d6f8bc72/pandas/core/reshape/merge.py#L1611'>pandas/core/reshape/merge.py:1611:24:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/ba4322431d1261fa7f4203aafad74621d6f8bc72/pandas/core/reshape/merge.py#L1626'>pandas/core/reshape/merge.py:1626:26:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/ba4322431d1261fa7f4203aafad74621d6f8bc72/pandas/core/reshape/merge.py#L1632'>pandas/core/reshape/merge.py:1632:26:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/ba4322431d1261fa7f4203aafad74621d6f8bc72/pandas/core/reshape/merge.py#L1639'>pandas/core/reshape/merge.py:1639:26:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/ba4322431d1261fa7f4203aafad74621d6f8bc72/pandas/core/reshape/merge.py#L1687'>pandas/core/reshape/merge.py:1687:19:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/ba4322431d1261fa7f4203aafad74621d6f8bc72/pandas/core/reshape/merge.py#L1689'>pandas/core/reshape/merge.py:1689:34:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/ba4322431d1261fa7f4203aafad74621d6f8bc72/pandas/core/reshape/merge.py#L1692'>pandas/core/reshape/merge.py:1692:19:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/ba4322431d1261fa7f4203aafad74621d6f8bc72/pandas/core/reshape/merge.py#L1694'>pandas/core/reshape/merge.py:1694:34:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/ba4322431d1261fa7f4203aafad74621d6f8bc72/pandas/core/reshape/merge.py#L1718'>pandas/core/reshape/merge.py:1718:15:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/ba4322431d1261fa7f4203aafad74621d6f8bc72/pandas/core/reshape/merge.py#L1912'>pandas/core/reshape/merge.py:1912:34:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/ba4322431d1261fa7f4203aafad74621d6f8bc72/pandas/core/reshape/merge.py#L2093'>pandas/core/reshape/merge.py:2093:9:</a> PLR6301 Method `_convert_values_for_libjoin` could be a function, class method, or static method
- <a href='https://github.com/pandas-dev/pandas/blob/ba4322431d1261fa7f4203aafad74621d6f8bc72/pandas/core/reshape/merge.py#L2394'>pandas/core/reshape/merge.py:2394:37:</a> PLR6201 Use a `set` literal when testing for membership
- <a href='https://github.com/pandas-dev/pandas/blob/ba4322431d1261fa7f4203aafad74621d6f8bc72/pandas/core/reshape/merge.py#L2396'>pandas/core/reshape/merge.py:2396:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/pandas-dev/pandas/blob/ba4322431d1261fa7f4203aafad74621d6f8bc72/pandas/core/reshape/merge.py#L2397'>pandas/core/reshape/merge.py:2397:13:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/pandas-dev/pandas/blob/ba4322431d1261fa7f4203aafad74621d6f8bc72/pandas/core/reshape/merge.py#L293'>pandas/core/reshape/merge.py:293:5:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/pandas-dev/pandas/blob/ba4322431d1261fa7f4203aafad74621d6f8bc72/pandas/core/reshape/merge.py#L426'>pandas/core/reshape/merge.py:426:62:</a> PLW0108 Lambda may be unnecessary; consider inlining inner function
- <a href='https://github.com/pandas-dev/pandas/blob/ba4322431d1261fa7f4203aafad74621d6f8bc72/pandas/core/reshape/merge.py#L876'>pandas/core/reshape/merge.py:876:9:</a> PLC0415 `import` should be at the top-level of a file
</pre>

</p>
</details>
<details><summary>Changes by rule (4 rules affected)</summary>
<p>

</p>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR6201 | 12 | 0 | 12 | 0 | 0 |
| PLC0415 | 4 | 0 | 4 | 0 | 0 |
| PLR6301 | 1 | 0 | 1 | 0 | 0 |
| PLW0108 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @dhruvmanila on 2023-11-02 06:17_

The results doesn't seem related but let me rebase.

---

_Comment by @dhruvmanila on 2023-11-02 06:18_

Current dependencies on/for this PR:
* main
  * **PR #8440** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8440" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8440?utm_source=stack-comment).

---

_Comment by @github-actions[bot] on 2023-11-02 06:47_

## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @dhruvmanila on 2023-11-02 07:51_

---

_Closed by @dhruvmanila on 2023-11-02 07:51_

---

_Branch deleted on 2023-11-02 07:51_

---

_Comment by @dhruvmanila on 2023-11-02 07:52_

\cc @zanieb Shouldn't the ecosystem check update the existing comment?

---
