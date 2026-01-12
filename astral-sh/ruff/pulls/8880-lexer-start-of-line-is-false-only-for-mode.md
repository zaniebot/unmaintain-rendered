```yaml
number: 8880
title: "Lexer start of line is false only for `Mode::Expression`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: dhruv/soft-keyword
created_at: 2023-11-28T20:27:21Z
updated_at: 2023-11-28T20:48:29Z
url: https://github.com/astral-sh/ruff/pull/8880
synced_at: 2026-01-12T15:55:27Z
```

# Lexer start of line is false only for `Mode::Expression`

---

_@dhruvmanila_

## Summary

This PR fixes the bug in the lexer where the `Mode::Ipython` wasn't being considered when initializing the soft keyword transformer which wraps the lexer. This means that if the source code starts with either `match` or `type` keyword, then the keywords were being considered as name tokens instead. For example,

```python
match foo:
    case bar:
        pass
```

This would transform the `match` keyword into an identifier if the mode is `Ipython`.

The fix is to reverse the condition in the soft keyword initializer so that any new modes are by default considered as the lexer being at start of line.

## Test Plan

Add a new test case for `Mode::Ipython` and verify the snapshot.

fixes: #8870 


---

_Comment by @dhruvmanila on 2023-11-28 20:27_

Current dependencies on/for this PR:
* main
  * **PR #8880** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8880?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8880?utm_source=stack-comment).

---

_Label `bug` added by @dhruvmanila on 2023-11-28 20:32_

---

_Label `parser` added by @dhruvmanila on 2023-11-28 20:32_

---

_@charliermarsh approved on 2023-11-28 20:33_

Wow, that's devious

---

_Merged by @dhruvmanila on 2023-11-28 20:38_

---

_Closed by @dhruvmanila on 2023-11-28 20:38_

---

_Branch deleted on 2023-11-28 20:38_

---

_Comment by @github-actions[bot] on 2023-11-28 20:48_

<!-- generated-comment ecosystem -->
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
