---
number: 5911
title: "`uv remove` summary is not intutive"
type: issue
state: closed
author: zanieb
labels:
  - cli
  - preview
assignees: []
created_at: 2024-08-08T14:25:06Z
updated_at: 2024-08-08T17:54:43Z
url: https://github.com/astral-sh/uv/issues/5911
synced_at: 2026-01-10T01:23:54Z
---

# `uv remove` summary is not intutive

---

_Issue opened by @zanieb on 2024-08-08 14:25_

e.g.

```
❯ uv remove anyio
Resolved 16 packages in 5ms
   Built example @ file:///Users/zb/workspace/example
Prepared 1 package in 165ms
Uninstalled 1 package in 0.41ms
Installed 1 package in 1ms
 - example==0.1.0 (from file:///Users/zb/workspace/example)
 + example==0.1.0 (from file:///Users/zb/workspace/example)
```

I want to see that `anyio` was removed but it's hidden in a summary.

---

_Label `cli` added by @zanieb on 2024-08-08 14:25_

---

_Label `preview` added by @zanieb on 2024-08-08 14:25_

---

_Comment by @charliermarsh on 2024-08-08 17:09_

Are you sure anyio was removed?

---

_Comment by @charliermarsh on 2024-08-08 17:09_

I don't think it was.

---

_Comment by @zanieb on 2024-08-08 17:40_

Oh... weird. Not sure how I got in that state?

```
❯ uv init
Initialized project `example`
❯ uv add anyio -q
❯ uv remove anyio
warning: `uv remove` is experimental and may change without warning
Resolved 1 package in 3ms
   Built example @ file:///Users/zb/workspace/example
Prepared 1 package in 155ms
Uninstalled 4 packages in 4ms
Installed 1 package in 0.82ms
 - anyio==4.4.0
 - example==0.1.0 (from file:///Users/zb/workspace/example)
 + example==0.1.0 (from file:///Users/zb/workspace/example)
 - idna==3.7
 - sniffio==1.3.1
 ```

---

_Closed by @zanieb on 2024-08-08 17:41_

---

_Comment by @charliermarsh on 2024-08-08 17:54_

Maybe you ran with `--no-sync`...? So it was present in your lockfile but not your env?

---
