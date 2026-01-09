---
number: 2631
title: "SIM108: autofix crashes due to introduced syntax error"
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2023-02-07T18:42:31Z
updated_at: 2023-02-07T20:18:54Z
url: https://github.com/astral-sh/ruff/issues/2631
synced_at: 2026-01-07T13:12:14-06:00
---

# SIM108: autofix crashes due to introduced syntax error

---

_Issue opened by @spaceone on 2023-02-07 18:42_

`ruff --select SIM108 --fix`
```python
class JSONRPC(BaseComponent):

    def _on_request(self, event, req, res):
        if isinstance(params, dict):
            value = yield self.call(rpc.create(method, **params), self.rpc_channel)
        else:
            value = yield self.call(rpc.create(method, *params), self.rpc_channel)

```

---

_Label `bug` added by @charliermarsh on 2023-02-07 18:53_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-07 18:53_

---

_Comment by @charliermarsh on 2023-02-07 18:53_

Oho!

---

_Comment by @spaceone on 2023-02-07 19:34_

with `await` it seems not to be a syntax error (but looks strange):

```diff
$ ruff --select SIM108 --fix --diff foo.py
--- foo.py
+++ foo.py
@@ -1,7 +1,4 @@
 class JSONRPC(BaseComponent):
 
     async def _on_request(self, event, req, res):
-        if isinstance(params, dict):
-            value = await self.call(rpc.create(method, **params), self.rpc_channel)
-        else:
-            value = await self.call(rpc.create(method, *params), self.rpc_channel)
+        value = await self.call(rpc.create(method, **params), self.rpc_channel) if isinstance(params, dict) else await self.call(rpc.create(method, *params), self.rpc_channel)

```

---

_Comment by @charliermarsh on 2023-02-07 20:16_

Disabling for now, for yield and awaits. Need to fix the underlying bug.

---

_Referenced in [astral-sh/ruff#2637](../../astral-sh/ruff/pulls/2637.md) on 2023-02-07 20:17_

---

_Closed by @charliermarsh on 2023-02-07 20:18_

---
