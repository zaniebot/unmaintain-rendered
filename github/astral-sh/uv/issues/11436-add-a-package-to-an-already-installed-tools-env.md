---
number: 11436
title: Add a package to an already installed tools env
type: issue
state: open
author: nixjdm
labels:
  - enhancement
  - needs-decision
assignees: []
created_at: 2025-02-12T05:33:08Z
updated_at: 2025-02-12T15:02:48Z
url: https://github.com/astral-sh/uv/issues/11436
synced_at: 2026-01-07T13:12:18-06:00
---

# Add a package to an already installed tools env

---

_Issue opened by @nixjdm on 2025-02-12 05:33_

### Summary

Follow-up from #11435 

I don't see a way to _add_ a tool to an already existing env. That would be very handy, particularly in the case of adding libraries over time to an ipython-based env. Otherwise a potentially long, growing chain of `--with ` parameters would be needed.

I personally reach for ipython and a few other things frequently, outside of projects, for many things. I'd love to grow that particular tool env to have many of the libs installed I regularly reach for, but not have it be a defined project.

### Example

_No response_

---

_Label `enhancement` added by @nixjdm on 2025-02-12 05:33_

---

_Comment by @zanieb on 2025-02-12 15:01_

This is intentional. The tool environments are intended to be immutable. I'm very hesitant to add this.

---

_Comment by @zanieb on 2025-02-12 15:02_

There's commentary on this in the original RFC, e.g., https://github.com/astral-sh/uv/issues/3560#issuecomment-2110365497

---

_Label `needs-decision` added by @zanieb on 2025-02-12 15:02_

---
