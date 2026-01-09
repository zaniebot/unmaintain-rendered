---
number: 7072
title: uv roadmap and project direction
type: issue
state: closed
author: cambriancoder
labels:
  - question
assignees: []
created_at: 2024-09-05T06:59:15Z
updated_at: 2024-09-05T18:38:58Z
url: https://github.com/astral-sh/uv/issues/7072
synced_at: 2026-01-07T13:12:17-06:00
---

# uv roadmap and project direction

---

_Issue opened by @cambriancoder on 2024-09-05 06:59_

Hi,

Love this project! I've been tracking this project for only a couple weeks and the pace of feature development is incredible. 

My company is deciding on our next set of tools for Python application and library development (which is currently a mix of setuptools with make, and poetry), and I discounted uv due to its lack of universal lock file support only two weeks ago. If there was a roadmap with rough timelines it would help with our decision making e.g. I'm currently proposing hatch with uv as the installer backend, but uv now has build support, and will likely replace a lot of other hatch cli features at this rate. 

Is there a roadmap for future features/releases and/or a target feature set for a stable (v1) release?

---

_Comment by @MrFizzyBubbs on 2024-09-05 13:47_

https://github.com/orgs/astral-sh/projects/11

---

_Comment by @zanieb on 2024-09-05 13:54_

That project is actually relatively outdated, as those were tasks we wanted to accomplish before announcing all the recent new features.

We don't have a public roadmap and, if we add one, we're very unlikely to provide timelines. We do intend to continue to grow the features for parity with other project management tools like Hatch, Rye, and Poetry.

---

_Label `question` added by @zanieb on 2024-09-05 13:54_

---

_Closed by @cambriancoder on 2024-09-05 18:38_

---
