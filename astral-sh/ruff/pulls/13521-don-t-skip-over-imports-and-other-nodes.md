```yaml
number: 13521
title: "Don't skip over imports and other nodes containing nested statements in import collector"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
assignees: []
merged: true
base: main
head: micha/fix-import-collector-skip
created_at: 2024-09-26T09:53:29Z
updated_at: 2024-09-26T12:06:24Z
url: https://github.com/astral-sh/ruff/pull/13521
synced_at: 2026-01-10T20:59:36Z
```

# Don't skip over imports and other nodes containing nested statements in import collector

---

_Pull request opened by @MichaReiser on 2024-09-26 09:53_

## Summary

This is a follow up for https://github.com/astral-sh/ruff/pull/13441/

`enter_node` is called for every node and it's important that it returns `true` for any node between `ModModule` and an import statement, including the import statement itself. 

Getting this right in `enter_node` seems hard and it's easy to forget adding a new "include" when a new compound statement gets added. 

This PR moves the traversal logic into the `visist_stmt` by requiring explicit match arms for each statement. I think this is easier to understand
and less error prone.


Also... `enter_node` doesn't get call when a visitor overrides `visit_stmt`. That's an oversight from my side when designing the visitor. 

---

_Label `bug` added by @MichaReiser on 2024-09-26 09:53_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-09-26 09:53_

---

_Comment by @github-actions[bot] on 2024-09-26 10:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-09-26 10:54_

Let me add a test for this

---

_@charliermarsh approved on 2024-09-26 11:37_

---

_Merged by @MichaReiser on 2024-09-26 11:57_

---

_Closed by @MichaReiser on 2024-09-26 11:57_

---

_Branch deleted on 2024-09-26 11:57_

---
