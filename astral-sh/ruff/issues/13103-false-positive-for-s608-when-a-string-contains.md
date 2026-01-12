```yaml
number: 13103
title: "False positive for `S608` when a string contains \"delete from\""
type: issue
state: open
author: dhruvmanila
labels:
  - bug
assignees: []
created_at: 2024-08-26T04:46:53Z
updated_at: 2025-04-16T23:48:45Z
url: https://github.com/astral-sh/ruff/issues/13103
synced_at: 2026-01-12T15:54:52Z
```

# False positive for `S608` when a string contains "delete from"

---

_@dhruvmanila_

i have this problem :(

S608 Possible SQL injection vector through string-based query construction

`logger.warning(f'The linked role <{1}> has been delete from the guild {2}')`

_Originally posted by @Arexils in https://github.com/astral-sh/ruff/issues/8723#issuecomment-2308555876_
            

---

_Label `bug` added by @dhruvmanila on 2024-08-26 04:46_

---

_Comment by @dhruvmanila on 2024-08-26 04:47_

I think this is because the string contains "delete from" in the middle. The regex isn't anchored to the start:

https://github.com/astral-sh/ruff/blob/39ad6b9472869337ff47dcd2a52e078b7246f2a3/crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs#L13-L16

---

_Comment by @JavadocMD on 2025-04-16 23:48_

I also ran into this. Example string `f"Please replace NA values with {foo}."`

---
