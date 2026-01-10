```yaml
number: 11867
title: "Avoid close cells on `notebookDocument/didClose`"
type: pull_request
state: closed
author: T-256
labels: []
assignees: []
base: jane/server/defer-cell-index-deletion
head: cells-on-document-close
created_at: 2024-06-14T01:50:34Z
updated_at: 2024-06-14T18:12:12Z
url: https://github.com/astral-sh/ruff/pull/11867
synced_at: 2026-01-10T21:56:00Z
```

# Avoid close cells on `notebookDocument/didClose`

---

_Pull request opened by @T-256 on 2024-06-14 01:50_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Fixes https://github.com/astral-sh/ruff/issues/11651
Fixes https://github.com/astral-sh/ruff/issues/11851

As a follow-up of https://github.com/astral-sh/ruff/pull/11864 
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
Close (Ctrl+F4) documents with type of notebook/interactive-window/untitled-notebook and check no popup error message shows. 

---

_@MichaReiser reviewed on 2024-06-14 07:58_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:353 on 2024-06-14 07:58_

Does that mean that VS code sends an explicit `didClose` request before (or after) for the cells?

---

_@T-256 reviewed on 2024-06-14 13:34_

---

_Review comment by @T-256 on `crates/ruff_server/src/session/index.rs`:353 on 2024-06-14 13:34_

Yes, it always will send `textDocument/didClose` either before or after.

* I noticed on regular `file://` urls it sends `textDocument/didClose` per cell _before_ `notebookDocument/didClose`:

```log
2024-06-14 16:46:48.544 [info] [Trace - 4:46:48 PM] Sending notification 'textDocument/didClose'.
2024-06-14 16:46:48.544 [info] Params: {
    "textDocument": {
        "uri": "vscode-notebook-cell:/e%3A/test/test.ipynb#W0sZmlsZQ%3D%3D"
    }
}


2024-06-14 16:46:48.544 [info] [Trace - 4:46:48 PM] Sending notification 'textDocument/didClose'.
2024-06-14 16:46:48.544 [info] Params: {
    "textDocument": {
        "uri": "vscode-notebook-cell:/e%3A/test/test.ipynb#W1sZmlsZQ%3D%3D"
    }
}


2024-06-14 16:46:48.544 [info] [Trace - 4:46:48 PM] Sending notification 'notebookDocument/didClose'.
2024-06-14 16:46:48.544 [info] Params: {
    "notebookDocument": {
        "uri": "file:///e%3A/test/test.ipynb"
    },
    "cellTextDocuments": [
        {
            "uri": "vscode-notebook-cell:/e%3A/test/test.ipynb#W0sZmlsZQ%3D%3D"
        },
        {
            "uri": "vscode-notebook-cell:/e%3A/test/test.ipynb#W1sZmlsZQ%3D%3D"
        }
    ]
}


2024-06-14 16:46:48.545 [info] [Trace - 4:46:48 PM] Received notification 'textDocument/publishDiagnostics'.
2024-06-14 16:46:48.545 [info] Params: {
    "diagnostics": [],
    "uri": "vscode-notebook-cell:/e%3A/test/test.ipynb#W0sZmlsZQ%3D%3D",
    "version": 2
}


2024-06-14 16:46:48.545 [info] [Trace - 4:46:48 PM] Received notification 'textDocument/publishDiagnostics'.
2024-06-14 16:46:48.545 [info] Params: {
    "diagnostics": [],
    "uri": "vscode-notebook-cell:/e%3A/test/test.ipynb#W1sZmlsZQ%3D%3D",
    "version": 2
}
```

* For "untitled:Untitled-1.ipynb" and "untitled:/Interactive-2.interactive" urls it will send `textDocument/didClose` per cell _after_ `notebookDocument/didClose`:

```log
2024-06-14 16:58:23.344 [info] [Trace - 4:58:23 PM] Sending notification 'notebookDocument/didClose'.
2024-06-14 16:58:23.344 [info] Params: {
    "notebookDocument": {
        "uri": "untitled:Untitled-1.ipynb?jupyter-notebook"
    },
    "cellTextDocuments": [
        {
            "uri": "vscode-notebook-cell:Untitled-1.ipynb?jupyter-notebook#W0sdW50aXRsZWQ%3D"
        }
    ]
}


2024-06-14 16:58:23.364 [info] [Trace - 4:58:23 PM] Sending notification 'textDocument/didClose'.
2024-06-14 16:58:23.364 [info] Params: {
    "textDocument": {
        "uri": "vscode-notebook-cell:Untitled-1.ipynb?jupyter-notebook#W0sdW50aXRsZWQ%3D"
    }
}


2024-06-14 16:58:23.365 [info] 1173.763966200s ERROR ruff:main ruff_server::server::api: An error occurred while running textDocument/didClose: Unable to take snapshot for document with URL vscode-notebook-cell:Untitled-1.ipynb?jupyter-notebook#W0sdW50aXRsZWQ%3D

2024-06-14 16:58:23.365 [info] [Trace - 4:58:23 PM] Received notification 'window/showMessage'.
2024-06-14 16:58:23.365 [info] Params: {
    "message": "Ruff encountered a problem. Check the logs for more details.",
    "type": 1
}
```

---

_Closed by @T-256 on 2024-06-14 18:12_

---
