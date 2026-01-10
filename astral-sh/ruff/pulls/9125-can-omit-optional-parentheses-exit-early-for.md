```yaml
number: 9125
title: "`can_omit_optional_parentheses`: Exit early for unparenthesized expressions"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: can-omit-exit-early-for-expressions-without-parens
created_at: 2023-12-14T05:39:29Z
updated_at: 2023-12-14T06:02:54Z
url: https://github.com/astral-sh/ruff/pull/9125
synced_at: 2026-01-10T23:31:11Z
```

# `can_omit_optional_parentheses`: Exit early for unparenthesized expressions

---

_Pull request opened by @MichaReiser on 2023-12-14 05:39_

## Summary

The `can_omit_optional_parentheses` layout tries to avoid parenthesizing an expression if 
breaking any of it's sub-expressions that have their own parentheses make the expression fit. 

This PR moves the check whether an expression contains any parenthesized expression further up
to ensure we avoid the slightly more expensive layout when there are no other parentheses by which we can break.

## Test Plan

ecosystem check reports no changes


---

_Comment by @MichaReiser on 2023-12-14 05:39_

Current dependencies on/for this PR:
* `main`
  * **PR #9125** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9125?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/9125?utm_source=stack-comment).

---

_Label `internal` added by @MichaReiser on 2023-12-14 05:40_

---

_Label `formatter` added by @MichaReiser on 2023-12-14 05:40_

---

_Comment by @github-actions[bot] on 2023-12-14 05:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @MichaReiser on 2023-12-14 06:02_

---

_Merged by @MichaReiser on 2023-12-14 06:02_

---

_Closed by @MichaReiser on 2023-12-14 06:02_

---

_Branch deleted on 2023-12-14 06:02_

---
