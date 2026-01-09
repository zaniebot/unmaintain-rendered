---
number: 6904
title: "Error on `uv export --package uv`"
type: issue
state: closed
author: minmax
labels: []
assignees: []
created_at: 2024-09-01T01:53:07Z
updated_at: 2024-09-01T02:56:19Z
url: https://github.com/astral-sh/uv/issues/6904
synced_at: 2026-01-07T13:12:17-06:00
---

# Error on `uv export --package uv`

---

_Issue opened by @minmax on 2024-09-01 01:53_

```bash
uv init
uv add uv
uv export --package uv

error: Package `uv` not found in workspace
```

but export without `--package` works
```bask
uv export

uv==0.4.1
```

`uv 0.4.1`


---

_Comment by @minmax on 2024-09-01 02:56_

I misunderstood the meaning of the `--package` parameter, I think the issuer can be closed.

---

_Closed by @minmax on 2024-09-01 02:56_

---
