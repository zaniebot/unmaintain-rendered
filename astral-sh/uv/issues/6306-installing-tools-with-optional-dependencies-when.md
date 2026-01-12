```yaml
number: 6306
title: Installing tools with optional dependencies, when we want to install all of them, fail
type: issue
state: closed
author: KhazAkar
labels: []
assignees: []
created_at: 2024-08-21T07:37:44Z
updated_at: 2024-08-21T07:49:34Z
url: https://github.com/astral-sh/uv/issues/6306
synced_at: 2026-01-12T15:59:02Z
```

# Installing tools with optional dependencies, when we want to install all of them, fail

---

_@KhazAkar_

Hi,
I wanted to test `uvx` portion with installing `python-lsp-server`, which is possible to install with various 'options', most notably with `[all]`, but uvx complains about name being invalid

```
$ uvx python-lsp-server[all]
error: Not a valid package or extra name: "python-lsp-server[all]". Names must start and end with a letter or digit and may only contain -, _, ., and alphanumeric characters.
```

When I do the same using pipx, it works fine:
```
$ pipx install python-lsp-server[all]
'python-lsp-server' already seems to be installed. Not modifying existing installation in '/home/khazakar/.local/pipx/venvs/python-lsp-server'. Pass '--force' to force installation.
$ pipx upgrade python-lsp-server
upgraded package python-lsp-server from 1.10.0 to 1.11.0 (location: /home/khazakar/.local/pipx/venvs/python-lsp-server)
```

uv version: 0.3.0
uv platform: Linux AMD64

---

_Comment by @konstin on 2024-08-21 07:49_

This is tracked in #6296

---

_Closed by @konstin on 2024-08-21 07:49_

---
