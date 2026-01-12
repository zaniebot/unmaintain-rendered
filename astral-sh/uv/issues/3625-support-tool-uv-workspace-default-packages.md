```yaml
number: 3625
title: "Support `tool.uv.workspace.default-packages`"
type: issue
state: closed
author: konstin
labels:
  - preview
assignees: []
created_at: 2024-05-16T13:39:23Z
updated_at: 2024-07-09T11:25:35Z
url: https://github.com/astral-sh/uv/issues/3625
synced_at: 2026-01-12T15:58:45Z
```

# Support `tool.uv.workspace.default-packages`

---

_@konstin_

When running commands such as `uv run` or `uv pip install -e .` in the root of a virtual workspace, we currently error because we don't know which package to install. Similar cargo, we should allow declaring a default target, in this case a package rather than a bin, as:

```toml
tool.uv.workspace.default-packages = ["<package name>", "<package name>"]
```

---

_Label `preview` added by @konstin on 2024-05-16 13:39_

---

_Renamed from "Support `too.uv.workspace.default-package`" to "Support `tool.uv.workspace.default-package`" by @konstin on 2024-05-16 13:39_

---

_Renamed from "Support `tool.uv.workspace.default-package`" to "Support `tool.uv.workspace.default-packages`" by @konstin on 2024-06-27 08:25_

---

_Renamed from "Support `tool.uv.workspace.default-packages`" to "Support `tool.uv.workspace.default-package`" by @konstin on 2024-06-27 08:25_

---

_Renamed from "Support `tool.uv.workspace.default-package`" to "Support `tool.uv.workspace.default-packages`" by @konstin on 2024-06-27 08:25_

---

_Closed by @konstin on 2024-07-09 11:25_

---
