---
number: 12810
title: "Auto-format `# noqa` directives"
type: issue
state: closed
author: charliermarsh
labels:
  - rule
  - suppression
assignees: []
created_at: 2024-08-11T23:17:27Z
updated_at: 2024-08-17T14:35:21Z
url: https://github.com/astral-sh/ruff/issues/12810
synced_at: 2026-01-10T01:22:52Z
---

# Auto-format `# noqa` directives

---

_Issue opened by @charliermarsh on 2024-08-11 23:17_

It'd be nice to have a lint rule to enforce consistent formatting of `# noqa` directives (e.g., `# noqa: F401, F841`), along with a fix to auto-format them.

---

_Label `rule` added by @charliermarsh on 2024-08-11 23:17_

---

_Label `suppression` added by @charliermarsh on 2024-08-11 23:17_

---

_Comment by @charliermarsh on 2024-08-11 23:17_

Partly inspired by some of the cases in #12808.

---

_Comment by @dhruvmanila on 2024-08-12 04:35_

I wonder if this should be in the formatter instead as there's probably only one way to format them (we shouldn't consider line length in this case i.e., not breaking them into multiple lines). Although, the linter has all the infrastructure for it but we could extract that out as a crate.

---

_Comment by @MichaReiser on 2024-08-12 06:44_

Related issue https://github.com/astral-sh/ruff/issues/10160

---

_Closed by @MichaReiser on 2024-08-17 14:35_

---
