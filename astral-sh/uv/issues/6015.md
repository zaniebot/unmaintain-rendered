```yaml
number: 6015
title: "`uv lock` mixes registry results with `--find-links` over HTTP"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
created_at: 2024-08-12T00:31:10Z
updated_at: 2024-08-12T12:54:11Z
url: https://github.com/astral-sh/uv/issues/6015
synced_at: 2026-01-10T04:53:49Z
```

# `uv lock` mixes registry results with `--find-links` over HTTP

---

_Issue opened by @charliermarsh on 2024-08-12 00:31_

Given:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["tqdm==4.64.1"]

[tool.uv]
find-links = ["https://download.pytorch.org/whl/torch_stable.html"]
no-build-package = ["tqdm"]
```

Produces:

```toml
[[package]]
name = "tqdm"
version = "4.64.1"
source = { registry = "https://download.pytorch.org/whl/torch_stable.html" }
dependencies = [
    { name = "colorama", marker = "platform_system == 'Windows'" },
]
sdist = { url = "https://files.pythonhosted.org/packages/c1/c2/d8a40e5363fb01806870e444fc1d066282743292ff32a9da54af51ce36a2/tqdm-4.64.1.tar.gz", hash = "sha256:5f4f682a004951c1b450bc753c710e9280c5746ce6ffedee253ddbcbf54cf1e4", size = 169599 }
wheels = [
    { url = "https://download.pytorch.org/whl/tqdm-4.64.1-py2.py3-none-any.whl" },
    { url = "https://files.pythonhosted.org/packages/47/bb/849011636c4da2e44f1253cd927cfb20ada4374d8b3a4e425416e84900cc/tqdm-4.64.1-py2.py3-none-any.whl", hash = "sha256:6fee160d6ffcd1b1c68c65f14c829c22832bc401726335ce92c52d395944a6a1", size = 78468 },
]
```

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-12 00:31_

---

_Label `bug` added by @charliermarsh on 2024-08-12 00:31_

---

_Label `preview` added by @charliermarsh on 2024-08-12 00:31_

---

_Closed by @charliermarsh on 2024-08-12 12:54_

---
