```yaml
number: 207
title: Build fails when there are requires but no build backend
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2023-10-26T18:54:58Z
updated_at: 2023-11-03T08:29:46Z
url: https://github.com/astral-sh/uv/issues/207
synced_at: 2026-01-12T15:58:22Z
```

# Build fails when there are requires but no build backend

---

_@konstin_

tokenizers 0.0.11 specifies

```toml
[build-system]
requires = ["setuptools", "wheel", "setuptools-rust"]
```

this is arguably a wrong usage of pyproject.toml, since PEP 517 says

> If the pyproject.toml file is absent, or the build-backend key is missing, the source tree is not using this specification, and tools should revert to the legacy behaviour of running setup.py (either directly, or by implicitly invoking the setuptools.build_meta:__legacy__ backend).

. Currently, puffin will ignore the requires and fall back to running `setup.py`. In the future, puffin a) emit a warning b) run `setup.py` in a venv with `build-system.requires` installed instead of the default set.

---

_Label `bug` added by @charliermarsh on 2023-10-26 19:03_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-10-26 19:04_

---

_Comment by @konstin on 2023-11-03 08:29_

Implemented in 1529def5633080987f006b36e9238b86494d3966

---

_Closed by @konstin on 2023-11-03 08:29_

---
