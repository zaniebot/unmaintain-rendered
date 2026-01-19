```yaml
number: 22717
title: "`PIE794`: Detect duplicated declared class fields"
type: pull_request
state: open
author: MichaReiser
labels:
  - bug
  - rule
assignees: []
base: main
head: micha/fix-PIE794-annotated-assignments
created_at: 2026-01-19T11:12:03Z
updated_at: 2026-01-19T11:15:20Z
url: https://github.com/astral-sh/ruff/pull/22717
synced_at: 2026-01-19T11:30:28Z
```

# `PIE794`: Detect duplicated declared class fields

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/22683

The rule skipped annotated assignments without a value. This seems an unintentional change that was introduced in https://github.com/astral-sh/ruff/pull/8634. This PR re-enables detecting duplicated class fields where a later declaration is an annotated assignment without a value.

We might have to preview gate this change if there are many ecosystem hits. Although I doubt that, given that this is unlikely very common.

## Test Plan

Added regression test. 




---

_Label `bug` added by @MichaReiser on 2026-01-19 11:12_

---

_Label `rule` added by @MichaReiser on 2026-01-19 11:12_

---
