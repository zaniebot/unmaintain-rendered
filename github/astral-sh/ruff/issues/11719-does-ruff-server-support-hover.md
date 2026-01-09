---
number: 11719
title: "Does ruff server support \"hover\"?"
type: issue
state: closed
author: KiYugadgeter
labels:
  - server
assignees: []
created_at: 2024-06-03T09:40:18Z
updated_at: 2024-06-03T13:20:17Z
url: https://github.com/astral-sh/ruff/issues/11719
synced_at: 2026-01-07T13:12:15-06:00
---

# Does ruff server support "hover"?

---

_Issue opened by @KiYugadgeter on 2024-06-03 09:40_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Does ruff sever support hover now?
Or, Will it support?

Now I cannot move from pyright because ruff-lsp does not support hover feature in vim-lsp.  
[vim-lsp](https://github.com/prabirshrestha/vim-lsp) does not support changing hoverProvider to false like neovim's example in README in ruff-lsp.  
Hence I want ruff server to support  hover correctly.  





---

_Comment by @dhruvmanila on 2024-06-03 09:51_

The server does support the hover feature but not in the same way as other language servers like Pyright. Ruff only detects `noqa` comments and it'll provide the rule documentation. Refer https://github.com/astral-sh/ruff/issues/10595 for a screenshot on what it looks like.

In the future, we'd like to expand it's capability to support providing information for symbols like variables, functions, etc.

Unfortunately, I don't know if it's possible to change the server capability in `vim-lsp` similar to how it's done in Neovim. I would suggest to open an issue in `vim-lsp` to check if it's possible as of today or as a feature request.



---

_Label `server` added by @dhruvmanila on 2024-06-03 09:51_

---

_Comment by @charliermarsh on 2024-06-03 13:18_

ruff server is intended to be used alongside Pyright since it's primary purpose is enabling formatting and lint fixes in the editor -- it's not a replacement for Pyright right now.

---

_Closed by @charliermarsh on 2024-06-03 13:20_

---
