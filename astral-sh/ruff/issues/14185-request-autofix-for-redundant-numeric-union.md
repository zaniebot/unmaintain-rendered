```yaml
number: 14185
title: "Request: Autofix for `redundant-numeric-union`/`PYI041` & `redundant-literal-union`/`PYI051`"
type: issue
state: open
author: Avasam
labels:
  - fixes
assignees: []
created_at: 2024-11-08T01:17:23Z
updated_at: 2025-10-30T17:01:24Z
url: https://github.com/astral-sh/ruff/issues/14185
synced_at: 2026-01-10T11:09:56Z
```

# Request: Autofix for `redundant-numeric-union`/`PYI041` & `redundant-literal-union`/`PYI051`

---

_Issue opened by @Avasam on 2024-11-08 01:17_

* [x] https://docs.astral.sh/ruff/rules/redundant-numeric-union/
* [ ] https://docs.astral.sh/ruff/rules/redundant-literal-union/

I lumped both rules together here because I think the fix is the exact same: drop a member of a union. Which is basically https://docs.astral.sh/ruff/rules/duplicate-union-member/ and sounds like a less complex version of https://docs.astral.sh/ruff/rules/unnecessary-literal-union/ and https://docs.astral.sh/ruff/rules/unnecessary-type-union/ which already have an autofix.

---

_Label `fixes` added by @MichaReiser on 2024-11-08 07:40_

---

_Comment by @kiran-4444 on 2025-01-22 07:30_

I'd like to takes this up to implement autofix for `PYI051`

---

_Comment by @kiran-4444 on 2025-03-22 19:50_

I lost track of this issue and forgot to work on it. Can someone please assign me to this so I don't get lost again? Thanks!

---

_Assigned to @kiran-4444 by @ntBre on 2025-03-23 14:45_

---

_Comment by @Avasam on 2025-10-30 17:01_

I thought these would go together, I feel bad for assuming and not splitting into two issues as I would usually do ^^"

---
