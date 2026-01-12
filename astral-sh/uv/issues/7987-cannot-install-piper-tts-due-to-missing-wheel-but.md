```yaml
number: 7987
title: Cannot install piper-tts due to missing wheel (but pip install works fine)
type: issue
state: closed
author: halflings
labels:
  - question
  - needs-mre
assignees: []
created_at: 2024-10-07T18:50:00Z
updated_at: 2024-10-21T21:55:15Z
url: https://github.com/astral-sh/uv/issues/7987
synced_at: 2026-01-12T15:59:18Z
```

# Cannot install piper-tts due to missing wheel (but pip install works fine)

---

_@halflings_

Repro steps:
uv init
uv pip install piper-tts

Output:
Using CPython 3.12.4 interpreter at: /usr/bin/python
Creating virtual environment at: .venv
Resolved 13 packages in 426ms
error: distribution piper-phonemize==1.1.0 @ registry+https://pypi.org/simple can't be installed because it doesn't have a source distribution or wheel for the current platform

Expected:
piper-tts to be installed correctly.
pip works fine in this case:
... Downloading piper_phonemize-1.1.0-cp310-cp310-manylinux_2_28_x86_64.whl (25.0 MB)
(maybe using a different Python version somehow?)

---

_Comment by @zanieb on 2024-10-07 19:14_

Looks like you're using Python 3.10 with pip and 3.12 with uv.

---

_Label `question` added by @zanieb on 2024-10-07 19:14_

---

_Label `needs-mre` added by @zanieb on 2024-10-21 21:55_

---

_Closed by @zanieb on 2024-10-21 21:55_

---
