```yaml
number: 75
title: Handle cancellation requests in LSP
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-04-06T08:23:29Z
updated_at: 2025-05-28T08:59:31Z
url: https://github.com/astral-sh/ty/issues/75
synced_at: 2026-01-10T02:34:09Z
```

# Handle cancellation requests in LSP

---

_Issue opened by @MichaReiser on 2025-04-06 08:23_

The LSP supports cancellation requests to "cancel" requested information that the client no longer needs, e.g. because the document was closed or changed since the request was sent. 

We should add support for cancellation requests. One way of implementing this is to keep a map of pending-requests (keyed by request id) and skipping requests if their id isn't in the map. Cancelling a request is than as simple as removing the id from said map (which we also need to do when completing requests). 



---

_Label `server` added by @MichaReiser on 2025-04-06 08:23_

---

_Renamed from "[red-knot] Handle cancellation requests in LSP" to "Handle cancellation requests in LSP" by @MichaReiser on 2025-05-07 15:13_

---

_Closed by @MichaReiser on 2025-05-28 08:59_

---
