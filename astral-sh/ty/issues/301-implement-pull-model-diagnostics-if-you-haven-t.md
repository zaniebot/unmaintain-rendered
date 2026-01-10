```yaml
number: 301
title: "Implement pull model diagnostics (if you haven't already)"
type: issue
state: closed
author: w0rp
labels:
  - server
assignees: []
created_at: 2025-05-10T03:50:21Z
updated_at: 2025-05-10T07:02:20Z
url: https://github.com/astral-sh/ty/issues/301
synced_at: 2026-01-10T02:34:09Z
```

# Implement pull model diagnostics (if you haven't already)

---

_Issue opened by @w0rp on 2025-05-10 03:50_

Hello! I did have to wait a good few years for Pyright to implement pull model diagnostics, and it would be a shame to wait years again for the same support from `ty`. Have a look at the history behind this change. See: https://github.com/microsoft/pyright/issues/3452

I was the original dev who suggested that LSP should have a pull model while working on ALE. Having support for pull diagnostics means the client has control over when diagnostics will be sent, and if implemented correctly on the server side I believe it can also be an optimization where the server does less processing overall. Note that it's important for servers to implement both push and pull models.

I haven't read the code or tested `ty` yet, so may have already implemented this. In any case, I'll add a linter definition for `ty` to ALE.

---

_Label `server` added by @carljm on 2025-05-10 05:41_

---

_Comment by @MichaReiser on 2025-05-10 07:02_

We only support pull diagnostics at the moment 

---

_Closed by @MichaReiser on 2025-05-10 07:02_

---
