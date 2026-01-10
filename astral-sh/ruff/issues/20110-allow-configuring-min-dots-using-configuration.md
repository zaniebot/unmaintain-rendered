```yaml
number: 20110
title: "Allow configuring `--min-dots` using configuration file"
type: issue
state: closed
author: InSyncWithFoo
labels:
  - configuration
assignees: []
created_at: 2025-08-27T03:12:23Z
updated_at: 2025-09-16T11:19:35Z
url: https://github.com/astral-sh/ruff/issues/20110
synced_at: 2026-01-10T11:09:59Z
```

# Allow configuring `--min-dots` using configuration file

---

_Issue opened by @InSyncWithFoo on 2025-08-27 03:12_

`ruff analyze graph` has both `--detect-string-imports` and `--min-dots`, but [`tool.ruff.analyze`/`analyze`](https://docs.astral.sh/ruff/settings/#analyze) only has [`detect-string-imports`](https://docs.astral.sh/ruff/settings/#analyze_detect-string-imports).

`analyze.min-dots` should be a thing, if only for consistency.

---

_Label `configuration` added by @ntBre on 2025-08-27 12:42_

---

_Comment by @TaKO8Ki on 2025-09-11 20:04_

I will take this one.

---

_Assigned to @TaKO8Ki by @ntBre on 2025-09-11 20:39_

---

_Closed by @MichaReiser on 2025-09-16 11:19_

---
