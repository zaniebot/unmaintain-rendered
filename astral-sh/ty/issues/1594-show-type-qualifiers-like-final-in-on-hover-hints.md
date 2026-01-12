```yaml
number: 1594
title: "Show type qualifiers like `Final` in on-hover hints"
type: issue
state: open
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-11-19T15:04:54Z
updated_at: 2025-11-24T09:17:12Z
url: https://github.com/astral-sh/ty/issues/1594
synced_at: 2026-01-12T15:54:25Z
```

# Show type qualifiers like `Final` in on-hover hints

---

_@MichaReiser_

> If I hovered over `X` for something like `X: Final = ["foo"]`, I'd expect something like

```
list[str | Unknown] (Final)
```

---

_Label `server` added by @MichaReiser on 2025-11-19 15:04_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-19 15:04_

---

_Comment by @AlexWaygood on 2025-11-19 15:06_

And same for other qualifiers: `ClassVar`, `ReadOnly`, `Required`, `NotRequired`, etc.

---

_Renamed from "Hovering a final definition should indicate that it's final" to "Show type qualifiers like `Final` in on-hover hints" by @sharkdp on 2025-11-24 09:17_

---
