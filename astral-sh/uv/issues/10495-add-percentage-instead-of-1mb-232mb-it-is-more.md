---
number: 10495
title: add percentage instead of 1MB/232MB it is  more compatible
type: issue
state: open
author: looijijohn
labels:
  - tracing
  - needs-decision
  - cli
assignees: []
created_at: 2025-01-11T13:54:10Z
updated_at: 2025-01-13T14:53:03Z
url: https://github.com/astral-sh/uv/issues/10495
synced_at: 2026-01-10T01:24:54Z
---

# add percentage instead of 1MB/232MB it is  more compatible

---

_Issue opened by @looijijohn on 2025-01-11 13:54_

uv pip install langflow

![Image](https://github.com/user-attachments/assets/ab27f905-8b28-43c9-a5eb-b1bc9be11b21)

---

_Comment by @shauneccles on 2025-01-12 00:02_

To my eyes the progress bar makes more sense than a %, since it is a visual representation of %.

The filesize done/remaining for a "big" wheel gives my brain an idea of how long it should take since I know how fast I (usually) download things at.

Adding a % would clutter things to me.

What might be nicer is aligning the graphs and thus the sizes to the longest text in the dependency tree so that it "lines up".

Just my thoughts.

---

_Label `tracing` added by @zanieb on 2025-01-12 01:10_

---

_Label `cli` added by @zanieb on 2025-01-12 01:10_

---

_Label `needs-decision` added by @zanieb on 2025-01-12 01:10_

---

_Comment by @morotti on 2025-01-13 14:52_

personally, I think the size is more meaningful.
I'd say it's essential for large packages on slow connection, something like torch/tensorflow is above 1 GB and take hours to download on broadband. 
it's not useful to show "XX%", it's better to show X/YYY MB, so the user can understand the package is enormous and the internet connection is meh. showing a stale percentage will actually mislead users into thinking packages are not downloading.

---
