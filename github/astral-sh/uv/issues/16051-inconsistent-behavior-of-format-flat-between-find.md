---
number: 16051
title: "Inconsistent behavior of format = \"flat\" between find-links and index-url in uv tool run"
type: issue
state: closed
author: byeblack
labels:
  - bug
  - question
assignees: []
created_at: 2025-09-28T14:21:43Z
updated_at: 2025-09-29T17:23:19Z
url: https://github.com/astral-sh/uv/issues/16051
synced_at: 2026-01-07T13:12:19-06:00
---

# Inconsistent behavior of format = "flat" between find-links and index-url in uv tool run

---

_Issue opened by @byeblack on 2025-09-28 14:21_

### Question

I'm experiencing inconsistent behavior with `format = "flat"` between `find-links` and `index-url` when using `uv tool run`. 

When installing and running an offline package using command-line flags with `find-links`, the tool fails to find the installed package:

```bash
❯ uv tool install --offline -n --no-config -f temp pip@25.0
Resolved 1 package in 2ms
Prepared 1 package in 71ms
Installed 1 package in 109ms
 + pip==25.0
Installed 3 executables: pip, pip3, pip3.12

❯ uv tool run --offline -n --no-config --from pip pip --version
  × No solution found when resolving tool dependencies:
  ╰─▶ Because pip was not found in the cache and you require pip, we can conclude that your requirements are unsatisfiable.

      hint: Packages were unavailable because the network was disabled. When the network is disabled, registry packages may only be read from the cache.
```

If I continue using `-f` to specify the path, it temporarily runs the pip package from the temp directory instead of the installed one:

```bash
❯ uv tool run --offline -n --no-config -f temp --from pip pip --version
Installed 1 package in 126ms
pip 25.2 ...
```

However, when using a `uv.toml` configuration file with `index-url` and `format = "flat"`, everything works correctly:

```toml
# uv.toml
[[index]]
name = "local"
url = 'temp'
format = "flat"
```

```bash
❯ uv tool install --offline -n --config-file uv.toml pip@25.0
Resolved 1 package in 2ms
Prepared 1 package in 71ms
Installed 1 package in 109ms
 + pip==25.0
Installed 3 executables: pip, pip3, pip3.12

❯ uv tool run --offline -n --config-file uv.toml --from pip pip --version
pip 25.0 from ...
```

Comparing the `uv-receipt.toml` files reveals the difference:

```toml
# From: uv tool install --offline -n --no-config -f temp pip@25.0
[tool.options]
find-links = ["..."]
```

```toml
# From: uv tool install --offline -n --config-file uv.toml pip@25.0
[tool.options]
index = [
    { name = "local", url = "...", explicit = false, default = false, format = "flat", authenticate = "auto" },
]
```
In my opinion, The behavior should be consistent between these two approaches.

### Platform

Windows 11 x86_64

### Version

uv 0.8.22 (ade2bdbd2 2025-09-23)

---

_Label `question` added by @byeblack on 2025-09-28 14:21_

---

_Renamed from "I'm experiencing inconsistent behavior with `format = "flat"` between `find-links` and `index-url` when using `uv tool run`." to "Inconsistent behavior of format = "flat" between find-links and index-url in uv tool run" by @byeblack on 2025-09-28 14:22_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-09-28 18:46_

---

_Label `bug` added by @charliermarsh on 2025-09-28 18:54_

---

_Referenced in [astral-sh/uv#16055](../../astral-sh/uv/pulls/16055.md) on 2025-09-28 19:05_

---

_Closed by @charliermarsh on 2025-09-29 17:23_

---
