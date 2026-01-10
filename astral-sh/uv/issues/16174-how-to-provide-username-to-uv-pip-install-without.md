---
number: 16174
title: How to provide username to uv pip install without pyproject.toml or inclusion in url?
type: issue
state: open
author: benbrummer
labels:
  - question
assignees: []
created_at: 2025-10-08T05:41:14Z
updated_at: 2025-12-18T16:25:01Z
url: https://github.com/astral-sh/uv/issues/16174
synced_at: 2026-01-10T01:26:04Z
---

# How to provide username to uv pip install without pyproject.toml or inclusion in url?

---

_Issue opened by @benbrummer on 2025-10-08 05:41_

### Question

Background:
- Windows Credential manager has an entry for private registry https://example.com

Starting with an empty folder
```
uv venv
$Env:UV_INDEX_DEFAULT_USERNAME='USERNAME'
uv pip install --keyring-provider subprocess --verbose --default-index=https://example.com/simple some-package
...
DEBUG Skipping keyring fetch for https://example.com/simple/some-package/ without username; use `authenticate = always` to force
...
```

How can I provide a Username without including it in the url?
How can I set `authenticate = always` without pyproject.toml?

### Platform

Windows 11 x64

### Version

0.9.0

---

_Label `question` added by @benbrummer on 2025-10-08 05:41_

---

_Renamed from "How to provide username to uv pip install without pyproject.toml or including it in the url?" to "How to provide username to uv pip install without pyproject.toml or inclusion in url?" by @benbrummer on 2025-10-08 05:41_

---

_Comment by @konstin on 2025-12-18 16:25_

There is currently no support for this in the `uv pip install` interface, we only support setting a default username through `uv.toml`/`pyproject.toml` files.

---
