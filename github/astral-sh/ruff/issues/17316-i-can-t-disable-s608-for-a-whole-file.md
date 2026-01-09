---
number: 17316
title: "I can't disable S608 for a whole file"
type: issue
state: closed
author: red8888
labels:
  - question
assignees: []
created_at: 2025-04-09T15:25:15Z
updated_at: 2025-04-28T08:32:36Z
url: https://github.com/astral-sh/ruff/issues/17316
synced_at: 2026-01-07T13:12:16-06:00
---

# I can't disable S608 for a whole file

---

_Issue opened by @red8888 on 2025-04-09 15:25_

### Summary

if I put this at the top of the file ruff immediately removes it when I save via `Unused `noqa` directive (unused: `S608`)Ruff[RUF100](https://docs.astral.sh/ruff/rules/unused-noqa)`

`# noqa: S608`

If I can add `RUF100` and it doesn't remove it but it still doesn't work:

# noqa: S608, RUF100

I still get `S608` errors for my sql query strings. I need to add `# noqa: S608` to every line to ignore the check.

How do I turn off S608 for a whole file?


### Version

_No response_

---

_Comment by @MichaReiser on 2025-04-09 15:30_

Can you try `# ruff: noqa: S608`, `# noqa: <code>` is for inline suppressions, see https://docs.astral.sh/ruff/linter/#disabling-fixes

---

_Label `question` added by @MichaReiser on 2025-04-09 15:30_

---

_Closed by @MichaReiser on 2025-04-28 08:32_

---
