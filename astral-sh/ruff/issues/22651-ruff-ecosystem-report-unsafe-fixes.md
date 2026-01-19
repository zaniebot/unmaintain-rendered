```yaml
number: 22651
title: ruff-ecosystem report unsafe fixes
type: issue
state: open
author: leandrobbraga
labels:
  - internal
  - ci
assignees: []
created_at: 2026-01-17T13:49:24Z
updated_at: 2026-01-19T14:58:14Z
url: https://github.com/astral-sh/ruff/issues/22651
synced_at: 2026-01-19T15:24:54Z
```

# ruff-ecosystem report unsafe fixes

---

_@leandrobbraga_

`ruff-ecosystem` should report unsafe fixes as well as regular fixes.

A fix was changed from safe to unsafe in a recent [PR](https://github.com/astral-sh/ruff/pull/22650). `ruff-ecosystem` reported a reduction of hundreds of fixes across the ecosystem but did not report the corresponding increase in unsafe fixes, which was misleading to me.

> ℹ️ ecosystem check detected linter changes. (+0 -0 violations, +0 -858 fixes in 5 projects; 50 projects unchanged)

https://github.com/astral-sh/ruff/pull/22650#issuecomment-3763744372

I believe the report should either be neutral (no change in number of fixes) or explicit (-858 fixes, +858 unsafe-fixes).

---

_Comment by @ntBre on 2026-01-19 14:58_

This sounds like a good idea to me! It may help to use the JSON output from Ruff and reconstruct the concise diagnostic only in the output, but that's just an idea.

---

_Label `internal` added by @ntBre on 2026-01-19 14:58_

---

_Label `ci` added by @ntBre on 2026-01-19 14:58_

---
