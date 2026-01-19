```yaml
number: 2135
title: "Windows | VSCode | Code completion doesn't work for local variables"
type: issue
state: open
author: LittleLittleCloud
labels:
  - needs-mre
  - server
  - completions
assignees: []
created_at: 2025-12-20T20:16:23Z
updated_at: 2026-01-19T08:50:00Z
url: https://github.com/astral-sh/ty/issues/2135
synced_at: 2026-01-19T09:26:44Z
```

# Windows | VSCode | Code completion doesn't work for local variables

---

_@LittleLittleCloud_

### Summary

Code completion doesn't work for local variables. from the video below you can see if I disable `ty` language server, the completion for `abc` shows up. After I enable `ty` language server, the code completion for local variables doesn't work, but code completion for property/method name still working

https://github.com/user-attachments/assets/97fc6f68-7ac2-4071-87f0-97ec6b7980bb

I think it's duplicate/regression of this issue #1864 

### Version

ruff==0.14.10 + ty==0.0.4 + ty extension==2025.74.0

---

_Label `server` added by @carljm on 2025-12-22 20:05_

---

_Comment by @dhruvmanila on 2025-12-23 08:43_

Thanks for the report! It seems to be working on my end:

https://github.com/user-attachments/assets/81bb82dc-8cd0-4c10-85c7-bf6fb4bd58ba

Could it be that whichever extension (or builtin feature) is providing the ghost text is somehow interfering with the completion request? Can you use `"ty.trace.server": "messages"` setting and check whether you see a `textDocument/completion` request under "ty Language Server Trace" channel:

```
[Trace - 14:11:54] Sending request 'textDocument/completion - (310)'.
...
[Trace - 14:11:54] Received response 'textDocument/completion - (310)' in 128ms.
```

https://github.com/user-attachments/assets/a8f8f5cf-7d20-4dcb-8a86-d79ae4549b8e

---

_Label `completions` added by @dhruvmanila on 2025-12-23 08:43_

---

_Label `needs-mre` added by @MichaReiser on 2025-12-23 08:53_

---

_Comment by @LittleLittleCloud on 2025-12-23 22:15_

@dhruvmanila Here's the trace message from my end. I put the first `completion` request in bold. It seems that the completion request didn't receive response immediately until get cancelled few seconds later 

```
[Trace - 2:12:36 PM] Sending notification 'textDocument/didChange'.
[Trace - 2:12:36 PM] Sending request 'textDocument/diagnostic - (1515)'.
[Trace - 2:12:36 PM] Received response 'textDocument/diagnostic - (1515)' in 2ms.
**[Trace - 2:12:36 PM] Sending request 'textDocument/completion - (1516)'.**
[Trace - 2:12:36 PM] Sending request 'textDocument/inlayHint - (1517)'.
[Trace - 2:12:36 PM] Sending request 'textDocument/diagnostic - (1518)'.
[Trace - 2:12:36 PM] Received response 'textDocument/inlayHint - (1517)' in 12ms.
[Trace - 2:12:36 PM] Received response 'textDocument/diagnostic - (1518)' in 17ms.
[Trace - 2:12:36 PM] Sending request 'textDocument/semanticTokens/full - (1519)'.
[Trace - 2:12:36 PM] Sending request 'textDocument/codeAction - (1520)'.
[Trace - 2:12:36 PM] Sending request 'textDocument/documentSymbol - (1521)'.
[Trace - 2:12:36 PM] Received response 'textDocument/semanticTokens/full - (1519)' in 7ms.
[Trace - 2:12:36 PM] Received response 'textDocument/documentSymbol - (1521)' in 7ms.
[Trace - 2:12:36 PM] Sending request 'textDocument/diagnostic - (1522)'.
[Trace - 2:12:36 PM] Received response 'textDocument/diagnostic - (1522)' in 2ms.
[Trace - 2:12:36 PM] Sending request 'textDocument/documentSymbol - (1523)'.
[Trace - 2:12:36 PM] Received response 'textDocument/documentSymbol - (1523)' in 3ms.
[Trace - 2:12:36 PM] Sending request 'textDocument/inlayHint - (1524)'.
[Trace - 2:12:36 PM] Received response 'textDocument/inlayHint - (1524)' in 1ms.
[Trace - 2:12:37 PM] Sending notification 'workspace/didChangeWatchedFiles'.
[Trace - 2:12:37 PM] Received request 'workspace/diagnostic/refresh - (2694)'.
[Trace - 2:12:37 PM] Sending response 'workspace/diagnostic/refresh - (2694)'. Processing request took 1ms
[Trace - 2:12:37 PM] Sending request 'textDocument/diagnostic - (1525)'.
[Trace - 2:12:37 PM] Sending request 'textDocument/diagnostic - (1526)'.
[Trace - 2:12:37 PM] Sending request 'textDocument/diagnostic - (1527)'.
[Trace - 2:12:37 PM] Received request 'workspace/inlayHint/refresh - (2695)'.
[Trace - 2:12:37 PM] Sending response 'workspace/inlayHint/refresh - (2695)'. Processing request took 1ms
[Trace - 2:12:37 PM] Received response 'textDocument/codeAction - (1520)' in 1226ms. Request failed: content modified (-32801).
[Trace - 2:12:37 PM] Received response 'textDocument/diagnostic - (1525)' in 4ms.
[Trace - 2:12:37 PM] Received response 'textDocument/diagnostic - (1526)' in 5ms.
[Trace - 2:12:37 PM] Received response 'textDocument/diagnostic - (1527)' in 6ms.
[Trace - 2:12:45 PM] Sending notification '$/cancelRequest'.
**[Trace - 2:12:45 PM] Received response 'textDocument/completion - (1516)' in 8808ms. Request failed: request was cancelled by client (-32800).**
[Trace - 2:12:45 PM] Sending request 'textDocument/diagnostic - (1528)'.
[Trace - 2:12:45 PM] Received response 'textDocument/diagnostic - (1528)' in 2ms.
[Trace - 2:12:45 PM] Sending request 'textDocument/diagnostic - (1529)'.
[Trace - 2:12:45 PM] Received response 'textDocument/diagnostic - (1529)' in 1ms.
[Trace - 2:12:45 PM] Sending request 'textDocument/diagnostic - (1530)'.
[Trace - 2:12:45 PM] Received response 'textDocument/diagnostic - (1530)' in 2ms.
```

---

_Comment by @dhruvmanila on 2025-12-24 05:25_

Thanks for the logs, those are helpful!

> ```
> **[Trace - 2:12:45 PM] Received response 'textDocument/completion - (1516)' in 8808ms. Request failed: request was cancelled by client (-32800).**
> ```

It seems that the completion request is taking a long time to complete as it got cancelled after ~8 seconds.

Even the code action request took a long time (~1.2 second) and got cancelled:

> ```
> [Trace - 2:12:37 PM] Received response 'textDocument/codeAction - (1520)' in 1226ms. Request failed: content modified (-32801).
> ```

It might be useful to get some insights on the codebase like how it's structured, is it a large codebase, what are the dependencies? If the codebase is open-source, could you provide us with the link?

---

_Comment by @LittleLittleCloud on 2025-12-24 06:32_

@dhruvmanila I do confirm that the code completion works on a fresh new project so maybe this bug only appears under certain conditions. Let me try creating a minimal reproducable example for you.

---

_Comment by @dhruvmanila on 2025-12-24 06:54_

> Let me try creating a minimal reproducable example for you.

That would be great, thank you!

---

_Comment by @MichaReiser on 2026-01-19 08:50_

@LittleLittleCloud have you been able to reproduce this again?

---
