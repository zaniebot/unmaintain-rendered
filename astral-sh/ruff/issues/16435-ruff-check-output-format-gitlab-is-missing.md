```yaml
number: 16435
title: "ruff check --output-format=gitlab is missing required property \"check_name\""
type: issue
state: closed
author: kaisengit
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-02-28T10:21:47Z
updated_at: 2025-02-28T15:19:38Z
url: https://github.com/astral-sh/ruff/issues/16435
synced_at: 2026-01-10T11:09:57Z
```

# ruff check --output-format=gitlab is missing required property "check_name"

---

_Issue opened by @kaisengit on 2025-02-28 10:21_

### Summary

When using `ruff check --output-format=gitlab` it will output all the required properties used by gitlab except for one: `check_name`. See: https://docs.gitlab.com/ci/testing/code_quality/#code-quality-report-format

Without this field, gitlab will not load the code quality report at all (or that is at least my assumption; I am not sure how to properly debug this).

### Version

ruff 0.9.8

---

_Label `bug` added by @MichaReiser on 2025-02-28 10:29_

---

_Label `help wanted` added by @MichaReiser on 2025-02-28 10:29_

---

_Closed by @MichaReiser on 2025-02-28 14:27_

---

_Comment by @kaisengit on 2025-02-28 15:11_

Wow, you guys are quick! Thank you!

---

_Comment by @MichaReiser on 2025-02-28 15:19_

You're welcome. All credit goes to @InSyncWithFoo . Thanks for reporting (and including a link to the reference!). 

---
