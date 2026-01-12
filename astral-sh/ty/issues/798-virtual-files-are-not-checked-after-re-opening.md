```yaml
number: 798
title: Virtual files are not checked after re-opening from an unsaved edit
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - server
assignees: []
created_at: 2025-07-11T05:36:47Z
updated_at: 2025-07-11T14:27:28Z
url: https://github.com/astral-sh/ty/issues/798
synced_at: 2026-01-12T15:54:24Z
```

# Virtual files are not checked after re-opening from an unsaved edit

---

_@dhruvmanila_

### Summary

Steps to reproduce:
1. Open a new project in VS Code
2. Press <kbd>C-n</kbd> to open a new untitled file
3. Select the language as Python
4. Update the file content as `x`
5. Notice that an unresolved reference diagnostic popup
6. Close the file without saving it
7. Press <kbd>C-n</kbd> again to open a new untitled file (notice that this will have the same name as in step 2)
8. Select the language as Python
9. Update the file content as `x` again
10. Notice that no diagnostic popup

Make sure that `ty.diagnosticMode` is set to `workspace`.

https://github.com/user-attachments/assets/a81b0fa6-369e-447c-8bfc-0fe9d494622e

### Version

_No response_

---

_Label `bug` added by @dhruvmanila on 2025-07-11 05:36_

---

_Label `server` added by @dhruvmanila on 2025-07-11 05:36_

---

_Comment by @dhruvmanila on 2025-07-11 05:48_

Looking through the trace logs from server and the client, I've noticed a few things:

Server does not send any response which are considered invalid as per the server e.g., we receive a text document diagnostic request after an unsaved untitled file is closed and as the server has no snapshot available for it, the server will ignore the request: https://github.com/astral-sh/ruff/blob/b0b65c24ff01dc9095f17b3768cf2b9a336a5a8c/crates/ty_server/src/server/api.rs#L233-L236 but this means that the request is open from the client side as there was no response for this request. The client then isn't sending any `textDocument/diagnostic` request.

These are the trace logs for step (6) above. VS Code first clears the file content in the untitled file by sending a `didChange` notification which I think triggers the diagnostic request from the client. The server responds to that with an empty list because the file is empty. Then, VS Code sends a `didClose` notification to signal that the virtual file is closed which again is triggering the diagnostic request from the client. Here, the server has no information about the closed file so it doesn't respond to the request id 21. Then, I don't see any client request for diagnostic for the untitled file when I re-open it again.

```
[Trace - 10:58:47 AM] Sending notification 'textDocument/didChange'.
Params: {
    "textDocument": {
        "uri": "untitled:Untitled-1",
        "version": 3
    },
    "contentChanges": [
        {
            "range": {
                "start": {
                    "line": 0,
                    "character": 0
                },
                "end": {
                    "line": 0,
                    "character": 1
                }
            },
            "rangeLength": 1,
            "text": ""
        }
    ]
}


[Trace - 10:58:47 AM] Sending request 'textDocument/diagnostic - (20)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "untitled:Untitled-1"
    }
}


[Trace - 10:58:47 AM] Sending notification 'textDocument/didClose'.
Params: {
    "textDocument": {
        "uri": "untitled:Untitled-1"
    }
}


[Trace - 10:58:47 AM] Received response 'textDocument/diagnostic - (20)' in 5ms.
Result: {
    "items": [],
    "kind": "full"
}


[Trace - 10:58:47 AM] Sending request 'textDocument/diagnostic - (21)'.
Params: {
    "identifier": "ty",
    "textDocument": {
        "uri": "untitled:Untitled-1"
    }
}
```

So, I thought that we should send an error response back. This closes the request but it still doesn't make VS Code send us the diagnostic request when the same untitled file is re-opened.

---

_Comment by @dhruvmanila on 2025-07-11 06:08_

Well, sending a default success response does resolve the issue i.e., client starts sending the request back again as usual.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-07-11 06:08_

---

_Comment by @dhruvmanila on 2025-07-11 06:35_

I think we should return an error response but it only seems to be working when I use the `ServerCancelled` error code which makes sense for the diagnostic request but the handler isn't specific to the diagnostic request. And, even with this response, the client will retrigger the diagnostic request because:

> error: code and message set in case an exception happens during the diagnostic request. A server is also allowed to return and error with code ServerCancelled indicating that the server can’t compute the result right now. A server can return a [DiagnosticServerCancellationData](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#diagnosticServerCancellationData) data to indicate whether the client should re-trigger the request. If no data is provided it defaults to { retriggerRequest: true }:

---

_Closed by @dhruvmanila on 2025-07-11 14:27_

---
