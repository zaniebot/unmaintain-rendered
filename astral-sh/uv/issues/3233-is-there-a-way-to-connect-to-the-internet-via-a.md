```yaml
number: 3233
title: Is there a way to connect to the Internet via a proxy server? 
type: issue
state: closed
author: tealgreen0503
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2024-04-24T05:52:20Z
updated_at: 2024-04-24T16:07:42Z
url: https://github.com/astral-sh/uv/issues/3233
synced_at: 2026-01-12T15:58:42Z
```

# Is there a way to connect to the Internet via a proxy server? 

---

_@tealgreen0503_

In pip, we can connect Internet via a proxy server using the `--proxy` option or by following the method described [here](https://pip.pypa.io/en/stable/user_guide/#using-a-proxy-server). How can we do the same in uv?

---

_Comment by @zanieb on 2024-04-24 14:14_

We support the standard `HTTP_PROXY`, `HTTPS_PROXY` and `ALL_PROXY` environment variables via `reqwest`

https://github.com/seanmonstar/reqwest/blob/580606308d94f20139548d4668e5d0f06068a70b/src/proxy.rs#L897-L920

We can add these to the README

---

_Label `documentation` added by @zanieb on 2024-04-24 14:14_

---

_Label `good first issue` added by @zanieb on 2024-04-24 14:14_

---

_Closed by @zanieb on 2024-04-24 16:07_

---
