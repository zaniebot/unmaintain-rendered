```yaml
number: 1779
title: "[completions] Rank symbols with the appropriate type higher after `except <CURSOR>` and `raise <EXPR> from <CURSOR>`"
type: issue
state: open
author: AlexWaygood
labels:
  - server
  - completions
assignees: []
created_at: 2025-12-05T15:00:44Z
updated_at: 2025-12-05T15:00:44Z
url: https://github.com/astral-sh/ty/issues/1779
synced_at: 2026-01-10T01:56:41Z
```

# [completions] Rank symbols with the appropriate type higher after `except <CURSOR>` and `raise <EXPR> from <CURSOR>`

---

_Issue opened by @AlexWaygood on 2025-12-05 15:00_

Similar to what we did for `raise <CURSOR>` in https://github.com/astral-sh/ruff/pull/21571:

- The type of a symbol after `except` must be assignable to `type[BaseException] | tuple[type[BaseException], ...]`
- The type of a symbol after `raise <EXPR> from` must be assignable to `BaseException | type[BaseException] | None`

---

_Label `server` added by @AlexWaygood on 2025-12-05 15:00_

---

_Label `completions` added by @AlexWaygood on 2025-12-05 15:00_

---
