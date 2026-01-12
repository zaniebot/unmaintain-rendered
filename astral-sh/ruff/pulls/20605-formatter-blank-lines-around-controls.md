```yaml
number: 20605
title: "[formatter] blank lines around controls"
type: pull_request
state: closed
author: SmikeForYou
labels:
  - formatter
assignees: []
base: main
head: feat/formatter/blank-lines-around-controls
created_at: 2025-09-28T12:01:04Z
updated_at: 2025-09-29T07:04:32Z
url: https://github.com/astral-sh/ruff/pull/20605
synced_at: 2026-01-12T15:57:06Z
```

# [formatter] blank lines around controls

---

_@SmikeForYou_

## Summary

Adds new formatter option: format.blank-lines-around-controls (default: false)
Optional style to visually separate control-flow blocks(if/while/whith/for) without impacting default behavior
No change to stable formatting by default; feature is opt-in
Applies at both top-level and within suites, respecting existing comment/import spacing logic
Minimal perf impact; uses existing trivia counting helpers

## Test Plan

New unit test in ruff_python_formatter asserts extra blank lines around control statements
Also tested manually


---

_Review requested from @MichaReiser by @SmikeForYou on 2025-09-28 12:01_

---

_Comment by @MichaReiser on 2025-09-29 07:04_

Thank you for giving this a try. Please first open an issue for formatter style changes as outlined in our [contributing guidelines](https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md#finding-ways-to-help).

---

_Closed by @MichaReiser on 2025-09-29 07:04_

---

_Label `formatter` added by @MichaReiser on 2025-09-29 07:04_

---
