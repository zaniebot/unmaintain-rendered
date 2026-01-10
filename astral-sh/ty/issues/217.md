```yaml
number: 217
title: "`--config-file`"
type: issue
state: closed
author: MichaReiser
labels:
  - cli
  - configuration
assignees: []
created_at: 2025-01-15T14:25:51Z
updated_at: 2025-05-28T09:16:01Z
url: https://github.com/astral-sh/ty/issues/217
synced_at: 2026-01-10T02:34:09Z
```

# `--config-file`

---

_Issue opened by @MichaReiser on 2025-01-15 14:25_

Path to a `knot.toml` configuration file (`pyproject.toml` files are not accepted). When provided, this file will be used in place of any discovered configuration (including user-level configuration). Red Knot will skip project discovery and default to the current working directory (or `--project` if specified). Paths are anchored at the current working directory.

---

_Renamed from "[red-knot] `--config-file`" to "`--config-file`" by @MichaReiser on 2025-05-07 15:27_

---

_Label `cli` added by @MichaReiser on 2025-05-09 05:52_

---

_Comment by @MichaReiser on 2025-05-09 05:52_

@thejchap is working on this

---

_Label `configuration` added by @AlexWaygood on 2025-05-11 07:49_

---

_Comment by @thejchap on 2025-05-14 00:44_

more discussion here https://github.com/astral-sh/ruff/pull/17697#discussion_r2080746392

---

_Closed by @MichaReiser on 2025-05-28 09:16_

---
