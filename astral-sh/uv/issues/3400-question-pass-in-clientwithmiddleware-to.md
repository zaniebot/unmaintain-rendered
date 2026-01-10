```yaml
number: 3400
title: "Question: Pass in `ClientWithMiddleware` to `RegistryClient` and `Builder`"
type: issue
state: open
author: tdejager
labels:
  - rustlib
assignees: []
created_at: 2024-05-06T09:38:48Z
updated_at: 2024-05-15T00:15:55Z
url: https://github.com/astral-sh/uv/issues/3400
synced_at: 2026-01-10T05:31:37Z
```

# Question: Pass in `ClientWithMiddleware` to `RegistryClient` and `Builder`

---

_Issue opened by @tdejager on 2024-05-06 09:38_

For use in pixi, it would be great if we could use our own middleware for the Reqwest Client, instead of having to rely on the middleware that uv provides :)

Would you guys be open for directly accepting a `ClientWithMiddleware` in the `RegistryClient` (I could make a PR for this)? I think this addition maybe does not make a lot of sense from a uv-as-a-binary perspective, as you most likely want to control your own middleware. But I believe it does make sense from a library perspective, as library users can add their own authentication middleware and whatnot. There is an open question in the design if it should be layered on top of your middleware or replace it, again from a library perspective I think replacing it make sense.

How would you feel about this?

---

_Label `rustlib` added by @charliermarsh on 2024-05-15 00:15_

---
