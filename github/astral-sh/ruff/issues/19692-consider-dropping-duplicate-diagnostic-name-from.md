---
number: 19692
title: "Consider dropping duplicate diagnostic name from `github` output"
type: issue
state: open
author: ntBre
labels:
  - needs-decision
  - diagnostics
assignees: []
created_at: 2025-08-01T16:40:58Z
updated_at: 2025-08-01T16:41:14Z
url: https://github.com/astral-sh/ruff/issues/19692
synced_at: 2026-01-07T13:12:16-06:00
---

# Consider dropping duplicate diagnostic name from `github` output

---

_Issue opened by @ntBre on 2025-08-01 16:40_

As noted in https://github.com/astral-sh/ruff/pull/19644#discussion_r2245915824, we currently report the diagnostic name (noqa code for lint rules or `invalid-syntax` for syntax errors) in both the title and message body in our `github` output format. This seems redundant, and we could probably remove one of them, depending on exactly how these are rendered by GitHub.

https://github.com/astral-sh/ruff/blob/932515827ff185e3130947d7f9de3a7b0a7379cb/crates/ruff/tests/snapshots/lint__output_format_github.snap#L19-L21

---

_Label `needs-decision` added by @ntBre on 2025-08-01 16:41_

---

_Label `diagnostics` added by @ntBre on 2025-08-01 16:41_

---

_Assigned to @ntBre by @ntBre on 2025-08-01 16:41_

---
