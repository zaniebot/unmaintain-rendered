```yaml
number: 6587
title: "Docs: Add Installation note for PowerShell 7.X"
type: pull_request
state: closed
author: jon-ton
labels:
  - documentation
  - releases
assignees: []
base: main
head: jonton-docs-windows-powershell
created_at: 2024-08-24T16:44:59Z
updated_at: 2024-09-17T17:04:33Z
url: https://github.com/astral-sh/uv/pull/6587
synced_at: 2026-01-12T16:07:26Z
```

# Docs: Add Installation note for PowerShell 7.X

---

_@jon-ton_

## Summary

If using PowerShell Core (7.X) on Windows the user would get an execution policy error even if set to RemoteSigned as the actual script would execute within Windows PowerShell. 

## Test Plan

Tested locally. Docs only. 


---

_@samypr100 reviewed on 2024-08-24 23:30_

---

_Review comment by @samypr100 on `docs/getting-started/installation.md`:21 on 2024-08-24 23:30_

iirc this would also apply to 6.x; might be worth saying something along the lines of 6.x or above

---

_@jon-ton reviewed on 2024-08-25 12:01_

---

_Review comment by @jon-ton on `docs/getting-started/installation.md`:21 on 2024-08-25 12:01_

Do more Windows users think of it in terms of Windows PowerShell vs. PowerShell Core, or 5.x vs 6/7.x?

---

_@samypr100 reviewed on 2024-08-25 20:36_

---

_Review comment by @samypr100 on `docs/getting-started/installation.md`:21 on 2024-08-25 20:36_

At least in my experience I do say `PowerShell Core` over 6.x/7.x, but unsure about the community.

---

_Label `documentation` added by @zanieb on 2024-08-26 17:09_

---

_Label `release` added by @zanieb on 2024-08-26 17:09_

---

_Assigned to @zanieb by @zanieb on 2024-08-26 17:10_

---

_Comment by @zanieb on 2024-09-17 17:00_

I believe this addressed with `-ExecutionPolicy ByPass` now.

Thanks for putting up the pull request though!

---

_Closed by @zanieb on 2024-09-17 17:00_

---

_Comment by @zanieb on 2024-09-17 17:04_

And don't hesitate to reach out if the current documentation doesn't help.

---
