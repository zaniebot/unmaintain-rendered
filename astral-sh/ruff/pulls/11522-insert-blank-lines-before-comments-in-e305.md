```yaml
number: 11522
title: Insert blank lines before comments in E305
type: pull_request
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
base: main
head: charlie/line
created_at: 2024-05-23T20:42:52Z
updated_at: 2024-05-27T17:05:16Z
url: https://github.com/astral-sh/ruff/pull/11522
synced_at: 2026-01-10T21:56:00Z
```

# Insert blank lines before comments in E305

---

_Pull request opened by @charliermarsh on 2024-05-23 20:42_

## Summary

Given:

```python
class A:
    pass

# ====== Cool constants ========
BANANA = 100
APPLE = 200
```

We now insert the blank line _before_ the comment, rather than inserting two blank lines _after_ the comment.

Closes https://github.com/astral-sh/ruff/issues/11508.


---

_Review requested from @MichaReiser by @charliermarsh on 2024-05-23 20:42_

---

_Review requested from @dhruvmanila by @charliermarsh on 2024-05-23 20:42_

---

_Review request for @MichaReiser removed by @charliermarsh on 2024-05-23 20:43_

---

_Label `bug` added by @charliermarsh on 2024-05-23 20:43_

---

_Comment by @github-actions[bot] on 2024-05-23 20:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2024-05-24 04:38_

---

_@dhruvmanila reviewed on 2024-05-24 04:39_

---

_Comment by @kaddkaka on 2024-05-24 07:38_

Why is ruff mistaken about what scope the comment belongs to? Should we be wary of more similar bugs with comments elsewhere (for example in implementation of other rules/fixes/formatting)?

---

_Comment by @dhruvmanila on 2024-05-24 07:40_

> Why is ruff mistaken about what scope the comment belongs to? Should we be wary of more similar bugs with comments elsewhere (for example in implementation of other rules/fixes/formatting)?

No, you don't need to worry about this elsewhere. It's probably just a logic error in this PR specifically.

---

_Comment by @charliermarsh on 2024-05-27 17:05_

Closing for now as I don't have time to fix the comment scoping issue. It's a shame because we already implement this very robustly in the formatter!

---

_Closed by @charliermarsh on 2024-05-27 17:05_

---
