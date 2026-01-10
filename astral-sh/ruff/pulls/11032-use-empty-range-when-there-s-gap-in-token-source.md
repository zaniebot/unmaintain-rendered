```yaml
number: 11032
title: "Use empty range when there's \"gap\" in token source"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: dhruv/node-range
created_at: 2024-04-19T10:09:37Z
updated_at: 2024-04-19T11:46:28Z
url: https://github.com/astral-sh/ruff/pull/11032
synced_at: 2026-01-10T22:37:01Z
```

# Use empty range when there's "gap" in token source

---

_Pull request opened by @dhruvmanila on 2024-04-19 10:09_

## Summary

This fixes a bug where the parser would panic when there is a "gap" in
the token source.

What's a gap?

The reason it's `<=` instead of just `==` is because there could be whitespaces between
the two tokens. For example:

```python
#     last token end
#     | current token (newline) start
#     v v
def foo \n
#      ^
#      assume there's trailing whitespace here
```

Or, there could tokens that are considered "trivia" and thus aren't emitted by the token
source. These are comments and non-logical newlines. For example:

```python
#     last token end
#     v
def foo # comment\n
#                ^ current token (newline) start
```

In either of the above cases, there's a "gap" between the end of the last token and start
of the current token.

## Test Plan

Add test cases and update the snapshots.


---

_Label `bug` added by @dhruvmanila on 2024-04-19 10:09_

---

_Label `parser` added by @dhruvmanila on 2024-04-19 10:09_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-19 10:09_

---

_Comment by @github-actions[bot] on 2024-04-19 10:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/ce049d505f1960e95a47f24cff29d15ce3cadb0b/stdlib/pathlib.pyi#L111'>stdlib/pathlib.pyi:111:89:</a> E999 SyntaxError: Expected ':', found newline
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E999 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/mod.rs`:268 on 2024-04-19 11:02_

Nit: i think this is specific to error recovery and instead is true for all "empty" nodes that don't consist of any tokens?

If that's the case, then I think we can remove the `missing_node_range` method that was only added to handle empty node ranges.

---

_@MichaReiser approved on 2024-04-19 11:04_

---

_@dhruvmanila reviewed on 2024-04-19 11:28_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/mod.rs`:268 on 2024-04-19 11:28_

I think `missing_node_range` is still useful in places where you don't have the `start` value or rather the start value itself is the current token start.

---

_Merged by @dhruvmanila on 2024-04-19 11:36_

---

_Closed by @dhruvmanila on 2024-04-19 11:36_

---

_Branch deleted on 2024-04-19 11:36_

---
