```yaml
number: 13572
title: "Update `dedent_to` to support blocks that are composed of comments"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/dedent-comment-block
created_at: 2024-09-30T16:16:33Z
updated_at: 2024-10-01T04:52:09Z
url: https://github.com/astral-sh/ruff/pull/13572
synced_at: 2026-01-12T15:55:44Z
```

# Update `dedent_to` to support blocks that are composed of comments

---

_@zanieb_

While looking into https://github.com/astral-sh/ruff/issues/13545 I noticed that we return `None` here if you pass a block of comments. This is annoying because it causes `adjust_indentation` to fall back to LibCST which panics when it cannot find a statement.

---

_Label `internal` added by @zanieb on 2024-09-30 16:16_

---

_Comment by @github-actions[bot] on 2024-09-30 16:35_

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

_Review requested from @charliermarsh by @zanieb on 2024-09-30 19:39_

---

_Review requested from @dhruvmanila by @zanieb on 2024-09-30 19:39_

---

_@dhruvmanila approved on 2024-10-01 03:07_

Thanks! Can you update the `adjust_indent` test case at the bottom to include this case?

---

_Comment by @dhruvmanila on 2024-10-01 03:13_

> This is annoying because it causes `adjust_indentation` to fall back to LibCST which panics when it cannot find a statement.

Where does it panic though? I see that the block is being added inside a dummy function body. Does it panic in `match_indented_block`?

---

_Comment by @zanieb on 2024-10-01 04:36_

`dedent_to` returns `None` and we fail at `match_statement` — it fails with `Failed to create fix for CollapsibleElseIf: Failed to extract statement from source`

---

_Merged by @zanieb on 2024-10-01 04:38_

---

_Closed by @zanieb on 2024-10-01 04:38_

---

_Branch deleted on 2024-10-01 04:38_

---

_Comment by @dhruvmanila on 2024-10-01 04:50_

> `dedent_to` returns `None` and we fail at `match_statement` — it fails with `Failed to create fix for CollapsibleElseIf: Failed to extract statement from source`

Ah right, that's because the following is invalid syntax:
```py
def f():
	# comment
	# comment
```

---
