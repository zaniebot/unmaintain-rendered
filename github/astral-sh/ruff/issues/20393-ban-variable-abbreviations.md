---
number: 20393
title: ban-variable-abbreviations
type: issue
state: open
author: Richienb
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-09-15T00:51:12Z
updated_at: 2025-11-24T14:45:43Z
url: https://github.com/astral-sh/ruff/issues/20393
synced_at: 2026-01-07T13:12:16-06:00
---

# ban-variable-abbreviations

---

_Issue opened by @Richienb on 2025-09-15 00:51_

### Summary

Disallow including certain abbreviations in variable names, instead requiring the full name instead

---

_Label `rule` added by @ntBre on 2025-09-15 12:44_

---

_Label `needs-decision` added by @ntBre on 2025-09-15 12:44_

---

_Comment by @alessio-locatelli on 2025-11-24 14:44_

I think this should be slightly different, but still support your use case. I would like to propose an **opt-in rule** where a user can set a **regular expression** in the Ruff configuration to ban specific variable names. Something like `banned_variable_names_regex = `. Because banning certain names is opinionated, the regex should be disabled/empty by default.

---
