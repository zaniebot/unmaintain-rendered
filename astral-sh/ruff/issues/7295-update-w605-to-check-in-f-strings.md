```yaml
number: 7295
title: "Update `W605` to check in f-strings"
type: issue
state: closed
author: dhruvmanila
labels:
  - rule
  - python312
assignees: []
created_at: 2023-09-12T09:43:14Z
updated_at: 2023-09-19T06:39:00Z
url: https://github.com/astral-sh/ruff/issues/7295
synced_at: 2026-01-10T11:09:49Z
```

# Update `W605` to check in f-strings

---

_Issue opened by @dhruvmanila on 2023-09-12 09:43_

`W605` checks for invalid escape sequences in a string.

For f-strings, we'll be looking for the same in a `FStringMiddle` token which is the non-expression part of the f-string. The rule extracts the body part which excludes the quotes and also skips the validation if it's a raw string:

https://github.com/astral-sh/ruff/blob/a4a4616f0a49c6b1ff7da93a71308d897d27d21f/crates/ruff/src/rules/pycodestyle/rules/invalid_escape_sequence.rs#L51-L64

The `FStringMiddle` token contains both the above information for free. We just need to check if the token is a `FStringMiddle` or a `String`.

---

_Label `rule` added by @dhruvmanila on 2023-09-12 09:47_

---

_Label `python312` added by @dhruvmanila on 2023-09-12 09:47_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-09-13 05:35_

---

_Closed by @dhruvmanila on 2023-09-19 06:39_

---
