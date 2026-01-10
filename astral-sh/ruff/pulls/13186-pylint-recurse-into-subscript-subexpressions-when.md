```yaml
number: 13186
title: "[`pylint`] Recurse into subscript subexpressions when searching for list/dict lookups (`PLR1733`, `PLR1736`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: alex/index-lookups
created_at: 2024-08-31T18:22:55Z
updated_at: 2024-09-01T16:22:46Z
url: https://github.com/astral-sh/ruff/pull/13186
synced_at: 2026-01-10T21:38:32Z
```

# [`pylint`] Recurse into subscript subexpressions when searching for list/dict lookups (`PLR1733`, `PLR1736`)

---

_Pull request opened by @AlexWaygood on 2024-08-31 18:22_

## Summary

The `SequenceIndexVisitor` currently does not recurse into subexpressions of subscripts when searching for subscript accesses that would trigger this rule. That means that we don't currently detect violations of the rule on snippets like this:

```py
data = {"a": 1, "b": 2}
column_names = ["a", "b"]
for index, column_name in enumerate(column_names):
    _ = data[column_names[index]]
```

Fixes #13183

## Test Plan

`cargo test -p ruff_linter`


---

_Label `bug` added by @AlexWaygood on 2024-08-31 18:22_

---

_Label `rule` added by @AlexWaygood on 2024-08-31 18:22_

---

_Renamed from "[`pylint`] Recurse into subexpressions of subscripts when searching for `unnecessary-list-index-lookup` violations (`PLR1736`)" to "[`pylint`] Recurse into subscript subexpressions when searching for `unnecessary-list-index-lookup` violations (`PLR1736`)" by @AlexWaygood on 2024-08-31 18:23_

---

_Comment by @github-actions[bot] on 2024-08-31 18:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/3da91e951cd03cfa0b9c67378224e348353f36a6/analytics/views/stats.py#L623'>analytics/views/stats.py:623:51:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
+ <a href='https://github.com/zulip/zulip/blob/3da91e951cd03cfa0b9c67378224e348353f36a6/analytics/views/stats.py#L625'>analytics/views/stats.py:625:44:</a> PLR1733 [*] Unnecessary lookup of dictionary value by key
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1733 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @AlexWaygood on 2024-08-31 18:48_

> ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)
> [zulip/zulip](https://github.com/zulip/zulip) (+2 -0 violations, +0 -0 fixes)

Nice, it looks like this also fixes false negatives in other rules that use this visitor

---

_Renamed from "[`pylint`] Recurse into subscript subexpressions when searching for `unnecessary-list-index-lookup` violations (`PLR1736`)" to "[`pylint`] Recurse into subscript subexpressions when searching for listd/dict lookups (`PLR1733`, `PLR1736`)" by @AlexWaygood on 2024-08-31 18:56_

---

_Renamed from "[`pylint`] Recurse into subscript subexpressions when searching for listd/dict lookups (`PLR1733`, `PLR1736`)" to "[`pylint`] Recurse into subscript subexpressions when searching for list/dict lookups (`PLR1733`, `PLR1736`)" by @AlexWaygood on 2024-08-31 18:56_

---

_Comment by @sbrugman on 2024-08-31 19:32_

LGTM. Thanks for the quick PR :)

---

_@charliermarsh approved on 2024-09-01 16:21_

---

_Merged by @AlexWaygood on 2024-09-01 16:22_

---

_Closed by @AlexWaygood on 2024-09-01 16:22_

---

_Branch deleted on 2024-09-01 16:22_

---
