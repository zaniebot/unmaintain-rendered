```yaml
number: 10296
title: Refactor with statement formatting to have explicit layouts
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: refactor-with-item-formatting
created_at: 2024-03-08T10:55:59Z
updated_at: 2024-03-08T23:40:40Z
url: https://github.com/astral-sh/ruff/pull/10296
synced_at: 2026-01-12T15:55:31Z
```

# Refactor with statement formatting to have explicit layouts

---

_@MichaReiser_

## Summary

This PR refactors the with item formatting to use more explicit layouts to make it easier to understand the different formatting cases.

The benefit of the explicit layout is that it makes it easier to reasons about layout transition between format runs. For example, today it's possible that `SingleWithTarget` or `ParenthesizeIfExpands` add parentheses around the with items for `with aaaaaaaaaa + bbbbbbbbbbbb: pass`, resulting in `with (aaaaaaaaaa + bbbbbbbbbbbb): pass`. The problem with this is that the next formatting pass uses the `SingleParenthesizedContextExpression` layout that uses `maybe_parenthesize_expression` which is different from `parenthesize_if_expands(&expr)` or `optional_parentheses(&expr)`. 

## Test Plan

`cargo test`

I ran the ecosystem checks locally and there are no changes.


---

_Label `internal` added by @MichaReiser on 2024-03-08 10:56_

---

_Label `formatter` added by @MichaReiser on 2024-03-08 10:56_

---

_@MichaReiser reviewed on 2024-03-08 10:57_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/builders.rs`:222 on 2024-03-08 10:57_

I noticed that we pass both options and `context`. This is unnecessary because the options can be retrieved from `context.options()`

---

_Comment by @github-actions[bot] on 2024-03-08 11:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@MichaReiser reviewed on 2024-03-08 11:10_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_with.rs`:166 on 2024-03-08 11:10_

The name here doesn't make sense as of this PR because it could as well be a with item without a target. This gets changed in https://github.com/astral-sh/ruff/pull/10274, and I didn't want to come up with new terminology just for the state between the two PRs.

---

_Renamed from "Refactor stmt_with formatting to have explicit layouts" to "Refactor with statement formatting to have explicit layouts" by @MichaReiser on 2024-03-08 16:12_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-03-08 16:12_

---

_@charliermarsh approved on 2024-03-08 23:39_

---

_Merged by @charliermarsh on 2024-03-08 23:40_

---

_Closed by @charliermarsh on 2024-03-08 23:40_

---

_Branch deleted on 2024-03-08 23:40_

---
