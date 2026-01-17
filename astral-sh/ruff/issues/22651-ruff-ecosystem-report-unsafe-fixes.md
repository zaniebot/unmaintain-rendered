```yaml
number: 22651
title: ruff-ecosystem report unsafe fixes
type: issue
state: open
author: leandrobbraga
labels: []
assignees: []
created_at: 2026-01-17T13:49:24Z
updated_at: 2026-01-17T13:51:16Z
url: https://github.com/astral-sh/ruff/issues/22651
synced_at: 2026-01-17T14:11:17Z
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
