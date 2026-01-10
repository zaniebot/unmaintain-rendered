```yaml
number: 83
title: Review used APIs in the LSP
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-03-14T10:25:38Z
updated_at: 2025-05-28T08:54:54Z
url: https://github.com/astral-sh/ty/issues/83
synced_at: 2026-01-10T02:34:09Z
```

# Review used APIs in the LSP

---

_Issue opened by @MichaReiser on 2025-03-14 10:25_

The LSP currently uses crate-internal APIs like `system_path_to_file` that can panic if there's a write in any other process (and it also reduces encapsulation). Review the APIs used in the server and decide on a facade between server/semantic model

---

_Label `server` added by @MichaReiser on 2025-03-14 10:25_

---

_Renamed from "[red-knot] Review used APIs in the LSP" to "Review used APIs in the LSP" by @MichaReiser on 2025-05-07 15:16_

---

_Comment by @MichaReiser on 2025-05-28 08:54_

Solved by https://github.com/astral-sh/ruff/pull/18254

---

_Closed by @MichaReiser on 2025-05-28 08:54_

---
