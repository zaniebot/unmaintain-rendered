```yaml
number: 12034
title: Is there a way to prevent packages from being installed from specified index when using pip interface?
type: issue
state: closed
author: amitkma
labels:
  - question
assignees: []
created_at: 2025-03-07T09:10:47Z
updated_at: 2025-03-10T16:24:45Z
url: https://github.com/astral-sh/uv/issues/12034
synced_at: 2026-01-12T16:00:53Z
```

# Is there a way to prevent packages from being installed from specified index when using pip interface?

---

_@amitkma_

### Question

My project has a few dependencies, such as `torch==2.6.0+cu124`, `numpy==2.2.3`, etc. I want to use a specific pytorch index to install pytorch (in this case CUDA 12.4) and NumPy from the PyPi index. But when I run `uv pip compile --extra-index-url=https://download.pytorch.org/whl/cu124 --output-file=req.cuda.txt req.cuda.in`, it finds the numpy on pytorch index that doesn't have the specified version of numpy. 

I can't use `index-strategy` because renovate at the moment doesn't support it. Please let me know if there is any solution or workaround. 

### Platform

Ubuntu 24.04.2 LTS

### Version

uv 0.6.4

---

_Label `question` added by @amitkma on 2025-03-07 09:10_

---

_Comment by @charliermarsh on 2025-03-07 17:23_

Unfortunately there isn't a great way to handle this in `uv pip` interface (other than `--index-strategy`), if you're using `requirements.txt`. I _believe_ this works with `uv pip install -r pyproject.toml` though:

```toml
[project]
name = "project"
version = "0.0.1"
requires-python = ">=3.9"
dependencies = [
    "torch"
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu" },
]
```

---

_Comment by @amitkma on 2025-03-10 16:24_

Thank you for the answer, @charliermarsh. I managed to solve it similarly to the one you mentioned. I am closing this issue. 

---

_Closed by @amitkma on 2025-03-10 16:24_

---
