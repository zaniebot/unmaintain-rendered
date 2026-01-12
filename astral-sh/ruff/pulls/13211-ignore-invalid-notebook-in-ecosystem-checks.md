```yaml
number: 13211
title: Ignore invalid notebook in ecosystem checks
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ci
assignees: []
merged: true
base: main
head: dhruv/ignore-ecosystem-notebook
created_at: 2024-09-02T09:52:07Z
updated_at: 2024-09-02T10:52:53Z
url: https://github.com/astral-sh/ruff/pull/13211
synced_at: 2026-01-12T15:55:43Z
```

# Ignore invalid notebook in ecosystem checks

---

_@dhruvmanila_

## Summary

The notebook is invalid because it contains a code cell whose content is in markdown:
https://github.com/openai/openai-cookbook/blob/457f4310700f93e7018b1822213ca99c613dbd1b/examples/chatgpt/gpt_actions_library/gpt_action_salesforce.ipynb#L106-L121

## Test plan

- [x] No ecosystem changes


---

_Label `ci` added by @dhruvmanila on 2024-09-02 09:52_

---

_Comment by @github-actions[bot] on 2024-09-02 10:10_

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

_Merged by @dhruvmanila on 2024-09-02 10:52_

---

_Closed by @dhruvmanila on 2024-09-02 10:52_

---

_Branch deleted on 2024-09-02 10:52_

---
