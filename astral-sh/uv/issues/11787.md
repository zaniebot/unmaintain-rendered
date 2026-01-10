```yaml
number: 11787
title: "`uv init` support for PEP 420 namespace packages"
type: issue
state: open
author: haarisr
labels:
  - enhancement
assignees: []
created_at: 2025-02-26T02:37:31Z
updated_at: 2025-07-08T06:40:34Z
url: https://github.com/astral-sh/uv/issues/11787
synced_at: 2026-01-10T03:32:45Z
```

# `uv init` support for PEP 420 namespace packages

---

_Issue opened by @haarisr on 2025-02-26 02:37_

### Summary

Would be great to see support for this. I believe there is currently no way in `uv` to do this.

For reference, https://packaging.python.org/en/latest/guides/packaging-namespace-packages/

This is related to #6575.

### Example

_No response_

---

_Label `enhancement` added by @haarisr on 2025-02-26 02:37_

---

_Comment by @zanieb on 2025-02-26 15:31_

Please share an example interface and output project structure if you can.

---

_Comment by @haarisr on 2025-02-26 18:20_

Current scenario

```bash
uv init snake
cd snake
uv init --lib packages/viper
uv init --lib packages/mamba
```
```bash
tree

.
├── main.py
├── packages
│   ├── mamba
│   │   ├── pyproject.toml
│   │   ├── README.md
│   │   └── src
│   │       └── mamba
│   │           ├── __init__.py
│   │           └── py.typed
│   └── viper
│       ├── pyproject.toml
│       ├── README.md
│       └── src
│           └── viper
│               ├── __init__.py
│               └── py.typed
├── pyproject.toml
└── README.md
```

Proposed scenario

```bash
uv init snake
cd snake
uv init --lib packages/viper --namespace snake
uv init --lib packages/mamba --namespace snake
```

which maybe something similar to

```bash
uv init snake
cd snake
uv init --lib packages/viper
uv init --lib packages/mamba
mkdir packages/mamba/src/snake
mv packages/mamba/src/mamba packages/mamba/src/snake
mkdir packages/viper/src/snake
mv packages/viper/src/viper packages/viper/src/snake
```

```bash
tree
.
├── main.py
├── packages
│   ├── mamba
│   │   ├── pyproject.toml
│   │   ├── README.md
│   │   └── src
│   │       └── snake
│   │           └── mamba
│   │               ├── __init__.py
│   │               └── py.typed
│   └── viper
│       ├── pyproject.toml
│       ├── README.md
│       └── src
│           └── snake
│               └── viper
│                   ├── __init__.py
│                   └── py.typed
├── pyproject.toml
└── README.md
```

With this you can do things like

```python3
from snake.mamba import foo
import snake.viper as sv
```

---

_Comment by @grantperry on 2025-03-12 23:54_

I believe this might be related to #7182.

---

_Comment by @Spenhouet on 2025-07-08 06:15_

If a `--namespace` is provided, this should also automatically set 

```toml
[tool.uv.build-backend]
module-name = "mynamespace.bar"
```
in pyproject.toml

https://docs.astral.sh/uv/concepts/build-backend/#namespace-packages

---
