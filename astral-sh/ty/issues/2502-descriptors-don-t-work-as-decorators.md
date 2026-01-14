```yaml
number: 2502
title: "Descriptors don't work as decorators."
type: issue
state: closed
author: vighneshbirodkar
labels: []
assignees: []
created_at: 2026-01-14T21:48:23Z
updated_at: 2026-01-14T23:08:09Z
url: https://github.com/astral-sh/ty/issues/2502
synced_at: 2026-01-14T23:42:12Z
```

# Descriptors don't work as decorators.

---

_@vighneshbirodkar_

### Summary

https://play.ty.dev/09e0c8fe-ace6-4207-ad81-8b6ab38acf2b

The same code works with based pyright
https://basedpyright.com/?typeCheckingMode=all&code=MYGwhgzhAEAiCmFgCcCWAHALge2QCgEoAuAKBOgugBN4AzaAfQdQDtVMm8J4RaAaRsXKURAOnHCKNekwDm8Dgy49%2B0AFQMC0ALQA%2BaK0ykRI5AoCuyFtAAMZEqEgwAsgE8AwuCjHKAAQRIaFi4ktCOUHDmALZRrj4m0OhOZHEGLJjQALzQbp5OorDRsUA

### Version

_No response_

---

_Comment by @carljm on 2026-01-14 23:08_

Thanks for the report! Descriptors do work as decorators, but we don't yet respect the return value of descriptors on classes. The same code works if decorating a function rather than a class: https://play.ty.dev/4009b4ae-c69f-42f0-afbe-9081a38b6b80

This is #143 

---

_Closed by @carljm on 2026-01-14 23:08_

---
