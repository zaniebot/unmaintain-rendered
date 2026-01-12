```yaml
number: 10589
title: "`ruff server` lacks unused code formatting"
type: issue
state: closed
author: charliermarsh
labels:
  - server
assignees: []
created_at: 2024-03-25T21:07:42Z
updated_at: 2024-03-28T11:30:36Z
url: https://github.com/astral-sh/ruff/issues/10589
synced_at: 2026-01-12T15:54:50Z
```

# `ruff server` lacks unused code formatting

---

_@charliermarsh_

In the existing LSP (notice the grayed-out text):

![Screenshot 2024-03-25 at 5 05 43 PM](https://github.com/astral-sh/ruff/assets/1309177/64ac8cb7-b6f6-4742-863a-c5325ba27d19)

With `ruff server` (white text):

![Screenshot 2024-03-25 at 5 06 02 PM](https://github.com/astral-sh/ruff/assets/1309177/3a19be52-6a43-41ec-8ce2-1e1305292a07)


---

_Assigned to @snowsignal by @snowsignal on 2024-03-25 22:30_

---

_Label `server` added by @dhruvmanila on 2024-03-26 03:33_

---

_Comment by @dhruvmanila on 2024-03-26 03:34_

Reference implementation in `ruff-lsp`: https://github.com/astral-sh/ruff-lsp/blob/187d7790be0783b9ac41ce025a724cf389bf575c/ruff_lsp/server.py#L739-L745

---

_Closed by @snowsignal on 2024-03-28 11:30_

---
