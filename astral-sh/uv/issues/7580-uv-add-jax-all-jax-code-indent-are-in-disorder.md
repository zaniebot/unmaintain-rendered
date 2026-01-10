---
number: 7580
title: uv add jax  ,    all  jax code indent are     in  disorder  
type: issue
state: closed
author: Super1Windcloud
labels:
  - needs-mre
assignees: []
created_at: 2024-09-20T11:39:21Z
updated_at: 2024-12-27T14:39:50Z
url: https://github.com/astral-sh/uv/issues/7580
synced_at: 2026-01-10T01:24:17Z
---

# uv add jax  ,    all  jax code indent are     in  disorder  

---

_Issue opened by @Super1Windcloud on 2024-09-20 11:39_


![图片](https://github.com/user-attachments/assets/43878069-fc07-45e7-88c2-c2e18fde3d5b)

# indent error 

![图片](https://github.com/user-attachments/assets/b9aa8d41-3dc5-4ef6-97f7-8aa1f9f4cd02)

win 11  
uv latest version 


---

_Comment by @charliermarsh on 2024-09-20 14:10_

Can you provide a reproduction? How did you get into this state?

---

_Label `needs-mre` added by @charliermarsh on 2024-09-20 14:10_

---

_Comment by @Super1Windcloud on 2024-09-20 14:42_

just   uv   add  jax , you can try   out to do  

---

_Comment by @Super1Windcloud on 2024-09-20 14:42_

uv  pip install jax    as  well 

---

_Comment by @notatallshaw on 2024-09-20 17:23_

JAX uses 2 space intending:

 * https://github.com/jax-ml/jax/blob/jaxlib-v0.4.32/jax/_src/tree_util.py#L332
 * https://github.com/jax-ml/jax/blob/jaxlib-v0.4.32/pyproject.toml#L112

The fact that your IDE is warning about that (probably because there is a user setting to 4 spaces) is unrelated to the traceback you are getting. If you care about the IDE warning you should seek out support for that IDE.

If you think the exception you are getting is related to uv, you need to provide steps on how you got to that exception for someone to be able to help you.

---

_Closed by @charliermarsh on 2024-12-27 14:39_

---
