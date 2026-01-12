```yaml
number: 1417
title: Reconsider union truncation
type: issue
state: open
author: MichaReiser
labels:
  - diagnostics
  - server
assignees: []
created_at: 2025-10-23T13:20:11Z
updated_at: 2026-01-12T14:37:30Z
url: https://github.com/astral-sh/ty/issues/1417
synced_at: 2026-01-12T15:54:25Z
```

# Reconsider union truncation

---

_@MichaReiser_

Our current union truncation is too aggressive and it often makes diagnostics (e.g. when using `--output-format=concise` or in editors not supporting related information) unactionable. 

For context, see the discussion here https://github.com/astral-sh/ruff/pull/21044#discussion_r2454860309

I'd prefer a smart union truncation where the diagnostic can say: Display this type, but don't dare to hide `<X>`. 

We should also disable truncation for hover or use a much larger limit

---

_Label `diagnostics` added by @MichaReiser on 2025-10-23 13:20_

---

_Comment by @MichaReiser on 2025-11-14 08:30_

Related to https://github.com/astral-sh/ty/issues/1418 but more specifically to the LSP use case

---

_Label `server` added by @MichaReiser on 2025-11-14 08:30_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 15:34_

---

_Comment by @AlexWaygood on 2026-01-12 14:37_

https://github.com/astral-sh/ty/issues/1307#issuecomment-3738504694 suggests that we could disable union truncation by default in type display when `-v` has been specified, which seems reasonable.

---
