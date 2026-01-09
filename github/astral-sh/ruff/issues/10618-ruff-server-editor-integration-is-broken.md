---
number: 10618
title: "`ruff server` editor integration is broken"
type: issue
state: closed
author: snowsignal
labels:
  - server
assignees: []
created_at: 2024-03-26T16:26:20Z
updated_at: 2024-03-26T20:53:57Z
url: https://github.com/astral-sh/ruff/issues/10618
synced_at: 2026-01-07T13:12:15-06:00
---

# `ruff server` editor integration is broken

---

_Issue opened by @snowsignal on 2024-03-26 16:26_

Diagnostics weren't showing up when debugging the new Ruff VS Code extension locally. While investigating this issue, I found these logs, which happen immediately following client/server initialization:
```
[info] [Trace - <TIME>] Sending request 'textDocument/codeAction - (2)'.
[info] [Trace - <TIME>] Received request 'client/registerCapability - (1)'.
...
ERROR ruff_server::server Expected request or notification, got response instead: Response { id: RequestId(I32(1)), result: None, error: None }
```

The issue here is that the client fires off a `codeAction` request before responding to the `registerCapability` request from the client. On the server side, we're awaiting and then flushing an incoming response to the `registerCapability` request. Instead, we flush the `codeAction` request by mistake, and this lack of response breaks the LSP integration because the JSON-RPC protocol requires that every request has a response.

This is also why the error log later appears. Because we only flush the first message we receive, the response from the client is handled in the main event loop, which isn't expected.

---

_Label `server` added by @snowsignal on 2024-03-26 16:26_

---

_Renamed from "`ruff server` may silently ignore a code action request when debugging locally, which breaks editor integration" to "`ruff server` editor integration is broken" by @snowsignal on 2024-03-26 16:56_

---

_Assigned to @snowsignal by @snowsignal on 2024-03-26 18:15_

---

_Referenced in [astral-sh/ruff#10620](../../astral-sh/ruff/pulls/10620.md) on 2024-03-26 18:23_

---

_Closed by @snowsignal on 2024-03-26 20:53_

---

_Referenced in [astral-sh/ruff#10597](../../astral-sh/ruff/pulls/10597.md) on 2024-03-26 23:10_

---

_Referenced in [astral-sh/ruff#10649](../../astral-sh/ruff/issues/10649.md) on 2024-04-04 22:39_

---
