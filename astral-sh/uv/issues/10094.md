```yaml
number: 10094
title: "uv add doesn't support URL fragments (e.g. subdirectory)"
type: issue
state: closed
author: MrNaif2018
labels:
  - bug
assignees: []
created_at: 2024-12-22T14:54:08Z
updated_at: 2024-12-22T15:23:28Z
url: https://github.com/astral-sh/uv/issues/10094
synced_at: 2026-01-10T04:36:21Z
```

# uv add doesn't support URL fragments (e.g. subdirectory)

---

_Issue opened by @MrNaif2018 on 2024-12-22 14:54_

```
$ /Users/alex/.cargo/bin/uv add "karrio @ https://github.com/karrioapi/karrio/archive/v2024.12rc9.zip#subdirectory=modules/sdk"
error: Failed to generate package metadata for `testbug==1.0.0 @ virtual+.`
  Caused by: Failed to parse entry: `karrio`
  Caused by: Fragments are not allowed in URLs: `https://github.com/karrioapi/karrio/archive/v2024.12rc9.zip#subdirectory=modules/sdk`
```

Continuing from #10088 

---

_Label `bug` added by @charliermarsh on 2024-12-22 14:57_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-22 15:11_

---

_Closed by @charliermarsh on 2024-12-22 15:23_

---
