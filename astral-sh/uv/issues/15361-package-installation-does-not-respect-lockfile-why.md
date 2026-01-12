```yaml
number: 15361
title: Package installation does not respect lockfile; why?
type: issue
state: closed
author: bocytko
labels:
  - question
assignees: []
created_at: 2025-08-18T20:46:25Z
updated_at: 2025-08-18T22:33:17Z
url: https://github.com/astral-sh/uv/issues/15361
synced_at: 2026-01-12T16:02:09Z
```

# Package installation does not respect lockfile; why?

---

_@bocytko_

### Question

There is a breaking change in openai 1.100.0 and we discovered that installing packages from source (`uv tool install https://github.com/...` or `pip install --editable .` does not respect the lockfile. Is this expected behavior?

Is there any flag that can be set during installation in order to enforce the lockfile to be used in during installation process?

---

`pyproject.toml`:

```toml
[project]
name = "uv-test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "openai>=1.90.0",
]
```

`uv.lock`:

```
cat uv.lock | grep openai
name = "openai"
sdist = { url = "https://files.pythonhosted.org/packages/8a/d2/ef89c6f3f36b13b06e271d3cc984ddd2f62508a0972c1cbcc8485a6644ff/openai-1.99.9.tar.gz", hash = "sha256:f2082d155b1ad22e83247c3de3958eb4255b20ccf4a1de2e6681b6957b554e92", size = 506992 }
    { url = "https://files.pythonhosted.org/packages/e8/fb/df274ca10698ee77b07bff952f302ea627cc12dac6b85289485dd77db6de/openai-1.99.9-py3-none-any.whl", hash = "sha256:9dbcdb425553bae1ac5d947147bebbd630d91bbfc7788394d4c4f3a35682ab3a", size = 786816 },
    { name = "openai" },
requires-dist = [{ name = "openai", specifier = ">=1.90.0" }]
```

---

Install:

```
pip install --editable .
Obtaining file:///Users/X/repos/uv-test
  Installing build dependencies ... done
  Checking if build backend supports build_editable ... done
  Getting requirements to build editable ... done
  Preparing editable metadata (pyproject.toml) ... done
Collecting openai>=1.90.0
  Downloading openai-1.100.0-py3-none-any.whl (786 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 786.5/786.5 kB 5.0 MB/s eta 0:00:00
```

Reinstall:

```
Requirement already satisfied: openai>=1.90.0 in /Users/X/.pyenv/versions/3.10.9/lib/python3.10/site-packages (from uv-test==0.1.0) (1.100.0)
```

### Platform

MacOS 15.5 (24F74), Darwin 24.5.0 arm64

### Version

uv 0.8.11 (Homebrew 2025-08-14)

---

_Label `question` added by @bocytko on 2025-08-18 20:46_

---

_Comment by @zanieb on 2025-08-18 20:53_

Yes `uv pip install` and `uv tool install` do not use the lockfile. Both of those interfaces interact with the project as a standard Python package, e.g., it's built and installed as it would be by any other tool. The lockfile is only used within the project, e.g., `uv run` and `uv sync`.

It'd be nice to have a `uv tool install --locked` mode that reads the lockfile, e.g., see https://github.com/astral-sh/uv/issues/13410

---

_Comment by @bocytko on 2025-08-18 22:33_

> It'd be nice to have a `uv tool install --locked` mode that reads the lockfile

Agreed! Thank you for the quick response.

---

_Closed by @bocytko on 2025-08-18 22:33_

---
