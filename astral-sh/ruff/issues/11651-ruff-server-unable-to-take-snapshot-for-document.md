```yaml
number: 11651
title: "`ruff server`: Unable to take snapshot for document"
type: issue
state: closed
author: T-256
labels:
  - bug
  - server
assignees: []
created_at: 2024-05-31T21:13:24Z
updated_at: 2024-06-21T17:53:32Z
url: https://github.com/astral-sh/ruff/issues/11651
synced_at: 2026-01-12T15:54:51Z
```

# `ruff server`: Unable to take snapshot for document

---

_@T-256_

Repro on VSCode: create new untitled notebook then close it.

```log

2024-06-01 00:37:05.063 [info] [Trace - 12:37:05 AM] Received notification 'window/showMessage'.
2024-06-01 00:37:58.052 [info] [Trace - 12:37:58 AM] Sending notification 'notebookDocument/didOpen'.
2024-06-01 00:37:58.053 [info] [Trace - 12:37:58 AM] Sending notification 'notebookDocument/didChange'.
2024-06-01 00:37:58.056 [info]  194.326137s DEBUG ruff_server::session::index Falling back to configuration of the only active workspace for the new document 'untitled:Untitled-2.ipynb?jupyter-notebook'.
 194.326153s DEBUG ruff_server::session::index Falling back to configuration of the only active workspace for the new document 'untitled:Untitled-2.ipynb?jupyter-notebook'.

2024-06-01 00:37:58.057 [info] [Trace - 12:37:58 AM] Received notification 'textDocument/publishDiagnostics'.
2024-06-01 00:37:58.057 [info]  194.329275s DEBUG ruff_server::session::index Falling back to configuration of the only active workspace for the new document 'untitled:Untitled-2.ipynb?jupyter-notebook'.
 194.329299s DEBUG ruff_server::session::index Falling back to configuration of the only active workspace for the new document 'untitled:Untitled-2.ipynb?jupyter-notebook'.

2024-06-01 00:37:58.058 [info] [Trace - 12:37:58 AM] Received notification 'textDocument/publishDiagnostics'.
2024-06-01 00:37:58.285 [info] [Trace - 12:37:58 AM] Sending request 'textDocument/codeAction - (18)'.
2024-06-01 00:37:58.286 [info]  194.558099s DEBUG ruff_server::session::index Falling back to configuration of the only active workspace for the new document 'untitled:Untitled-2.ipynb?jupyter-notebook'.
 194.558120s DEBUG ruff_server::session::index Falling back to configuration of the only active workspace for the new document 'untitled:Untitled-2.ipynb?jupyter-notebook'.

2024-06-01 00:37:58.286 [info] [Trace - 12:37:58 AM] Received response 'textDocument/codeAction - (18)' in 1ms.
2024-06-01 00:38:08.026 [info] [Trace - 12:38:08 AM] Sending notification 'notebookDocument/didClose'.
2024-06-01 00:38:08.048 [info] [Trace - 12:38:08 AM] Sending notification 'textDocument/didClose'.
2024-06-01 00:38:08.049 [info]  204.321227s DEBUG ruff_server::session::index Falling back to configuration of the only active workspace for the new document 'vscode-notebook-cell:Untitled-2.ipynb?jupyter-notebook#W0sdW50aXRsZWQ%3D'.
 204.321249s DEBUG ruff_server::session::index Falling back to configuration of the only active workspace for the new document 'vscode-notebook-cell:Untitled-2.ipynb?jupyter-notebook#W0sdW50aXRsZWQ%3D'.
 204.321259s ERROR ruff_server::server::api An error occurred while running textDocument/didClose: Unable to take snapshot for document with URL vscode-notebook-cell:Untitled-2.ipynb?jupyter-notebook#W0sdW50aXRsZWQ%3D

2024-06-01 00:38:08.049 [info] [Trace - 12:38:08 AM] Received notification 'window/showMessage'.
```

---

_Label `bug` added by @charliermarsh on 2024-05-31 21:42_

---

_Label `server` added by @charliermarsh on 2024-05-31 21:42_

---

_Comment by @charliermarsh on 2024-05-31 21:47_

I wonder if the notebook close gets called, then the cell closes get called?

---

_Comment by @T-256 on 2024-05-31 22:06_

Also, the logs show duplicated `ruff_server::session::index Falling back to configuration ...` sent for each `textDocument/publishDiagnostics` responses from server. Perhaps on second try there is no available notebook cell?

---

_Comment by @dhruvmanila on 2024-06-01 06:22_

Can you try this with Ruff `0.4.7`? It could be a problem because the server didn't support untitled documents but it has been fixed with the latest release.

---

_Comment by @MichaReiser on 2024-06-01 07:08_

@dhruvmanila this should be the latest version from the logs.

The fallback messages are printed because untitled documents belong to no workspace, so we had to implement our own fallback logic 

---

_Comment by @T-256 on 2024-06-14 00:31_

