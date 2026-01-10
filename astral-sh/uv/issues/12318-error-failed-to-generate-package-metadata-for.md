---
number: 12318
title: "`error: Failed to generate package metadata` for locally installed package (`Failed to deserialize cache entry`)"
type: issue
state: closed
author: pnovotnak
labels:
  - bug
assignees: []
created_at: 2025-03-19T16:53:04Z
updated_at: 2025-03-19T17:15:15Z
url: https://github.com/astral-sh/uv/issues/12318
synced_at: 2026-01-10T01:25:18Z
---

# `error: Failed to generate package metadata` for locally installed package (`Failed to deserialize cache entry`)

---

_Issue opened by @pnovotnak on 2025-03-19 16:53_

### Summary

I have a service written in python (not an installable package). I just began using `uv run` within this project as a wrapper for project venv binaries and project-provided scripts. I am seeing this error after introducing `uv run`. I'm not 100% sure what conditions triggered it, but `uv clean` fixed the issue.

![Image](https://github.com/user-attachments/assets/84a9f638-5ded-4426-83b3-0cbc10785758)

I have the following in my `pyproject.toml`:

```toml
[tool.uv.sources]
my-package = { path = "generated" }
```

And the following in my `uv.lock`:

```toml
[[package]]
name = "my-package"
version = "0.0.0"
source = { directory = "generated" }
```

Finally, in `generated/setup.py`:

```python
from setuptools import find_packages
from setuptools import setup


setup(
    name="my_package",
    packages=find_packages(),
    package_data={"": ["py.typed", "*.pyi"]},
    include_package_data=True,
)
```

I'm wondering a few things:
1. Are there changes I can make to my configuration to prevent this? If not:
    1. Can this cache entry be automatically ignored in this case?
    2. Could the cache entry be automatically purged?

Apologies for not being able to provide all conditions to reproduce.

<hr />

Notes:

1. *It's possible the uv version that created the entry is older than 0.6.8.*

### Platform

macOS 15.3.2 (24D81)

### Version

0.6.8 (installed via brew), 0.6.8 (installed in venv)

### Python version

venv: 3.10.16

---

_Label `bug` added by @pnovotnak on 2025-03-19 16:53_

---

_Comment by @charliermarsh on 2025-03-19 16:55_

Thanks, these should be non-fatal. @Gankra I think this suggests there's something we missed here...

---

_Referenced in [astral-sh/uv#12319](../../astral-sh/uv/pulls/12319.md) on 2025-03-19 17:02_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-19 17:03_

---

_Closed by @charliermarsh on 2025-03-19 17:15_

---
