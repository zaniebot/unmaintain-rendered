---
number: 2640
title: Configuration file Support
type: issue
state: closed
author: RianKoja
labels:
  - duplicate
assignees: []
created_at: 2024-03-24T20:31:33Z
updated_at: 2024-03-24T20:41:14Z
url: https://github.com/astral-sh/uv/issues/2640
synced_at: 2026-01-10T01:23:20Z
---

# Configuration file Support

---

_Issue opened by @RianKoja on 2024-03-24 20:31_

https://github.com/astral-sh/uv/blob/a7b251f65e6d24855185c49bcbf64fc8bdfd7861/crates/uv/src/compat/mod.rs#L161

According to this line of code, there is no support for a configuration file. Is there any method to persistently set configurations?

What I used to wiht pip was:
```
pip config set global.trusted-host <my local nentwork pypi-server IP:port>
pip config set global.index-url http://<my local nentwork pypi-server IP:port>
```

I don't want to specify this all the time. Is there a workaround? I would need this both for windows and linux.

---

_Comment by @charliermarsh on 2024-03-24 20:36_

You can specify an index URL via environment variables (UV_INDEX_URL). But in general I’d recommend participating in the conversation here, since this request is mostly a duplicate of that issue: https://github.com/astral-sh/uv/issues/1404#issuecomment-2015778851.

---

_Comment by @charliermarsh on 2024-03-24 20:37_

(Merging into that issue.)

---

_Closed by @charliermarsh on 2024-03-24 20:37_

---

_Label `duplicate` added by @zanieb on 2024-03-24 20:41_

---
