```yaml
number: 968
title: Low CPU saturation
type: issue
state: closed
author: MichaReiser
labels:
  - performance
assignees: []
created_at: 2025-08-11T09:51:44Z
updated_at: 2025-08-11T19:58:35Z
url: https://github.com/astral-sh/ty/issues/968
synced_at: 2026-01-10T02:06:24Z
```

# Low CPU saturation

---

_Issue opened by @MichaReiser on 2025-08-11 09:51_

I noticed that running ty on a large project overall shows low CPU saturation (specifically the longer checking runs)

https://github.com/user-attachments/assets/3efc9db9-51e6-4a80-af0f-16465b98d1ef

I did record a CPU profile and one thing that stands out is that we spend a lot of time in salsa's `intern_id` for `CallableType` 

<img width="1555" height="465" alt="Image" src="https://github.com/user-attachments/assets/8a13c057-f8e9-44a9-baaf-8a2d7b7afb62" />

([full profile](https://share.firefox.dev/3J7UomG))

and 

<img width="1555" height="465" alt="Image" src="https://github.com/user-attachments/assets/aa4c00c7-fa27-4a40-8ece-0c2590044acd" />

All call sites come from `ReachabilityConstraints::evaluate`. 

Ideally, we'd reduce the cost of interning in salsa. If that doesn't work, we may want to take a look if we can reduce the number of callable types that we construct.


---

_Label `performance` added by @MichaReiser on 2025-08-11 09:51_

---

_Closed by @MichaReiser on 2025-08-11 19:58_

---
