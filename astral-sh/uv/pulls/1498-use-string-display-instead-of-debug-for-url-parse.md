```yaml
number: 1498
title: Use string display instead of debug for url parse trace
type: pull_request
state: merged
author: zanieb
labels:
  - tracing
assignees: []
merged: true
base: main
head: zb/url-trace
created_at: 2024-02-16T15:09:34Z
updated_at: 2024-02-16T15:13:17Z
url: https://github.com/astral-sh/uv/pull/1498
synced_at: 2026-01-10T15:33:24Z
```

# Use string display instead of debug for url parse trace

---

_Pull request opened by @zanieb on 2024-02-16 15:09_

e.g. 

`uv_client::html::parse url=https://download.pytorch.org/whl/torch_stable.html` 

instead of

`uv_client::html::parse url=Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("download.pytorch.org")), port: None, path: "/whl/torch_stable.html", query: None, fragment: None }`

---

_Label `enhancement` added by @zanieb on 2024-02-16 15:09_

---

_Label `enhancement` removed by @zanieb on 2024-02-16 15:12_

---

_Label `tracing` added by @zanieb on 2024-02-16 15:12_

---

_Merged by @zanieb on 2024-02-16 15:13_

---

_Closed by @zanieb on 2024-02-16 15:13_

---

_Branch deleted on 2024-02-16 15:13_

---
