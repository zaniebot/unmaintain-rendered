```yaml
number: 14630
title: "Gate `is_multiline` change behind preview"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: dhruv/f-string-multiline-preview
created_at: 2024-11-27T09:45:38Z
updated_at: 2024-11-27T10:20:30Z
url: https://github.com/astral-sh/ruff/pull/14630
synced_at: 2026-01-10T20:50:57Z
```

# Gate `is_multiline` change behind preview

---

_Pull request opened by @dhruvmanila on 2024-11-27 09:45_

## Summary

Ref: https://github.com/astral-sh/ruff/pull/14624#pullrequestreview-2464127254

## Test Plan

The test case in the follow-up PR showcases the difference between preview and non-preview formatting: https://github.com/astral-sh/ruff/pull/14624/files#diff-dc25bd4df280d9a9180598075b5bc2d0bac30af956767b373561029309c8f024


---

_Label `formatter` added by @dhruvmanila on 2024-11-27 09:45_

---

_Label `preview` added by @dhruvmanila on 2024-11-27 09:45_

---

_Marked ready for review by @dhruvmanila on 2024-11-27 09:47_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-11-27 09:47_

---

_@dhruvmanila reviewed on 2024-11-27 09:48_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/string/mod.rs`:122 on 2024-11-27 09:48_

The change in this diff block removes the `else` block because the entire function call is behind `is_f_string_formatting_enabled` check.

---

_@MichaReiser approved on 2024-11-27 09:50_

---

_Comment by @github-actions[bot] on 2024-11-27 09:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @dhruvmanila on 2024-11-27 10:20_

---

_Closed by @dhruvmanila on 2024-11-27 10:20_

---

_Branch deleted on 2024-11-27 10:20_

---
