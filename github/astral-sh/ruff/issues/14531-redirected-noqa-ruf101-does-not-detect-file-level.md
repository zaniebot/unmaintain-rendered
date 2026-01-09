---
number: 14531
title: "`redirected-noqa` (`RUF101`) does not detect file-level `# ruff: noqa` comments"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-11-22T14:04:15Z
updated_at: 2024-11-27T17:25:49Z
url: https://github.com/astral-sh/ruff/issues/14531
synced_at: 2026-01-07T13:12:16-06:00
---

# `redirected-noqa` (`RUF101`) does not detect file-level `# ruff: noqa` comments

---

_Issue opened by @AlexWaygood on 2024-11-22 14:04_

Ruff supports file-level `noqa` comments: `# ruff: noqa: TC002` on its own line at the top of a file will lead to the `TC002` code being suppressed for the whole file. `RUF101`, however, appears not to detect these file-level comments when it looks for redirected rule codes; it only flags inline `noqa` comments of the form `# noqa: TCH001`.

For example, I would expect Ruff to emit a `RUF101` diagnostic on the first line of this snippet, since Ruff's `flake8-type-checking` rules have been recoded from `TCH` to `TC`. However, no `RUF101` diagnostic is emitted:

```py
# ruff: noqa: TCH002

from __future__ import annotations

import local_module


def func(sized: local_module.Container) -> int:
    return len(sized)
```

https://play.ruff.rs/26c8939c-fc09-4d85-a25c-f80bea97a842

---

_Label `bug` added by @AlexWaygood on 2024-11-22 14:04_

---

_Label `help wanted` added by @MichaReiser on 2024-11-22 14:32_

---

_Referenced in [astral-sh/ruff#14635](../../astral-sh/ruff/pulls/14635.md) on 2024-11-27 14:35_

---

_Closed by @MichaReiser on 2024-11-27 17:25_

---
