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
updated_at: 2026-01-19T16:03:51Z
url: https://github.com/astral-sh/ruff/issues/22651
synced_at: 2026-01-19T16:26:54Z
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

_Comment by @leandrobbraga on 2026-01-19 15:36_

If you think this is useful I'm willing to do the work.

---

_Comment by @ntBre on 2026-01-19 16:03_

Sure, go for it! What kind of output are you picturing? I think it probably makes sense to preserve the diff and just update the summary line.

It would be kind of nice to do something with the diff lines since the pairs of +/- lines hit the truncation limit twice as fast, but in some cases it's still nice to see which lines were affected.

---

_Assigned to @leandrobbraga by @ntBre on 2026-01-19 16:03_

---
