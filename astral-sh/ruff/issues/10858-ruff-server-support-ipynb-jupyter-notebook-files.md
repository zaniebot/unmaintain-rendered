```yaml
number: 10858
title: "`ruff server`: Support `.ipynb` (Jupyter Notebook) files"
type: issue
state: closed
author: snowsignal
labels:
  - server
assignees: []
created_at: 2024-04-10T06:01:50Z
updated_at: 2024-05-21T22:29:32Z
url: https://github.com/astral-sh/ruff/issues/10858
synced_at: 2026-01-10T11:09:53Z
```

# `ruff server`: Support `.ipynb` (Jupyter Notebook) files

---

_Issue opened by @snowsignal on 2024-04-10 06:01_

`ruff server` needs to support `.ipynb` (aka Jupyter Notebook) files to have feature parity with `ruff-lsp`. In the LSP specification, these files are represented as [notebook documents](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#notebookDocument_synchronization). Ruff can format and lint notebook documents, but their internal schema is different from the schema used by the LSP, so we'll need to convert between the two.

---

_Label `server` added by @snowsignal on 2024-04-10 06:01_

---

_Assigned to @snowsignal by @snowsignal on 2024-04-10 06:01_

---

_Added to milestone `Ruff Server: Beta` by @snowsignal on 2024-04-10 23:36_

---

_Closed by @snowsignal on 2024-05-21 22:29_

---
