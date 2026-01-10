```yaml
number: 824
title: Support streaming and caching for workspace diagnostics
type: issue
state: closed
author: dhruvmanila
labels:
  - performance
  - server
assignees: []
created_at: 2025-07-15T05:43:03Z
updated_at: 2025-08-04T10:26:40Z
url: https://github.com/astral-sh/ty/issues/824
synced_at: 2026-01-10T02:06:24Z
```

# Support streaming and caching for workspace diagnostics

---

_Issue opened by @dhruvmanila on 2025-07-15 05:43_

The current support for workspace diagnostics is implemented using the newly introduced [`workspace/diagnostic`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#workspace_diagnostic) endpoint which uses the pull model.

We noticed that VS Code keeps sending the workspace diagnostic request every ~2 seconds which is because the workspace diagnostics needs to be implemented as long-polling (https://github.com/microsoft/vscode-languageserver-node/issues/1261). This lead us to realize that the server could stream diagnostics via [`WorkspaceDiagnosticReportPartialResult`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#workspaceDiagnosticReportPartialResult). We spent some time discussing this internally and we've realized that this would be important to implement but is not a high priority and would be good to have before the beta release. For now, we're thinking of either of the following option leaning more towards option 2:

### Option 1

- When we get the first workspace diagnostic request, we keep this request open indefinitely and start streaming diagnostics for the entire workspace. This means that updating the diagnostics is controlled by the server.
- It might be useful to have this in a separate thread so as to not block the worker thread.
- Whenever any changes happen e.g., a file content changed, we queue up a `ReCheck` event for the corresponding `ProjectDatabase`. This event will be consumed by main loop and re-checks the project and streams the diagnostics aka refreshing them.
- In this case, the [previous result id] cannot be used because we won't be refreshing them for workspace diagnostics request.

### Option 2

- Implement the [previous result id] by hashing all the diagnostics for a document similar to how the Go language server does.
- When a workspace diagnostic request arrives, check whether it has previous result ids in the request payload:
    - If it does not, perform the check, compute the result ids and send the full diagnostic report
    - If it does, perform the check, compute the result ids and check it against the previous result ids. After filtering out the ones that are unchanged, if there are any remaining diagnostics, return them. But if there are none, we keep this request open.
    - Whenever any changes happen e.g., a file content changed, we ask to `ReCheck` for workspace diagnostics and then perform the above check again. At this point, if the diagnostics changed, return them and close the request. If it's not, do not close the request.

The reason that this is important to have is because currently we send the entire diagnostic payload every time VS Code asks us for workspace diagnostics which as mentioned is ~2 seconds. Even with salsa caching, we still would be spending a lot of time serializing and deserializing the payload which is expensive for large projects having dozens of diagnostics.

[previous result id]: https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#previousResultId

---

_Label `server` added by @dhruvmanila on 2025-07-15 05:43_

---

_Label `performance` added by @dhruvmanila on 2025-07-15 05:43_

---

_Added to milestone `Beta` by @dhruvmanila on 2025-07-17 15:30_

---

_Comment by @MichaReiser on 2025-07-21 16:21_

See https://github.com/astral-sh/ty/issues/863#issue-3248466289 for a case where not having this optimization results in a laggy VS code experience

---

_Closed by @MichaReiser on 2025-08-04 10:26_

---
