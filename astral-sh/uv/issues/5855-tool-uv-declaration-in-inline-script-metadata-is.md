```yaml
number: 5855
title: "`[tool.uv]` declaration in inline script metadata is ignored"
type: issue
state: closed
author: zanieb
labels:
  - bug
  - preview
assignees: []
created_at: 2024-08-07T13:07:20Z
updated_at: 2024-08-10T03:11:11Z
url: https://github.com/astral-sh/uv/issues/5855
synced_at: 2026-01-12T15:58:59Z
```

# `[tool.uv]` declaration in inline script metadata is ignored

---

_@zanieb_

e.g., we do not respect `offline` here

```python
# /// script
# dependencies = ["anyio"]
#
# [tool.uv]
# offline = true
# ///

import anyio
```

```
❯ uv run --refresh -v script.py
DEBUG uv 0.2.32
warning: `uv run` is experimental and may change without warning
Reading inline script metadata from: script.py
DEBUG Searching for Python interpreter in managed installations or system path
DEBUG Found `cpython-3.12.3-macos-aarch64-none` at `/Users/zb/workspace/example/.venv/bin/python3` (active virtual environment)
DEBUG Caching via base interpreter: `/opt/homebrew/opt/python@3.12/bin/python3.12`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.3
DEBUG Adding direct dependency: anyio*
DEBUG Found stale response for: https://pypi.org/simple/anyio/
DEBUG Sending revalidation request for: https://pypi.org/simple/anyio/
DEBUG Found not-modified response for: https://pypi.org/simple/anyio/
DEBUG Searching for a compatible version of anyio (*)
DEBUG Selecting: anyio==4.4.0 [compatible] (anyio-4.4.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7b/a2/10639a79341f6c019dedc95bd48a4928eed9f1d1197f4c04f546fc7ae0ff/anyio-4.4.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for anyio==4.4.0: idna>=2.8
DEBUG Adding transitive dependency for anyio==4.4.0: sniffio>=1.1
DEBUG Found stale response for: https://pypi.org/simple/idna/
DEBUG Sending revalidation request for: https://pypi.org/simple/idna/
DEBUG Found stale response for: https://pypi.org/simple/sniffio/
DEBUG Sending revalidation request for: https://pypi.org/simple/sniffio/
DEBUG Found not-modified response for: https://pypi.org/simple/idna/
DEBUG Searching for a compatible version of idna (>=2.8)
DEBUG Selecting: idna==3.7 [compatible] (idna-3.7-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e5/3e/741d8c82801c347547f8a2a06aa57dbb1992be9e948df2ea0eda2c8b79e8/idna-3.7-py3-none-any.whl.metadata
DEBUG Found not-modified response for: https://pypi.org/simple/sniffio/
DEBUG Searching for a compatible version of sniffio (>=1.1)
DEBUG Selecting: sniffio==1.3.1 [compatible] (sniffio-1.3.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl.metadata
DEBUG Tried 3 versions: anyio 1, idna 1, sniffio 1
DEBUG Split specific environment resolution took 0.078s
Resolved 3 packages in 78ms
DEBUG Using Python 3.12.3 interpreter at: /Users/zb/Library/Caches/uv/archive-v0/aDaa7cf4eRe69wDeluSdZ/bin/python3
DEBUG Running `python script.py`
```

---

_Label `bug` added by @charliermarsh on 2024-08-07 15:45_

---

_Label `preview` added by @charliermarsh on 2024-08-07 15:45_

---

_Comment by @charliermarsh on 2024-08-07 15:45_

Thanks yeah, should probably be considered a bug.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-09 14:36_

---

_Comment by @charliermarsh on 2024-08-09 14:36_

At the very least, we should warn that it's unsupported.

---

_Comment by @zanieb on 2024-08-09 14:44_

(both in the documentation and from the CLI when the section is present)

---

_Closed by @charliermarsh on 2024-08-10 03:11_

---

_Closed by @charliermarsh on 2024-08-10 03:11_

---
