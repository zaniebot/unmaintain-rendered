```yaml
number: 12097
title: How to list unused dependencies
type: issue
state: closed
author: andreas-vester
labels:
  - question
assignees: []
created_at: 2025-03-10T14:54:32Z
updated_at: 2025-06-10T12:50:37Z
url: https://github.com/astral-sh/uv/issues/12097
synced_at: 2026-01-12T16:00:55Z
```

# How to list unused dependencies

---

_@andreas-vester_

### Question

Is there a way to list unused dependencies, which I can safely remove from ``pyproject.toml``?

### Platform

_No response_

### Version

uv 0.6.5

---

_Label `question` added by @andreas-vester on 2025-03-10 14:54_

---

_Comment by @jslatane on 2025-04-14 16:37_

I also had this question but realized it may be out of scope for `uv`. There is a tool that does this called `deptry`:  
<https://deptry.com/>

It seems to work as advertised, at least on the very minimal example I tried just now.

---

_Comment by @zanieb on 2025-06-10 12:50_

We'll track this in https://github.com/astral-sh/uv/issues/13904

---

_Closed by @zanieb on 2025-06-10 12:50_

---
