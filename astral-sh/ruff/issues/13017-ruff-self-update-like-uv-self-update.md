```yaml
number: 13017
title: ruff self update (like uv self update)
type: issue
state: open
author: gertcuykens
labels: []
assignees: []
created_at: 2024-08-20T19:53:54Z
updated_at: 2024-08-20T21:34:06Z
url: https://github.com/astral-sh/ruff/issues/13017
synced_at: 2026-01-10T11:09:55Z
```

# ruff self update (like uv self update)

---

_Issue opened by @gertcuykens on 2024-08-20 19:53_

 Can `ruff self update` be implemented please to be consistent with `uv self update` ?


---

_Comment by @charliermarsh on 2024-08-20 20:01_

I think the one downside here is that we'd then have to ship an HTTP stack with Ruff, which would probably increase the binary size quite significantly.

---

_Comment by @gertcuykens on 2024-08-20 21:31_

Maybe we don't need a full HTTP stack but just a udp socket that collects chunks of data from a fixed ip?

---