Detailed log with `ruff.trace.server = verbose`:
```log
2024-06-14 03:50:29.068 [info] Server: Start requested.
2024-06-14 03:51:24.256 [info] [Trace - 3:51:24 AM] Sending notification '$/setTrace'.
2024-06-14 03:51:24.256 [info] Params: {
    "value": "verbose"
}


2024-06-14 03:51:56.972 [info] [Trace - 3:51:56 AM] Sending notification 'notebookDocument/didOpen'.
2024-06-14 03:51:56.972 [info] Params: {
    "notebookDocument": {
        "uri": "untitled:Untitled-1.ipynb?jupyter-notebook",
        "notebookType": "jupyter-notebook",
        "version": 0,
        "cells": [
            {
                "kind": 2,
                "document": "vscode-notebook-cell:Untitled-1.ipynb?jupyter-notebook#W0sdW50aXRsZWQ%3D",
                "metadata": {
                    "metadata": {}
                }
            }
        ],
        "metadata": {
            "cells": [],
            "metadata": {
                "language_info": {
                    "name": "python"
                }
            },
            "indentAmount": " "
        }
    },
    "cellTextDocuments": [
        {
            "uri": "vscode-notebook-cell:Untitled-1.ipynb?jupyter-notebook#W0sdW50aXRsZWQ%3D",
            "languageId": "python",
            "version": 1,
            "text": ""
        }
    ]
}


2024-06-14 03:51:56.973 [info] [Trace - 3:51:56 AM] Sending notification 'notebookDocument/didChange'.
2024-06-14 03:51:56.973 [info] Params: {
    "notebookDocument": {
        "version": 1,
        "uri": "untitled:Untitled-1.ipynb?jupyter-notebook"
    },
    "change": {
        "metadata": {
            "cells": [],
            "metadata": {},
            "nbformat": 4,
            "nbformat_minor": 2
        },
        "cells": {
            "data": [
                {
                    "kind": 2,
                    "document": "vscode-notebook-cell:Untitled-1.ipynb?jupyter-notebook#W0sdW50aXRsZWQ%3D"
                }
            ]
        }
    }
}


2024-06-14 03:51:56.978 [info] [Trace - 3:51:56 AM] Received notification 'textDocument/publishDiagnostics'.
2024-06-14 03:51:56.978 [info] Params: {
    "diagnostics": [],
    "uri": "vscode-notebook-cell:Untitled-1.ipynb?jupyter-notebook#W0sdW50aXRsZWQ%3D",
    "version": 0
}


2024-06-14 03:51:56.978 [info] [Trace - 3:51:56 AM] Received notification 'textDocument/publishDiagnostics'.
2024-06-14 03:51:56.978 [info] Params: {
    "diagnostics": [],
    "uri": "vscode-notebook-cell:Untitled-1.ipynb?jupyter-notebook#W0sdW50aXRsZWQ%3D",
    "version": 1
}


2024-06-14 03:51:57.147 [info] [Trace - 3:51:57 AM] Sending request 'textDocument/codeAction - (8)'.
2024-06-14 03:51:57.147 [info] Params: {
    "textDocument": {
        "uri": "vscode-notebook-cell:Untitled-1.ipynb?jupyter-notebook#W0sdW50aXRsZWQ%3D"
    },
    "range": {
        "start": {
            "line": 0,
            "character": 0
        },
        "end": {
            "line": 0,
            "character": 0
        }
    },
    "context": {
        "diagnostics": [],
        "triggerKind": 2
    }
}


2024-06-14 03:51:57.148 [info] [Trace - 3:51:57 AM] Received response 'textDocument/codeAction - (8)' in 1ms.
2024-06-14 03:51:57.148 [info] Result: [
    {
        "data": "vscode-notebook-cell:Untitled-1.ipynb?jupyter-notebook#W0sdW50aXRsZWQ%3D",
        "kind": "source.fixAll.ruff",
        "title": "Ruff: Fix all auto-fixable problems"
    },
    {
        "data": "vscode-notebook-cell:Untitled-1.ipynb?jupyter-notebook#W0sdW50aXRsZWQ%3D",
        "kind": "source.organizeImports.ruff",
        "title": "Ruff: Organize imports"
    }
]


2024-06-14 03:51:59.037 [info] [Trace - 3:51:59 AM] Sending notification 'notebookDocument/didClose'.
2024-06-14 03:51:59.037 [info] Params: {
    "notebookDocument": {
        "uri": "untitled:Untitled-1.ipynb?jupyter-notebook"
    },
    "cellTextDocuments": [
        {
            "uri": "vscode-notebook-cell:Untitled-1.ipynb?jupyter-notebook#W0sdW50aXRsZWQ%3D"
        }
    ]
}


2024-06-14 03:51:59.057 [info] [Trace - 3:51:59 AM] Sending notification 'textDocument/didClose'.
2024-06-14 03:51:59.057 [info] Params: {
    "textDocument": {
        "uri": "vscode-notebook-cell:Untitled-1.ipynb?jupyter-notebook#W0sdW50aXRsZWQ%3D"
    }
}


2024-06-14 03:51:59.058 [info]   89.959415800s ERROR ruff:main ruff_server::server::api: An error occurred while running textDocument/didClose: Unable to take snapshot for document with URL vscode-notebook-cell:Untitled-1.ipynb?jupyter-notebook#W0sdW50aXRsZWQ%3D

2024-06-14 03:51:59.058 [info] [Trace - 3:51:59 AM] Received notification 'window/showMessage'.
2024-06-14 03:51:59.058 [info] Params: {
    "message": "Ruff encountered a problem. Check the logs for more details.",
    "type": 1
}

```

---

_Added to milestone `Ruff Server: Stable` by @snowsignal on 2024-06-17 16:34_

---

_Assigned to @snowsignal by @snowsignal on 2024-06-19 18:32_

---

_Closed by @snowsignal on 2024-06-21 17:53_

---

_Closed by @snowsignal on 2024-06-21 17:53_

---
