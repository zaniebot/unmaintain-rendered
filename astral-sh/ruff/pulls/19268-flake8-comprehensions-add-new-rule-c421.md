```yaml
number: 19268
title: "[flake8-comprehensions] add new rule C421 - unnecessary literal within `set` call"
type: pull_request
state: open
author: bluetech
labels:
  - rule
  - needs-decision
assignees: []
base: main
head: unnecessary-literal-within-set-call
created_at: 2025-07-10T17:22:57Z
updated_at: 2025-07-11T20:35:13Z
url: https://github.com/astral-sh/ruff/pull/19268
synced_at: 2026-01-10T18:33:12Z
```

# [flake8-comprehensions] add new rule C421 - unnecessary literal within `set` call

---

_Pull request opened by @bluetech on 2025-07-10 17:22_

This corresponds to the existing similar rules for tuple, list and dict. The code closely follows unnecessary-literal-within-dict-call (C418).

Fix #19267.

---

_Comment by @github-actions[bot] on 2025-07-10 17:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/e635c3e4c8bc817fb33c3005649648b9d7c22599/pandas/core/resample.py#L146'>pandas/core/resample.py:146:27:</a> C421 Unnecessary set literal passed to `set()` (remove the outer call to `set()`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/5b5eee30a9daa722b4b5449b3860882fb067e8ec/rotkehlchen/db/upgrades/v46_v47.py#L154'>rotkehlchen/db/upgrades/v46_v47.py:154:46:</a> C421 Unnecessary set comprehension passed to `set()` (remove the outer call to `set()`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| C421 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/e635c3e4c8bc817fb33c3005649648b9d7c22599/pandas/core/resample.py#L146'>pandas/core/resample.py:146:27:</a> C421 Unnecessary set literal passed to `set()` (remove the outer call to `set()`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/5b5eee30a9daa722b4b5449b3860882fb067e8ec/rotkehlchen/db/upgrades/v46_v47.py#L154'>rotkehlchen/db/upgrades/v46_v47.py:154:46:</a> C421 Unnecessary set comprehension passed to `set()` (remove the outer call to `set()`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| C421 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @ntBre on 2025-07-10 18:02_

---

_Label `needs-decision` added by @ntBre on 2025-07-10 18:02_

---

_Comment by @tdulcet on 2025-07-11 16:37_

Maybe check for `frozenset` as well:
```py
frozenset({1, 2, 3})               # -> frozenset((1, 2, 3))
frozenset({x for x in range(10)})  # -> frozenset(x for x in range(10))
```

---

_Comment by @bluetech on 2025-07-11 20:35_

@tdulcet Stylistically your suggestion is shorter, though logically I wouldn't want to warn about it. It makes some sense to me to pass a set to `frozenset`; if I see it wouldn't remove the curlies. I also did a quick timeit benchmark on Python 3.13 and the set literal version seems a bit quicker

---
