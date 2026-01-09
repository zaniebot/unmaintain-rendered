---
number: 15205
title: How to manage multiple virtual environments in a single uv project?
type: issue
state: open
author: ziminz19
labels:
  - question
assignees: []
created_at: 2025-08-10T17:32:32Z
updated_at: 2025-08-11T10:49:25Z
url: https://github.com/astral-sh/uv/issues/15205
synced_at: 2026-01-07T13:12:19-06:00
---

# How to manage multiple virtual environments in a single uv project?

---

_Issue opened by @ziminz19 on 2025-08-10 17:32_

### Question

I have a ML project repo that is created by uv. Now, I’m trying to merge an eval folder from [another repo](https://github.com/PRIME-RL/PRIME/tree/main/eval) to my main repo created by uv, so that other people can reproduce my numbers. However, I need to use separate virtual envs to run different benchmarks’ evaluation scripts because the math parsing dependencies are different (as stated in their repo). 

Can I put multiple venv under the same project? Or is there better way to handle the situation where two different versions of a package are needed for different python scripts in the project?

### Platform

Linux

### Version

uv 0.8.0

---

_Label `question` added by @ziminz19 on 2025-08-10 17:32_

---

_Comment by @hoechenberger on 2025-08-11 10:49_

Hello, have you considered using dependency groups for your different test setups?

---
