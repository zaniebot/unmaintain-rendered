```yaml
number: 13874
title: "`uv add --index <name>` assumes a path in the local directory as the index?"
type: issue
state: closed
author: zanieb
labels:
  - bug
assignees: []
created_at: 2025-06-05T23:17:41Z
updated_at: 2025-06-09T17:28:40Z
url: https://github.com/astral-sh/uv/issues/13874
synced_at: 2026-01-12T16:01:38Z
```

# `uv add --index <name>` assumes a path in the local directory as the index?

---

_@zanieb_

### Summary

```
❯ uv init example
Initialized project `example` at `/Users/zb/workspace/uv/example`
❯ cd example
❯ uv add requests --index test
Using CPython 3.13.2
Creating virtual environment at: .venv
Resolved 6 packages in 3ms
Installed 5 packages in 5ms
 + certifi==2025.4.26
 + charset-normalizer==3.4.2
 + idna==3.10
 + requests==2.32.3
 + urllib3==2.4.0
```

results in

```toml
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13.2"
dependencies = [
    "requests>=2.32.3",
]

[[tool.uv.index]]
url = "file:///Users/zb/workspace/uv/example/test"
```

### Platform

macOS

### Version

0.7.10

### Python version

n/a

---

_Label `bug` added by @zanieb on 2025-06-05 23:17_

---

_Comment by @zanieb on 2025-06-05 23:18_

1. It's so weird this gets interpreted as a local path
2. It's wild this resolves
3. This will ignore an existing index with the name `test`, if defined

---

_Assigned to @jtfmumm by @jtfmumm on 2025-06-08 23:30_

---

_Closed by @jtfmumm on 2025-06-09 17:28_

---

_Closed by @jtfmumm on 2025-06-09 17:28_

---
