---
number: 11649
title: "[feature request] pretty progress bar like in pip"
type: issue
state: closed
author: gmankab
labels:
  - enhancement
assignees: []
created_at: 2025-02-20T00:03:34Z
updated_at: 2025-03-14T00:40:17Z
url: https://github.com/astral-sh/uv/issues/11649
synced_at: 2026-01-07T13:12:18-06:00
---

# [feature request] pretty progress bar like in pip

---

_Issue opened by @gmankab on 2025-02-20 00:03_

### Summary

hello!

pip has very pretty progress bar, and uv progress bar looks worse compared to pip

can you implement same progress bar design as in pip?


### Example

_No response_

---

_Label `enhancement` added by @gmankab on 2025-02-20 00:03_

---

_Comment by @zanieb on 2025-02-20 00:12_

:( you don't think it looks nice?

Seems unlikely we'll just change to another progress bar without a bit more motivation.

I believe we do far more operations in parallel, which makes a nice progress bar design harder.

Can you share some comparisons? Explain why one is preferable?

---

_Comment by @gmankab on 2025-02-20 00:56_

![Image](https://github.com/user-attachments/assets/2e6f7d76-02b8-4d41-9ece-93e85b4dfea0)

![Image](https://github.com/user-attachments/assets/8d81ef8a-d7fb-4228-8825-5cac4c6aa14c)

pip progress bar looks much more nicer for me, but feel free to close issue if you don't think so

---

_Closed by @charliermarsh on 2025-03-14 00:40_

---

_Closed by @charliermarsh on 2025-03-14 00:40_

---
