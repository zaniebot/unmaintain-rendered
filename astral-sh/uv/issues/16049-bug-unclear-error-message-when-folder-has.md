```yaml
number: 16049
title: "Bug: unclear error message when folder has different capitalisation vs project name"
type: issue
state: closed
author: MosheSteinberg
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-09-28T10:56:01Z
updated_at: 2025-11-08T19:06:43Z
url: https://github.com/astral-sh/uv/issues/16049
synced_at: 2026-01-12T16:02:22Z
```

# Bug: unclear error message when folder has different capitalisation vs project name

---

_@MosheSteinberg_

### Summary

Very similar to #15702, but instead with a mismatch in the capitalisation of the folder and the project:

```toml
[project]
name = "proj"

...

[tool.uv.build-backend]
module-root = "Source"
```

The code itself is in `Source\Proj`. Running `uv build` gives 
```
Expected a Python module at:
      XXXX\AppData\Local\uv\cache\sdists-v9\.tmpLqGkpp\proj-0.1.0\Source\proj\__init__.py
```

Similarly, trying to run `uv run file.py` where file.py contains:
```py
from proj import function
```
gives `No module named proj`

Once the folder is renamed to "Source\proj`, this all works as expected.

### Platform

Windows 11

### Version

0.8.22

### Python version

3.13.7

---

_Label `bug` added by @MosheSteinberg on 2025-09-28 10:56_

---

_Renamed from "Bug: unclear error message when folder is has different capitalisation vs project" to "Bug: unclear error message when folder has different capitalisation vs project name" by @MosheSteinberg on 2025-09-28 10:56_

---

_Comment by @charliermarsh on 2025-09-28 18:44_

Hmm, isn't the error message correct here?

---

_Comment by @DhavalGojiya on 2025-10-03 12:50_

@MosheSteinberg 
I have tried your project structure but was unable to reproduce the issue on either Linux or Windows 11.

---

_Label `needs-mre` added by @zanieb on 2025-11-08 19:06_

---

_Closed by @zanieb on 2025-11-08 19:06_

---
