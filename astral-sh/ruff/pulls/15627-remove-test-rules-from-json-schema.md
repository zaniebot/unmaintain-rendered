```yaml
number: 15627
title: Remove test rules from JSON schema
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/test-rules
created_at: 2025-01-21T03:24:00Z
updated_at: 2025-01-24T04:48:01Z
url: https://github.com/astral-sh/ruff/pull/15627
synced_at: 2026-01-12T15:55:51Z
```

# Remove test rules from JSON schema

---

_@dhruvmanila_

Closes: #15707

---

_Label `internal` added by @dhruvmanila on 2025-01-21 03:24_

---

_Comment by @github-actions[bot] on 2025-01-21 03:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-01-21 07:15_

I'm not sure this is the right fix but I can see how any other fix is annoying. 

The reason why I don't think we should enable `testing` in `ruff_dev` is because users can't select the `testing` rules and, therefore, they shouldn't be in the json schema. If they currently are, then that's a bug and we should remove them. 

---

_Comment by @dhruvmanila on 2025-01-21 07:50_

> I'm not sure this is the right fix but I can see how any other fix is annoying.
> 
> The reason why I don't think we should enable `testing` in `ruff_dev` is because users can't select the `testing` rules and, therefore, they shouldn't be in the json schema. If they currently are, then that's a bug and we should remove them.

Oh, right. That makes more sense. Thanks for catching that, I've updated the PR.

---

_Renamed from "Use `ruff_linter` with the "test-rules" features in `ruff_dev`" to "Remove test rules from JSON schema" by @dhruvmanila on 2025-01-21 07:50_

---

_Merged by @dhruvmanila on 2025-01-24 04:47_

---

_Closed by @dhruvmanila on 2025-01-24 04:47_

---

_Branch deleted on 2025-01-24 04:48_

---
