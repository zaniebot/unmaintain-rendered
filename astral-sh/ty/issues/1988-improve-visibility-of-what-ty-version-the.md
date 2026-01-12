```yaml
number: 1988
title: Improve visibility of what ty version the extension is using
type: issue
state: open
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-12-17T08:49:55Z
updated_at: 2026-01-12T09:25:20Z
url: https://github.com/astral-sh/ty/issues/1988
synced_at: 2026-01-12T09:56:37Z
```

# Improve visibility of what ty version the extension is using

---

_Issue opened by @MichaReiser on 2025-12-17 08:49_

A common issue is that the extension uses a much older ty version than the bundled version without users noticing. Can we make it easier for users to notice when the extension uses the wrong ty version? Should we show the version in the status bar? Can we improve the ty discovery?

---

_Label `server` added by @MichaReiser on 2025-12-17 08:49_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-17 08:50_

---

_Comment by @dhruvmanila on 2026-01-12 09:25_

Yeah, I think we can use the status bar to show the ty version and possibly could also display the source, so something like (similar to Python extension's version and venv):
- `ty 0.0.11 (bundled)`
- `ty 0.0.11 (environment)`

<img width="668" height="26" alt="Image" src="https://github.com/user-attachments/assets/ae71b937-1d88-4d00-aecb-80169d69388e" />

Or, we could start with just include the version initially.

---
