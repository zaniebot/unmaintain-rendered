```yaml
number: 2134
title: "Intermittent \"channel closed\" errors when using `pypiserver` with high levels of concurrency"
type: issue
state: closed
author: zanieb
labels:
  - bug
  - internal
  - external
assignees: []
created_at: 2024-03-02T18:15:49Z
updated_at: 2024-05-11T18:10:53Z
url: https://github.com/astral-sh/uv/issues/2134
synced_at: 2026-01-12T15:58:35Z
```

# Intermittent "channel closed" errors when using `pypiserver` with high levels of concurrency

---

_@zanieb_

When testing against a local [`pypiserver`](https://pypi.org/project/pypiserver/), `uv` errors with

```
error: error sending request for url (http://localhost:3141/simple/albatross/): channel closed
    Caused by: channel closed
```

This is resolved by limiting parallelism of the test threads. There are no errors in the server logs.

Looks like this may be an upstream issue:

- https://github.com/seanmonstar/reqwest/issues/1221

---

_Label `internal` added by @zanieb on 2024-03-02 18:17_

---

_Label `bug` added by @zanieb on 2024-03-02 18:17_

---

_Comment by @patrick-kidger on 2024-05-10 14:53_

I'm hitting this error a lot when installing from a local server.

> This is resolved by limiting parallelism of the test threads. 

What is the appropriate way to limit the number of threads here please? :) I'd be happy to make things a bit slower to resolve this issue.

---

_Comment by @zanieb on 2024-05-11 02:14_

Sorry you're hitting it. I kind of think it's a pypiserver bug, I haven't seen this elsehwere. 

For limiting concurrency, see #3493 — we're about to release `UV_CONCURRENT_DOWNLOADS` for this purpose.

---

_Label `upstream` added by @zanieb on 2024-05-11 02:14_

---

_Comment by @patrick-kidger on 2024-05-11 15:07_

Ah, thank you! Indeed that seems to resolve things for me.

For the sake of any future readers: in my case the local server is an implementation I control. For reasons that are a little beyond my ken, switching the implementation from `http.server` to `flask` also seems to have resolved this issue (without needing to restrict `uv`'s concurrency).

---

_Closed by @zanieb on 2024-05-11 18:10_

---
