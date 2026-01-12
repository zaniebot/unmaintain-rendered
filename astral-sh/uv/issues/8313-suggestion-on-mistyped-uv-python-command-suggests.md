```yaml
number: 8313
title: "Suggestion on mistyped `uv python` command suggests `uv pip`"
type: issue
state: closed
author: adamtheturtle
labels:
  - bug
assignees: []
created_at: 2024-10-17T23:35:29Z
updated_at: 2025-05-12T01:44:55Z
url: https://github.com/astral-sh/uv/issues/8313
synced_at: 2026-01-12T15:59:23Z
```

# Suggestion on mistyped `uv python` command suggests `uv pip`

---

_@adamtheturtle_

I typed:

```
> uv python remove cpython-3.13.0-macos-aarch64-none
error: unrecognized subcommand 'remove'

  tip: a similar subcommand exists: 'uv pip uninstall'
```

I'd expect that the "similar subcommand" would be `uv python uninstall` rather than `uv pip uninstall`.

`uv 0.4.23 (Homebrew 2024-10-17)`

---

_Comment by @zanieb on 2024-10-18 00:45_

Thanks! This looks weirdly hard to fix.

---

_Label `bug` added by @zanieb on 2024-10-18 00:46_

---

_Closed by @zanieb on 2025-01-06 20:08_

---

_Comment by @metavucks on 2025-05-12 01:44_

 unrecognized subcommand 'venu'
 some similar subcommands exist: 'v', 'venv'

Usage: uv [OPTIONS] <COMMAND>
이 문제를 어떻게 해결 하나요?

---
