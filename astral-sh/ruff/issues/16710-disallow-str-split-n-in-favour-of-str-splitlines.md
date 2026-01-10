```yaml
number: 16710
title: "Disallow str.split(\"\\n\") in favour of str.splitlines()"
type: issue
state: open
author: lukaszett
labels:
  - rule
assignees: []
created_at: 2025-03-13T14:59:41Z
updated_at: 2025-03-14T19:32:00Z
url: https://github.com/astral-sh/ruff/issues/16710
synced_at: 2026-01-10T11:09:57Z
```

# Disallow str.split("\n") in favour of str.splitlines()

---

_Issue opened by @lukaszett on 2025-03-13 14:59_

### Summary

`str.splitlines()` is almost always preferrable to `str.split("\n")`.

Consider the case of handling user input in a webapp: Input coming from Mac or Linux machines will be handled correctly. However due to windows line endings, for inputs coming from windows machines, all results of `str.split("\n")` will have the unexpected line ending `\r`.

EDIT: Just noting that this is a rule request.

---

_Label `rule` added by @MichaReiser on 2025-03-13 15:13_

---

_Comment by @MichaReiser on 2025-03-13 15:14_

Seems reasonable as a restriction rule that intentionally limits the allowed syntax. However, I don't think we should add it to the RUF category and should, instead, wait for #1774

---

_Comment by @ember91 on 2025-03-14 19:31_

A difference is that if a string ends with newline `split()` will have an extra element while `splitlines()` won't.

---
