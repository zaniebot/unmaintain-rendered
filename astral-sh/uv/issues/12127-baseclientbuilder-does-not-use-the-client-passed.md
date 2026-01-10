---
number: 12127
title: BaseClientBuilder does not use the client passed in via self.client() function
type: issue
state: closed
author: gzm55
labels:
  - bug
  - good first issue
  - internal
assignees: []
created_at: 2025-03-12T01:56:30Z
updated_at: 2025-03-12T10:43:23Z
url: https://github.com/astral-sh/uv/issues/12127
synced_at: 2026-01-10T01:25:15Z
---

# BaseClientBuilder does not use the client passed in via self.client() function

---

_Issue opened by @gzm55 on 2025-03-12 01:56_

### Summary

code: 

Although BaseClientBuilder [accepts](https://github.com/astral-sh/uv/blob/main/crates/uv-client/src/base_client.rs#L131) a custom client , it seems the `self.build()` always create a new client from the ground. Is this a mistake or a behavior is by design?

### Platform

all

### Version

all

### Python version

all

---

_Label `bug` added by @gzm55 on 2025-03-12 01:56_

---

_Comment by @zanieb on 2025-03-12 02:04_

That looks like an unused method to me, thanks!

---

_Label `good first issue` added by @zanieb on 2025-03-12 02:04_

---

_Label `internal` added by @zanieb on 2025-03-12 02:04_

---

_Comment by @zanieb on 2025-03-12 02:04_

(I think we should remove it and see if anything breaks)

---

_Comment by @gzm55 on 2025-03-12 02:14_

> (I think we should remove it and see if anything breaks)

yes, break the project call this function, but should be fixed easily.

---

_Referenced in [prefix-dev/pixi#3334](../../prefix-dev/pixi/issues/3334.md) on 2025-03-12 02:29_

---

_Comment by @gzm55 on 2025-03-12 03:13_

@zanieb ExtraMiddleware is not exported, then we cannot direct call `base_client::extra_middleware()`.

---

_Referenced in [astral-sh/uv#12133](../../astral-sh/uv/pulls/12133.md) on 2025-03-12 10:11_

---

_Closed by @konstin on 2025-03-12 10:43_

---

_Closed by @konstin on 2025-03-12 10:43_

---

_Referenced in [prefix-dev/pixi#3320](../../prefix-dev/pixi/pulls/3320.md) on 2025-03-12 15:52_

---

_Referenced in [prefix-dev/pixi#3306](../../prefix-dev/pixi/pulls/3306.md) on 2025-03-13 03:00_

---
